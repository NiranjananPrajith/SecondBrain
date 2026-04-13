# Reddit Ad Strategy

**Created:** 2026-04-12
**Status:** PLANNING
**Type:** Reddit advertising

---

## Overview

Reddit is a text-native platform. The Instagram video ad that works there will not work here. Reddit ads need to look like authentic community content — a screenshot, a chat exchange, something a real user might share. This document covers the Reddit-specific creative strategy and ad structure.

**Budget:** $30 for 2 weeks (~$2.15/day)
**Target:** Option B — Interest + keyword targeting (not subreddit-specific)

---

## Why Reddit Needs a Different Creative

### What works on Instagram vs Reddit

| Instagram | Reddit |
|-----------|--------|
| Video, looping, no text needed | Screenshot + text, legible headline required |
| Autoplay catches attention | Community is reading, not watching |
| Visual brand aesthetic | Text-native, authenticity-first |
| Works as standalone ad | Must feel like community content to avoid downvote |

### The Core Problem With the Current Video Ad on Reddit

- Looks like a brand ad — gets downvoted or ignored
- No text readable at feed resolution
- The headline ("rvr.chat") is illegible in a small thumbnail
- Reddit users are trained to scroll past obvious marketing
- The "uncensored AI" angle requires more context than a 30-second video can deliver in text form

---

## Reddit Ad Strategy

### Targeting: Option B (Interest + Keyword)

| Parameter | Value |
|-----------|-------|
| Interests | `AI products, chatbots, anime, roleplaying games, Indian entertainment` |
| Keywords | `Character AI alternative, uncensored AI chat, Hinglish AI, Tanglish, Kanglish, Manglish, Indic AI, AI companion India` |
| Budget | $30 for 2 weeks (~$2.15/day) |

This is more scalable than subreddit-only targeting and reaches people browsing related interests outside of specific communities.

### Reddit Ad Formats Available

1. **Image + Title + Body** — single image + headline + description (recommended)
2. **Video + Title + Body** — video ad (not recommended — same problem as Instagram video)
3. **Text + Body** — no image, purely text-based (low CTR, not recommended for cold traffic)

**Recommendation:** Image + Title + Body format.

---

## Creative Strategy: Card Screenshot Posts

### What to Show

The creative should be a **character card screenshot + a chat exchange**. This demonstrates:
1. The product exists (card image)
2. The multilingual claim is real (chat in Hinglish/Tanglish in the exchange)
3. It's authentic (looks like something a real user would share)

### Format

**Image:** A character card + a short chat exchange in Tanglish/Hinglish. Use the ImageStackTemplate to generate this.

**Headline (Title):** Direct, personal, community-first. Something a real user might say in a post.

**Body:** Brief description — no marketing speak, no corporate tone.

### Example Creative

**Headline:** "Finally tried an AI that actually speaks Hinglish. Not bad."

**Image:** Card screenshot showing a character + chat exchange in Tanglish

**Body:** "RVR Chat — free to start. Supports Hinglish, Tanglish, Manglish, and more. No filters."

---

## Alternative Creative Approach: Lore Card Showcase

For communities like r/CharacterAIrunaways, a **lore card post** performs better than a direct ad:

**Headline:** "Made a Bollywood-inspired character. Here's how she responds to Hinglish insults."

**Image:** Character card image (dark luxury aesthetic)

**Body:** None needed — the image does the work. The caption is the ad.

**Note:** This format is organic-style rather than explicitly promotional. It performs well in communities that ban self-promotion because it doesn't read as an ad.

---

## Image Assets to Generate

For the Reddit ad campaign, generate the following using ImageStackTemplate:

### IMAGE 1 — Card + Chat Screenshot (Primary Reddit Creative)

- 16:9 landscape format
- Show a character card (extracted from JSON) + a short chat exchange in Tanglish
- Dark luxury aesthetic, legible text in the chat exchange
- No watermark, no logo

### IMAGE 2 — Lore Card Showcase (For r/CharacterAIrunaways)

- 9:16 portrait format
- Character card only — no chat, just the card visual
- Dark luxury aesthetic, premium look
- No watermark, no logo

### IMAGE 3 — Language Comparison (Optional)

- 16:9 landscape
- Side-by-side or stacked: English chat vs Tanglish chat — demonstrates multilingual capability
- Could be a split-screen with both exchanges visible

---

## Recommended Creative Mix

| Creative | Format | Targeting | Use |
|----------|--------|-----------|-----|
| Card + Chat Screenshot | Image + Title + Body | Interest + keyword | Primary Reddit ad |
| Lore Card Showcase | Image + Title + Body | r/CharacterAIrunaways | Community-specific |

Start with **Card + Chat Screenshot** (primary). Test **Lore Card Showcase** in r/CharacterAIrunaways after 3-4 days.

---

## Ad Copy Guidelines

### Reddit-Specific Tone

- Write like a community member, not a brand
- No emojis in the headline (Reddit downvotes emoji-heavy text)
- No "free trial" or "sign up now" — be conversational
- No "uncensored AI" in the headline (downvote bait in some communities — use "no filters" instead)

### Good Headlines

- "Finally tried an AI that actually speaks Hinglish. Not bad."
- "Made a character that speaks Tanglish. Here's the result."
- "If you're looking for an AI that actually understands Hinglish..."
- "This AI speaks Hinglish better than most people I know."

### Bad Headlines (Avoid)

- "Sign up for RVR Chat today!" (too salesy)
- "The best uncensored AI chat platform" (reads as an ad)
- Headlines with emojis

### Body Copy

- Keep it short — 2-3 sentences max
- No lists, no bullet points
- One benefit, one CTA
- Example: "RVR Chat — free to start. Supports Hinglish, Tanglish, Manglish, and more. No filters.👉 rvr.chat"

---

## Campaign Launch Checklist

- [ ] Generate card + chat screenshot image (ImageStackTemplate)
- [ ] Generate lore card showcase image (optional)
- [ ] Write headlines (3 variants for A/B testing)
- [ ] Write body copy (2-3 variants)
- [ ] Set up Reddit Ads account with $30 budget
- [ ] Create ad set with Option B targeting
- [ ] Launch with primary creative
- [ ] After 3-4 days, review metrics and test lore card showcase

---

## Subreddit Targeting (Phase 2, if Option B underperforms)

If Option B targeting doesn't convert well, fall back to subreddit-specific targeting:

| Subreddit | Type | Targeting Approach |
|-----------|------|--------------------|
| r/CharacterAIrunaways | High intent | Product demo, "no filters" pitch |
| r/CharacterAi_NSFW | High intent | Can be direct about uncensored — this community expects it |
| r/IndianGaming | Broad | Cross-promotion, gaming adjacent, SFW framing |

---

## Dependencies

- Image generation via ImageStackTemplate
- Creative assets ready before Reddit Ads launch
- Reddit Ads account set up (see PaidAdSprint.md)
- $30 budget allocated for 2 weeks
