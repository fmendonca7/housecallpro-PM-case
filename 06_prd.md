# PRD — Competitor Signal Intelligence (MVP)

## Internal Tooling · Lead Enrichment · HCP Sales & Growth

---

## 1. Overview

**Product:** AI-powered lead enrichment tool that detects which competitor FSM software a prospective home service business uses, so HCP sales reps can personalize outreach, prioritize leads, and accelerate pipeline conversion.

**Target Users:** SDR/BDR team (primary), RevOps (secondary)

**MVP Goal:** Demonstrate the core value loop — input a prospect, get a competitor classification with confidence score and suggested talk track — in a functional, demoable application.

---

## 2. User Stories

### Must Have (MVP)

| # | As a... | I want to... | So that... |
|---|---|---|---|
| US-1 | SDR | input a business name + location and get enrichment results | I can see what software they use before outreach |
| US-2 | SDR | see a confidence score for each prediction | I know how much to trust the result |
| US-3 | SDR | see the evidence behind each prediction | I can verify and feel confident using it |
| US-4 | SDR | see a suggested competitive talk track | I can immediately personalize my outreach |
| US-5 | SDR | mark a prediction as correct or incorrect | the system improves over time |
| US-6 | SDR | quickly scan a list of pre-enriched leads | I can prioritize my outreach queue |

### Should Have (V1.1)

| # | As a... | I want to... | So that... |
|---|---|---|---|
| US-7 | SDR | batch-upload a CSV of prospects and get them all enriched | I don't have to do it one by one |
| US-8 | RevOps | see aggregate competitor market share by region/vertical | I can plan territory strategy |
| US-9 | SDR | filter leads by detected competitor | I can run targeted campaigns against specific competitors |

### Won't Have (MVP) — V2 Roadmap

| # | Feature | Why Not Now |
|---|---|---|
| US-10 | Salesforce integration | Requires CRM API setup and auth — mocked in MVP |
| US-11 | Automated outreach triggering | Reps should control outreach — trust must build first |
| US-12 | Real-time web scraping at scale | Expensive infra — MVP uses pre-enriched data + on-demand single lookups |
| US-13 | Admin config panel | Not needed for demo scope |
| US-14 | Multi-tenant / auth | Single-user MVP |

---

## 3. Functional Requirements

### 3.1 Core Enrichment Pipeline

| ID | Requirement | Priority |
|---|---|---|
| FR-1 | Accept business name + city/state as input | Must |
| FR-2 | Tier 0: Query GBP for booking/appointment links and match against known FSM URL patterns | Must |
| FR-3 | Tier 1: Fetch prospect website HTML and scan for FSM widget fingerprints (scripts, iframes, CSS classes) | Must |
| FR-4 | Tier 2: Extract FSM tool mentions from available text sources (stubbed in MVP — simulated with representative data) | Should |
| FR-5 | Tier 3: LLM inference combining all available signals into a probabilistic competitor classification | Must |
| FR-6 | Return: detected competitor, confidence tier (🟢🟡🔴⚪), evidence list, suggested talk track | Must |
| FR-7 | Handle "no signal" gracefully — return ⚪ Unknown with contextual note | Must |

### 3.2 User Interface

| ID | Requirement | Priority |
|---|---|---|
| FR-8 | **Lead List View:** Table of prospects with competitor badge, confidence indicator, and key signals | Must |
| FR-9 | **Lead Detail View:** Drill-down showing full evidence trail, confidence breakdown, talk track | Must |
| FR-10 | **Enrich Action:** Button to trigger single-lead enrichment with loading state | Must |
| FR-11 | **Feedback Loop:** ✅ Correct / ❌ Wrong buttons on each prediction | Must |
| FR-12 | **Error/empty states:** Clear messaging for no-data, timeout, and API errors | Must |
| FR-13 | **Pre-loaded demo data:** 10-15 leads pre-enriched to showcase the dashboard | Must |

### 3.3 Data & Content

| ID | Requirement | Priority |
|---|---|---|
| FR-14 | Competitive talk tracks for: ServiceTitan, Jobber, Workiz, FieldEdge, No Software, Unknown | Must |
| FR-15 | Known fingerprint patterns for top 4 competitors (URL patterns, HTML signatures) | Must |
| FR-16 | Pre-researched demo dataset of 15-20 real home service businesses with known FSM tools | Must |

---

## 4. Non-Functional Requirements

| ID | Requirement | Target |
|---|---|---|
| NFR-1 | **Latency:** Single enrichment completes in < 30 seconds | Must |
| NFR-2 | **Availability:** Deployed with public URL, accessible by panel during demo | Must |
| NFR-3 | **Modifiability:** Code structured for live iteration during 30-min session | Must |
| NFR-4 | **Readability:** Code and architecture easy to understand by panel reviewers | Should |
| NFR-5 | **Visual quality:** UI looks professional (not a raw Streamlit default) | Should |

---

## 5. Scope Decisions

### IN Scope (MVP)

| Feature | Rationale |
|---|---|
| Single-lead enrichment (on-demand) | Core demo flow — shows the pipeline working end-to-end |
| Pre-enriched lead dashboard | Shows the output format and rep workflow at a glance |
| Confidence tiers with visual badges | Demonstrates trust design — critical to the case evaluation |
| Evidence trail per prediction | Shows transparency / "show your work" design |
| Feedback buttons (correct/incorrect) | Shows the learning loop — how trust builds over time |
| Competitive talk tracks | Shows actionable output — enrichment alone isn't enough |
| Error/loading/empty states | Required by case: "failure handling must be demoable" |

### OUT of Scope (MVP)

| Feature | Rationale |
|---|---|
| Batch enrichment (CSV upload) | Adds complexity without demonstrating new value — show the pipeline, not the plumbing |
| Real-time web scraping of arbitrary sites | Unreliable during live demo (timeouts, bot blocking). Use pre-fetched data + mock for stability |
| Salesforce integration | Mocked — show wire diagram, not live CRM sync |
| User auth / multi-tenant | Single-user demo — no auth needed |
| Admin configuration | Out of scope for MVP — hard-coded configs are fine |
| Mobile responsive | Desktop-first demo for panel review session |

---

## 6. Data Flow Architecture

```
┌──────────────┐     ┌───────────────────┐     ┌─────────────────┐
│  UI (Frontend)│────▶│  API (Backend)     │────▶│  AI Pipeline     │
│               │◀────│                   │◀────│                 │
│  - Lead List  │     │  POST /enrich     │     │  Tier 0: GBP    │
│  - Detail View│     │  GET /leads       │     │  Tier 1: HTML   │
│  - Feedback   │     │  POST /feedback   │     │  Tier 2: NLP    │
│               │     │                   │     │  Tier 3: LLM    │
└──────────────┘     └───────────────────┘     └─────────────────┘
                                                       │
                                               ┌───────▼──────┐
                                               │  Data Store   │
                                               │  (JSON/SQLite)│
                                               │  - Leads      │
                                               │  - Results    │
                                               │  - Feedback   │
                                               └──────────────┘
```

---


---

## 7. Trust Design

> *The core challenge: this system makes claims that SDRs will act on in live sales calls. One confident wrong answer is more damaging than fifty correct ones are valuable. Trust must be earned incrementally, not assumed.*

### When the system is uncertain

Uncertainty is a first-class output, not a failure state. The system has four confidence tiers communicated distinctly in the UI:

| Tier | Badge | SDR Behavior Implied |
|---|---|---|
| 🟢 Confirmed (85-100%) | Green — hard evidence | Use the competitive angle. Booking URL or widget was directly observed. |
| 🟡 Likely (50-84%) | Yellow — strong signals | Mention it, but probe first: *"Are you currently using any software to manage your jobs?"* |
| 🔴 Possible (25-49%) | Red — weak signals | Don't lead with it. Use as background context only. |
| ⚪ Unknown | Grey | Proceed with standard outreach. Evidence trail shows why no signal was found. |

**Unknown is honest.** A business with no detectable signals gets ⚪, never a low-confidence guess. "I don't know" is a trustworthy answer. "Possibly ServiceTitan 20%" is noise that erodes credibility.

### Scoring Formula (Detection Layer → Confidence Score)

The score within each tier is determined by the **combination of detection layers** and the **quantity of independent signals** found. More independent sources = lower false-positive risk = higher score.

| Detection | Evidence | Score | Why |
|---|---|---|---|
| **T0 + T1** | GBP booking URL **and** HTML fingerprints, same competitor | **95–100%** | Dual-source confirmation: two completely independent methods agree. Score increases +0.5% per additional T1 fingerprint (cap 100%). |
| **T0 alone** | Booking URL observed on GBP, site blocked/inaccessible | **92%** | Single source of truth. GBP may have a lag — without T1 corroboration there's a small switch risk. |
| **T0 + T2 case study** | Booking URL + vendor published the business as a customer | **94%** | Two independent sources; T2 case study corroborates but doesn't add as much as a second deterministic scan. |
| **T1 alone — 20+ fingerprints** | Deep widget embed: scripts, CSS classes, API calls | **97%** | Volume of independent in-page references makes false positive near-impossible. |
| **T1 alone — 10+ fingerprints** | Multiple widget references in HTML | **95%** | |
| **T1 alone — 5+ fingerprints** | Several fingerprints across page | **93%** | |
| **T1 alone — 3 fingerprints** | Script + class + reference | **90%** | |
| **T1 alone — 2 fingerprints** | Two independent HTML signals | **87%** | |
| **T1 alone — 1 fingerprint** | Single widget signal (e.g., browser modal) | **83%** | Single observed T1 signal; enough for 🟢 but lowest confidence within the tier. |
| **T2 — vendor case study** | Business explicitly named in vendor's published success stories | **82%** | Strong T2: the business consented to be named. But it's external and may be outdated. Still 🟡 Likely. |
| **T2 — job postings / mentions** | Tool mentioned in hiring posts, reviews, or web text | **35–55%** | Indirect signals; weaker because the business didn't explicitly confirm the tool. |
| **T3 — AI inference only** | No T0/T1/T2; LLM reasons from vertical, size, region | **30–45%** | 🔴 Possible only. Profile matching without observed signal. Never used to drive outreach. |

> **Note on AI role:** The LLM classifier synthesizes all available evidence and is calibrated to follow this same rubric. It may only use `confirmed` if it was explicitly given a T0 or T1 observation. For T3 inference (no deterministic signals), it outputs `possible` (25–49%). The deterministic scoring block always overrides AI scores when T0/T1/T2 signals exist.


### When the system is wrong

1. **Every prediction shows its evidence** — the SDR sees exactly why the system made its call before acting on it
2. **Feedback buttons on every result** — one click to mark ✅ Correct or ❌ Wrong. No form, no friction.
3. **Wrong predictions don't cascade** — a single correction flags that lead for review. It doesn't affect other leads.
4. **SDR stays in control** — outreach is never automated from a prediction. The human makes the call.

### How trust builds over time

| Stage | Trust Signal |
|---|---|
| **Day 1** | System gets Plumbline Services right. Rep sees the evidence trail. First impression: "it shows its work." |
| **Week 1** | Rep uses it on 10 leads. 7 useful, 2 marked wrong, 1 unknown. Pattern: T0 results are reliable; 🔴 Possible can be skipped. |
| **Month 1** | Feedback corrections visible in aggregate. RevOps sees correction rate dropping. Confidence tiers are calibrated to rep experience. |
| **Month 3** | SDR corrections feed retraining. Precision improves. Reps start leading with competitive angles more aggressively because the system has earned it. |

---

## 8. Measurement & Success

### Quality bar before shipping

The product should not go to reps if any of the following are true:
- E1 (Confident-Wrong) rate > 3% on the eval test set
- Coverage < 55% of leads return any signal
- Any evidence in a 🟢 result was generated by the LLM rather than observed

### Business outcome metrics

| Metric | Goal | How to Measure |
|---|---|---|
| **Conversion rate lift** | +15% for enriched vs. non-enriched leads | A/B test: enriched cohort vs. control over 60 days |
| **Time per lead qualification** | From ~15 min → < 3 min | SDR self-report + CRM timestamp delta |
| **SDR adoption rate** | ≥ 70% of outbound leads enriched within 30 days | % of leads with enrichment record in system |
| **Feedback correction rate** | < 10% of 🟢 results marked wrong | Aggregate from feedback log |

### Leading indicators (Week 1–4)

Before conversion data accumulates:
- SDRs open the evidence trail, not just read the badge — shows engagement with reasoning, not blind trust
- Feedback is being submitted — shows reps feel ownership, not passive distrust
- ⚪ Unknown results don't trigger abandonment — shows reps understand the system's limits

### If adoption stalls

If SDR adoption is < 40% at 30 days, the problem isn't accuracy — it's workflow friction. The fix is CRM integration (enrichment auto-runs on new leads), not model improvement.

---

## 9. V2 Roadmap (3-Month Vision)

| Month | Focus | Key Features |
|---|---|---|
| **M1** | Foundation | MVP → Production: Salesforce integration, batch enrichment, team access |
| **M2** | Scale | Automated nightly pipeline for all CRM leads. Accuracy monitoring dashboard. Feedback loop → model retraining |
| **M3** | Intelligence | Aggregate competitive market share by region/vertical. Competitive trend alerts (prospect changed tools). |

---

*Document v2.0 — Added Trust Design (§7) and Measurement (§8). Scope intentionally tight for MVP — every OUT decision has a "why."*
