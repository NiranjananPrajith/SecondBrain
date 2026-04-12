---
title: RVR Stripe Payment Routes — UUID Validation Fix
created: 2026-04-11
tags: [rvr, bug-fix, stripe, security]
---

# RVR Stripe Payment Routes — UUID Validation Fix

**Date:** 2026-04-11
**Status:** COMPLETED ✅

## Bugs Fixed

### 1. Foreign Key Crash on UUID lookups (Critical)
No UUID format validation before passing `userId` to `prisma.user.findUnique({ where: { id: rawUserId } })`. A Patreon ID (non-UUID string) passed to the `id` field would crash Prisma before the fallback `patreonId` lookup.

**Fix:** Added `isUUID` regex check before each UUID-based lookup in all 4 routes.

### 2. Stripe SDK Validation Error on null customer
In `create-checkout`, passing `customer: null` to Stripe's `checkout.sessions.create()` caused a Stripe SDK validation error.

**Fix:** Changed `customer: user?.stripeCustomerId` → `customer: user?.stripeCustomerId || undefined`.

### 3. Inconsistent User Lookup
`create-checkout` and `customer-portal` only looked up by `id` — missed Patreon users not yet migrated. Inconsistent with `create-subscription` and `webhook` which used dual lookup (UUID → patreonId).

**Fix:** Unified all 4 routes to UUID-first dual lookup pattern.

## Files Modified

- `src/app/api/payments/create-checkout/route.ts` ✅
- `src/app/api/payments/create-subscription/route.ts` ✅
- `src/app/api/payments/customer-portal/route.ts` ✅
- `src/app/api/payments/webhook/route.ts` ✅

## isUUID Helper (all 4 files)

```typescript
const isUUID = (id: string) => /^[0-9a-f]{8}-[0-9a-f]{4}-[1-5][0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$/i.test(id);
```

## Verification

- ✅ `npm run build` — passes, no TypeScript errors
- All 4 routes compile successfully
