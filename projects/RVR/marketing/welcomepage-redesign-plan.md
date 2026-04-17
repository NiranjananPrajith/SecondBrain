# WelcomePage Redesign — Landing Page Plan

**Created:** 2026-04-12
**Updated:** 2026-04-12 (Option A: full-screen language selector as landing state)
**Status:** PLANNING
**Type:** Frontend redesign

---

## Overview

The WelcomePage (homepage, `/`) is the primary landing destination for Instagram and Reddit ads. It must immediately communicate RVR's core value proposition — uncensored AI chat in Indian languages — without requiring the user to explore or figure anything out.

**Goal:** Convert ad clicks into engaged chat users in under 10 seconds.

**Design Decision:** Option A — Full-screen language selector as the landing state. The page opens with 7 language buttons prominent. No chat widget, no demo, no scrolling required. One tap to enter chat. Minimal cognitive load.

---

## Conversion Funnel Logic

**Ad click** → **Language selector** → **Varsha card loads in chosen language** → **User explores or chats**

The language selector IS the conversion action. No other decision required at the top of the page.

---

## Page Structure

### Section 1 — Hero (Above the Fold)

**Full-screen language selector.** Nothing else above the fold on mobile. On desktop, a headline + subheadline sit above the widget.

**Desktop layout:**
```
[ Headline: "The AI that actually speaks your language." ]
[ Subheadline: "Choose your language and start chatting." ]
[ ─────────────────────────────────────────────── ]
[         LANGUAGE SELECTOR WIDGET              ]
[    (7 buttons, 2-col grid, centered)           ]
[ ─────────────────────────────────────────────── ]
[    "Free to start · No filters"                ]
```

**Mobile layout:**
```
[ Headline (small) ]
[ LANGUAGE SELECTOR WIDGET ]
[    (7 buttons, 2-col grid, full-width)      ]
[ "Free to start · No filters" ]
```

No scroll required. No other decision. One tap.

### Language Selector Widget

7 language buttons rendered as UI. On click, routes to the chosen Varsha card:

| Button | Destination |
|--------|-------------|
| 🇬🇧 English | `/scenario/varsha-welcome-en` |
| 🇮🇳 हिंदी/Hinglish | `/scenario/varsha-welcome-hi` |
| 🇮🇳 தமிழ்/Tanglish | `/scenario/varsha-welcome-ta` |
| 🇮🇳 ಕನ್ನಡ/Kanglish | `/scenario/varsha-welcome-kn` |
| 🇮🇳 മലയാളം/Manglish | `/scenario/varsha-welcome-ml` |
| 🇮🇳 తెలుగు/Tenglish | `/scenario/varsha-welcome-te` |
| 🇮🇳 বাংলা/Benglish | `/scenario/varsha-welcome-bn` |

Button style: Large, readable, flag emoji + language name. Not buttons with text that say "choose language" — actual language options visible immediately.

### Varsha's Prompt in the Widget

The widget header text (always English):
```
Hey! Welcome to RVR! 🌟
I'm Varsha — your digital friend!
Choose your language to get started:
```

---

### Section 2 — Below the Fold (Visible After Scroll)

For users who scroll without selecting a language first, show:

**Social proof bar:**
- "Join X registered users" or card count
- "Available in 7 languages"
- "Free to start · No filters"

**Feature highlights (3-4 blocks, icon + one-liner):**
- **Multilingual AI** — "Speaks Hinglish, Tanglish, Kanglish, Manglish, Benglish — naturally."
- **Voice AI** — "Voice responses in Indian accents."
- **Memory** — "Your character's lore persists across sessions."
- **Privacy** — "Your conversations stay private. Always."

No walls of text. Scannable.

**Card showcase:**
- Official cards — top 5-8 SFW cards, horizontal scroll
- Community cards — top 5-8 community cards, horizontal scroll

**Why scroll:** For users who aren't ready to click immediately. The below-fold content gives them reasons to stay and choose a language.

---

### Section 3 — Final CTA

For users who scrolled past the hero without selecting:

```
[ Start Chatting ] → `/scenario/varsha-welcome-en` (defaults to English)
```

Or a secondary "Explore Library" link.

---

## Desktop vs Mobile Layout

### Desktop

- Centered column, max-width ~600px for the widget
- Headline above widget, centered
- Subheadline below headline
- Widget centered below
- Social proof + features below the fold (visible after small scroll)

### Mobile

- Full-width widget
- Headline + subheadline stacked above
- Social proof + features as user scrolls
- No horizontal scrolling in any section except card showcases

---

## What Happens After Language Selection

Once user clicks a language button, they go to `/scenario/varsha-welcome-{lang}`. Varsha's card handles the next fork:

- Varsha opens in Romanized script (her first message is in the chosen language)
- User can type → model auto-switches to native script if they use native script
- Varsha offers the fork: explore cards or start chatting
- If "Explore" → `/explore`
- If "Chat" → user picks a character or continues with Varsha

See `varsha-creation-plan.md` for full Varsha flow.

---

## Content Details

### Hero Headline
"The AI that actually speaks your language."

### Hero Subheadline
"Free to start. No filters. Choose your language to begin."

### Below-Fold Tagline
"Join thousands chatting in their own language."

### Feature Blocks

| Feature | Title | One-liner |
|---------|-------|-----------|
| Multilingual AI | "Speaks your language" | "Hinglish, Tanglish, Kanglish, Manglish, Benglish — naturally." |
| Voice AI | "Voice responses" | "Indian accents. Indian emotions. Indian context." |
| Memory | "Remembers you" | "Your character's lore persists across sessions." |
| Privacy | "Private by default" | "Your conversations stay between you and your AI." |

---

## Implementation Notes

### varsha-welcome Cards Required Before Launch

7 cards total — one per language. The homepage routes to these cards directly. None of the 7 can be missing at launch.

**Storage: Static JSON in codebase, not in database.** Unlike user-created characters, Varsha's cards live as static JSON files in `data/system_assistant/varsha/` and are loaded directly by the website — no database lookup needed.

Fallback: if cards are not ready, the widget can route to a placeholder with a "coming soon" message. But the widget should not go live with missing card URLs.

### Ad UTMs

The homepage must support UTM parameters for ad tracking:
- `?utm_source=instagram`
- `?utm_source=reddit`
- `?utm_campaign=...`

This data should be captured in session or analytics for conversion attribution.

### SFW Compliance

All content must be SFW. The homepage is the public face — it must pass app store compliance review. No NSFW content, no suggestive imagery, no references to uncensored AI above the fold.

---

## Deliverables

1. **Redesigned WelcomePage** (`/`) — hero is full-screen language selector (Option A)
2. **7 varsha-welcome cards** — created and live (see `varsha-creation-plan.md`)
3. **Language selection widget** — reusable component
4. **UTM tracking** — analytics integration
5. **Mobile-first QA** — tested on iOS Safari and Android Chrome

---

## Dependencies

- All 7 varsha-welcome JSON files present in `data/system_assistant/varsha/`
- Language selection UI component built and routed
- SFW review of all homepage content
- UTM param handling in analytics

---

## Priority

**Phase 1 (now):** Hero language selector + below-fold social proof + features + card showcase
**Phase 2 (later):** UTM tracking + analytics integration
**Phase 3 (later):** Voice AI showcase section (once voice tier is live)
