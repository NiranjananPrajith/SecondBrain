---
title: current-priorities
created: 2026-04-09
tags: [status, active]
---

# Current Priorities (as of 2026-04-10)

## HIGH PRIORITY

### RVR Website — LAUNCH READY ✅
- **Supabase OAuth:** Complete (Google, Discord, Email login + guest migration)
- **Stripe Checkout:** Live (credit packages: 500/$4.99 → 10000/$74.99)
- **Website:** Public-ready at rvr.chat
- **Status:** LAUNCHING — Paid ad sprint active (see `projects/RVR/marketing/PaidAdSprint.md`)

### Marketing Month 1 — Community + Fandom Seeding
- **Goal:** 50 new subscribers
- **Ad Budget:** $100 (Reddit: $40, Instagram: $30, Reserve: $30) — X dropped to $0
- **Channels:** Reddit (community), Instagram (ads — SFW only), Discord, Patreon, X
- **Reddit account:** u/_rvrdev_ (6 post karma, 33 comment karma)
- **First post:** r/CharacterAi_NSFW — multilingual support announcement, 2.2k views, 1 upvote (15h ago)
- **Plan:** See `projects/RVR/marketing/CommunityAndFandomPlan.md`
- **NEW — Paid Sprint:** See `projects/RVR/marketing/PaidAdSprint.md` (parallel, Day 1-7)
  - Phase 0: Reddit account exists ✓, continue observing
  - Phase 1: 30 character cards (8 Bollywood, 10 mythology-inspired archetypes — no real deities, 5 anime, 5 modern, 2 fantasy)
  - Phase 2: Next Reddit post (voice demo or lore card showcase)
  - Phase 3: Discord recruitment after 2 weeks observing
  - Phase 4: Voice demo + content expansion (week 3-4)
  - Phase 7: Instagram setup + first SFW posts (Month 2)
- **SFW Policy:** Public content must be SFW-approved. Uncensored AI stays in private chats.
- **Status:** READY TO START

### Create 30-50 Public Scenario Cards
- **Progress:** 13 official scenarios in CustomCharacter model
- **Target:** 30 seed cards (see fandom plan for breakdown)
- **Status:** IN_PROGRESS

### Mobile App — Capacitor Evaluation
- **Plan:** `projects/RVR/mobile/CapacitorConversionPlan.md`
- **Key risk:** OAuth deep linking + cross-domain SSO from native webview
- **Next steps (in order):**
  1. Apple Developer Account ($99/year)
  2. Firebase project for push notifications
  3. Add `rvrchat://` URL scheme to server-side `isValidRedirect()` in 4 callback routes
  4. Test cross-domain SSO from mobile browser first
- **Status:** EVALUATION — not started

## PENDING TASKS

### Card Submission Guidelines — IN PROGRESS
- **Created:** `projects/RVR/content/CardSubmissionGuidelines.md`
- **Status:** Core guidelines written. Add edge cases as they arise.
- **Next:** Add to website and wire into admin approval flow

### RVR: Security Hardening v2
- BLOCKED — v1 broke auth, needs fail-open + generic errors + input validation

### RVR: Admin Console DB error - lastConversationAt field missing
- Was fixed Apr 2, may need verification

### RVR: UpgradePageContent.tsx bug
- null reference + Stripe quota logic bug (MEDIUM-HIGH)

### transformImageUrl deduplication
- 3 files still need cleanup

### PrismaClient duplication in og-image.ts

### Official cards sort order (displayOrder)

## MARKETING IMPROVEMENTS (from 2026.04.09 Indic AI Market Analysis)

### Memory Architecture — RAG/Lorebook System
- BLOCKING — context rot destroys user immersion
- Implement RAG-based session summarization
- Add lorebook injection for persistent character memory
- Vector DB integration (Qdrant or Pinecone)

### Voice AI Tier Integration
- Default: Veena AI (Maya Research) for native Hinglish/Indic TTS
- Premium: ElevenLabs for zero-shot voice cloning
- Free fallback: Google Wavenet

### Community Acquisition
- Target: r/CharacterAIrunaways, r/IndianGaming, r/LocalLLaMA, r/CharacterAi_NSFW
- United India Roleplay Discord (9K+ members)
- Position as "Unfiltered Voice of India"

### Fandom Seeding Campaign
- Bollywood archetypes scenario cards
- Regional mythology-inspired archetypes (no real deities)
- Community character creation tools
- Card submission guidelines: `projects/RVR/content/CardSubmissionGuidelines.md`
