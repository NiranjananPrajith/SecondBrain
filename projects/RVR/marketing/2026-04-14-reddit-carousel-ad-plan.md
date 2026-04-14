# Reddit Carousel Ad — Plan

**Project:** RVR Chat Reddit Ad (replacing PaidAdSprint Reddit creative)
**Date:** 2026-04-14
**Status:** PLANNING

---

## Context

Marketing Month 1 is active with a $100 ad budget across Instagram ($70) and Reddit ($30). The Reddit ad creative in `PaidAdSprint.md` uses a single Image + Title + Body format with a card + chat screenshot. Reddit has approved carousel ads for this campaign, so we're replacing the single-image Reddit ad with a 7-slide carousel that showcases all 6 supported Indic languages.

**Goal:** Demonstrate RVR's multilingual Indic language capabilities through a carousel that introduces the platform on slide 1 and walks through each language with authentic character + chat samples on slides 2-7.

---

## Carousel Structure

### Slide 1 — Platform Introduction
**Purpose:** Hook the viewer, establish the core value prop

| Element | Content |
|---------|---------|
| **Visual** | Dark luxury branded card — RVR logo + velvet background, all 6 language names rendered in their native scripts: हिंदी (Hinglish), தமிழ் (Tanglish), മലയാളം (Manglish), ಕನ್ನಡ (Kanglish), বাংলা (Benglish), తెలుగు (Tenglish) |
| **Headline** | "Finally, an AI that speaks your language." |
| **Body** | "RVR Chat — AI companions that understand Hinglish, Tanglish, Manglish, and more. No filters, no lectures. Try free → rvr.chat" |

### Slides 2-7 — Language Samples (one per slide)

Each slide showcases one language with:
- A culturally-grounded character card (dark luxury aesthetic, character appears)
- A short chat exchange demonstrating authentic code-mixed dialogue
- Language name + tagline in native script

| Slide | Language | Character Type | Chat Sample Theme |
|-------|----------|----------------|-------------------|
| 2 | Hinglish | Bollywood hero archetype | Flirty/playful banter |
| 3 | Tanglish | Modern Chennai girl | Casual conversation |
| 4 | Manglish | College student Kerala | Friendly chat |
| 5 | Kanglish | Bangalorean tech vibe | Casual tech-speak |
| 6 | Benglish | Kolkata personality | Warm/witty exchange |
| 7 | Tenglish | Hyderabad flavor | Mix of traditional + modern |

---

## Creative Specifications

### Image Format
- **Aspect ratio:** 1:1 square (1080×1080px) — optimal for Reddit carousel
- **Aesthetic:** Dark luxury — deep reds, blacks, gold accents, cinematic lighting
- **Character:** Must appear in every slide image
- **No text in generated images** — text is overlaid via ad copy

### Image Generation Approach
Use the existing `ImageStackTemplate.md` as a guide, but adapted:
- Generate character portraits at **1:1 ratio** (not 9:16 portrait)
- For each language: generate 1 character card image + pair with a chat overlay
- Chat overlay can be generated as part of the image OR added as a text overlay in Reddit Ads creative tools

**Image Generation Tools:** Nano Banana or Google Flow (per PaidAdSprint)

### Ad Copy Specs

**Slide 1 (Intro):**
- Headline: "Finally, an AI that speaks your language." (≤10 words)
- Body: "Chat with Indian AI companions that understand your language, culture, and slang. Free to start." (≤40 words)

**Slides 2-7 (Language Samples):**
- Headline: "[Language name in native script] — [1-line cultural hook]" (≤8 words)
  - e.g., "हिंदी — Bollywood ke apne rules" or "தமிழ் — Chennai vibes only"
- Body: "[Character name] speaks [language]. No filters, no scripts. Try her → rvr.chat" (≤25 words)
- Chat sample: shown as image overlay (screenshot-style, dark mode)

---

## Assets to Create

### New Character Cards (one per language)

Rather than generating full scenario card JSONs, we create simplified character reference cards for ad use:

| # | Language | Character Name | Character Type | First Message Sample |
|---|----------|----------------|----------------|---------------------|
| 1 | Hinglish | Veergati | Bollywood revenge hero | "Kya kar rahi hai itni raat? Wait, mujhe pata hai tu kaun hai." |
| 2 | Tanglish | Meera | Modern Chennai girl | "Enna da, late night chat pannuringa? Ava, Naan romba excited!" |
| 3 | Manglish | Arjun | Kerala college student | "Aire bhai, ippol enthu parayanam? Kanditt nondi moneypoyi." |
| 4 | Kanglish | Nikhil | Bangalorean tech bro | "Boss, ella right ide, adre helbeku. Innovationnu support madidri!" |
| 5 | Benglish | Moitra | Kolkata banter queen | "O dekh Tora, amra Kolkata te ektu alpo kotha shonabo." |
| 6 | Tenglish | Priya | Hyderabad mix | "Kya yaar,武汉病毒不是开玩笑的。 Lekin chai toh sabki jaan hai!" |

*Note: These are ad copy samples — actual character cards should be created following the `CharacterCardSystemPrompt.md` format for the AI to stay in character.*

### Chat Samples

Each chat sample should be 3-4 lines showing:
1. User input (in English or simple version)
2. AI response in the target language
3. Natural code-mixing appropriate to that language

**Format:** Dark mode chat screenshot style — similar to what a real phone screen would show.

### Image Prompts (per slide)

Each slide needs 1 character portrait image. Prompt structure:

```
[Character Name], [brief appearance from card], [setting appropriate to character type].
Dark luxury aesthetic, deep red and black with gold accents, cinematic lighting.
1:1 square format, 1080x1080px, no text, no logos, no watermarks.
Character as focal point, portrait style.
```

---

## Reddit Ad Configuration

| Parameter | Value |
|-----------|-------|
| **Format** | Carousel |
| **Carousel Slides** | 7 (1 intro + 6 languages) |
| **Targeting** | Interest + Keyword (per RedditAdStrategy.md): `AI products, chatbots, anime, roleplaying games, Indian entertainment` + keywords: `Character AI alternative, uncensored AI chat, Hinglish AI, Tanglish, Kanglish, Manglish, Indic AI, AI companion India` |
| **Budget** | $30/2 weeks (existing allocation — this replaces the previous single-image Reddit ad) |
| **CTA** | "Try rvr.chat" → rvr.chat |
| **Placements** | Reddit feed, Reddit banner |

---

## Content Creation Steps

### Step 1: Character Card Generation
1. Write 6 character card JSONs (one per language) following `CharacterCardSystemPrompt.md`
2. Generate character portrait images at 1:1 ratio using image AI
3. Review and select best output per language

### Step 2: Chat Sample Creation
1. For each language, write 3-4 line chat exchange (user + AI in target language)
2. Generate chat screenshot images (dark mode, phone-style frame)
3. OR use Reddit Ads creative tools to compose carousel with text overlays

### Step 3: Assemble Carousel
1. Upload all 7 images to Reddit Ads creative library
2. Write headline + body for each slide in Reddit Ads manager
3. Configure carousel card sequence
4. Set CTA per card or single CTA for the whole ad

### Step 4: Launch + Monitor
1. Launch with Option B targeting from RedditAdStrategy.md
2. Monitor CPM, CTR, and cost-per-engagement daily for first 3 days
3. Swap worst-performing carousel card after Day 4 if needed

---

## Verification Checklist

- [ ] 6 character card images generated (1:1, dark luxury)
- [ ] 7 carousel slides configured in Reddit Ads Manager
- [ ] Each slide has headline + body copy
- [ ] Reddit ad approved and running
- [ ] Metrics tracked at Day 3 (CPM, CTR, engagement rate)

---

## Dependencies

- Reddit Ads account active (per PaidAdSprint ✅)
- Image generation tool access (Nano Banana or Google Flow)
- $30 budget allocated
- rvr.chat live and functional
