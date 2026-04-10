---
title: ImageStackTemplate
created: 2026-04-10
tags: [rvr, marketing, content, instagram]
---

# Image Stack Template — 5-Card Instagram Carousel

Template for generating a 5-card cinematic deck from any scenario JSON. Each deck is posted as an Instagram carousel (swipeable 5-card post).

**Use with:** Nano Banana or Google Flow image generation
**Aspect ratio:** 9:16 (1080×1920px)
**Aesthetic:** Dark luxury — deep reds, blacks, gold accents, cinematic

---

## CARD 1 — COVER

**Image Prompt:**
> Cinematic, dark, luxury movie-poster style image prompt. Mood: mysterious, premium. Subject: [CHARACTER NAME], [brief scene description from JSON]. Style: Ornate, dark, rich colors — deep reds, blacks, gold accents. Composition: Full character or atmospheric scene, vertically oriented (9:16). Quality: 4K, high detail, no text, no logos, no watermarks. Avoid: blurry, low quality, cluttered, busy backgrounds.

**Title:** The scenario title, formatted dramatically (all caps or title case)

**Short Description (2 sentences):**
Intriguing hook capturing the scenario's core tension. Focus on emotional or situational hook. PG-13, never explicit.

**Tagline:**
A single compelling line that creates mystery and intrigue.

---

## CARD 2 — SCENARIO DESCRIPTION

**Image Prompt:**
> Cinematic image prompt. Mood: atmospheric, story-setting, evocative. Subject: A key scene from the scenario — [extract vivid moment from context or lorebook]. Style: Mysterious, cinematic, slightly dark lighting. Composition: Scene-focused, environment or character in context, vertically oriented (9:16). Quality: 4K, high detail. Avoid: blurry, low quality, cluttered, text.

**Shortened Scenario Description (2-3 sentences):**
Condensed version of the scenario. Focus on central conflict, tension, or hook. Intriguing but not explicit.

---

## CARD 3 — CHARACTER APPEARANCE

**Image Prompt:**
> Portrait image prompt. Mood: warm, inviting, premium. Subject: [CHARACTER NAME] — [extract key appearance details from character_definition]. Style: Portrait orientation, clean background or softly blurred environment, warm lighting. Composition: Head and shoulders or 3/4 shot, character looking at or slightly toward camera. Quality: 4K, high detail, clean portrait. Note: If character is in a specific setting (kitchen, doorway, etc.), incorporate it subtly. Avoid: blurry, low quality, busy background, revealing clothing, text.

**Shortened Character Appearance (2-3 sentences):**
Describe key visual features: hair, eyes, expression, clothing style, distinctive marks. No body measurements or sexualized details.

---

## CARD 4 — PERSONA

**Image Prompt:**
> Cinematic image prompt. Mood: reflective, intimate, quiet tension. Subject: [CHARACTER NAME] in a key moment reflecting their personality or inner world — [extract from persona/lorebook]. Style: Cinematic, slightly moody lighting, intimate scene. Composition: Character engaged in an activity revealing personality (reading, cooking, looking out a window, etc.). Quality: 4K, high detail, vertically oriented (9:16). Avoid: blurry, low quality, cluttered, text.

**Shortened Persona (2-3 sentences):**
Character backstory. What makes them compelling? Inner conflict or desire? Keep mysterious and intriguing.

---

## CARD 5 — DIALOGUE / CTA

**Image Prompt:**
> Close-up portrait prompt. Mood: inviting, slightly mysterious, warm smile or intriguing expression. Subject: [CHARACTER NAME], close-up portrait, looking directly at viewer. Style: Warm, engaging, premium portrait quality. Composition: Head and shoulders, slightly from below or straight on, warm lighting. Quality: 4K, high detail. Avoid: blurry, low quality, revealing clothing, text.

**First Message (verbatim from JSON):**
Copy the opening_line exactly as provided in the JSON. Do not paraphrase.

**Dialogue Teaser (1-2 sentences):**
Narrative hook setting the scene and tone. Written as if narrating a movie trailer.

**CTA Text:**
`Chat with [CHARACTER NAME] at rvr.chat`

---

## TONE RULES

- **PG-13 only** — mysterious and intriguing, never explicit
- Hint at tension without revealing plot
- Focus on: loneliness, longing, curiosity, tension
- Cinematic, premium — movie trailer narration
- Avoid: Explicit sexual content, crude humor, spoilers, generic phrasing

---

## IMPORTANT RULES

1. Extract ALL image prompts from JSON content — do not invent details not in the JSON
2. Character appearance must be DIRECTLY from JSON — do not add features not described
3. First Message must be copied EXACTLY from JSON — do not paraphrase
4. Each image prompt should be self-contained and clearly describe ONE scene or subject
5. Keep all text PG-13 — never explicit
6. Output in clean markdown with clear section headers

---

## EXAMPLE GENERATION

**Input scenario:**
```json
{
  "character_name": "Swetha",
  "scenario_title": "A Stranger at the Door",
  "tags": ["Housewife", "Indian", "MILF"],
  "character_definition": {
    "persona": "Swetha is an Indian housewife...",
    "appearance": "Slightly chubby with a curvy MILF body..."
  },
  "scenario_definition": {
    "for_ai": {
      "opening_line": "\"Yes...? Who are you?\"",
      "context": "The scene is the front porch of Swetha's suburban home..."
    }
  }
}
```

**Output:** Generate all 5 cards following the format above.
