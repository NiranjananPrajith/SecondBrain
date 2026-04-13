# WelcomePage Redesign — Landing Page Plan

**Created:** 2026-04-12
**Updated:** 2026-04-12 (7 language cards, language selection flow, Romanized default)
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
- Chat widget showing Varsha's universal English prompt with 7 language buttons
- On language button click → redirects to `/scenario/varsha-welcome-{lang}`
- Varsha's portrait visible alongside

**Right column (desktop only):**
- Headline: The multilingual promise in one punchy line
- Subheadline: Supporting proof point
- Feature bullet list (3-4 items, icon + text)
- CTA: "Start Chatting" (primary) + "Explore Library" (secondary)

**Mobile:** Stacked — headline → chat widget → buttons → features

### Feature Highlights Section (below hero)

3-4 feature blocks in a row (stacked on mobile):
- **Multilingual AI** — "Speaks Hinglish, Tanglish, Kanglish, Manglish naturally"
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
- Button: "Start Chatting" → opens chat with selected Varsha card (or defaults to English)

### Footer (standard)

Links, legal, copyright.

---

## Language Selection Flow (Hero Widget)

The chat widget in the hero is Varsha's language selection interface:

```
[ Widget: 280-320px wide ]

Hey! Welcome to RVR! 🌟
I'm Varsha — your digital friend!
Choose your language to get started:

[ 🇬🇧 English ]
[ 🇮🇳 हिंदी/Hinglish ]
[ 🇮🇳 தமிழ்/Tanglish ]
[ 🇮🇳 ಕನ್ನಡ/Kanglish ]
[ 🇮🇳 മലയാളം/Manglish ]
[ 🇮🇳 తెలుగు/Tenglish ]
[ 🇮🇳 বাংলা/Benglish ]
```

On click, routes to:
- `/scenario/varsha-welcome-en` (English)
- `/scenario/varsha-welcome-hi` (Hinglish)
- `/scenario/varsha-welcome-ta` (Tanglish)
- `/scenario/varsha-welcome-kn` (Kanglish)
- `/scenario/varsha-welcome-ml` (Manglish)
- `/scenario/varsha-welcome-te` (Tenglish)
- `/scenario/varsha-welcome-bn` (Benglish)

### Button Layout in Widget

2-column grid inside the widget. Each button: flag emoji + language name (English name, not native script).

---

## Interaction Design

### Chat Widget (Hero)

- Appears as a small chat window (280-320px wide)
- Shows Varsha's universal English prompt + 7 language buttons on page load
- No typing required — just one click to enter chat in chosen language
- Mobile: Full-width widget

### Buttons

Routing for all CTAs:
- Language buttons in widget → `/scenario/varsha-welcome-{lang}`
- "Explore karo" → `/explore`
- "Start chatting" → `/scenario/varsha-welcome-en` (defaults to English)
- "Explore Library" (secondary) → `/explore`

### What Happens After Varsha

Varsha's card flow (as specified in `varsha-creation-plan.md`) handles the fork:
- User picks "explore" → redirected to /explore
- User picks "chat" → proceeds with chat session in chosen language
- Model auto-switches to native script if user types in native script

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

**Recommendation:** Option 1 or 2 for headline.

### Feature Blocks (proposed)

| Feature | Title | One-liner |
|---------|-------|-----------|
| Multilingual AI | "Speaks your language" | "Hinglish, Tanglish, Kanglish, Manglish, Benglish — naturally. No translation mode." |
| Voice AI | "Voice responses" | "Indian accents. Indian emotions. Indian context." |
| Memory | "Remembers you" | "Your character's lore persists across sessions." |
| Privacy | "Private by default" | "Your conversations stay between you and your AI. Always." |

### Card Showcase Criteria

**Official cards:** SFW-approved, multilingual-themed or popular official cards. Displayed as thumbnails with card name overlay.

**Community cards:** Top-rated community cards. Displayed same format.

Max 8 per row. Horizontal scroll if more.

---

## Implementation Notes

### varsha-welcome Cards Integration

7 cards total — one per language. Homepage must be updated only AFTER all 7 cards are created and live in the database. Card IDs must be confirmed before linking.

Fallback: if cards not ready, show a static "Varsha" intro with language buttons that route to a placeholder.

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

1. **Redesigned WelcomePage** (`/`) — new hero with language selection, features, card showcase
2. **7 Varsha cards live** — created and integrated (see `varsha-creation-plan.md`)
3. **UTM tracking** — analytics integration for ad traffic
4. **Mobile-first QA** — tested on iOS Safari and Android Chrome

---

## Dependencies

- All 7 varsha-welcome cards created (varsha-creation-plan.md)
- Language selection UI component built
- Image assets for feature section (optional — can use iconography)
- UTM param handling in analytics
- SFW review of all homepage content

---

## Priority

**Phase 1 (now):** Hero section with Varsha language selection + feature highlights + card showcase
**Phase 2 (later):** UTM tracking + analytics integration
**Phase 3 (later):** Voice AI showcase section (once voice tier is live)
