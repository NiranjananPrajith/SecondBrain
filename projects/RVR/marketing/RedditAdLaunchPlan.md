---
title: Reddit Ad Launch Plan
created: 2026-04-14
tags: [rvr, marketing, reddit, advertising, launch]
---

# Reddit Ad Launch Plan — First Campaign

**Status:** READY TO EXECUTE
**Budget:** $30 for 2 weeks (~$2.15/day)
**Objective:** Drive qualified Indian traffic to rvr.chat, targeting CTR > 0.5%, cost/signup < $3

**Source documents:**
- [[RedditAdStrategy]] — master strategy (targeting, format, tone, copy guidelines)
- [[PaidAdSprint]] — sprint plan + budget allocation
- [[ImageStackTemplate]] — image generation system prompt
- [[CharacterCardSystemPrompt]] — character card JSON generation

---

## Step 1: Character Cards

### Primary: Remya (Hinglish)
- **Why:** First card reveal in promo video (Ace of Hearts). Brand's visual anchor. Speaks Hinglish — largest code-mixed language by speaker count. Cross-channel consistency.
- **Status:** Character file exists at `content/characters/remya.md` in RVR codebase. Image at `public/images/characters/remya.webp`. Card data is minimal — may need enrichment for ad creative.
- **Action:** Verify Remya's full card JSON (with speech patterns, appearance details) exists in database. If not, generate via CharacterCardSystemPrompt.

### Secondary: Tanglish character (Sakura or Kanna)
- **Purpose:** Day 4+ A/B test with Tanglish angle for South Indian reach
- **Status:** Not yet created (planned under anime/waifu category)
- **Action:** Generate card JSON using CharacterCardSystemPrompt

---

## Step 2: Creative Assets

All images: dark luxury aesthetic (deep reds, blacks, gold accents, cinematic lighting). No watermarks, no logos. Generated via [[ImageStackTemplate]].

### Asset A — Card + Chat Screenshot (PRIMARY)
- **Format:** 16:9 landscape, 1080x608px
- **Layout:** Left half = Remya character card visual. Right half = short Hinglish chat exchange.
- **Chat exchange:**
  - User: "Tum kitne smart ho actually"
  - Remya: "Itne smart ki tum impressed ho jaoge, dekh lena"
  - One more exchange showing personality
- **Constraint:** Text must be legible at Reddit feed thumbnail size. Large sans-serif font, white/gold on dark.

### Asset B — Lore Card Showcase (SECONDARY)
- **Format:** 9:16 portrait, 1080x1920px
- **Content:** Tanglish character card only — no chat overlay
- **Background:** Dark luxury (not white — white reads as clinical in Reddit feeds)

### Asset C — Language Comparison (OPTIONAL)
- **Format:** 16:9 landscape, 1080x608px
- **Content:** Split-screen — English chat (left) vs Tanglish chat (right)
- Only create if time permits

---

## Step 3: Ad Copy Variants

### Headlines (3 for A/B testing)

| ID | Headline | Angle |
|----|----------|-------|
| A | `Finally tried an AI that actually speaks Hinglish. Not bad.` | First-person, Hinglish hook |
| B | `Made a character that speaks Tanglish. Here's the result.` | Curiosity, Tanglish angle |
| C | `If you're looking for an AI that actually understands Hinglish...` | Problem/solution |

### Body Copy (2 variants)

| ID | Body Text | Style |
|----|-----------|-------|
| 1 | `RVR Chat — free to start. Supports Hinglish, Tanglish, Manglish, and more. No filters. rvr.chat` | Feature-led |
| 2 | `Chat with Indian AI companions that understand your language, culture, and slang. Free to start. rvr.chat` | Benefit-led |

### A/B Test Matrix

| Variant | Headline | Body | Image | When |
|---------|----------|------|-------|------|
| 1 (launch) | A (Hinglish) | 1 (Direct) | Asset A (Remya card+chat) | Day 0 |
| 2 (test) | B (Tanglish) | 2 (Conversational) | Asset B (Tanglish lore card) | Day 4-7 if CTR low |
| 3 (swap) | C (Problem) | 1 (Direct) | Asset A (Remya card+chat) | Day 8-10 if needed |

---

## Step 4: Reddit Ads Dashboard Setup

Log into ads.reddit.com with u/_rvrdev_ account.

### Campaign
- **Name:** `RVR Launch v1 — Reddit 2wk`
- **Objective:** Traffic (not Brand Awareness, not Conversions)
- **Budget type:** Daily — $2.15/day

### Ad Set
- **Name:** `RVR v1 — Interest+Keyword India`
- **Placement:** Feed only (de-select Conversation + Sidebar)
- **Geo:** India (entire country)
- **Age:** 18-35
- **Gender:** All
- **Interests:** AI Products, Chatbots, Anime, Roleplaying Games, Indian Entertainment
- **Keywords:** `Character AI alternative`, `uncensored AI chat`, `Hinglish AI`, `Tanglish`, `Kanglish`, `Manglish`, `Indic AI`, `AI companion India`, `AI roleplay`, `no filter AI`

### Ad Creative
- **Format:** Image
- **Image:** Asset A (Remya card+chat, 1080x608)
- **Headline:** Variant A
- **Body:** Variant 1
- **CTA:** Visit
- **Destination URL:** `https://rvr.chat/?utm_source=reddit&utm_medium=cpc&utm_campaign=rvr_launch_v1&utm_content=headline_a_remya`
- **Display URL:** `rvr.chat`

### UTM Tracking Per Variant
- Variant 1: `utm_content=headline_a_remya`
- Variant 2: `utm_content=headline_b_tanglish_lore`
- Variant 3: `utm_content=headline_c_remya`

---

## Step 5: Launch Timeline

### Pre-Launch (Day -3 to Day -1)
- **Day -3:** Generate/enrich character cards (Remya + Tanglish character). Write chat exchange script.
- **Day -2:** Generate all image assets via ImageStackTemplate. Finalize all copy variants.
- **Day -1:** Configure Reddit Ads dashboard. Upload assets. Submit for review. Verify UTM tracking on rvr.chat.

### Launch (Day 0)
- Ad goes live after Reddit approval (~24h from submission).
- Screenshot the ad as it appears in feed.
- Record start date/time.

---

## Step 6: Post-Launch Optimization

### Day 1-3: Observe (Do Not Touch)
- Record daily: impressions, clicks, CTR, CPC, spend
- Expected benchmarks: CPM $0.60-$1.50, CPC $0.10-$0.25
- **Red flag:** CTR < 0.15% after Day 2 → prepare Variant 3
- **Green flag:** CTR > 0.5% → do nothing, it's working

### Day 4: First Checkpoint

| CTR Range | Action |
|-----------|--------|
| > 0.5% | Keep running. Add Variant 2 (lore card) as second ad. |
| 0.3-0.5% | Swap headline to Variant C. Same image. Monitor 3 more days. |
| < 0.3% | Full creative swap: new headline (C) + new image (Asset B). |
| < 0.1% | Pause. Check for delivery issues in dashboard. |

### Day 8-10: Phase 2 Test
- If CTR > 0.3%, launch Variant 2 (lore card + Tanglish headline) as second ad.
- Reddit auto-allocates budget to the better performer.

### Day 11-14: Coast
- Let the winner run. Export all data on Day 13.

### Day 15: Decision

| Outcome | Next Step |
|---------|-----------|
| CTR > 0.5%, cost/signup < $3 | Scale to $50/month Reddit budget |
| CTR > 0.5%, cost/signup > $3 | Fix WelcomePage before scaling |
| CTR 0.3-0.5%, some signups | Test subreddit-specific targeting (r/CharacterAIrunaways) |
| CTR < 0.3%, no signups | Shift remaining budget to Instagram |

---

## Risk Mitigation

- **Ad disapproval:** Reddit may flag "no filters." Fallback: change to "Your conversations stay private." Remove "uncensored" from keywords.
- **Low karma:** Only matters for organic posts, not paid ads. Continue organic engagement in parallel.
- **Budget exhaustion:** At $2.15/day with CPM $0.60-$1.50, expect ~1,400-3,600 impressions/day. If spend runs hot, narrow keywords.
- **Landing page not ready:** Verify WelcomePage (language selector) is live before ad runs. Cold traffic on a generic page = wasted spend.

---

## Dependencies

- [x] Reddit Ads account set up
- [ ] Remya full character card JSON (verify in database)
- [ ] Asset A image (card+chat, 16:9) — generate via ImageStackTemplate
- [ ] Asset B image (lore card, 9:16) — generate via ImageStackTemplate
- [ ] Ad copy variants finalized
- [ ] UTM tracking verified on rvr.chat
- [ ] $30 budget + payment method confirmed
- [ ] WelcomePage live with language selector
