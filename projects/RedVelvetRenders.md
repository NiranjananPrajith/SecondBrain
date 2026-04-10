---
title: RedVelvetRenders
created: 2026-03-17
tags: [project, rvr]
aliases: [RVR]
---

# RedVelvetRenders (RVR)

*Part of the [[MEMORY]] system. See [[TASKS]] for development tracking.*

## Overview

## Overview
AI-powered fantasy chat platform with comics, novels, games, and interactive storytelling.

## Projects

### 1. Kaliyuga Charitham
- **Type:** Sci-fi visual novel (comic format)
- **Status:** 4 parts published
- **Format:** Multi-panel comics

### 2. The Mind Control Chatbot
- **Type:** Prose thriller + audio
- **Status:** 3 chapters published
- **Setting:** Future of Kaliyuga universe
- **Format:** Novel + TTS audio versions

### 3. Knowledge is Power (KIP)
- **Type:** Text-based RPG / Point-and-click adventure
- **Status:** In development (HTML5/Godot planned)
- **Setting:** Before Kaliyuga Charitham

### 4. RVR Chat
- **Type:** AI character chat platform
- **Status:** Active
- **Features:**
  - Scenario-based roleplay
  - User-created characters
  - TTS (Basic/Advanced/Custom)
  - Cloud saving

## Tech Stack

| Component | Technology |
|----------|------------|
| Frontend | Next.js 14 (App Router), TypeScript, Tailwind |
| Database | PostgreSQL + Prisma ORM |
| Auth | iron-session + Patreon OAuth + Supabase Auth |
| Caching | Redis |
| Storage | Supabase Storage |
| AI/LLM | AIML API (GPT-3.5-turbo, Fish Audio, VibeVoice) |
| Hosting | Hostinger VPS (Ubuntu) |

## Domains

| URL | Purpose |
|-----|---------|
| redvelvetrenders.com | Main site, content hub |
| chat.redvelvetrenders.com | Chat platform |
| rvr.chat | Chat platform (short) |

## User System

| Tier | Access | Credits |
|------|--------|---------|
| Guest | 10 total messages | - |
| Free (OAuth) | 30 credits/day | 30/day |
| Patreon Spark | $3/mo | 1000/mo |
| Patreon Flame | $6/mo | 2500/mo |
| Patreon Inferno | $9/mo | 5000/mo |

## TTS Costs

| Level | Credits per message |
|-------|-------------------|
| Basic (Google) | 5 |
| Advanced (Fish) | 10 |
| Custom (VibeVoice) | 20 |

## Marketing Channels

### Working
- DeviantArt: 318k views/year
- Literotica: 150k reads
- Reddit: 4k+ views (40% US)
- RedGIFs: 250k+ views
- Free Vault landing page

### Not Working (Dropped)
- Instagram brand page
- Daily Patreon posts (now weekly)

## Current Development

### Phase 1: OAuth Login ✅ COMPLETED
- Google login
- Discord login
- Email login/signup
- Guest → Account migration

### Phase 2: Stripe Integration ✅ COMPLETED
- Set up Stripe account
- Connect credit purchase buttons
- Credit packages:
  - 500 credits: $4.99
  - 1000 credits: $8.99
  - 3000 credits: $24.99
  - 5000 credits: $39.99
  - 10000 credits: $74.99

### Phase 3: Patreon Migration ⏳ FUTURE
- Migrate Patreon to Supabase Auth
- Unified account system

## Supabase Project
- URL: yqqwmcxpnxmktqtiaufr.supabase.co

## Todo
- [x] Test login flows
- [x] Stripe setup (✅ LIVE)
- [x] Connect payment buttons

## Notes

- Revenue goal: Make RVR profitable → work on [[projects/CuratorHub]] full-time
- Revenue goal: Make RVR profitable → work on CuratorHub full-time
- Solo developer
- Updates: Weekly (not daily)
