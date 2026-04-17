---
title: CuratorHub Studio
created: 2026-03-22
tags: [project, curatorhub, studio]
---

# CuratorHub Studio

*The Universal Media Conversion Platform*

---

## Overview

A visual storytelling and creative production platform where creators move fluidly between text, image, audio, video, and 3D — using every medium interchangeably.

**Core innovation:** Not a collection of AI tools. A **media graph** — where every medium can generate every other medium. The value is in the edges between media, not the media themselves.

**Domain:** curatorhub.studio (future) → localhost for now

---

## The Media Graph (Phased Roadmap)

```
                    TEXT
                   ↗   ↘
                  ↗     ↘
           IMAGE ←       → AUDIO
            ↓ ↘     ↗
            ↓   ↘ ↗
            VIDEO → 3D
```

### Phase 1 — MVP (NOW)
**Edges enabled:**
- Text ↔ Image (Stable Diffusion via RunPod, Qwen Image Edit)
- Text → Audio (TTS via AIML / Fish Audio)
- Text + Image → Audio (scene narration)

**Why these first:** These are the edges RVR needs to produce all its content — character images, TTS voices, scene narration.

### Phase 2 — Audio Expansion
**Edges added:**
- Audio → Text (transcription/captioning)
- Image → Video (Runway / Pika)
- Text → Video (when quality improves)

**Trigger:** When RVR's production workflow needs video, or when video quality reaches acceptable thresholds.

### Phase 3 — 3D Integration
**Edges added:**
- Image → 3D (Tripo3D API — serverless)
- 3D → Image
- 3D → Video

**Trigger:** When 3D generation tech matures. Not in a rush — waiting for quality and cost to reach production levels.

---

## The Four Spaces

```
Dashboard → Blueprint → Workshop → Assembly
    ↓          ↓          ↓          ↓
 Overview    Plan      Generate    Finish
```

### Dashboard
- All projects
- Recent work
- Quick start templates
- Media conversion history

### Blueprint
- Planning space
- Story outlines (text)
- Scene breakdowns
- Media conversion paths (e.g., "this scene needs: image → audio")
- Define which media conversions are needed per scene

### Workshop
- Generation space
- Text generation (Mistral Nemo via AIML)
- Image generation (Stable Diffusion, Qwen Image Edit via RunPod)
- Audio generation (TTS via AIML / Fish Audio)
- Video generation (Phase 2)
- 3D generation (Phase 3)
- **Media conversion:** Run any enabled edge (Image → Audio, Text + Image → Audio, etc.)

### Assembly
- Finish space
- Combine text + images + audio into scenes
- Arrange scenes into full narratives
- Export to various formats
- Finalize projects

---

## AI Stack

| Task | Model | Provider |
|------|-------|----------|
| **Text → Image** | Stable Diffusion + Qwen | RunPod Flash |
| **Text ↔ Audio** | Mistral / Fish Audio / VibeVoice | AIML API |
| **Text Generation** | Mistral Nemo | AIML API |
| **Planning/Orchestration** | Mistral Large 2 | AIML API |
| **Image → Video** | Runway / Pika | API (Phase 2) |
| **Text → Video** | Sora / Luma | API (Phase 2) |
| **Image → 3D** | Tripo3D | API (Phase 3, serverless) |

---

## Key Features

### Phase 1 (MVP) — In Development
- [ ] Blueprint space with story planning + media path definition
- [ ] Workshop with text generation
- [ ] Workshop with image generation (Stable Diffusion + Qwen)
- [ ] Workshop with TTS audio generation
- [ ] Text + Image → Audio scene narration
- [ ] Assembly space for combining outputs
- [ ] Export to image sequences (for RVR)

### Phase 2
- [ ] Audio → Text transcription
- [ ] Image → Video generation
- [ ] Video → Text captioning
- [ ] Collaborative editing

### Phase 3
- [ ] Image → 3D character generation
- [ ] 3D → Image / Video
- [ ] Full 3D scene assembly

---

## RVR as First Customer

**RVR is the first paying beta user of CuratorHub Studio.**

RVR needs this workflow to produce content:
1. Generate character image (Text → Image)
2. Write dialogue (Blueprint + Workshop)
3. Generate TTS audio (Text → Audio)
4. Generate scene narration (Text + Image → Audio)
5. Assemble into RVR scene

This is the real production test. CuratorHub Studio must be good enough for RVR's actual content pipeline before it's ready for external users.

**RVR promotes CuratorHub but stays separate.** It is not part of CuratorHub. It is a customer.

---

## Tech Stack

- **Frontend:** Next.js 14, TypeScript, Tailwind CSS
- **Database:** Supabase + Prisma
- **Auth:** Supabase Auth + iron-session
- **AI Text/Audio:** AI-ML API (Mistral, Fish Audio, VibeVoice)
- **AI Images/Video:** RunPod (Stable Diffusion, Qwen), Runway/Pika (Phase 2)
- **AI 3D:** Tripo3D API (Phase 3, serverless)
- **Storage:** Supabase Storage

---

## Differentiation

Unlike generic AI studios (Midjourney, Runway, etc.):
- **Media graph, not media silos** — tools that work together, not just separately
- **Integrated workflow** — Plan → Create → Convert → Assemble, all in one platform
- **Universal conversion** — not just "generate image from text" but "generate audio from image AND text"
- **Visual storytelling focus** — built for narrative content (comics, visual novels, games, reels)
- **Serverless-first** — using API services to avoid infrastructure complexity

---

## Pricing

- **Phase 1 (MVP):** Usage-based or flat subscription (TBD)
- **Serverless-first principle:** Prefer API services with no infra overhead
- **RVR pays from day one** — commercial relationship from the start

---

## Status

**Building now** — MVP targeting RVR production workflow

---

## Backlinks

- [[curator-hub]] — Parent ecosystem
- [[dev]] — Development platform
- [[network]] — Social & marketplace
