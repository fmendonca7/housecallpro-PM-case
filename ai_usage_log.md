# 🤖 AI Usage Disclosure

> *This log documents how AI tools were used throughout this case study — not as a tool list, but as a record of judgment calls. Where AI accelerated the work, where it got it wrong, and the three moments where PM instinct overrode AI output to protect product integrity.*

---

## Tools Used

| Tool | What I Used It For |
|---|---|
| **Claude (Anthropic)** via Gemini Code Assist | End-to-end thinking partner: competitive research, architecture design, full-stack coding, documentation. Chosen for long-context memory — it held the full project state across research → design → build → test. |
| **GPT-4o-mini (OpenAI API)** | Runtime AI engine *inside* the product — classifies evidence and generates talk tracks at ~$0.001/request. Deliberately chose a small, fast model to keep enrichment under 30 seconds. |
| **Playwright (headless Chromium)** | Renders real websites with full JavaScript execution. This was a mid-project pivot — I switched from `httpx` (static HTTP) after discovering that 60-70% of FSM widgets load via JS. |
| **Manual browser testing** | I manually tested 6+ real business websites, clicking booking buttons and inspecting what loads. This created the ground truth dataset that I later used to catch AI hallucinations. |

---

## Top 3 Prompts That Shaped the Outcome

### 1. "Don't theorize — go find real businesses and test detection on them."

**Context:** The AI had built an elegant signal taxonomy with HTML scraping as the primary detection method. It looked great on paper. But I've learned that the gap between "this should work" and "this actually works" is where most AI products fail.

**What I asked:** *"Test these methods on real businesses right now — Plumbline Services, Blue Sky Plumbing, Applewood, Atzinger Gardens, Green Can Cleaner. Show me exactly what each method finds."*

**What the data revealed:**
- Static HTML scraping only worked 2 out of 5 times (40%). Cloudflare blocked us, JavaScript loaded widgets dynamically, geo-restrictions kicked in.
- Google Business Profile booking links worked on every single business we tested — even the ones with 403-protected websites.

**The decision I made:** Completely restructured the pipeline priority. GBP booking URL detection became **Tier 0** (primary), website scanning dropped to **Tier 1** (fallback). This was the opposite of what the AI recommended — but the data was clear.

**Why this matters for the product:** If I had shipped the AI's original architecture, 60% of enrichment attempts would have failed silently. By validating before building, I saved weeks of future debugging and prevented the product from launching with a fundamental detection gap.

---

### 2. "It doesn't make sense for the SDR to paste the link manually. The system should find it."

**Context:** The first prototype asked SDRs for the business name, location, *website URL*, and *booking URL*. That's essentially asking the user to do the research the tool is supposed to automate.

**What I said:** *"Não faz sentido o SDR colocar o link manual. O sistema de LEAD ao receber o Lead deve rodar o próprio sistema com AI para verificar Tier 0, Tier 1, Tier 2... onde não necessita nenhum input do SDR."*

**The chain reaction this caused:**
1. Built auto-discovery layer (search engines + LLM fallback) to find websites automatically
2. Auto-discovery found wrong websites 30% of the time → I course-corrected again
3. Final decision: website is a *required* field, but one the SDR copies from the CRM (5 seconds), not from manual research (15 minutes)

**The tradeoff I made:** Full automation sounds better in a pitch. But I chose **reliability over automation** — because this is a trust-dependent product. One wrong result ("I see you're on ServiceTitan!" to a business that isn't) costs more than 100 correct ones earn.

---

### 3. "How is the system actually getting this data? Show me under the hood."

**Context:** The system returned "🟢 ServiceTitan — 98% confirmed" for Rite Plumbing NYC. I had manually visited their website the day before. They have no FSM software. No scheduling widget. Nothing.

**What I asked:** *"Quais sistemas e AI está utilizando para verificar o link do Google My Business? Google Places API? Firecrawl? Show me the actual code path."*

**What I discovered:** The LLM was hallucinating. When DuckDuckGo was rate-limited, the system fell back to GPT-4o-mini, which invented a plausible booking URL (`book.servicetitan.com/...`) from its training data. The pipeline treated this hallucinated URL as "Tier 0 evidence" — the highest confidence tier.

**The system was presenting LLM guesses as observed facts.** This is the single worst failure mode for a trust-dependent product.

**What I did:**
1. Architecturally separated evidence *collection* (must be deterministic — real HTML, real URLs, real network requests) from evidence *synthesis* (where LLM adds value — classifying, scoring, reasoning)
2. Replaced static HTML scanning with Playwright (headless browser → full JS rendering → real widget detection)
3. The LLM can now *classify* evidence but can never *generate* it

**Why this matters:** I didn't just fix a bug. I established an architectural constraint: **LLMs synthesize evidence, they don't create it.** This principle now governs every tier of the pipeline and prevents the entire category of "confident hallucination" failures.

---

## Where AI Got It Wrong

### 🔴 The hallucination that almost shipped as a feature

**The failure:** GPT-4o-mini returned `{"booking_url": "book.servicetitan.com", "confidence": "high"}` for a business that has never used ServiceTitan. The pipeline displayed this as "🟢 Confirmed — 98%."

**Why I caught it:** Because in Phase 0, I had manually researched Rite Plumbing NYC. I knew they didn't use FSM software. Without that ground truth, I would have trusted the green badge — just like an SDR would.

**The fix wasn't technical — it was architectural.** I redrew the boundary between what the AI is allowed to do (classify observed signals) and what it must never do (generate signals). This is the difference between a demo that works and a product that earns trust.

**Production principle:** In any system where AI outputs inform human decisions, the evidence backing a confidence score must be independently verifiable. "The AI said so" is never sufficient evidence for a Confirmed tier.

### ⚠️ Auto-discovery found the wrong website

The LLM inferred `riteplumbing.com` (Orland Park, IL) when the user had already provided `riteplumbingnyc.com` (New York). The AI contradicted user-provided data with its own inference.

**Design rule established:** User-provided data always overrides AI-inferred data. The system augments human input — it never contradicts it.

---

## What I Deliberately Did Without AI

### 1. Creating ground truth through manual research

Before building anything, I spent 45 minutes manually researching real businesses — clicking booking buttons, inspecting HTML source code, searching Google Business Profiles. This manual work created the dataset I later used to catch the hallucination.

**Why not AI:** You can't QA an AI system by asking the AI if it's correct. Ground truth requires independent verification from a source the model doesn't control.

### 2. Reframing FieldEdge's invisibility as the core argument for AI

FieldEdge has no public widget, no booking URL, no detectable signal. The AI treated this as a limitation to acknowledge. I reframed it as the strategic centerpiece: *"Without AI inference, FieldEdge users are completely invisible to rules-based detection — proving that a hybrid approach isn't optional, it's essential."*

**Why not AI:** The AI optimizes for accuracy. The PM optimizes for narrative. Turning a weakness into the strongest argument for the solution requires strategic judgment that AI assistants don't have.

### 3. The scope tradeoff: reliability > automation

The hardest PM decision in this project was making the website field required. "Just name + location" is a cleaner pitch. But auto-discovery fails 30% of the time on free API tiers, and every wrong website produces a wrong result. For a product where trust is the core value proposition, I chose the version that works reliably over the version that demos better.

**Why not AI:** This is a judgment call about user psychology and product positioning. An AI assistant will optimize for the technically elegant solution. A PM must optimize for the solution users will actually trust.

---

## Technical Stack

```
Frontend:  HTML + Vanilla JS + CSS (no framework — chosen for live iteration speed)
Backend:   Python FastAPI + Uvicorn  
Scanner:   Playwright headless Chromium (JS rendering + network capture)
AI:        OpenAI GPT-4o-mini (evidence synthesis + classification)
Storage:   JSON file (sufficient for MVP, SQLite for V2)
Deploy:    Render (Docker container with Chromium)
Patterns:  Regex-based FSM fingerprinting (7 competitors)
```

---

## With More Time

1. **Search API integration** (Serper.dev) for reliable GBP booking URL extraction — removes browser-based search dependency
2. **Confidence calibration** — track predicted confidence vs. actual accuracy to tighten scoring over time
3. **Indeed/LinkedIn integration** for Tier 2 NLP — the only viable path to detecting FieldEdge at scale
4. **A/B testing** — measure conversion rate lift for enriched vs. non-enriched outreach
5. **Feedback-driven recalibration** — use SDR corrections to improve the classifier, not just store them
