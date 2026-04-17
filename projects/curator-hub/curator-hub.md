---
title: CuratorHub
created: 2026-03-22
tags: [project, curatorhub]
---

# CuratorHub Ecosystem

*The Creator's Operating System*

---

## Vision

A platform where text, image, audio, video, and 3D work as an interchangeable system — not isolated features. The value lives in the **edges between media**, not the media themselves.

Creators plan in Blueprint, generate in Workshop, and assemble in Assembly — moving between media types fluidly.

**Core principle:** Every medium should be able to generate every other medium. Text → Image. Image → Audio. Audio → Text. Video → Image. Image → 3D. And so on.

---

## The Media Graph

The platform is built around media conversion edges:

```
                    TEXT
                   ↗   ↘
                  ↗     ↘
           IMAGE ←     → AUDIO
            ↓ ↘     ↗
            ↓   ↘ ↗
            VIDEO → 3D (eventually)
```

**MVP edges (Phase 1):**
- Text ↔ Image ✅ (via RunPod Stable Diffusion + Qwen)
- Text → Audio ✅ (via AIML TTS)
- Text + Image → Audio (scene narration)

**Phase 2:**
- Audio → Text (captioning/transcription)
- Image → Video (Runway/Pika)
- Text → Video (when quality improves)

**Phase 3:**
- Image → 3D (via Tripo3D API — when tech matures)
- 3D → Image
- 3D → Video

---

## The Three Platforms

| Platform | Domain | Purpose |
|---------|--------|---------|
| [[studio]] | curatorhub.studio | Content creation (text, image, audio, video, 3D) |
| [[dev]] | curatorhub.dev | AI-powered application development |
| [[network]] | curatorhub.net | Social sharing and marketplace |

---

## Architecture — The Four Spaces

Each platform follows the same UX flow:

```
Dashboard → Blueprint → Workshop → Assembly
    ↓          ↓          ↓          ↓
 Overview    Plan      Generate    Finish
```

### Blueprint
Planning space — outline, structure, sketch ideas, define media conversion paths

### Workshop
Generation space — create text, images, audio, video; run media conversions

### Assembly
Finish space — combine outputs across media types, export, finalize

---

## Domains

| Domain | Purpose |
|--------|---------|
| curatorhub.co | Homepage & marketing |
| curatorhub.studio | Content creation platform |
| curatorhub.dev | Development platform |
| curatorhub.net | Social & marketplace |

---

## Brand Identity

**Tagline:** "The Creator's OS"

**Core Concept:** Assembly-line production for creative content across all media types.

**Platform Names:**
- Studio — Content creation (text, image, audio, video, 3D)
- Dev — Application development
- Network — Social & marketplace

---

## Why This Architecture

Each platform serves a distinct creative domain, but they share:
- The same four-space workflow
- The same media-graph philosophy
- The same tech stack (Supabase, Next.js, AI-ML API, RunPod)

Studio validates the platform concept. Dev extends it to applications. Network enables sharing and monetization.

---

## Relationship to Other Projects

| Project | Relationship |
|---------|-------------|
| [[red-velvet-renders]] | **First paying customer / beta tester** — promotes CuratorHub but stays separate. RVR dog-foods Studio's workflow for all RVR content production. |
| [[agro-tech-company]] | Future project (after RVR/CuratorHub succeed) |

**RVR is not part of CuratorHub.** It is a separate product that uses CuratorHub Studio as its production tool. This is intentional — RVR validates the platform with real production needs without being structurally coupled.

---

## Status

- **Phase 1:** CuratorHub Studio MVP (building now)
- **Phase 2:** CuratorHub Dev (after Studio stabilizes)
- **Phase 3:** CuratorHub Network (after Dev)

---

## Navigation

- [[studio]] — Content creation platform
- [[dev]] — Development platform
- [[network]] — Social marketplace
