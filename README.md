# housecallpro-PM-case
# 🏠 Competitor Signal Intelligence — HCP Case Study

## AI-Powered Lead Enrichment for Housecall Pro Sales

An internal tool that detects which competitor FSM (Field Service Management) software a prospective home service business uses, enabling SDRs to personalize outreach and accelerate pipeline conversion.

---

## Quick Start

```bash
# 1. Install dependencies
pip install fastapi uvicorn httpx openai playwright
python -m playwright install chromium

# 2. Set up environment
cp .env.example .env
# Add your OPENAI_API_KEY to .env

# 3. Run
uvicorn main:app --reload --port 8000

# 4. Open
open http://localhost:8000
```

---

## Architecture

```
┌─── SIGNAL COLLECTION ──────────────────────┐
│  T0: Booking URL Detection (GBP + Website) │  Deterministic: URL pattern matching
│  T1: Website Fingerprinting (Playwright)   │  Deterministic: DOM/network patterns
│  T2: Web Mentions Search (NLP)             │  AI-powered: search + extract
└────────────────────────────────────────────┘
        │ all signals
        ▼
┌─── AI CLASSIFIER ──────────────────────────┐
│  Synthesizes all evidence from T0+T1+T2    │
│  Applies confidence rules from trust spec  │
│  Generates classification + talk track     │
└────────────────────────────────────────────┘
```

**Detection Coverage:** ServiceTitan, Jobber, Workiz, FieldEdge, HCP, Kickserv, ServiceFusion

---

## Tech Stack

| Layer | Technology | Purpose |
|---|---|---|
| **Frontend** | HTML + Vanilla JS + CSS | Dashboard, detail panel, enrichment modal |
| **Backend** | Python FastAPI + Uvicorn | API endpoints, pipeline orchestration |
| **Scanner** | Playwright (headless Chromium) | JS rendering, network capture, DOM analysis |
| **AI** | OpenAI GPT-4o-mini | Evidence synthesis, classification, talk tracks |
| **Storage** | JSON file | Lead persistence, feedback storage |

---

## Project Structure

```
├── main.py              # FastAPI server + API endpoints
├── enrichment.py        # Detection pipeline (T0/T1/T2 + AI Classifier)
├── data.py              # Demo leads, FSM patterns, talk tracks
├── store.py             # JSON persistence layer
├── static/
│   ├── index.html       # Dashboard UI
│   ├── styles.css       # Design system
│   └── app.js           # Frontend logic
├── docs/                # Process artifacts
│   ├── 01_problem_framing.md
│   ├── 02_research_competitive.md
│   ├── 06_prd.md
│   ├── eval_spec.md
│   ├── ai_usage_log.md
│   ├── poc_signal_validation.md
│   ├── workflow_log_and_poc.md
│   └── ...
└── .env.example         # Environment template
```

---

## Process Artifacts (docs/)

| Document | Purpose |
|---|---|
| `01_problem_framing.md` | Pain point, success/failure definitions, trust framework, metrics |
| `02_research_competitive.md` | 5 competitor profiles, signal taxonomy, market gaps |
| `06_prd.md` | User stories, requirements (FR/NFR), scope decisions, V2 roadmap |
| `eval_spec.md` | Success criteria, error taxonomy, 20-case test set with 6 validated |
| `ai_usage_log.md` | Tools used, top prompts, AI failures caught, human decisions |
| `poc_signal_validation.md` | Real-world testing of 6+ businesses across 4 detection methods |
| `workflow_log_and_poc.md` | Chronological research decisions, prompt evolution |

---

## Key Design Decisions

1. **Reliability > Automation** — Website field is required (not auto-discovered) because auto-discovery found wrong websites 30%+ of the time
2. **Honesty > Confidence** — System returns "Unknown" rather than guessing. Zero confident-wrong results.
3. **Rules + AI Hybrid** — Deterministic URL/HTML matching for T0/T1, AI only for synthesis and edge cases
4. **Evidence Transparency** — Every prediction shows exactly what was found and where

---

*Built as a case study for HCP Senior PM II — Internal Tooling position.*
