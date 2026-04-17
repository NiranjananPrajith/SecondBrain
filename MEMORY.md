---
title: Memory
created: 2026-03-17
tags: [memory, system]
---

# MEMORY.md - Long-term Memory

## About Dr. Rowen
- **Name:** Dr. Rowen
- **Location:** UnknownS
- **Timezone:** America/New_York (EDT)
- **Occupation:** Solo creator/developer (works ~2-3 hours/day alongside a full-time job)
- **Personality:** Down-to-earth, values authenticity, respects boundaries

## RedVelvetRenders (RVR)
*See [[red-velvet-renders]] for full project details*

### Projects
1. **Kaliyuga Charitham** - Flagship sci-fi visual novel (comic format)
2. **The Mind Control Chatbot** - Prose thriller + audio (TTS versions)
3. **Knowledge is Power** - Text-based RPG (planning Godot/HTML5 upgrade)
4. **RVR Chat** - AI character chat platform with TTS (crown jewel)

### Tech Stack
- **Frontend:** Next.js 14 (App Router), TypeScript, Tailwind CSS
- **Database:** PostgreSQL + Prisma ORM
- **Auth:** iron-session + Patreon OAuth (planning to add Supabase Auth)
- **Caching/Rate Limiting:** Redis
- **Storage:** Supabase Storage
- **AI/LLM:** AIML API (GPT-3.5-turbo, Fish Audio TTS, VibeVoice for custom cloning)
- **Hosting:** Hostinger VPS (Ubuntu)

### Domains
- **Main site:** redvelvetrenders.com
- **Chat:** chat.redvelvetrenders.com / rvr.chat
- **Supabase Project:** yqqwmcxpnxmktqtiaufr

### User System
- Guest users: 10 total messages (device fingerprint)
- Free users: 30 credits/day
- Patreon tiers: Spark ($3), Flame ($6), Inferno ($9) - monthly credits
- TTS costs: Basic (5 credits), Advanced (10), Custom (20)

### Current Development
- **Phase 1:** Google/Discord OAuth login — ✅ DONE
- **Phase 2:** Stripe for credit purchases — ✅ LIVE
- **Phase 3:** Patreon to Supabase Auth migration — pending
- **In Progress:** Marketing Month 1, migration cleanup

### Marketing Channels (Working)
- DeviantArt (318k views/year)
- Literotica (150k reads)
- Reddit (4k+ views, 40% US traffic)
- RedGIFs (250k+ views)
- "Free Vault" landing page for conversions

### Marketing Channels (Not Working)
- Instagram brand page - dropped
- Daily Patreon posts - switched to weekly

## System Setup
- **OS:** Fedora KDE (switched from Kali Linux on April 2, 2026)
- Repos cloned to `~/Documents/Obsidian/SecondBrain/code/`:
  - chat.redvelvetrenders.com
  - redvelvetrenders.com
  - curatorhub.co
- Terminal tools: ranger, lazygit, htop, batcat, eza, fastfetch, tldr
- Firewall enabled, automatic security updates enabled

## Current Status (April 2, 2026)

### ✅ COMPLETED (recent)
- [x] OfficialScenario → CustomCharacter migration (April 1)
- [x] ChatHistory UUID migration (legacy string IDs → UUIDs)
- [x] Explore page ISR cached 1 week, DB query limited to 150 records
- [x] Tags display on official scenario cards (dashboard)
- [x] Explore cache rebuild system complete
- [x] Stale "Deleted Scenario Card" filtered from dashboard
- [x] ISR cache properly cleared on card create/update

### 🔄 IN PROGRESS
- Marketing Month 1 (50 subs, $100 ad budget)

### ⏳ PENDING
- Verify migration ran in production (needs SSH to Hostinger)
- transformImageUrl deduplication (3 files)
- PrismaClient duplication in og-image.ts
- Official cards sort order (displayOrder)
- Admin Console `lastConversationAt` error
- Account merging (Patreon ↔ existing account)

### ✅ COMPLETED
- [x] Supabase security (RLS, migrations table)
- [x] Database schema updated (googleId, discordId, credits, loginProvider)
- [x] Google OAuth login
- [x] Discord OAuth login
- [x] Email login/signup
- [x] LoginModal + UpgradeModal redesign
- [x] Guest → Account chat migration
- [x] OfficialScenario → CustomCharacter migration
- [x] Stripe integration for credit purchases

### 🔄 IN PROGRESS
- Marketing Month 1
- Migration cleanup (deduplication, sort order, production verify)

### ⏳ PENDING
- Account merging (link Patreon to existing account)
- Admin Console `lastConversationAt` error fix
- [[projects/curator-hub]] development
- Patreon → Supabase Auth migration

### ⏳ PENDING (Future)
- [[agro-tech-company]] (after RVR/CuratorHub succeed)
- Game logic AI
- Story planner tool

## Personal Interests
- [[evo2]] - DNA Foundation Models & AI x Biology
- **Lifelong learner** - Deep interest in AI, biology, science, and emerging tech

## Three-System Architecture
Full setup documented in `SYSTEM_ARCHITECTURE.md`:
- **System 1 (Sandbox):** Analysis, prompts, auditing. Git clones for reference only.
- **System 2 (Dr. Rowen):** Development — Claude Code, pushes to GitHub.
- **System 3 (Server):** Production — pulls from GitHub, PM2, Hostinger VPS.

## Workflow
1. Sandbox analyzes code → writes prompts → Dr. Rowen runs in Claude Code
2. Dr. Rowen runs prompts in Claude Code → pushes to GitHub
3. Server pulls from GitHub → rebuilds → PM2 serves live

## Commitment
Dr. Rowen's goal: Make RVR profitable → work on CuratorHub full-time
My role: Keep track of goals, hold accountable, motivate, iterate, help learn
