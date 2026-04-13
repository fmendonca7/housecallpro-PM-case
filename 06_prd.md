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

## 7. V2 Roadmap (3-Month Vision)

| Month | Focus | Key Features |
|---|---|---|
| **M1** | Foundation | MVP → Production: Salesforce integration, batch enrichment, team access |
| **M2** | Scale | Automated nightly pipeline for all leads in CRM. Accuracy monitoring dashboard. Feedback loop → model retraining |
| **M3** | Intelligence | Aggregate competitive market share analytics. Territory-level insights for RevOps. Competitive trend alerts (prospect changed tools) |

---

*Document v1.0 — Scope intentionally tight for 3-5 hour build. Every OUT decision has a "why" — we're demonstrating judgment, not feature volume.*
