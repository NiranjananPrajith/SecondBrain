# Tech Tools — Research & Findings

> Tools and platforms evaluated for potential use in RVR and CuratorHub.

---

## Blacksmith.sh

**What it is:** AI API gateway and infrastructure platform — unified interface to 30+ LLM providers, caching, fallbacks, analytics.

**Key features:**
- Unified API to OpenAI, Anthropic, Google, Meta, Mistral, etc.
- Intelligent LLM response caching (reduce costs/latency)
- Automatic fallback to backup models
- Prompt versioning and management
- Usage tracking and cost monitoring per model

**Pricing:** Usage-based, pay per token. Free tier available.

**RVR Fit:** Medium — useful for AI optimization later. Might be overkill if AIML's infrastructure is already stable. Good for reducing AI costs at scale.

**Link:** https://www.blacksmith.sh/

**Status:** Monitor — revisit if AIML costs become a concern.

---

## Clerk

**What it is:** Complete authentication platform for modern web apps. Handles signup, login, MFA, SSO, user management, and organization management out of the box.

**Key features:**
- Pre-built components (drop-in sign-in, sign-up, profile)
- Multi-factor auth (TOTP, SMS, passkeys)
- Enterprise SSO / SAML
- Social login (Google, GitHub, Microsoft, Apple)
- User management dashboard
- Organization/team support
- Webhooks + Actions for database sync

**Pricing:** Free up to 10,000 MAUs, then $0.02/month per MAU after.

**RVR Fit:** High — could replace/supplement Supabase Auth in future. Polished UI components could simplify login significantly. Strong candidate after current Supabase migration is complete.

**Link:** https://clerk.com/

**Status:** Worth exploring when RVR grows. Keep in mind for auth evolution post-Supabase.

---

_Investigated: 2026-04-08_