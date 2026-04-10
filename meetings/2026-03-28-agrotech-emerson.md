---
title: AgroTech Meeting - Emerson
date: 2026-03-28
attendees: Dr. Rowen, Emerson
goal: Define minimum-budget path to start building/testing/researching AgroTechCompany
---

# AgroTech Meeting — March 28, 2026

## Context

AgroTechCompany is a **tech company** (not a farming company) that develops hardware + software for indoor farming. Core thesis: outdoor farming will become climate-untenable; indoor controlled-environment agriculture (CEA) is the future but growers lack integrated tech solutions.

**Team roles:**
- Dr. Rowen — Tech Lead
- Emerson — Agriculture / Research
- Cityboy — Business Development

**Goal today:** Find the cheapest, fastest way to start building prototypes and validating the concept.

---

## Discussion Points

### 1. What Are We Actually Building?

Four focus areas (software-intensive + minimal hardware to start):

| Area | Description | AI Role |
|------|-------------|---------|
| **Crop Monitoring** | Sensor dashboard — temp, humidity, soil moisture, light | Anomaly detection, alerts |
| **Indoor Farming Automation** | Automated watering, lighting, nutrient dosing | Schedule optimization |
| **Yield Prediction** | Historical data → growth forecasts | ML regression models |
| **IoT Control Center** | Central hub to monitor + control all devices | NLP querying ("turn off lights") |

Start with ONE focus area — recommendation: **crop monitoring dashboard** as the foundation everything else builds on.

---

### 2. Minimum Budget Prototype Stack

```
Sensors → ESP32 (WiFi) → MQTT Broker (Raspberry Pi) → Node-RED Dashboard
                          ↘ TimescaleDB (PostgreSQL extension)
                          ↘ AIML API (anomaly detection, insights)
                          ↘ Grafana (visualization)
```

**Hardware:**
- ESP32 dev board: ~$5 (has WiFi, more capable than Arduino)
- Soil moisture sensor: ~$3
- DHT22 temp/humidity: ~$4
- BH1750 light sensor: ~$3
- 4-channel relay module: ~$5
- Raspberry Pi Zero 2 W: ~$25 (MQTT broker + database)
- **Total: ~$45-60** for one fully-instrumented grow shelf

**Software (all free tier / open source):**
- MQTT Broker: Mosquitto (free, self-hosted)
- Dashboard: Node-RED (free, browser-based drag-drop)
- Database: TimescaleDB community (free, self-hosted)
- Visualization: Grafana (free, self-hosted)
- AI: Existing AIML API access (already have)

**No cloud costs. No subscription hardware.**

---

### 3. The "Minimum Viable Farm" Concept

Start with **ONE grow shelf** — fully instrumented, end-to-end working.

**The full loop we want to prove:**
```
Sensor reads soil moisture → ESP32 publishes to MQTT
→ Pi receives → Node-RED evaluates → IF moisture < threshold
→ Raspberry Pi sends relay command → pump turns on
→ Dashboard shows: "Pump activated at 2:34 PM"
```

Once this loop works:
- Add more sensors
- Replace manual thresholds with ML model
- Add NLP control ("Hey, check on the tomatoes")
- Add second grow shelf, then third...

---

### 4. AI Layer — Where It Actually Adds Value

| AI Application | Why It's Valuable | Why It's NOT Too Early |
|---------------|-------------------|----------------------|
| **Anomaly detection** | Plant stress visible in sensor data before肉眼可见 | Sensors already deployed, just need the model |
| **NLP dashboard queries** | "Which zone used most water this week?" | Easy API call to LLM with sensor context |
| **Automated schedule optimization** | "Lights were on 2h too long — saved $4/week" | Historical data needed first, but basic rules can start now |
| **Yield forecasting** | "Based on last 30 days, expect harvest in ~47 days" | Need 2-3 grow cycles of data first |

**Start now:** Rule-based alerts (IF moisture < 30% → notify)
**Start later:** ML anomaly detection (after 1-2 months of baseline data)
**Start much later:** Yield prediction (after multiple full grow cycles)

---

### 5. Data Strategy

First, build a **time-series sensor database** — every reading, forever.

This data is the moat:
- Train models on YOUR specific setup
- Competitors can't replicate (they don't have your grow data)
- Improves with every grow cycle

**TimescaleDB** handles this natively — PostgreSQL-compatible, automatic time-indexing, SQL queries.

---

### 6. Emerson's Role Today

1. **Define the grow environment** — what's the actual setup? (grow tent, shelf, greenhouse?)
2. **Sensor placement** — where should sensors go? (soil depth, canopy height, etc.)
3. **Crop selection** — what are we actually growing? (herbs? leafy greens? saffron?)
4. **Success metrics** — what does "working" look like? (yield improvement? reduced water usage?)

---

### 7. Budget Decision Tree

| Budget | Path |
|--------|------|
| **~$50** | ESP32 + 3 sensors + Pi Zero 2 W. Manual thresholds. No ML. |
| **~$100** | Above + second ESP32 + more sensors. Start collecting baseline data. |
| **~$200** | Full shelf instrumented. MQTT + Node-RED + TimescaleDB running. Begin rule-based automation. |
| **~$500+** | Multiple zones. Basic ML anomaly detection via AIML API. NLP dashboard queries. |

**Recommended starting point: ~$100** — enough to prove the loop, start collecting data.

---

## Decisions to Make Today

1. **What are we growing first?** (determines sensor requirements)
2. **Where is the grow happening?** (determines environment challenges)
3. **Who does what?** — split: Emerson (agriculture), Dr. Rowen (software/hardware), Cityboy (business)
4. **Timeline?** — first prototype working by [when]?

---

## Next Steps After Meeting

- [ ] Emerson: Research crop-specific sensor requirements
- [ ] Dr. Rowen: Order ESP32 + sensor kit (~$40-60)
- [ ] Dr. Rowen: Set up Pi Zero 2 W + Mosquitto + TimescaleDB
- [ ] Cityboy: Research competitor indoor farming tech platforms
- [ ] Schedule follow-up: 2 weeks to have hardware ordered, 4 weeks to have MQTT loop working
