---
title: RVR Chat Patreon-Only Auth Migration Prompt
created: 2026-04-17
archived: 2026-04-17
tags: [archived, rvr, migration]
status: archived
---

## Context

RVR Chat is migrating from a hybrid auth system (Supabase O'Auth + Patreon + Stripe) to **Patreon-only auth**. The goal is to reduce engineering burden — Patreon handles all account management, email verification, password reset, and subscription billing. Stripe is retained only for one-time credit purchases as a fallback pending Patreon Shop research.

**Decision made 2026-04-17.** See `SecondBrain/projects/RVR/auth-payments-comparison.md` for full decision context.

---

## High-Level Summary of Changes

| Area | Change |
|------|--------|
| **Auth** | Remove Google/Discord/Email (Supabase) login. Keep only Patreon OAuth. |
| **Payments** | Remove Stripe recurring subscriptions. Keep Stripe one-time credit purchases as fallback. Patreon handles all recurring credit tiers. |
| **Account management** | Remove custom account settings (username, profile, connected accounts). Replace with links to Patreon settings pages. |
| **Existing users** | Manual migration for paid users. Free users must link Patreon to continue. |
| **Database** | Keep schema as-is (don't drop columns). Existing Supabase OAuth fields remain but are dormant. |

---

## Phase 1: Auth Infrastructure

### 1.1 Files to DELETE (entirely)

Delete all Supabase OAuth routes and email auth:

```
src/app/api/auth/google/route.ts
src/app/api/auth/google/callback/route.ts
src/app/api/auth/discord/route.ts
src/app/api/auth/discord/callback/route.ts
src/app/api/auth/email/login/route.ts
src/app/api/auth/email/signup/route.ts
src/app/api/auth/email/forgot-password/route.ts
src/app/api/auth/email/reset-password/route.ts
src/app/api/auth/set-session/route.ts
src/components/modals/ForgotPasswordModal.tsx
src/app/reset-password/page.tsx
src/app/api/auth/patreon/connect/route.ts
src/app/api/auth/patreon/connect/callback/route.ts
```

### 1.2 Files to MODIFY

#### `src/components/OAuthButtons.tsx`
- Remove Google button, Discord button, inline email form entirely
- Remove all `emailMode` state — simplify to single "Login with Patreon" button
- The button links to `/api/auth/patreon/login`
- Remove `handleEmailSubmit`, `handleEmailButtonClick` and all email-related logic
- Props: remove `showEmail`. The component should only render the Patreon button

#### `src/components/modals/LoginModal.tsx`
- Remove `ForgotPasswordModal` import and conditional render
- Update modal text to reflect Patreon-only login (e.g., "Sign in to RVR Chat via Patreon")
- The modal should just show the simplified `OAuthButtons` component with only Patreon

#### `src/lib/session.ts`
- Remove the entire `oauth?: { ... }` interface from `SessionData`
- The `primaryLoginProvider` type loses `'google' | 'discord' | 'email'` — keep only `'patreon'` (or remove the field entirely from the patreon session since it's always 'patreon')
- Keep `patreon` and `guest` session types

#### `src/lib/ratelimit.ts`
- Remove: `googleOAuthRateLimit`, `discordOAuthRateLimit`, `emailLoginRateLimit`, `emailSignupRateLimit`
- Keep all other rate limiters

#### `src/app/api/auth/patreon/status/route.ts`
- Remove the entire `if (session.oauth)` block
- Remove `determineOAuthTier()` function entirely — Patreon users use `tierName` directly
- Remove `oauthUser` from the response JSON — only return `patreonUser` data
- Remove all Stripe-related fields (`stripeSubscriptionId`, `stripePriceId`, `stripeCurrentPeriodEnd`, `stripeQuota`) from the response
- The response should be: `{ patreonUser: {...}, guest: {...}, redirectUrl }`

#### `src/app/api/auth/patreon/logout/route.ts`
- Simplify to only handle Patreon session logout
- Remove cross-domain SSO handoff (`/api/auth/sync-out` redirect to `redvelvetrenders.com`) — this was for linking Patreon to OAuth users on the main domain. Since OAuth is removed, cross-domain sync is no longer needed.

#### `src/app/api/user/account/disconnect/route.ts`
- Remove entirely — account linking/disconnection is no longer needed (users manage everything via Patreon)

### 1.3 Session Check Pattern: Update ALL API Routes

Every API route currently checks: `session.patreon?.userId || session.oauth?.userId`

Change all of these to only check `session.patreon?.userId`. Sessions with `oauth` will no longer exist.

Files needing this change (all `src/app/api/` routes that check auth):

```
src/app/api/chat/route.ts
src/app/api/chat/history/route.ts
src/app/api/chat/threads/route.ts
src/app/api/chat/voice/route.ts
src/app/api/chat/voice/cache-status/route.ts
src/app/api/chat/voice/download/[id]/route.ts
src/app/api/chat/migrate-guest/route.ts
src/app/api/user/profile/route.ts
src/app/api/user/username/route.ts
src/app/api/user/username/check/route.ts
src/app/api/user/display-name/route.ts
src/app/api/user/creations/route.ts
src/app/api/user/avatar-upload/route.ts
src/app/api/user/voice-upload/route.ts
src/app/api/user/voice-delete/route.ts
src/app/api/user/preferences/route.ts
src/app/api/cards/create/route.ts
src/app/api/cards/review/route.ts
src/app/api/cards/report/route.ts
src/app/api/reports/route.ts
src/app/api/reportaproblem/route.ts
src/app/api/analytics/track/route.ts
src/app/api/audio/voice/[...filename]/route.ts
src/app/api/debug/session/route.ts
src/app/api/admin/pending/route.ts
src/app/api/admin/moderate/route.ts
src/app/api/admin/reports/route.ts
src/app/api/admin/reports/[id]/route.ts
```

For each file: find the auth check pattern `session.patreon?.userId || session.oauth?.userId` and replace with `session.patreon?.userId`. Also remove any `session.oauth` references.

---

## Phase 2: Frontend — Remove OAuth References

### Components consuming `oauthUser` from `authStatus`

Update these components to only use `patreonUser`:

#### `src/components/Header.tsx`
- Remove `oauthUser` variable entirely
- Remove `isOAuthUser` check and any `oauthUser?.` references
- The `UserStatusDisplay` should only reference `authStatus?.patreonUser`
- Remove `effectiveTierName` override that references OAuth tier

#### `src/components/ChatInterface.tsx`
- Remove all `authStatus?.oauthUser` references
- Only pass `patreonUser` data to `useTTS`

#### `src/components/QuotaBar.tsx`
- Remove `type: 'stripe'` and `type: 'oauth'` entries from the credits array — keep only `daily`, `patreonBonus`, `purchased`
- The popover should only show these three credit pools

#### `src/components/WelcomeScreen.tsx`
- Remove any `oauthUser` auth status checks

#### `src/components/LeftSidebar.tsx`
- Remove `oauthUser` auth status checks

#### `src/components/modals/UserStatusModal.tsx`
- Remove `oauthUser` references

#### `src/hooks/useTTS.ts`
- Remove `oauthUser` / `session.oauth` branch

#### `src/lib/credit-balances.ts`
- Remove `oauthUser` from type checks in `calculateCreditBreakdown()` — the function should only handle `patreonUser` and `guest` sessions

#### `src/app/HomePageContent.tsx`
- Remove `oauthUser` from any `authStatus` destructuring
- Keep the `?payment=success&credits=X` URL param handling for Stripe one-time purchase redirects

#### `src/app/dashboard/page.tsx`
- Remove `oauthUser` from `authStatus` destructuring
- Remove "Stripe Subscription" block from Premium Credits panel
- Change upgrade links from `/upgrade#subscription` to `/upgrade#credits`

---

## Phase 3: Payments — Stripe as One-Time Fallback

### 3.1 Files to DELETE (Stripe subscription-only)

```
src/app/api/payments/create-subscription/route.ts
src/app/api/payments/customer-portal/route.ts
src/app/api/payments/update-subscription/route.ts
src/upgrade/components/SubscriptionCard.tsx
```

### 3.2 Files to MODIFY

#### `src/app/api/payments/webhook/route.ts`
- Strip out ALL subscription-related event handling:
  - Remove `customer.subscription.updated` handler
  - Remove `customer.subscription.deleted` handler
  - Remove `invoice.payment_succeeded` renewal handler
  - In `checkout.session.completed`: only process if `credits > 0` (one-time purchase). If `tierId` is present (subscription checkout), ignore it — no new Stripe subscriptions will be created
- Keep only one-time credit purchase fulfillment
- Remove Facebook CAPI calls for `Subscribe`, `Unsubscribe`, `Renewal` events — keep only `Purchase` event for one-time credits
- The webhook handler should remain functional for existing Stripe webhook events from historical subscriptions

#### `src/lib/stripe-subscription-config.ts`
- Remove `STRIPE_SUBSCRIPTION_TIERS` object (Basic/Pro/Ultra tier definitions)
- Remove `getStripeTierByPriceId()`, `getStripeQuotaKey()` functions
- Keep `STRIPE_CREDIT_PACKAGES` and `getCreditPackageByPriceId()` for one-time purchases

#### `src/lib/stripe-server.ts`
- Remove `subscriptions` and `billingPortal` accessors
- Keep `checkout`, `customers`, `webhooks` accessors
- Remove any `stripe.subscriptions.*` calls

#### `src/lib/facebook-capi-events.ts`
- Remove: `buildSubscribeEvent()`, `buildSubscriptionChangeEvent()`, `buildUnsubscribeEvent()`, `buildRenewalEvent()`
- Keep: `buildPurchaseEvent()` for one-time credit purchases

#### `src/app/api/auth/patreon/status/route.ts` (already flagged in Phase 1)
- Also remove `stripeQuota`, `stripeCurrentPeriodEnd`, `stripePriceId` from the response
- Remove all `getStripeQuotaKey` usage

#### `src/app/upgrade/UpgradePageContent.tsx`
- Remove all Stripe subscription tiers (Basic/Pro/Ultra 3-card grid)
- Remove `handleSubscribe`, `handleUpgrade`, `handleDowngrade` handlers
- Remove all `loadStripe`, `stripePromise`, `STRIPE_SUBSCRIPTION_TIERS` imports
- Keep only the "Buy Credits" section (credit package cards)
- The page should have no subscription section at all — links from Account Settings to `/upgrade` should go to `/upgrade#credits`
- The `UpgradeHero` component should not display Stripe subscription status

#### `src/upgrade/components/UserStatusCard.tsx`
- Remove the Stripe Subscription section entirely from this card
- The credit breakdown shown should be: Daily (30), Patreon Bonus (from tier), Purchased Credits (DB)

#### `src/upgrade/components/UpgradeHero.tsx`
- Remove `stripeSubscription` from the `balance` prop passed to `UserStatusCard`
- The hero should not advertise Stripe subscription benefits

#### `src/components/modals/UpgradeModal.tsx`
- Remove all Stripe subscription UI (3 tier cards, manage subscription link)
- Remove `/api/payments/create-subscription`, `/api/payments/update-subscription`, `/api/payments/customer-portal` calls
- Keep only the credit package purchase section (500–10,000 credits)
- Remove `loadStripe` import, `stripePromise`, `STRIPE_SUBSCRIPTION_TIERS`

#### `src/components/dashboard/AccountSettings.tsx`
- Change "Upgrade" button link from `/upgrade#subscription` to `/upgrade#credits`
- Remove "Disconnect Google/Discord" buttons from Connected Accounts section — Patreon connection is the only one
- The Connected Accounts section should only show Patreon status (or be removed entirely if there's nothing else to show)

---

## Phase 4: Account Settings — Link to Patreon

In `src/components/dashboard/AccountSettings.tsx`:
- Add links to Patreon settings pages:
  - Account details: `https://www.patreon.com/settings/basics`
  - Email/password: `https://www.patreon.com/settings/account`
- Remove all custom account management code (username change, email change, password change, connected account management)
- The settings page should point users to Patreon for anything account-related

---

## Phase 5: Chat Route — Credit Quota Logic

In `src/app/api/chat/route.ts`:
- Remove the Stripe subscription quota check path (`getStripeQuotaKey` + Redis lookup)
- The credit priority order becomes:
  1. Daily Free (30/day via Redis, key `quota:patreon:{userId}:{ISTDate}`)
  2. Patreon Bonus (500–5,000 via Redis, key `quota:patreon:bonus:{userId}`)
  3. Purchased Credits (DB `User.credits`, checked against `creditsExpiresAt`)
  4. Block request (403)
- All OAuth-specific credit logic (`session.oauth`) is removed — only `session.patreon` remains

In `src/lib/credit-balances.ts`:
- Update `calculateCreditBreakdown()` to only handle `patreonUser` session type
- Remove `stripeSubscription` from the returned balance object

---

## Phase 6: Database — Keep Schema, No Migrations

**Do NOT modify the Prisma schema or run any migrations.** The following fields remain in `prisma/schema.prisma` but are dormant:

- `googleId`, `discordId`, `emailAuthId` — no longer written, but may have values for existing users
- `primaryLoginProvider`, `secondaryLoginProvider` — no longer updated
- `stripeCustomerId`, `stripeSubscriptionId`, `stripePriceId`, `stripeCurrentPeriodEnd` — no longer written for new users, but existing Stripe subscribers still have values
- `isMember`, `creditsClaimedAt`, `patreonEmail`, `messageLimit` — these fields were already identified as dead code in the codebase, but leave them as-is

Existing users with Supabase OAuth accounts will need manual migration by the admin (Rowen). New signups will only create Patreon-based records.

---

## Implementation Order

1. **Phase 1 (Auth deletion)** — Delete OAuth route files first
2. **Phase 1 (Session cleanup)** — Update session.ts and rate limit, then update all API route auth checks
3. **Phase 2 (Frontend)** — Update components that consume `authStatus.oauthUser`
4. **Phase 3 (Payments)** — Delete Stripe subscription routes, modify webhook, update upgrade page
5. **Phase 4 (Account settings)** — Replace custom account management with Patreon links
6. **Phase 5 (Chat quota)** — Simplify credit quota logic
7. **Phase 6 (Database)** — No changes needed

---

## Existing Files Reference

- Session interface: `src/lib/session.ts`
- Patreon config (tiers, limits): `src/lib/patreon-config.ts`
- Patreon auth callback: `src/app/api/auth/patreon/callback/route.ts`
- Patreon webhook: `src/app/api/webhooks/patreon/route.ts`
- Stripe webhook: `src/app/api/payments/webhook/route.ts`
- Chat route (quota logic): `src/app/api/chat/route.ts`
- Auth status endpoint: `src/app/api/auth/patreon/status/route.ts`
- Credit breakdown: `src/lib/credit-balances.ts`
- Stripe subscription config: `src/lib/stripe-subscription-config.ts`
- Prisma schema: `prisma/schema.prisma`

---

## Verification

After implementation:

1. **Login flow**: Visit rvr.chat → LoginModal shows only "Continue with Patreon" button → clicking redirects to Patreon OAuth → after auth, user lands on site with Patreon session
2. **Credit quota**: Exhaust daily free (30) → Patreon bonus credits activate → exhaust those → purchased credits deduct from DB → 403 when all exhausted
3. **Upgrade page**: Visit /upgrade → only "Buy Credits" section visible (500/1000/3000/5000/10000 packages) → no subscription tiers
4. **Account settings**: Settings page shows Patreon account info with links to patreon.com/settings pages → no custom account forms
5. **Existing user migration**: An existing Google/Discord/Email user should be prompted to link their Patreon account to continue (handled manually per the migration plan)
6. **Stripe webhook**: A Stripe `checkout.session.completed` event for one-time credits still grants purchased credits (the existing webhook path should remain functional)
