# 🤖 AI Usage Disclosure

> *This log documents how AI tools were used throughout this case study — not as a tool list, but as a record of judgment calls. Where AI accelerated the work, where it got it wrong, and the three moments where PM instinct overrode AI output to protect product integrity.*

---

## Tools Used

| Tool | What I Used It For |
|---|---|
| **Claude (Anthropic)** via Gemini Code Assist | End-to-end thinking partner: competitive research, architecture design, full-stack coding, documentation. Chosen for long-context memory — it held the full project state across research → design → build → test. |
| **GPT-4o-mini (OpenAI API)** | Runtime AI engine *inside* the product — classifies evidence and generates talk tracks at ~$0.001/request. Deliberately chose a small, fast model to keep enrichment under 30 seconds. |
| **Playwright (headless Chromium)** | Renders real websites with full JavaScript execution. This was a mid-project pivot — I switched from `httpx` (static HTTP) after discovering that 60-70% of FSM widgets load via JS. |
| **Firecrawl API** | Anti-bot bypass layer with residential proxy rotation. Added after discovering that Playwright on cloud servers (Render/AWS) gets "soft-blocked" — sites serve stripped pages to datacenter IPs. Firecrawl uses residential proxies to get the real page content. Also provides `/v1/map` for sitemap-based subpage discovery. |
| **Serper.dev** | Google Search API for T0 booking URL discovery (GBP links) and T2 NLP signal extraction (job postings, vendor case studies). Replaced DuckDuckGo which was rate-limited. |
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

**Context:** The first prototype had four input fields: business name, location, website, and booking URL. An SDR using this tool would have to manually research and paste a URL that the system is supposed to discover automatically. I caught this before it was shown to anyone.

**The PM question I asked first:** *Who currently owns this friction, and what does it cost?*

Before touching the code, I mapped the problem:
- **Current state:** SDR identifies a prospect → researches their website → searches GBP → finds booking URL → pastes into tool → tool confirms what they already found. The tool was automating the *last 5%* and leaving the hardest 95% to the human.
- **Who is harmed:** High-velocity SDRs who work 100+ leads/week. Manual URL lookup adds ~8–12 minutes per lead. At 100 leads/week, that's 20 hours of non-selling time per rep per month.
- **The activation problem:** A tool SDRs don't trust will not be used. If it requires effort to set up, adoption collapses — regardless of how good the output is. The input friction *is* the product risk.
- **The blast radius:** If auto-discovery returns the wrong website (which it does ~30% of the time on free tiers), every downstream result is compromised. A wrong website produces a wrong competitor, a wrong talk track, and a lost deal. The failure is invisible until the rep is already on the call.

**What I said:** *"Não faz sentido o SDR colocar o link manual. O sistema de LEAD ao receber o Lead deve rodar o próprio sistema com AI para verificar Tier 0, Tier 1, Tier 2... onde não necessita nenhum input do SDR."*

**The attack plan before writing a single line of code:**
1. **Map what's actually unique per business:** Name repeats (there are 400+ "John's Plumbing" in the US). Location approximates. The *website domain* is the only globally unique identifier — it's what distinguishes the right "Len The Plumber" from the other three.
2. **Use the domain as the anchor, not the name:** Every search, every cross-reference, every validation should start from the website. If the website is wrong, everything downstream is garbage.
3. **Define the automation boundary honestly:** Full automation (zero SDR input) fails 30% of the time. Required website input fails 0% of the time. The right boundary is: SDR provides the domain (5 seconds from CRM), system finds everything else.
4. **Design for trust, not for demos:** "Just name + location" demos better. But I chose the version that works reliably — because in a sales context, one wrong result costs more credibility than ten correct results earn.

**The chain reaction this caused:**
1. Built auto-discovery layer (Google Local tab, Serper, Firecrawl) to find booking URLs from the domain — no SDR input needed for that
2. Auto-discovery found wrong websites when given only names → confirmed the domain-anchoring decision
3. Final architecture: website field required (from CRM in 5 seconds), booking URL fully automated via Google Local tab cross-referenced by domain

**The tradeoff I made explicit:** Full automation is a better pitch. Reliable automation is a better product. For a trust-dependent tool used in live sales calls, I chose the version that doesn't embarrass the rep.

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

### ⚠️ Cloud IP cloaking broke production detection

**The failure:** Detection worked perfectly on localhost (Workiz detected at 98% via Firecrawl subpage scan) but failed on Render production (only 35% via T2 NLP). Same code, same site, dramatically different results.

**Root cause:** `bestproservicesinc.com` uses **cloud IP cloaking** — a bot protection technique where the site serves HTTP 200 to datacenter IPs (like Render's AWS servers) but with a **stripped/minimal HTML page** (only 12 network requests vs 100+ normally). On localhost, Playwright got HTTP 403 (explicit block) which triggered the Firecrawl fallback. On Render, Playwright "succeeded" with a technically valid but empty page — so Firecrawl was never invoked.

**The fix:** If Playwright loads a page successfully but finds zero FSM signals and zero booking URLs, automatically try Firecrawl as a "second opinion." Firecrawl uses residential proxies that receive the real, complete page content.

**Production principle:** Never trust a "successful" response as proof the page was fully rendered. A 200 status with minimal content is often more deceptive than a clean 403 block. Always verify that the scan actually *found* something before treating it as definitive.

### 🔴 T0 search returned a booking URL from the WRONG business

**The failure:** Searching for "Plumber in DC" booking URLs returned `book.housecallpro.com/book/Northern-Climate/...` — a booking page for **Northern Climate**, a completely different business. The pipeline accepted it as "🟢 HCP — 98% confirmed" for Plumber in DC.

**Why I caught it:** I manually checked the Google Business Profile for "Plumber in DC" and there was no "Book" button — only "Site" and "Directions." The booking URL's slug said "Northern-Climate," which clearly isn't "Plumber in DC."

**Root cause:** The Serper search `"Plumber in DC" + book online + site:book.housecallpro.com` returns results for **any** HCP customer nearby — not just our target lead. The pipeline blindly accepted the first booking URL without verifying it belonged to the same business.

**The fix:** Website-anchored T0 search. The lead's website domain is the unique identifier — business names repeat, domains never do.

1. **Search 1 (primary):** `"plumberindc.com" site:book.housecallpro.com OR site:...` — any result is automatically verified because the domain is unique
2. **Search 2 (fallback):** Name + location, but requires cross-reference — the result must contain the lead's website domain, or the booking URL slug must match the business name

**Design principle:** In search-based discovery, **the website domain is the unique key**, not the business name. Any signal attributed to a lead must be cross-referenced against their domain to prevent cross-business contamination.

### 🟢 Google Local tab as a booking URL source (breakthrough)

**The problem:** ServiceTitan booking URLs use dynamic session tokens (`book.servicetitan.com/u21l229r?rwg_token=...`). These are never indexed by Google — they don't exist as static pages. Serper searches return zero results. And when the business site has bot protection (like Nitro CDN on Len The Plumber), Playwright can't scan the site either. Detection dropped from 98% to 30%.

**The insight:** The user noticed that hovering over the GBP "Book" button shows the booking URL in the browser's status bar. This URL is in the actual HTML of the Google Local results tab (`tbm=lcl`), not dynamically loaded via JavaScript.

**The implementation:** Navigate Playwright to `google.com/search?q=BUSINESS+LOCATION&tbm=lcl&hl=en`. Each GBP listing is inside a `<DIV class="VkpGBb">` container that contains both the business website link and the booking button `<A>` with the FSM booking URL as `href`.

**Cross-reference protocol:** For each booking `<A>`, walk up the DOM to the `VkpGBb` parent container, extract the sibling website link, and compare its domain with the lead's website. Only accept if domains match exactly.

**Result:** Len The Plumber went from 30% (timeout) back to 98% confirmed — without needing to load the business's actual website at all. The Google Local tab is a reliable, bot-protection-immune source of booking URLs.

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

1. ~~**Search API integration** (Serper.dev) for reliable GBP booking URL extraction~~ ✅ **Done**
2. ~~**Anti-bot resilience** for production environments~~ ✅ **Done** — Firecrawl API with residential proxies
3. ~~**Subpage navigation** for booking widgets not on the homepage~~ ✅ **Done** — intelligent menu link scoring
4. ~~**Google Local tab (GBP) booking URL extraction**~~ ✅ **Done** — domain-anchored cross-reference via Playwright
5. **Confidence calibration** — track predicted confidence vs. actual accuracy to tighten scoring over time
6. **Indeed/LinkedIn integration** for Tier 2 NLP — the only viable path to detecting FieldEdge at scale
7. **A/B testing** — measure conversion rate lift for enriched vs. non-enriched outreach
8. **Feedback-driven recalibration** — use SDR corrections to improve the classifier, not just store them
9. **Salesforce integration** — push enrichment results directly into lead records on save; trigger enrichment automatically when a new lead is created. Removes the tool-switching step entirely and makes the signal part of the rep's existing workflow.
