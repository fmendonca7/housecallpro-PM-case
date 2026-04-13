# 🏠 Competitor Signal Intelligence — HCP Case Study

## AI-Powered Lead Enrichment for Housecall Pro Sales

An internal tool that detects which competitor FSM (Field Service Management) software a prospective home service business uses, enabling SDRs to personalize outreach and accelerate pipeline conversion.

**Live demo:** [hcp-signal-intel.onrender.com](https://hcp-signal-intel.onrender.com)

---

## Quick Start

```bash
# 1. Install dependencies
pip install fastapi uvicorn httpx openai playwright firecrawl-py
python -m playwright install chromium

# 2. Set up environment
cp .env.example .env
# Fill in your keys (see .env.example for all required vars)

# 3. Run
uvicorn main:app --reload --port 8000

# 4. Open
open http://localhost:8000
```

---

## Architecture

```
┌─── T0: BOOKING URL DETECTION ──────────────────────────────┐
│  1. Google Local tab (tbm=lcl) via Playwright               │
│     → Finds GBP booking button href directly in DOM         │
│     → Cross-references listing website with lead domain     │
│  2. Serper.dev (Google Search API) for indexed booking URLs │
│  Deterministic — URL pattern matching, domain cross-ref     │
└────────────────────────────────────────────────────────────┘
         │ if no T0 signal
         ▼
┌─── T1: WEBSITE FINGERPRINTING ─────────────────────────────┐
│  Playwright (headless Chromium) renders the lead's website  │
│  with full JS execution + network request capture           │
│  → If timeout/403: falls back to Firecrawl                  │
│  Firecrawl: residential proxy rotation bypasses bot blocks  │
│  Deterministic — DOM/network pattern matching               │
└────────────────────────────────────────────────────────────┘
         │ if no T1 signal
         ▼
┌─── T2: WEB MENTIONS (NLP) ─────────────────────────────────┐
│  Serper.dev searches job postings, case studies, reviews    │
│  LLM extracts competitor mentions from search snippets      │
│  Only path to detecting FieldEdge (no web signals)         │
└────────────────────────────────────────────────────────────┘
         │ all signals
         ▼
┌─── AI CLASSIFIER ──────────────────────────────────────────┐
│  GPT-4o-mini synthesizes T0+T1+T2 evidence                 │
│  Applies confidence rules — never invents evidence          │
│  Returns classification + evidence trail + talk track       │
└────────────────────────────────────────────────────────────┘
```

**Detection Coverage:** ServiceTitan, Jobber, Workiz, FieldEdge, HCP, Kickserv, ServiceFusion

---

## Tech Stack

| Layer | Technology | Why |
|---|---|---|
| **Frontend** | HTML + Vanilla JS + CSS | Speed of iteration; no build step |
| **Backend** | Python FastAPI + Uvicorn | Async-native for concurrent scanning |
| **JS Renderer** | Playwright (headless Chromium) | 60-70% of FSM widgets load via JS; static HTTP misses them |
| **Anti-bot bypass** | [Firecrawl API](https://firecrawl.dev) | Residential proxies serve real page content to sites that block datacenter IPs |
| **Search** | Serper.dev | Google Search API for GBP discovery + T2 NLP signal extraction |
| **AI** | OpenAI GPT-4o-mini | Evidence synthesis — classifies signals, never generates them |
| **Storage** | JSON file | Sufficient for MVP; SQLite for V2 |
| **Deploy** | Render (Docker + Chromium) | Full browser support in container |

---

## Key Design Decisions

1. **Website domain as the unique key** — Business names repeat (400+ "John's Plumbing"). Domains don't. Every search and cross-reference is anchored to the lead's domain to prevent cross-business signal contamination.

2. **Google Local tab for booking URLs** — ServiceTitan booking URLs use dynamic session tokens and are never indexed by Google. The only reliable source is the GBP listing itself, which appears in Google's Local tab (`tbm=lcl`) with the booking URL in the `<a href>` — no clicking required.

3. **Reliability > full automation** — Auto-discovery (name-only input) fails ~30% of the time. The system requires the website field (5 seconds from CRM), then automates everything else. One wrong result in a live sales call costs more credibility than ten correct results earn.

4. **LLMs synthesize evidence — they never create it** — After discovering GPT-4o-mini hallucinating booking URLs as "confirmed" signals, the architecture was restructured: deterministic tiers (T0/T1/T2) collect real-world evidence; the LLM's role is classification only.

5. **Firecrawl as the production anti-bot layer** — Playwright running on cloud servers (Render/AWS) gets "soft-blocked" by many sites: HTTP 200 response with stripped page content (12 network requests instead of 100+). Firecrawl's residential proxies receive the full, real page.

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
│   └── poc_signal_validation.md
└── .env.example         # Environment template
```

---

## Process Artifacts (docs/)

| Document | Purpose |
|---|---|
| `01_problem_framing.md` | Pain point, success/failure definitions, trust framework, metrics |
| `02_research_competitive.md` | 5 competitor profiles, signal taxonomy, market gaps |
| `06_prd.md` | User stories, requirements (FR/NFR), scope decisions, V2 roadmap |
| `eval_spec.md` | Success criteria, error taxonomy, 6 live-validated cases |
| `ai_usage_log.md` | Tools used, top prompts, AI failures caught, human decisions |
| `poc_signal_validation.md` | Real-world testing of 6+ businesses across 4 detection methods |

---

## With More Time

- **Salesforce integration** — trigger enrichment automatically on new lead creation; push results directly into lead records. Removes tool-switching entirely.
- **LinkedIn/Indeed integration** — structured job posting data for FieldEdge detection at scale
- **Confidence calibration** — track predicted vs. actual accuracy to tighten scoring over time
- **A/B testing** — measure conversion rate lift for enriched vs. non-enriched outreach

---

*Built as a case study for HCP Senior PM II — Internal Tooling position.*
