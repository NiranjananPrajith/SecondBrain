# RVR Paid Ad Sprint — Instagram + Reddit (Week 1)

**Created:** 2026-04-10
**Status:** ACTIVE — Launch-ready site, running parallel to organic seeding plan

---

## Context

Website is functionally complete (Supabase OAuth ✅, Stripe Checkout ✅, core chat ✅). Need **meaningful traffic within days**, not months of organic seeding. This plan runs **parallel** to `CommunityAndFandomPlan.md` (organic long-term) and `CardSubmissionGuidelines.md`.

---

## Phase 1: Pre-Launch (Days 1-2)

### Instagram Setup
- [x] Convert to **Instagram Business account** ✅ (2026-04-11)
- [x] Bio: "Uncensored AI chats in English, हिंदी/Hinglish, বাংলা/Benglish, তমিল/Tanglish, ಕನ್ನಡ/Kanglish, മലയാളം/Manglish, తెలుగు/Tenglish." ✅ (2026-04-11)
- [x] Enable **Shopping/Link in bio** (rvr.chat) ✅ (2026-04-11)

### Creative Assets
**A. Promo Video (already exists)**
- 30s, 9:16 vertical card-flip video
- Card reveals: Remya → Swetha → Anya → Shika
- Ends with velvet shadow + "rvr.chat" button
- Audio: Sultry R&B, female vocalist
- **Primary Instagram Reels/Story ad creative**

**B. Image Stacks (per scenario)**
- 5-card cinematic carousel per scenario (see `ImageStackTemplate.md`)
- Format: 9:16, 1080×1920px, dark luxury aesthetic
- Post 3-5 as organic carousels, boost best 2

### Ad Accounts
- [ ] Set up **Meta Ads Manager** (Instagram + Facebook)
- [ ] Set up **Reddit Ads** account
- [ ] Define audiences now (don't wait until launch day)

---

## Phase 2: Launch Ads (Days 2-3)

### Instagram Ads

**Creative A: Promo Video (Primary)**
- Format: Reels/Story ad, 9:16, 30s
- Objective: Website clicks / signups
- Targeting: India, 18-35, interests: AI, chatbots, RPGs, anime, Indian cinema, Hinglish, Tanglish, Kanglish, Manglish
- Budget: $25/day × 7 days (Instagram allocation: $70/2 weeks)
- CTA: "Learn more" → rvr.chat

**Creative B: Carousel Boost (×2)**
- Boost top 2 scenario card image stacks
- Targeting: Same as above
- Budget: $10/day each

### Reddit Ads

**Why Reddit:** r/CharacterAIrunaways + r/CharacterAi_NSFW audiences are actively hunting for alternatives. High intent, lenient policies on "uncensored AI" framing. Lower CPM than Instagram.

**Targeting: Option B — Interest + Keyword**
- Interests: `AI products, chatbots, anime, roleplaying games, Indian entertainment`
- Keywords: `Character AI alternative, uncensored AI chat, Hinglish AI, Tanglish, Kanglish, Manglish, Indic AI, AI companion India`
- Budget: $30/2 weeks (~$15/day × 2 weeks)

**Creative:**
- Headline: "Finally, an AI that speaks Hinglish. No filters."
- Body: "Chat with Indian AI companions that understand your language, culture, and slang. Free to start."
- CTA: "Try rvr.chat →"

### Budget Allocation

| Channel | Budget | Duration | Daily Rate |
|---------|--------|----------|------------|
| Instagram Reels video ad | $70 | 2 weeks | ~$5/day |
| Instagram carousel boost (×2) | — | 2 weeks | part of IG budget |
| Reddit ads (Option B) | $30 | 2 weeks | ~$2.15/day |
| **Total** | **$100** | **2 weeks** | |

*Cap: $100 for 2 weeks. Reassess at Day 14 — add budget only if ROAS > 1.*

---

## Phase 3: Optimize (Days 4-7)

- [ ] Review Instagram metrics — which creative has lowest CPM + highest CTR?
- [ ] Review Reddit metrics — which subreddit targeting converts best?
- [ ] Keep only winning ads running
- [ ] Post 2-3 more scenario card image stacks organically each day
- [ ] Engage on boosted post comments (boosted posts get comment visibility)

---

## Success Metrics (Week 1)

| Metric | Target |
|--------|--------|
| New signups | 20-50 |
| Instagram followers | +50-100 |
| Instagram ad CTR | >2% |
| Reddit ad CTR | >0.5% |
| Cost per signup | <$3 |

---

## Promo Video Reference

| Time | Visual | Audio |
|------|--------|-------|
| 00:00-00:01 | Cards on velvet, gold particles, top card lifts | R&B track begins |
| 00:02-00:08 | Ace of Hearts → "REMYA" — "Hey, check out RVR chat" | Soft music |
| 00:08-00:11 | Morphs to "SWETHA" — "Explore our huge collection of fantasy scenarios" | — |
| 00:12-00:16 | Morphs to "ANYA" — "Chat with any of these characters" | — |
| 00:16-00:20 | Morphs to "SHIKA" — "Or create characters and scenarios of your own" | — |
| 00:20-00:25 | Card flips to back: "VISIT rvr.chat TO START CHATTING" | Vocal: "Lace on the floor..." |
| 00:25-00:29 | Cards drop away into black shadow | "Teeth on my shoulder..." |
| 00:30-00:37 | Black screen, white "rvr.chat" button | "Candles drip slow..." fades out |

---

## Dependencies

- Website must be live at rvr.chat with OAuth + Stripe functional ✅
- At least 3-5 scenario cards ready for image stack generation
- Image generation access (Nano Banana or Google Flow)
- $100 ad budget available (capped for 2 weeks)
- **Meta Pixel** installed on rvr.chat for retargeting (Instagram)
