# RVR Chat: Post-Audit TypeScript Fixes — AI Coding Agent Prompt

## Context

TypeScript compilation (`npx tsc --noEmit`) reveals 6 remaining errors from the Patreon-only auth migration. Fix all issues below. After fixing, verify with `npx tsc --noEmit` (must be zero errors), `npm test` (tests must pass), and `npm run build` (build must succeed).

---

## Issue 1: `src/lib/credits.ts` — Dead `subscriptionRemaining` reference

**File:** `src/lib/credits.ts`
**Line:** 90
**Error:** `Property 'subscriptionRemaining' does not exist on type 'CreditBalances'`

**Root cause:** `calculateTotalRemaining` function references `balances.subscriptionRemaining` but the `CreditBalances` interface (lines 15-24) only has: `daily`, `dailyUsed`, `dailyLimit`, `dailyRemaining`, `premium`, `premiumUsed`, `premiumRemaining`, `totalRemaining`.

**Fix:**
```typescript
// Line 90 - Change FROM:
return balances.dailyRemaining + balances.subscriptionRemaining + balances.premiumRemaining;

// TO:
return balances.dailyRemaining + balances.premiumRemaining;
```

---

## Issue 2: `src/lib/credits.test.ts` — Non-existent fields in test data

**File:** `src/lib/credits.test.ts`
**Lines:** 60, 61, 62, 63 (first object), 81, 82, 83, 84 (second object), 100, 101, 102, 103 (third object)
**Error:** `Object literal may only specify known properties, and 'subscription' does not exist in type 'CreditBalances'`

**Root cause:** Three test objects include `subscription`, `subscriptionUsed`, `subscriptionLimit`, `subscriptionRemaining` fields that do not exist in the `CreditBalances` interface.

**Fix:** Remove these four fields from all three test balance objects:

**First object (lines ~55-68):** Remove lines 60-63:
```typescript
subscription: 100,
subscriptionUsed: 500,
subscriptionLimit: 2500,
subscriptionRemaining: 2000,
```

**Second object (lines ~76-89):** Remove lines 81-84:
```typescript
subscription: 0,
subscriptionUsed: 2500,
subscriptionLimit: 2500,
subscriptionRemaining: 0,
```

**Third object (lines ~95-108):** Remove lines 100-103:
```typescript
subscription: 0,
subscriptionUsed: 100,
subscriptionLimit: 2500,
subscriptionRemaining: 2400,
```

Update the expected sums in the tests accordingly:
- First test: `expect(result).toBe(10 + 2000 + 40)` → `expect(result).toBe(10 + 40)` = 50
- Third test: `expect(result).toBe(2433)` → recalculate: `25 + 8` = 33

---

## Issue 3: `src/app/dashboard/page.tsx` — Undefined `stripeSubscription`

**File:** `src/app/dashboard/page.tsx`
**Line:** 519
**Error:** `Cannot find name 'stripeSubscription'`

**Root cause:** `stripeSubscription` is referenced in the conditional `{(patreonBonus || stripeSubscription || purchased.credits > 0)` but was never defined in this component. The `stripeSubscription` variable was already removed from destructuring at line 340 but the conditional was not updated.

**Fix:**
```typescript
// Line 519 - Change FROM:
{(patreonBonus || stripeSubscription || purchased.credits > 0) ? (

// TO:
{(patreonBonus || purchased.credits > 0) ? (
```

---

## Issue 4: `src/app/api/chat/voice/route.ts` — Wrong variable name

**File:** `src/app/api/chat/voice/route.ts`
**Line:** 89
**Error:** `Cannot find name 'userId'. Did you mean 'user'?`

**Root cause:** `userId` is used but should be `patreonUserId`. The variable `userId` is not defined in this scope — `patreonUserId` is the correct variable containing the Patreon user ID.

**Fix:**
```typescript
// Line 89 - Change FROM:
if (tierName === 'Insider') monthlyKey = `quota:patreon:${userId}:${getISTMonth()}`;

// TO:
if (tierName === 'Insider') monthlyKey = `quota:patreon:${patreonUserId}:${getISTMonth()}`;
```

---

## Issue 5: `src/app/HomePageContent.tsx` — Invalid prop passed to UpgradeModal

**File:** `src/app/HomePageContent.tsx`
**Line:** ~422 (search for `onContinueWithEmail` usage with UpgradeModal)
**Error:** `Property 'onContinueWithEmail' does not exist on type 'IntrinsicAttributes & Props'`

**Root cause:** `UpgradeModal` component does not accept an `onContinueWithEmail` prop, but it is being passed one. The `onContinueWithEmail` prop is only valid on `UserStatusModal`.

**Fix:** Remove the `onContinueWithEmail` prop from the `UpgradeModal` component usage. The current usage at line ~422 passes `onContinueWithEmail={() => {...}}` to `UpgradeModal` — remove this prop entirely.

---

## Issue 6: `src/components/modals/UserStatusModal.tsx` — Same invalid prop

**File:** `src/components/modals/UserStatusModal.tsx`
**Line:** ~314 (search for `onContinueWithEmail` usage with UpgradeModal)
**Error:** `Property 'onContinueWithEmail' does not exist on type 'IntrinsicAttributes & Props'`

**Root cause:** Same issue — `UpgradeModal` doesn't accept `onContinueWithEmail` but it is being passed one.

**Fix:** Remove the `onContinueWithEmail` prop from the `UpgradeModal` component inside `UserStatusModal`. The `UserStatusModal` receives `onContinueWithEmail` as a prop (for its own use) but should NOT pass it to `UpgradeModal`.

---

## Summary

| # | File | Line | Fix |
|---|------|------|-----|
| 1 | `src/lib/credits.ts` | 90 | Remove `+ balances.subscriptionRemaining` |
| 2 | `src/lib/credits.test.ts` | 60,81,100 | Remove `subscription*` fields from test objects |
| 3 | `src/app/dashboard/page.tsx` | 519 | Remove `\|\| stripeSubscription` |
| 4 | `src/app/api/chat/voice/route.ts` | 89 | `userId` → `patreonUserId` |
| 5 | `src/app/HomePageContent.tsx` | ~422 | Remove `onContinueWithEmail` prop |
| 6 | `src/components/modals/UserStatusModal.tsx` | ~314 | Remove `onContinueWithEmail` prop |

## Verification Steps

After making all fixes:

1. **TypeScript check:** `npx tsc --noEmit` — must output zero errors
2. **Tests:** `npm test` — all tests must pass
3. **Build:** `npm run build` — build must complete successfully

If any errors remain, fix them until all three verification steps pass.
