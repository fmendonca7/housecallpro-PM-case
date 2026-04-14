# Problem Framing — Competitor Signal Intelligence

## Lead Enrichment for HCP Sales & Growth Teams

---

## 1. Problem Statement

### 1.1 The Business Problem

HCP's customer acquisition cost (CAC) is inflated by a sales motion that burns rep capacity on research instead of selling. Three dynamics compound to create a growth ceiling:

1. **Pipeline velocity is throttled.** Reps contact 15-25 leads/day — industry benchmark for tool-assisted teams is 60-100+. The gap is not effort; it's that reps spend 15-20 minutes per lead on manual research before picking up the phone.
2. **Conversion is suppressed.** Outreach is overwhelmingly generic — less than 10% of cold emails reference the prospect's competitive context. Personalized outreach (mentioning the competitor by name, citing a specific switching advantage) converts 2-3x higher, but reps simply don't have the data to personalize.
3. **High-fit leads are invisible.** Without knowing what software a prospect runs, reps can't prioritize. A ServiceTitan user at $250+/tech/month is a fundamentally different opportunity than a greenfield business on paper — but both look identical in a CRM lead list.

The root cause across all three: **reps cannot reliably determine which competitor FSM software a prospect currently uses.** The workflow that produces this gap:

1. Google the business name → visit website (if one exists)
2. Check Google Business Profile, Yelp, and review sites
3. Try to infer what tools the prospect uses
4. Craft outreach — usually generic, because step 3 almost always fails

At scale (hundreds of prospects/week), this is a structural drag on growth — not a workflow inconvenience.

### 1.2 The Specific Pain Point We're Targeting

> **"Sales reps lack structured intelligence about which competitor FSM software a prospect is running — so they can't personalize outreach, can't prioritize high-fit leads, and waste time on prospects who aren't a match."**

This is the **highest-leverage** pain point because:

| Factor | Why It's Highest Leverage |
|---|---|
| **Universality** | Affects every single outbound interaction — not a niche problem |
| **Direct revenue impact** | Personalized outreach converts 2-3x higher than generic |
| **Compounding effect** | Better prioritization → reps work fewer, better leads → lower CAC, higher win rate |
| **Data already exists** | Our POC proved: competitor signals are publicly available (GBP links, HTML widgets, job postings) — they just aren't being captured |
| **No existing solution** | No tool in the market specifically detects FSM software usage for home service SMBs |

### 1.3 Why NOT Other Pain Points?

| Alternative Pain Point | Why It's Lower Leverage |
|---|---|
| "Reps waste time on bad leads (wrong ICP)" | Important, but solved by ICP definition + lead scoring — a process problem, not a data problem |
| "Contact data is stale" | Solved by ZoomInfo/Apollo/Clearbit — existing tools handle this adequately |
| "Reps don't know when a prospect is ready to buy" | Intent data (6sense, Bombora) partially solves this — and it's much harder to build from scratch |
| "Outreach messaging isn't optimized" | Downstream of knowing the prospect's context — solve the data gap first, messaging improves automatically |

---

## 2. What "Good" Looks Like vs. What "Failure" Looks Like

### 2.1 Definition of Good

**"Good" at the business level** means three measurable shifts:

| Outcome | Before | After | Why It Matters |
|---|---|---|---|
| **Leads contacted per rep per day** | 15-25 | 50+ | Pipeline velocity directly drives top-of-funnel volume |
| **Outreach personalization rate** | ~10% mention competitor | 60%+ | Personalized cold outreach converts 2-3x higher |
| **Rep research time per lead** | 15-20 min | < 2 min | Time returned to selling = more conversations = more pipeline |

If these three numbers move in the right direction, the downstream effects follow: higher reply rates, more meetings booked, shorter sales cycles, and lower CAC.

**What this looks like in practice — three scenarios:**

**Scenario A: High-value competitive conversion (ServiceTitan user)**
> Rep opens lead "Plumbline Services, Denver" in the enrichment dashboard.
> System shows: **🟢 ServiceTitan (Scheduling Pro) — 95% confidence**
> Evidence: "GBP booking link: book.servicetitan.com/am8bmapr"
> Suggested pitch: "You're paying $250+/tech/month. HCP delivers 80% of the value at a fraction of the cost, with zero implementation fee."
>
> **Business impact:** Rep sends personalized cold email mentioning ServiceTitan directly. Prospect replies: "How did you know? We've been frustrated with the costs." Meeting booked. Without competitive intelligence, this email would have been generic — and ignored.

**Scenario B: Research time eliminated (likely Jobber)**
> Rep opens lead "Green Can Cleaners, Boulder."
> System shows: **🟡 Likely Jobber — 68% confidence**
> Evidence: "Client Hub booking page found via Google index"
> Note: "Verify before outreach — medium confidence"
>
> **Business impact:** Rep opens the evidence link, confirms in 30 seconds, tailors pitch. What used to take 15 minutes of manual research now takes under a minute. Multiplied across 50 leads/day, that's 12+ hours of selling capacity returned per rep per week.

**Scenario C: Greenfield identification (no FSM detected)**
> Rep opens lead "Joe's Electrical, Colorado Springs."
> System shows: **⚪ No FSM software detected — likely manual/paper-based**
> Evidence: "No website, 7 Google reviews, established 2023. No booking links. No job postings."
> Suggested pitch: "Still running on paper? HCP is the easiest way to go digital."
>
> **Business impact:** Even the absence of signals is actionable intelligence. The rep uses the "digital transformation" talk track instead of a competitive angle. This is a different type of lead — and now reps can segment and prioritize accordingly.

### 2.2 Definition of Failure

**Failure Type 1: Confident Wrong** ⛔ (WORST)
> System says: "🟢 ServiceTitan — 92% confidence"
> Reality: Prospect uses Jobber.
> **Consequence:** Rep opens with "I see you're on ServiceTitan..." — prospect is confused, HCP looks sloppy and untrustworthy. Deal is dead before it started.
>
> **Threshold:** This must happen <2% of the time for "High Confidence" (🟢) predictions.

**Failure Type 2: Confident When No Signal** ⚠️
> System says: "🟡 Likely FieldEdge — 70% confidence"
> Reality: Prospect has no FSM software at all.
> **Consequence:** Rep wastes time on competitive pitch when a "go digital" pitch would work. Less damaging than Type 1, but still wastes effort and misaligns messaging.
>
> **Threshold:** <5% of "likely" predictions.

**Failure Type 3: Missed Signal** 🔍
> System says: "⚪ Unknown — no signal"
> Reality: Prospect clearly uses Workiz (booking iframe on their website).
> **Consequence:** Missed opportunity for personalization. Rep sends generic outreach. Not catastrophic but reduces conversion potential.
>
> **Threshold:** <15% miss rate for prospects with detectable signals.

**Failure Type 4: System Unusable**
> Enrichment takes 5+ minutes per lead.
> UI is confusing. Results are hard to interpret.
> **Consequence:** Reps ignore the tool and go back to manual research.
>
> **Threshold:** Enrichment < 30 seconds per lead. UI clarity score > 4/5 in user testing.

---

## 3. Solution Design

### 3.1 The Approach: AI-Powered Competitive Signal Intelligence

A **multi-stage hybrid pipeline** that combines deterministic rules with LLM inference to detect and classify which FSM software a prospect is likely using.

```
                    ┌─────────────────────────────┐
                    │     LEAD INPUT               │
                    │  Business name + Location    │
                    └──────────┬──────────────────┘
                               │
                    ┌──────────▼──────────────────┐
                    │  TIER 0: GBP Link Analysis   │  Rules-based
                    │  Google Places API            │  ~90% coverage
                    │  → Match booking URL domain   │  100% confidence when found
                    └──────────┬──────────────────┘
                               │
                         Found? ──YES──→ 🟢 Return result
                               │
                              NO
                               │
                    ┌──────────▼──────────────────┐
                    │  TIER 1: Website HTML Scan    │  Rules-based
                    │  Fetch + parse HTML source   │  ~40% coverage
                    │  → Regex for widget patterns  │  100% confidence when found
                    └──────────┬──────────────────┘
                               │
                         Found? ──YES──→ 🟢 Return result
                               │
                              NO
                               │
                    ┌──────────▼──────────────────┐
                    │  TIER 2: Signal Extraction    │  AI-powered (NLP)
                    │  Job postings, reviews,       │  Variable coverage
                    │  social media                │  Medium confidence
                    │  → LLM extracts tool mentions │
                    └──────────┬──────────────────┘
                               │
                         Found? ──YES──→ 🟡 Return result + evidence
                               │
                              NO
                               │
                    ┌──────────▼──────────────────┐
                    │  TIER 3: AI Inference         │  AI-powered (LLM)
                    │  Combine all weak signals:    │  Universal coverage
                    │  vertical, size, region,      │  Low-medium confidence
                    │  website sophistication,     │
                    │  pricing language             │
                    │  → LLM reasons probabilistically │
                    └──────────┬──────────────────┘
                               │
                               ▼
                    🟡/🔴/⚪ Return result + reasoning + evidence
```

### 3.2 What AI Does Autonomously vs. Hands Back to Human

| Action | AI (Autonomous) | Human (Rep) |
|---|---|---|
| Fetch GBP data and scan booking URLs | ✅ | |
| Scan website HTML for widget fingerprints | ✅ | |
| Extract tool mentions from reviews/job posts | ✅ | |
| Classify competitor and assign confidence | ✅ | |
| Generate suggested talk track | ✅ | |
| **Decide whether to use the inference in outreach** | | ✅ Rep decides |
| **Override incorrect classification** | | ✅ Feedback loop |
| **Handle 🔴 low-confidence results** | Flag for review only | ✅ Rep manually researches |
| **Final outreach execution** | | ✅ Always human |

**The line is clear:** AI handles research and classification. Humans handle judgment and action. AI never sends an email or makes a call — it arms the rep with intelligence.

### 3.3 Non-AI Alternative Considered

**Alternative: Rules-Only Detection Engine**

A system that uses only deterministic regex rules to scan websites and GBP links for known FSM widget patterns, with no LLM component.

| Dimension | Rules-Only | Hybrid (Rules + AI) |
|---|---|---|
| **Tier 0+1 (GBP + HTML widgets)** | ✅ Identical performance | ✅ Identical performance |
| **Tier 2 (NLP from reviews/jobs)** | ❌ Fails — regex can't understand "we just migrated from ServiceTitan" vs "we compete with ServiceTitan" | ✅ LLM understands context and intent |
| **Tier 3 (Multi-signal inference)** | ❌ Impossible — rules can't reason across weak signals | ✅ LLM excels at probabilistic reasoning: "HVAC + 50 techs + major metro + no detected widget = likely ServiceTitan or FieldEdge" |
| **FieldEdge detection** | ❌ ~0% detection (no public widget) | ✅ ~45% via inference from indirect signals |
| **Handling new/unknown competitors** | ❌ Every new competitor = new manual rules | ✅ Update prompt, LLM generalizes |
| **Confidence scoring** | Binary (found/not found) | Calibrated probability with evidence chain |
| **Maintenance** | High — regex breaks when competitors change embed patterns | Low — AI adapts, rules only for stable patterns |

**Why AI is the better choice:**

Rules-only captures ~60% of the value (Tier 0+1 detection) but misses the most strategically important cases:
1. **FieldEdge users** — completely invisible without AI inference
2. **Context-dependent signals** — "we're moving away from Jobber" vs "we love Jobber" require understanding
3. **Composite confidence** — combining 5 weak signals into one actionable score requires reasoning, not thresholds
4. **The "Unknown" classification** — AI can give useful output ("likely no FSM — greenfield prospect") even when there's no direct signal

The hybrid approach captures ~85%+ of value: rules do the cheap/fast work, AI does the hard/valuable work.

---

## 4. Trust & Failure Framework

### 4.1 Confidence Tiers — What the Rep Sees

```
🟢 CONFIRMED (85%+)
   Direct signal found (GBP link, website widget, explicit mention)
   → Rep CAN reference in outreach with confidence
   → UI: Green badge, "Confirmed: [Tool]", evidence link
   → Example: "book.servicetitan.com found on GBP"

🟡 LIKELY (50-84%)
   Indirect signals or AI inference with moderate evidence
   → Rep SHOULD verify before using in cold outreach
   → UI: Yellow badge, "Likely: [Tool]", "Verify before outreach" note
   → Example: "Job posting on Indeed mentions FieldEdge + HVAC vertical match"

🔴 POSSIBLE (25-49%)
   Weak signals, conflicting evidence, or inference with low support
   → Rep should treat as hypothesis, NOT fact
   → UI: Red badge, "Possible: [Tool]", "Low confidence — manual review suggested"
   → Example: "Company profile matches typical FieldEdge customer, but no direct evidence"

⚪ UNKNOWN (below 25% or no signal)
   No signals found at all
   → System explicitly states "No data" rather than guessing
   → UI: Gray badge, "No software detected", context note
   → Example: "No website, limited online presence. Likely manual/paper-based."
```

### 4.2 When the System Is Wrong

Every prediction displays:
- **Evidence trail:** Links and snippets showing exactly WHY the system concluded what it did
- **Confidence breakdown:** "GBP link: +40%, HTML widget: +35%, vertical match: +15%"
- **Feedback buttons:** "✅ Correct" / "❌ Wrong — they actually use [X]"

When a rep marks a prediction as wrong:
1. Correction is logged immediately
2. The lead's record is updated with ground truth
3. Aggregated corrections feed accuracy dashboards
4. Over time, corrections improve the inference model's calibration

### 4.3 How Trust Builds Over Time

| Week | Trust Level | What Happens |
|---|---|---|
| **Week 1** | Low — "Is this accurate?" | Reps verify every prediction manually. Accuracy dashboard shows initial results. |
| **Week 2-3** | Growing — "It's usually right" | Accuracy data visible to all reps. High-confidence predictions prove reliable. Reps start trusting 🟢 tier. |
| **Month 2** | Moderate — "I rely on it for prep" | Reps use predictions for pre-call research. High-confidence outreach yields higher reply rates. |
| **Month 3+** | High — "I don't start outreach without it" | Tool is embedded in daily workflow. Accuracy is trending up from feedback loop. |

**Key trust design principles:**
1. **Show your work** — every prediction has visible evidence (not a black box)
2. **Never pretend to know** — "Unknown" is always an option (no false certainty)
3. **Make correction easy** — one-click feedback, not a form
4. **Show track record** — accuracy stats visible per competitor, per confidence tier

---

## 5. Measurement

### 5.1 Outcome Metrics (Business Impact)

| Metric | Baseline (Estimated) | Target | How to Measure |
|---|---|---|---|
| **Rep research time per lead** | 15-20 min | < 2 min | Time tracking in CRM |
| **Leads contacted per rep per day** | 15-25 | 50+ | CRM activity tracking |
| **Outreach personalization rate** | ~10% mention competitor | 60%+ mention competitor | Outreach audit / template usage |
| **Reply rate on cold outreach** | 3-5% | 8-12% | Email/sequence analytics |
| **Pipeline conversion rate** | Current baseline | +20% improvement | CRM pipeline tracking |
| **Sales cycle length** | Current baseline | -15% reduction | CRM deal stage timestamps |

### 5.2 AI Quality Metrics (Model Performance)

| Metric | Quality Bar Before Ship | Ongoing Target |
|---|---|---|
| **Precision @ 🟢 (Confirmed)** | ≥ 90% | ≥ 95% |
| **Precision @ 🟡 (Likely)** | ≥ 70% | ≥ 80% |
| **Recall (top 4 competitors)** | ≥ 60% | ≥ 75% |
| **Confident-Wrong rate** | < 3% | < 1% |
| **Coverage (% of leads with some signal)** | ≥ 50% | ≥ 70% |
| **Latency per enrichment** | < 30 seconds | < 10 seconds |
| **Confidence calibration** | Predicted 80% = actual 75-85% | Tighter calibration over time |

### 5.3 Quality Bar Before Shipping

> [!IMPORTANT]
> **Do not ship if:**
> - Confident-Wrong rate (🟢 predictions that are incorrect) exceeds 3%
> - Overall precision across all tiers is below 70%
> - More than 20% of reps report the tool made them "look bad" to a prospect
> - Enrichment latency exceeds 60 seconds (reps won't wait)

### 5.4 How to Monitor in Production

1. **Accuracy dashboard:** Updated weekly from rep feedback (✅/❌ buttons)
2. **Confidence calibration chart:** Predicted confidence vs. actual accuracy
3. **Coverage trends:** % of leads enriched over time, by region and vertical
4. **Adoption metrics:** % of reps using the tool daily, time-in-tool
5. **Downstream impact:** Reply rate and conversion rate for enriched vs. non-enriched leads (A/B)

---

## 6. Users and Jobs To Be Done

### Primary User: SDR/BDR (Outbound)

> **"When I'm preparing outreach for a prospect, I need to know what tools they currently use so I can customize my pitch, prioritize my time on high-fit prospects, and accelerate the path to a conversation."**

### Secondary User: RevOps / Growth Team

> **"When I'm building lead scoring models and territory plans, I need aggregated competitive intelligence to understand market share by region and vertical, so I can allocate reps to the highest-opportunity segments."**

### Tertiary User: Sales Leadership

> **"When I'm evaluating team performance and market strategy, I need to understand our win/loss rates by competitor, so I can invest in the right competitive positioning and enablement."**

---

## 7. Edge Case: Existing HCP Customer Exclusion

The enrichment pipeline has a secondary use case: **detecting businesses already using HCP** and flagging them for exclusion from the prospect pipeline. The booking widget pattern `book.housecallpro.com/book/[ID]` is detectable via the same Tier 0/1 methods. This prevents wasted outreach on existing customers and improves CRM data hygiene.

---

*Document v1.1 — Informed by validated POC data from Phase 0 (6+ real businesses tested across 4 competitors, 4 detection methods validated). Updated after staff PM self-audit to address HCP exclusion gap and Google Places API constraint.*
