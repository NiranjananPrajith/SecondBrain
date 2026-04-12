# WelcomePage Redesign — Landing Page Plan

**Created:** 2026-04-12
**Status:** PLANNING
**Type:** Frontend redesign

---

## Overview

The WelcomePage (homepage, `/`) is the primary landing destination for Instagram and Reddit ads. It must immediately communicate RVR's core value proposition — uncensored AI chat in Indian languages — without requiring the user to explore or figure anything out.

**Goal:** Convert ad clicks into engaged users in under 10 seconds.

---

## Why Redesign the Homepage

The current homepage ("Explore Library" from explore page hero) answers "what is this?" too slowly. The new design front-loads:

1. **Proof** — The multilingual claim is demonstrated, not described
2. **Product** — The chat experience is visible, not just promised
3. **People** — Community cards show the product is alive and used
4. **Path** — Clear next action at every scroll depth

---

## Proposed Structure

### Hero Section

**Left column (desktop) / Full width (mobile):** Varsha as receptionist anchor
- Chat widget showing Varsha's first message (live, interactive)
- Below the chat: two buttons — "Explore karo" and "Start chatting"
- Varsha's portrait/character card visible alongside

**Right column (desktop only):**
- Headline: The multilingual promise in one punchy line
- Subheadline: Supporting proof point
- Feature bullet list (3-4 items, icon + text)
- CTA: "Start Chatting" (primary) + "Explore Library" (secondary)

**Mobile:** Stacked — headline → chat widget → buttons → features

### Feature Highlights Section (below hero)

3-4 feature blocks in a row (stacked on mobile):
- **Multilingual AI** — "Speaks Hinglish, Tanglish, Manglish naturally"
- **Voice AI** — "Voice responses in Indian accents"
- **Memory** — "Remembers your conversations across sessions"
- **Privacy** — "Your chats stay private. Always."

Each block: icon + title + one-line description. No wall of text.

### Card Showcase Section

**Two rows:**
1. **Official Cards** — Top 5-8 SFW official cards, horizontal scroll or grid
2. **Community Cards** — Top 5-8 community cards, horizontal scroll or grid

Each card: thumbnail + name + language tag. Clicking opens the card. This section is social proof — the product is active, has variety, has users.

### Final CTA Section

Large, simple:
- Headline: "Ready to chat?"
- Subtext: "Free to start. No filters."
- Button: "Start Chatting with Varsha" → opens chat with varsha-welcome card

### Footer (standard)

Links, legal, copyright.

---

## Interaction Design

### Chat Widget (Hero)

- Appears as a small chat window (280-320px wide)
- Varsha's first message displayed in the widget on page load
- User can type a response (limited interaction — redirects to full chat on submit)
- Submit button: "Chat with Varsha" → opens full chat session on /scenario/varsha-welcome
- Mobile: Full-width widget

### Buttons

- "Explore karo" → `/explore`
- "Start chatting" → `/scenario/varsha-welcome`
- "Start Chatting" (hero CTA) → `/scenario/varsha-welcome`
- "Explore Library" (secondary CTA) → `/explore`

### What Happens After Varsha

Varsha's card flow (as specified in `varsha-creation-plan.md`) handles the fork:
- User picks "explore" → redirected to /explore
- User picks "chat" → proceeds with chat session

---

## Mobile Responsiveness

- Hero: stacked, Varsha chat widget full-width
- Features: 2-column grid (1 on very small screens)
- Card showcase: horizontal scroll, 2 visible at a time
- CTA: full-width button

---

## Content Details

### Hero Headline Options

**Option 1 (direct):**
"The AI that actually speaks your language."

**Option 2 (conversational):**
"Finally, an AI that understands Hinglish."

**Option 3 ( Varsha-voice):**
"Aapki digital dost aapke liye ready hai."

**Recommendation:** Option 1 or 2 for headline. Option 3 works as sub-headline or Varsha's widget intro.

### Feature Blocks (proposed)

| Feature | Title | One-liner |
|---------|-------|-----------|
| Multilingual AI | "Speaks your language" | "Hinglish, Tanglish, Manglish — naturally. No translation mode." |
| Voice AI | "Voice responses" | "Indian accents. Indian emotions. Indian context." |
| Memory | "Remembers you" | "Your character's lore persists across sessions." |
| Privacy | "Private by default" | "Your conversations stay between you and your AI. Always." |

### Card Showcase Criteria

**Official cards:** SFW-approved, multilingual-themed or popular official cards. Displayed as thumbnails with card name overlay.

**Community cards:** Top-rated community cards. Displayed same format.

Max 8 per row. Horizontal scroll if more.

---

## Implementation Notes

### varsha-welcome Card Integration

- Homepage must be updated only AFTER varsha-welcome card is created and live in the database
- The card ID for varsha-welcome must be confirmed before linking
- Fallback: if card not ready, show a static "Varsha" intro instead of live chat widget

### Ad UTMs

The homepage must support UTM parameters for ad tracking:
- `?utm_source=instagram` — tracks Instagram ad traffic
- `?utm_source=reddit` — tracks Reddit ad traffic
- `?utm_source=instagram` + `?utm_campaign=...` — full tracking

This data should be captured in the session or stored for analytics.

### SFW Compliance

All content above the fold must be SFW. The homepage is the public face — it must pass app store compliance review. No exceptions above the hero.

---

## Deliverables

1. **Redesigned WelcomePage** (`/`) — new hero, features, card showcase
2. **varsha-welcome card live** — created and integrated (see `varsha-creation-plan.md`)
3. **UTM tracking** — analytics integration for ad traffic
4. **Mobile-first QA** — tested on iOS Safari and Android Chrome

---

## Dependencies

- varsha-welcome card creation (varsha-creation-plan.md)
- Image assets for feature section (optional — can use iconography)
- UTM param handling in analytics
- SFW review of all homepage content

---

## Priority

**Phase 1 (now):** Hero section with Varsha + feature highlights + card showcase
**Phase 2 (later):** UTM tracking + analytics integration
**Phase 3 (later):** Voice AI showcase section (once voice tier is live)
