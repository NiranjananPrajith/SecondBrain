# RVR Chat: Patreon-Only Migration — Post-Audit Fix Prompt

## Context

The Patreon-only auth migration was implemented. This document is the post-migration audit findings. An AI coding agent should fix the issues listed below.

**Reference:** `SecondBrain/projects/RVR/auth-payments-comparison.md` and `SecondBrain/projects/RVR/migration-prompt.md` for migration context.

---

## Critical Issues (Must Fix)

These are runtime errors or broken functionality — `oauthUser` is always `undefined` since the `oauth` session type was removed from `SessionData`, but the code still reads it.

### 1. `src/components/QuotaBar.tsx` — `oauthUser` and `stripeSubscription` references

**Lines 175, 177, 303-313, 326:**
```tsx
// Line 175: stripeSubscription still destructured from balance
const { daily, patreonBonus, stripeSubscription, purchased } = balance;

// Line 177: oauthUser still referenced — THIS IS A BUG
const tierName = authStatus.patreonUser?.tierName || authStatus.oauthUser?.tierName || 'Free';

// Lines 303-313: stripeSubscription UI elements still rendered in the popover
// (The entire stripeSubscription block in the popover should be removed)

// Line 326: stripeSubscription still in the credits array
const credits: CreditPool[] = [
  { type: 'daily', ... },
  { type: 'patreonBonus', ... },
  { type: 'stripe', label: stripeSubscription.sourceLabel, limit: stripeSubscription.limit, used: stripeSubscription.used, remaining: stripeSubscription.remaining, isExternal: true },
  { type: 'purchased', ... },
];
```

**Fix required:**
- Change line 177 to: `const tierName = authStatus.patreonUser?.tierName || 'Free';`
- Remove `stripeSubscription` from the destructuring on line 175
- Remove the entire `stripeSubscription` block from the credits array (line ~326) — delete the entry with `type: 'stripe'`
- Remove all `stripeSubscription` UI elements in the popover (lines 303-313) — these are the popover items showing Stripe subscription details
- The credits array should only have: `daily`, `patreonBonus`, `purchased`

---

### 2. `src/app/scenario/[id]/ScenarioPageContent.tsx` — `oauthUser` in owner check

**Line 333:**
```tsx
isOwner={!!(authStatus?.isLoggedIn) && (authStatus?.oauthUser?.userId === card?.userId || authStatus?.patreonUser?.userId === card?.userId)}
```

**Fix required:**
Change to:
```tsx
isOwner={!!(authStatus?.isLoggedIn) && authStatus?.patreonUser?.userId === card?.userId}
```
(Remove the `oauthUser` part since OAuth sessions no longer exist)

---

### 3. `src/app/u/[username]/PublicUserProfileContent.tsx` — `oauthUser` in owner check

**Line 142:**
```tsx
const isOwner = authStatus?.oauthUser?.userId === profile?.id || authStatus?.patreonUser?.userId === profile?.patreonId;
```

**Fix required:**
Change to:
```tsx
const isOwner = authStatus?.patreonUser?.userId === profile?.patreonId;
```
(The `profile?.id` was the Prisma UUID for OAuth users — Patreon users use `patreonId`)

---

## High-Priority Issues

### 4. `src/components/QuotaBar.tsx` — Import of `CreditBalances` type may have `stripeSubscription`

Read the file and verify the `CreditBalances` interface used in this file still has `stripeSubscription`. If it does, the type should be updated. The interface in `src/lib/credit-balances.ts` should only have:
```typescript
interface CreditBalances {
  daily: { limit: number; used: number; remaining: number; };
  patreonBonus: { limit: number; used: number; remaining: number; };
  purchased: { limit: number; used: number; remaining: number; };
}
```

---

### 5. `src/app/dashboard/page.tsx` — Dead `stripeSubscription` destructuring

**Line 340:**
```tsx
const { daily, patreonBonus, stripeSubscription, purchased } = balance;
```

`stripeSubscription` is destructured but NOT used in the template. Only `patreonBonus` and `purchased` are rendered on line 519.

**Fix required:**
Remove `stripeSubscription` from the destructuring:
```tsx
const { daily, patreonBonus, purchased } = balance;
```

---

## Medium-Priority Issues

### 6. `src/lib/credits.ts` — Dead `subscription*` fields in `CreditBalances` interface

**Lines 20-23:**
The `CreditBalances` interface has unused `subscription`, `subscriptionUsed`, `subscriptionLimit`, `subscriptionRemaining` fields. These were for the Stripe subscription quota system which is now removed.

**Fix required:**
Remove these four fields from the `CreditBalances` interface. The interface should match the three credit pools: `daily`, `patreonBonus`, `purchased`.

---

### 7. `src/components/dashboard/AccountSettings.tsx` — Dead `googleId`/`discordId` in user prop type

**Lines 26-27:**
```typescript
// In the UserForSettings interface:
googleId: string | null;
discordId: string | null;
```

These are never rendered or used in the component.

**Fix required:**
Remove `googleId` and `discordId` from the interface. Also check if the interface still references `emailAuthId` — remove that too if present.

---

### 8. `src/app/dashboard/page.tsx` — Still passing dead props to AccountSettings

**Lines 676-677:**
```tsx
googleId: userProfile?.googleId,
discordId: userProfile?.discordId,
```

These are passed to `AccountSettings` but the component no longer uses them.

**Fix required:**
Remove `googleId` and `discordId` from the `user` prop passed to `AccountSettings`. Also check for and remove `primaryLoginProvider` if it's no longer meaningful (though it can be left as-is since it's hardcoded to `'patreon'`).

---

### 9. `src/app/api/user/profile/route.ts` — Still selecting dead OAuth fields

**Lines 31-32, 60-61:**
```typescript
select: {
  googleId: true,
  discordId: true,
  ...
}
// AND in the fallback response:
googleId: null,
discordId: null,
```

**Fix required:**
Remove `googleId` and `discordId` from the `select` object. Remove `googleId: null` and `discordId: null` from the fallback response. Keep all other profile fields (`id`, `patreonId`, `email`, `personaName`, `avatarUrl`, `bio`, `tierName`, `messageLimit`, `credits`, `creditsExpiresAt`, `stripeCustomerId`, `theme`, `voicePreference`, `username`, `displayName`, `isPublic`, `publicCardsCount`, `totalLikesReceived`).

---

### 10. `src/lib/cache-constants.ts` — Dead OAuth cache key constants

**Lines 26, 29:**
```typescript
DAILY_QUOTA_OAUTH = 'quota:oauth:{userId}:{ISTDate}'
MONTHLY_QUOTA_OAUTH = 'quota:oauth:bonus:{userId}'
```

No file in `src/` calls these keys — they are dead code.

**Fix required:**
Remove the two OAuth cache key constants. Keep `DAILY_QUOTA_PATREON` and `MONTHLY_QUOTA_PATREON`.

---

## Lower-Priority Issues (Stale but Not Broken)

### 11. `tests/setup/vitest-setup.ts` — Stale `oauth: null` in mock session

**Line 57:**
```ts
oauth: null, // still present in mock session
```

The mock session still has an `oauth` key that will always be null.

**Fix required:**
Remove the `oauth: null` line from the mock session object. The mock should only have `patreon`, `guest`, or both.

---

### 12. E2E tests for deleted OAuth routes — `tests/e2e/auth.spec.ts`

**Lines 5-38:**
```ts
googleTest('Google OAuth login flow'...)  // tests deleted /api/auth/google route
discordTest('Discord OAuth login flow'...)  // tests deleted /api/auth/discord route
```

These tests reference the deleted `/api/auth/google` and `/api/auth/discord` routes.

**Fix required:**
Remove the `googleTest` and `discordTest` blocks from `tests/e2e/auth.spec.ts`. Keep only the Patreon OAuth login flow test. Rename the test file or the describe block if appropriate — the file is still named `auth.spec.ts` which is fine.

---

### 13. `OAuthButtons` component — Stale name but functionally correct

The component `src/components/OAuthButtons.tsx` only renders a Patreon button now, but is still imported with the "OAuth Login Buttons" label in `LoginModal.tsx` and `PatreonTiersModal.tsx`.

**Fix optional (lower priority):**
- Rename the component to `PatreonLoginButton.tsx` or similar
- Update the label in the modals from "OAuth Login Buttons" to "Login with Patreon"
- This is cosmetic — the component works correctly

---

### 14. Legacy OAuth comments in `src/app/api/auth/patreon/callback/route.ts`

**Lines 159, 165:**
Comments still read:
- "No user with this patreonId - check if email exists **(OAuth user linking Patreon)**"
- "**Case 2: OAuth user linking Patreon for the first time**"

These comments describe a flow that no longer exists (linking an old email/Google/Discord account to Patreon). The actual code at lines 159-179 is a self-healing fallback for Patreon users who somehow don't have a `patreonId` in our DB — it looks up by email as a fallback.

**Fix optional:**
Update the comments to reflect the current Patreon-only reality:
- Line 159: Change comment to something like "No user with this patreonId - check if email exists as fallback for existing Patreon users"
- Line 165: Change comment to something like "**Case 2: Patreon user with no existing record, email found — create new Patreon user**"

---

### 15. Documentation files — Stale OAuth references (lower priority)

**`README.md` (lines 20, 36):** Now correctly says "Auth: iron-session with Patreon OAuth only + guest mode" — this is accurate, no change needed.

**`LLMs.md`:** Multiple references to OAuth. Since LLMs.md is a project documentation file, these should be updated to reflect Patreon-only auth. Check and update:
- Line 9, 19, 53, 90, 296 — any references to "primary OAuth entry point" or Google/Discord/Email OAuth

**`CLAUDE.md` (lines 10, 59):** Check if "Patreon OAuth only" is already correctly stated — should be accurate.

These documentation fixes are lower priority but should be addressed so future developers aren't confused.

---

## Summary — Fix Priority

| Priority | Issues | Files |
|----------|--------|-------|
| **Critical** | `oauthUser` always undefined (runtime bug) | `QuotaBar.tsx`, `ScenarioPageContent.tsx`, `PublicUserProfileContent.tsx` |
| **High** | `stripeSubscription` dead destructuring in dashboard | `dashboard/page.tsx` |
| **Medium** | Dead OAuth fields in profile API, cache constants, credits interface, AccountSettings props | `profile/route.ts`, `cache-constants.ts`, `credits.ts`, `AccountSettings.tsx`, `dashboard/page.tsx` |
| **Lower** | Stale test mocks, E2E tests for deleted routes, component naming, comments | `vitest-setup.ts`, `auth.spec.ts`, `OAuthButtons.tsx`, `patreon/callback/route.ts`, docs files |

---

## Files to Modify

1. `src/components/QuotaBar.tsx` — Critical fix
2. `src/app/scenario/[id]/ScenarioPageContent.tsx` — Critical fix
3. `src/app/u/[username]/PublicUserProfileContent.tsx` — Critical fix
4. `src/app/dashboard/page.tsx` — High priority fix
5. `src/lib/credits.ts` — Medium priority
6. `src/components/dashboard/AccountSettings.tsx` — Medium priority
7. `src/app/api/user/profile/route.ts` — Medium priority
8. `src/lib/cache-constants.ts` — Medium priority
9. `tests/setup/vitest-setup.ts` — Lower priority
10. `tests/e2e/auth.spec.ts` — Lower priority
11. `src/app/api/auth/patreon/callback/route.ts` — Optional (comments only)
12. `README.md`, `LLMs.md`, `CLAUDE.md` — Optional (docs)

---

## Verification After Fixes

1. **Runtime check**: Visit any page — no console errors about `oauthUser` being undefined
2. **QuotaBar**: Shows only Daily (30), Patreon Bonus, Purchased — no Stripe entry
3. **Scenario page**: Owner badge shows correctly for Patreon-only logged-in users
4. **Profile page**: Owner badge shows correctly on public profiles
5. **E2E tests**: `auth.spec.ts` only tests Patreon OAuth flow — no broken test references
6. **Dashboard**: No dead `stripeSubscription` destructuring
7. **Vitest tests**: Mock session only has `patreon` and `guest` — no `oauth: null`
