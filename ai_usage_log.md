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

### 2. "Getting it wrong is worse than saying 'I don't know.' Design for the asymmetry."

**Context:** During problem framing, before writing a single line of code, I was mapping the SDR workflow and asking a hard question: *when does this product actually backfire?*

**What I asked:** *"If the system says 98% confident ServiceTitan and it's wrong — what happens in the sales call? And if the system says Unknown — what happens?"*

**What I realized:** These two failure modes have radically different costs. A false negative (Unknown) wastes maybe 10 minutes — the SDR checks manually. A false positive (wrong competitor at high confidence) plays out on a live call. The SDR opens with a competitive angle. The prospect says "we don't use ServiceTitan." The SDR looks uninformed before the conversation has started. That's not a 10-minute cost — that's a poisoned relationship, and word travels fast on a small sales floor.

**The asymmetry:** One confident wrong answer costs more than fifty correct ones earn.

**What this changed about the entire design — before any code was written:**
- "Unknown" became a first-class output, not a failure state — always preferred over a plausible guess
- Confidence tiers were calibrated conservatively: T0 confirmed only on directly observed signals, never inferred
- The LLM was architecturally prohibited from generating evidence — it can only classify what was actually found
- The evidence trail UI was made default-visible — so SDRs can sanity-check the reasoning before acting, not just trust the badge

**Why this matters as a PM call:** This was a product strategy decision disguised as a UX decision. Most AI product teams optimize for the "it worked!" case. I designed from the failure case first — because in a trust-dependent product sold to a skeptical sales org, the failure case sets the ceiling on how far the tool can scale.

---

### 3. "The system is using the wrong identifier. Business names repeat — domains don't."

**Context:** The system returned "🟢 HCP — 98% confirmed" for Plumber in DC. I manually checked their Google Business Profile: no "Book" button. Only "Site" and "Directions." The booking URL that triggered the detection said `/book/Northern-Climate/` — a completely different business in the same city.

**What I said:** *"T0 needs to cross-reference against the lead's website — to verify it's actually looking at the right business. It shouldn't search by name. It should search by website, since that's the unique piece of information for each business. Names repeat."*

**The root cause I identified:** The search query was `"Plumber in DC" + book online + site:book.housecallpro.com`. Google returned results for *any* HCP customer in the DC area — because the search anchor was the business *name*, which isn't a unique identifier. "Plumber in DC" could match dozens of businesses in Washington. The system was finding the right *type* of result, but attributing it to the wrong *entity* — a cross-business contamination bug wearing a 98% confidence badge.

**The reframe:** Business names repeat. Domains don't. Every business has exactly one website, which is a globally unique, externally verifiable identifier — and one that CRM data already captures. The search anchor should be `"plumberindc.com"`, not `"Plumber in DC"`.

**What this changed:**
1. Primary search became domain-anchored: `"plumberindc.com" book.servicetitan.com OR housecallpro.com...` — any result is automatically verified because domains are unique
2. Name-based fallback added mandatory cross-reference: result must contain the lead's domain before acceptance
3. Governing principle established: **website domain is the primary key for lead identity** throughout the entire pipeline

**Why this matters as a PM call:** This is the type of silent failure that destroys trust at exactly the wrong moment — in a live demo or a real sales call. The fix wasn't more data or a better model. It was recognizing that the system was asking the right question with the wrong key. Changing the identifier changed the answer.

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

**The insight:** I noticed that hovering over the GBP "Book" button shows the booking URL in the browser's status bar. That URL is sitting in the actual HTML of the Google Local results tab (`tbm=lcl`) — not dynamically loaded via JavaScript. It's been there the whole time.

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
4. **Confidence calibration** — track predicted confidence vs. actual accuracy to tighten scoring over time
5. **Indeed/LinkedIn integration** for Tier 2 NLP — the only viable path to detecting FieldEdge at scale
6. **A/B testing** — measure conversion rate lift for enriched vs. non-enriched outreach
7. **Feedback-driven recalibration** — use SDR corrections to improve the classifier, not just store them
