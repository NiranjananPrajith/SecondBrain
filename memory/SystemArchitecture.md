---
title: system-architecture
created: 2026-04-09
tags: [system]
---

# Three-System Architecture

## System 1 (Sandbox / Claude Code)
- Analysis, prompts, auditing
- Git clones at `~/Documents/Obsidian/SecondBrain/code/` for reference only
- Claude Code runs here, pushes to GitHub

## System 2 (Dr. Rowen / Development)
- Claude Code, pushes to GitHub
- Direct code work

## System 3 (Server / Production)
- Hostinger VPS (Ubuntu)
- Pulls from GitHub, PM2 serves live

## Workflow
1. Sandbox analyzes code → writes prompts → Dr. Rowen runs in Claude Code
2. Claude Code pushes to GitHub
3. Server pulls from GitHub → rebuilds → PM2 serves live

## Tech Stack Notes
- Repos at `~/Documents/Obsidian/SecondBrain/code/redvelvetrenders/` and `~/Documents/Obsidian/SecondBrain/code/curatorhub/`
- RVR Chat codebase: `/home/d0e0a0d/Documents/VS_Code/redvelvetrenders/chat.redvelvetrenders.com`
- Terminal tools: ranger, lazygit, htop, batcat, eza, fastfetch, tldr

## RVR Chat Codebase Notes
- **Build output:** `.next` (not `out`)
- **Auth:** iron-session + Patreon OAuth + Google OAuth + Discord OAuth + Email
- **Session cookie:** `rvr-ai-chat-session`, 7-day expiry
- **Cross-domain SSO:** via `redvelvetrenders.com/api/auth/sync`
- **No PWA currently exists**
- **OAuth redirect validation:** checks against `rvr.chat`, `*.rvr.chat`, `redvelvetrenders.com`, `*.redvelvetrenders.com`
