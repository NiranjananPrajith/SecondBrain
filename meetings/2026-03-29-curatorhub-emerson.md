---
title: CuratorHub Discussion - Emerson
date: 2026-03-29
attendees: Dr. Rowen, Emerson
goal: Introduce CuratorHub vision, media graph concept, and explore further possibilities
---

# CuratorHub Discussion — March 29, 2026

---

## Context

**CuratorHub Studio** is being developed as the foundational creative OS for all Dr. Rowen's content production — and eventually for external creators. This meeting introduces the core concept to Emerson and explores where it could go.

---

## The Core Idea: Media as an Interchangeable System

Most AI studios are **feature silos**:
- "We have image generation"
- "We have video generation"
- "We have audio tools"

But they're isolated. A creator has to manually move outputs between different tools, formats break, context is lost.

**CuratorHub's innovation:** Think in **edges between media**, not media themselves.

```
                    TEXT
                   ↗   ↘
                  ↗     ↘
           IMAGE ←       → AUDIO
            ↓ ↘     ↗
            ↓   ↘ ↗
            VIDEO → 3D (eventually)
```

Every arrow is a **conversion** — and that's where the value lives.

- Text + Image → Audio = scene narration
- Image → Audio = voice from character portrait
- Audio → Text = captioning / transcription
- Image → Video = still image to animated clip
- Video → Image = frame extraction
- Image → 3D = character model from portrait
- And so on...

**The platform doesn't sell "image generation." It sells the ability to move between media without friction.**

---

## The Three Platforms

| Platform | Purpose |
|----------|---------|
| **Studio** | Text, image, audio, video, 3D — the full media graph |
| **Dev** | AI-powered application development (same four-space workflow) |
| **Network** | Social sharing and marketplace for outputs from Studio and Dev |

Studio validates the concept. Dev extends it. Network monetizes it.

---

## CuratorHub Studio — Phased Roadmap

### Phase 1 — MVP (Building NOW)

**Media edges enabled:**
- Text ↔ Image ✅
- Text → Audio ✅
- Text + Image → Audio (scene narration) ✅

**Why these first:**
These are exactly the edges Dr. Rowen needs to produce all [[red-velvet-renders]] content — character images, TTS voices, scene narration. The MVP isn't hypothetical. It's RVR's production pipeline, built as a platform.

**Tech:**
- Images: RunPod Stable Diffusion + Qwen
- Audio: AIML API (Fish Audio / VibeVoice)
- Text: Mistral Nemo

---

### Phase 2 — Audio + Video

**Edges added:**
- Audio → Text (transcription/captioning)
- Image → Video (Runway / Pika)
- Text → Video (when quality improves)

**Trigger:** When RVR's workflow needs video, or when video quality reaches production levels.

---

### Phase 3 — 3D Integration

**Edges added:**
- Image → 3D (character models from portraits)
- 3D → Image
- 3D → Video

**Note:** Not in a rush. Waiting for 3D generation tech to mature. Tripo3D API is serverless and promising but quality isn't production-ready yet.

---

## The Four-Space Architecture

Every platform follows the same workflow — this is what makes it an "operating system":

```
Dashboard → Blueprint → Workshop → Assembly
    ↓          ↓          ↓          ↓
 Overview    Plan      Generate    Finish
```

| Space | What Happens |
|-------|-------------|
| **Dashboard** | All projects, recent work, quick-start templates |
| **Blueprint** | Plan the project, outline scenes, define media conversion paths |
| **Workshop** | Generate text, images, audio; run media conversions |
| **Assembly** | Combine outputs, arrange scenes, export, finalize |

---

## RVR's Role — First Paying Customer

[[red-velvet-renders]] is **not** part of CuratorHub. It stays completely separate.

But it is the **first paying customer** and beta tester.

**What this means:**
- Dr. Rowen pays for CuratorHub Studio from day one
- RVR's production needs drive the roadmap
- Every feature must be good enough for real content production before launch
- RVR promotes CuratorHub but they remain legally and structurally separate

**RVR's production workflow (what CuratorHub must support):**
1. Generate character image (Text → Image)
2. Write character dialogue (Blueprint + Workshop)
3. Generate TTS audio (Text → Audio)
4. Generate scene narration (Text + Image → Audio)
5. Assemble into RVR scene

If CuratorHub can do that reliably, it's ready for external creators.

---

## Discussion Points for Emerson

### 1. Is the Media Graph Concept Compelling?

Does this framing — "edges between media" vs "media silos" — resonate? Does it feel like a real insight, or does it sound like marketing?

### 2. What's Missing from the Vision?

The current vision is very production-focused. What about:
- Discovery? (finding pre-built assets, templates)
- Collaboration? (multiple creators on same project)
- Licensing? (who owns what when AI is involved)

### 3. Business Model

Studio is usage-based or subscription (TBD). But:
- Should there be a free tier for creators to test?
- How does pricing scale with usage?
- Network (Phase 3) has platform fees — is that enough to sustain the business?

### 4. Competition

Who are we actually competing with?
- [[https://www.leonardo.ai|Leonardo.ai]] — AI image platform
- [[https://www.runwayml.com|Runway]] — Video AI
- [[https://www.figma.com|Figma]] — Design platform
- Generic: ChatGPT, Claude, Midjourney

How is CuratorHub different from just "using a bunch of AI tools together"?

### 5. Dev and Network — How Do They Fit?

Studio is concrete. Dev and Network are Phase 2 and 3. But:
- Does Dev (vibecoding) have a clear enough problem to solve?
- Does Network (marketplace) need Studio/Dev to succeed first?

### 6. Open Questions

- What does "done" look like for Phase 1?
- What's the timeline for external users?
- What does Emerson want to own in this project?

---

## Open Questions to Resolve

- [ ] What is the minimum viable Studio product? (Define MVP scope)
- [ ] What is the pricing model? (Usage-based? Subscription? Free tier?)
- [ ] Who is the target user beyond Dr. Rowen?
- [ ] What is the go/no-go for Phase 2?
- [ ] How does Emerson want to participate?

---

## Resources

- [[projects/curator-hub]] — Main project page
- [[studio]] — Studio details
- [[dev]] — Dev platform details
- [[network]] — Network marketplace details
- [[red-velvet-renders]] — First customer and proof of concept
- [[agro-tech-company]] — Future project (unrelated)

---

*Last updated: 2026-03-29*
