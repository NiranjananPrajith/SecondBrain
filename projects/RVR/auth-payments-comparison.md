---
title: "RVR Auth & Payments: Patreon Only vs Supabase+Stripe Comparison"
created: 2026-04-17
tags: [rvr, auth, payments, patreon, stripe, supabase, analysis]
---

# Auth & Payments Architecture Comparison

> [!info]
> This note compares **Setup 1 (Patreon Only)** vs **Setup 2 (Current: Supabase + Patreon + Stripe)** for RVR Chat authentication and payments.

---

## Decision

> [!success]
> **Decision: Migrate to Patreon-only auth.** Based on answers gathered 2026-04-17.

### Chosen Approach

| Area | Decision |
|------|----------|
| **Auth** | Patreon-only login flow. Users create/sign in via Patreon OAuth, which offers Google, Apple, Facebook, or email — so multi-provider is unnecessary |
| **Email verification** | Required (Patreon's OAuth guarantees verified email) |
| **Payments** | Patreon pledges for recurring credit tiers. One-time purchases: research Patreon Shop API integration; if not feasible, fall back to Stripe for one-time credit purchases only |
| **Account management** | Link to Patreon settings pages (`/settings/basics`, `/settings/account`) — no custom account management code |
| **Existing users** | Paid users: manual migration to Patreon accounts. Free users: must link Patreon to continue using the service |
| **Database schema** | Simplified to `patreonId` + `email` + `name` + `avatar` (from Patreon). Remove `googleId`, `discordId`, `emailAuthId` fields and related Supabase OAuth code |
| **Stripe** | Kept as fallback for one-time credit purchases if Patreon Shop integration isn't viable |

### Pending Research

- **Patreon Shop API**: Can Shop one-time purchases be linked to rvr.chat via API/webhook? If not, Stripe one-time purchases remain as fallback.

---

## Quick Summary

| Aspect | Setup 1: Patreon Only | Setup 2: Supabase + Patreon + Stripe |
|--------|---------------------|--------------------------------------|
| **Auth Providers** | Patreon only | Google, Discord, Patreon, Email |
| **Payments** | Patreon | Stripe + Patreon (bonus credits) |
| **Account Management** | Patreon handles all | We must implement |
| **Engineering Burden** | Low | High |
| **User Experience** | Simple, fewer choices | More options, more complexity |

---

## Setup 1: Patreon Only

### How It Works

1. User clicks Login/Signup → taken to Patreon OAuth flow
2. Patreon offers: Google, Apple, Facebook, or email/password
3. Upon completion, user gets a free RVR account with initial credits
4. When credits exhaust, user pledges on Patreon → gets bonus credits
5. All account management lives on Patreon

### What Patreon Handles

- Account creation (any of their OAuth providers)
- Email verification
- Password management (reset, change)
- Login security (2FA if enabled on Patreon)
- Subscription management (upgrade, downgrade, cancel)
- Payment processing
- Refunds and disputes
- Email notifications
- GDPR compliance (data portability, deletion requests)

### Pros

1. **Zero backend account code** — No password reset flow, no email verification, no username management
2. **No payment infrastructure** — No Stripe webhook handlers, no checkout sessions, no billing portal
3. **No dispute resolution** — Chargebacks and disputes go through Patreon
4. **Free identity verification** — Patreon's OAuth includes email verification
5. **Simpler database schema** — No need for `emailAuthId`, `googleId`, `discordId`, etc.
6. **Less GDPR exposure** — Patreon handles data requests
7. **Automatic email deliverability** — Patreon emails don't get spam-filtered
8. **Patreon integration is a feature** — For some users, already being a Patreon supporter is a loyalty signal

### Cons

1. **Patreon as the only login option** — Users who aren't Patreon users must create a Patreon account just to use RVR Chat
2. **No direct credit purchase** — Must go through Patreon pledge cycle
3. **Less control over UX** — Can't customize the login flow or account settings
4. **Patreon takes a cut** — Patreon takes 5-12% of payments
5. **Dependent on Patreon uptime** — Login down = RVR Chat down
6. **No separate Stripe subscription** — Can't offer Stripe-only tiers or promotions
7. **Limited email outreach** — Can't send transactional emails directly (welcome, usage alerts)

---

## Setup 2: Current (Supabase + Patreon + Stripe)

### How It Works

1. User clicks Login/Signup → LoginModal with Google, Discord, Patreon, Email options
2. User authenticates via chosen provider (Supabase handles email/password, OAuth for others)
3. Account created in both Supabase Auth (identity) and Prisma DB (app data)
4. Initial credits granted (30 free credits)
5. When credits exhaust → Stripe Checkout (subscription or one-time credits)
6. Patreon bonus credits applied on top (additive, not exclusive)
7. Account settings managed via custom API endpoints

### What We Must Handle

- Email verification (or skip via `email_confirm: true`)
- Password reset flow
- Username registration and validation
- Profile management (avatar, display name, bio)
- Connected account linking/disconnection
- Stripe webhook reliability (retry logic, ordering)
- Subscription status reconciliation (DB + Redis + Stripe)
- Customer portal integration
- Payment failure alerts
- Refund requests
- GDPR data export/deletion requests

### Pros

1. **Multiple login providers** — Google, Discord, email — no forced Patreon account
2. **Direct credit purchases** — Users can buy credits without subscription
3. **Flexible pricing** — Stripe-only tiers independent of Patreon
4. **No Patreon cut on direct purchases** — 0% fees on one-time credit buys
5. **More user data** — We own the user record, not just linked to Patreon
6. **Promotional pricing** — Can offer discounts, trials via Stripe directly
7. **Cross-domain SSO** — Patreon login syncs to `redvelvetrenders.com` via Redis token handoff
8. **Account merging** — Users can link Patreon to existing OAuth accounts

### Cons

1. **Significantly more code** — Auth routes for 4 providers, payment routes, webhook handlers
2. **Account management burden** — Password reset, username validation, profile APIs
3. **Stripe webhook complexity** — Idempotency, retry logic, event ordering
4. **Database schema complexity** — Multiple OAuth ID fields, subscription fields
5. **GDPR compliance** — Must implement data export, account deletion ourselves
6. **Email deliverability** — Supabase email auth can have spam issues
7. **Two payment systems** — Stripe AND Patreon must stay in sync
8. **Redis dependency** — Session sync across domains relies on Redis
9. **Debugging harder** — Two auth systems + two payment systems + webhooks

---

## Detailed Comparison by Category

### Authentication

| Feature | Setup 1 (Patreon) | Setup 2 (Supabase) |
|--------|-------------------|-------------------|
| Login providers | Google, Apple, Facebook, Email (via Patreon) | Google, Discord, Patreon, Email (direct) |
| Email verification | Automatic via Patreon | Bypassed (`email_confirm: true`) or manual |
| Password reset | Patreon handles | Supabase handles |
| Session management | iron-session + Patreon tokens | iron-session + Supabase session |
| Account linking | N/A | Patreon can link to existing OAuth account |
| Cross-domain SSO | Via Redis token handoff | Via Redis token handoff |

### Payments

| Feature | Setup 1 (Patreon) | Setup 2 (Supabase) |
|--------|-------------------|-------------------|
| Recurring subscriptions | Via Patreon pledge | Via Stripe subscription |
| One-time credits | Via Patreon pledge add-on | Via Stripe Checkout |
| Bonus credits from Patreon | Automatic | Automatic (additive) |
| Payment fees | Patreon takes 5-12% | Stripe takes 2.9% + $0.30 |
| Webhook handling | Patreon webhooks only | Stripe webhooks + Patreon webhooks |
| Refunds | Patreon handles | We must handle via Stripe |
| Failed payment alerts | Patreon handles | We should implement (webhook has empty handler) |
| Billing portal | N/A | Via Stripe Customer Portal |

### Account Management

| Feature | Setup 1 (Patreon) | Setup 2 (Supabase) |
|--------|-------------------|-------------------|
| Username | N/A | We implement (`/api/user/username`) |
| Email change | N/A | Email-password users only |
| Password change | N/A | Via Supabase |
| Connected accounts | N/A | Connect/disconnect Google, Discord, Patreon |
| Profile (bio, avatar) | N/A | We implement |
| Account deletion | Via Patreon | We must implement (GDPR) |
| Data export | Via Patreon | We must implement (GDPR) |

### Engineering Effort

| Area | Setup 1 (Patreon) | Setup 2 (Supabase) |
|------|-------------------|-------------------|
| Auth routes | ~4 (Patreon login/callback/connect/logout) | ~12 (4 providers × 2-3 routes each) |
| Payment routes | 0 | ~5 (checkout, subscription, portal, webhook, update) |
| Account routes | 0 | ~6 (profile, username, avatar, display-name, disconnect) |
| Database schema | Simple | Complex (multiple IDs, subscription tracking) |
| External webhooks | Patreon only | Patreon + Stripe |
| Rate limiting | Basic | Multiple limiters per flow |
| Testing surface | Smaller | Larger (more flows, more providers) |

---

## The Core Trade-off

```
Setup 1 (Patreon Only):          Setup 2 (Supabase + Stripe):
┌─────────────────────────┐    ┌─────────────────────────────┐
│ Patreon owns the user    │    │ We own the user record      │
│ Patreon handles payments │    │ We handle payments (Stripe)│
│ Patreon handles support  │    │ We handle disputes          │
│ Low engineering burden   │    │ High engineering burden     │
│ User must use Patreon     │    │ User can use any provider   │
│ Patreon takes a cut       │    │ We keep more revenue        │
└─────────────────────────┘    └─────────────────────────────┘
```

---

## Key Questions to Decide

1. **Revenue priority** — Is the extra revenue from no Patreon cut worth the engineering cost?
2. **User convenience** — Is requiring Patreon a real barrier, or do users not mind?
3. **Engineering capacity** — Can we reliably maintain two payment systems?
4. **Dispute tolerance** — Are we ready to handle chargebacks and refund requests ourselves?
5. **GDPR readiness** — Do we have a plan for account deletion and data export?

## Answers (2026-04-17)

| Question | Answer |
|----------|--------|
| Auth provider preference | Keep it simple. Patreon offers Google/Apple/Facebook/Email via OAuth — same provider choice as current setup, without our own auth infrastructure |
| Email verification | Require verification — Patreon OAuth provides this automatically |
| Payment model | Patreon for all subscriptions. Patreon Shop for one-time purchases (pending research). Stripe remains as fallback for one-time credit purchases only if Shop doesn't work |
| Account management | Link to Patreon settings pages (`/settings/basics`, `/settings/account`) — no custom code needed |
| Existing non-Patreon users | Paid users: manual migration. Free users: must connect Patreon to continue |
| Database schema | Simplify to Patreon ID + email + name + avatar (from Patreon). Remove Supabase OAuth fields |
| Patreon Shop | Not yet confirmed integrable — need further research before deciding |

---

## Related

- [[projects/red-velvet-renders]] — RVR project overview
- [[projects/RVR/patreon/patreon-post-instructions]] — Brand voice for Patreon posts
- [RVR Chat LLMs.md](../../VS_Code/redvelvetrenders/chat.redvelvetrenders.com/LLMs.md) — Technical architecture doc
