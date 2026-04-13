# 🔍 Phase 0 — Competitive Intelligence & Market Research

## Deliverable: `02_research_competitive.md`

---

## 1. The Market Landscape: Home Service FSM Software

### 1.1 Market Overview

The Field Service Management (FSM) software market for home services is **large, fragmented, and fiercely competitive**. Key dynamics:

- **50,000+ businesses** on HCP alone; ServiceTitan serves 11,800+ trade customers
- Market segments are clearly tiered by **company size** (solo → SMB → mid-market → enterprise)
- **Switching costs are moderate** — businesses migrate when they outgrow or get frustrated
- The competitive landscape creates **a strong sales intelligence opportunity**: knowing which tool a prospect uses reveals their maturity, budget, pain points, and what arguments will resonate

### 1.2 Competitive Profiles — The Big 5

---

#### 🟠 ServiceTitan — The Enterprise Gorilla

| Attribute | Detail |
|---|---|
| **Segment** | Mid-market to Enterprise (10-200+ techs) |
| **Verticals** | HVAC, Plumbing, Electrical, Roofing |
| **Pricing** | $245-$500+/tech/month + $5K-$50K implementation fee |
| **Revenue** | $961M FY2026 (24% YoY growth) |
| **Customers** | 11,800+ trade businesses |
| **Key Differentiators** | Enterprise-grade reporting, AI features (Atlas), fintech/payments, massive module ecosystem ("Pro" add-ons), publicly traded (TTAN) |
| **Contract** | 12+ month minimum, often multi-year |
| **Deal Signal** | Prospect using ST = high revenue, complex operation, high switching cost. **HCP pitch: simplicity, lower cost, faster onboarding** |

**Detectable Online Signals:**
| Signal | Detection Method | Confidence |
|---|---|---|
| Scheduling widget on website | Script: `embed.scheduler.servicetitan.com` or `embed.scheduleengine.net` | 🟢 Very High |
| CSS class on booking button | Class: `se-booking-show` or `st-booking-show` | 🟢 Very High |
| JS function calls | `_scheduler.show()` or `ScheduleEngine.show()` | 🟢 Very High |
| Google Business Profile "Book Online" | GBP API integration via "Reserve with Google" | 🟢 High |
| Job postings mentioning ServiceTitan | Job boards: "Experience with ServiceTitan required" | 🟡 Medium |
| Review mentions | Google/Yelp reviews: "booked through their app", "portal" | 🟡 Medium |

---

#### 🟢 Jobber — The SMB Favorite

| Attribute | Detail |
|---|---|
| **Segment** | Solo to SMB (1-30 techs) |
| **Verticals** | Landscaping, Cleaning, HVAC, Plumbing, Electrical, Carpentry, Locksmith |
| **Pricing** | Tiered: Core → Connect → Grow (approx $49-$299/mo) |
| **Key Differentiators** | User-friendly, transparent pricing, strong mobile app, "Jobber Copilot" AI, 14-day free trial |
| **Deal Signal** | Prospect using Jobber = SMB, digital-savvy, may be outgrowing. **HCP pitch: more features, better growth tools, competitive pricing** |

**Detectable Online Signals:**
| Signal | Detection Method | Confidence |
|---|---|---|
| Client Hub booking page | URL: `clienthub.getjobber.com/booking/[AccountID]` | 🟢 Very High |
| Work request form | URL: `clienthub.getjobber.com/client_hubs/[ID]/public/work_request/new` | 🟢 Very High |
| Wix Jobber plugin | Wix widget with Jobber-specific scripts | 🟢 High |
| GBP "Reserve with Google" | Direct API integration | 🟢 High |
| Job postings | "Experience with Jobber" or "Jobber CRM" | 🟡 Medium |
| Review mentions | Customer mentions of "online portal", "estimate app" | 🔴 Low |

---

#### 🔵 Workiz — The Communication-First Player

| Attribute | Detail |
|---|---|
| **Segment** | Solo to SMB (1-25 techs) |
| **Verticals** | HVAC, Junk Removal, Plumbing, Locksmith, Appliance Repair |
| **Pricing** | Lite (free, limited) → Kickstart → Standard → Pro → Ultimate (custom) |
| **Key Differentiators** | Built-in phone system (call tracking, recording, routing), "Genius AI Suite" (lead scoring, call insights, AI answering), heavy automation, aggressive marketing |
| **Deal Signal** | Prospect using Workiz = communication-heavy, lead-focused, moderate digital maturity. **HCP pitch: broader platform, better ecosystem, comparable comms** |

**Detectable Online Signals:**
| Signal | Detection Method | Confidence |
|---|---|---|
| Online booking iframe | `src="https://online-booking.workiz.com/?ac=[AccountID]"` | 🟢 Very High |
| Booking page link | URL pattern: `online-booking.workiz.com/?ac=` | 🟢 Very High |
| Tracking parameters | `&ad_group=` parameters in Workiz URLs | 🟢 High |
| Job postings | "Experience with Workiz" | 🟡 Medium |
| Phone system signals | Call recording disclosures, Workiz-specific voicemail | 🟡 Medium |

---

#### 🟣 FieldEdge — The Mid-Market Traditionalist

| Attribute | Detail |
|---|---|
| **Segment** | SMB to Mid-market (5-100 techs) |
| **Verticals** | HVAC, Plumbing, Electrical (focused) |
| **Pricing** | Quote-based, custom. Tiers: Select → Premier → Elite |
| **Key Differentiators** | Deep QuickBooks integration (real-time sync), flat-rate pricing/price book, service agreements focus, "MarketingEdge" module |
| **Deal Signal** | Prospect using FieldEdge = operations-focused, QuickBooks-dependent, values accounting accuracy. **HCP pitch: modern UX, mobile-first, easier tech onboarding** |

**Detectable Online Signals:**
| Signal | Detection Method | Confidence |
|---|---|---|
| Website booking widget | **No standard public widget** — uses custom API implementations or partner integrations | 🔴 Low direct signal |
| Job postings | "FieldEdge experience", "FieldEdge Flat Rate" | 🟡 Medium |
| Review mentions | "FieldEdge" name in reviews or testimonials | 🟡 Medium |
| Pricing page patterns | Flat-rate "good-better-best" pricing language | 🔴 Low (inferential) |

> ⚠️ **Key insight:** FieldEdge is the **hardest to detect** via web signals. This is exactly where AI inference adds value — combining indirect signals (vertical, team size, region, pricing model language) to estimate likelihood.

---

#### 🏠 Housecall Pro — The Protagonist

| Attribute | Detail |
|---|---|
| **Segment** | Solo to Mid-market (1-50+ techs) — "entry-level all-in-one" |
| **Verticals** | HVAC, Plumbing, Electrical, Cleaning, Landscaping |
| **Pricing** | Basic → Essentials → MAX (+ add-ons, payment processing fees) |
| **Customers** | 50,000+ businesses |
| **Key Differentiators** | Ease of use, mobile-first, payment processing, "Business Coaching" brand, affordable entry point |
| **Competitive Weakness** | "Cost creep" with add-ons, less suitable for complex/enterprise operations |
| **Growth Lever for This Case** | Converting prospects from competitors where HCP has a clear advantage (simplicity, price, UX) |

---

### 1.3 Additional Competitors (Not Mentioned in Case, But Relevant)

| Competitor | Segment | Notes |
|---|---|---|
| **Service Fusion** | SMB-Midmarket | Predictable costs, straightforward UX |
| **FieldPulse** | SMB | Growing challenger, aggressive marketing |
| **Kickserv** | Solo-SMB | Very affordable, basic feature set |
| **GorillaDesk** | SMB (Pest control focus) | Niche vertical strength |
| **mHelpDesk** | SMB | Mid-tier, moderate feature depth |
| **Zuper** | SMB-Enterprise | AI-driven, newer entrant |

---

## 2. SDR/BDR Workflow & Pain Points

### 2.1 Current State: Manual Prospecting Workflow

```
TYPICAL SDR DAILY WORKFLOW (Home Service SaaS Sales)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Morning Block (High Energy — Outreach)
├── Pull list of prospects from CRM/lead source
├── FOR EACH PROSPECT:
│   ├── Google the business name → find website
│   ├── Check website for scheduling tool clues
│   ├── Check Google Business Profile
│   ├── Skim Google/Yelp reviews for tool mentions
│   ├── Check social media for software mentions
│   ├── Check job postings (if any)
│   ├── *** TRY TO FIGURE OUT WHAT SOFTWARE THEY USE ***
│   ├── Customize outreach based on findings
│   └── Send email / make call / LinkedIn
│
│   ⏱️ Time per lead: 10-20+ minutes for manual research
│   ⏱️ High-performing teams: <2 minutes (with data tools)
│
Afternoon Block (Follow-ups & Admin)
├── Follow up on previous outreach
├── Update CRM records
├── Prep for next day's outreach
└── Team syncs
```

### 2.2 Key Pain Points

| Pain Point | Impact | Frequency |
|---|---|---|
| **"I spend 40%+ of my time on admin/research, not selling"** | Directly reduces pipeline velocity | Very Common |
| **"I don't know what software the prospect uses, so my pitch is generic"** | Lower conversion rate, looks unprofessional | Universal |
| **"I waste time on prospects that aren't a fit"** | CAC inflation, rep frustration | Very Common |
| **"Our lead lists have outdated/wrong contact data"** | Wasted outreach, low deliverability | Common |
| **"I can't tell if a prospect is actively looking to switch"** | Missed timing, cold outreach | Common |
| **"Each new rep has to learn the competitive landscape from scratch"** | Slow ramp time for new hires | Recurring |

### 2.3 Key Metric: Time to Meaningful Outreach

| Scenario | Research Time/Lead | Leads/Day | Conversion Rate |
|---|---|---|---|
| **Manual (current state)** | 15-20 min | 15-25 | Low (generic pitch) |
| **Partially automated** | 2-5 min | 40-60 | Medium (some personalization) |
| **AI-enriched (target state)** | <1 min (pre-enriched) | 60-100+ | High (tailored competitive pitch) |

---

## 3. Existing Tools & Their Limitations

### 3.1 Technographic Detection Tools

| Tool | What It Does | Limitations for Home Service |
|---|---|---|
| **BuiltWith** | Massive tech stack database, historical tracking | FSM widget coverage is spotty for small businesses. Enterprise pricing. |
| **Wappalyzer** | Browser extension + API for instant tech stack detection | Misses businesses without websites (very common in home service). Free tier limited. |
| **WhatRuns** | Free Chrome extension | Good for individual lookups, impractical at scale |

### 3.2 Lead Enrichment Platforms

| Tool | Gap for HCP's Use Case |
|---|---|
| **ZoomInfo** | SMB home service coverage is weak. Expensive ($15K+/yr). No FSM-specific detection. |
| **Clearbit** (HubSpot Breeze) | Great for inbound. Doesn't detect FSM-specific tools. |
| **Apollo.io** | Contact-focused. No FSM technographic detection. |
| **6sense** | Enterprise ABM tool. Overkill for SMB home service. |
| **Clay** | Powerful aggregation. But none of its 150+ sources specifically detect Jobber/Workiz/FieldEdge. |

### 3.3 The Gap — Why No Existing Tool Solves This

> [!IMPORTANT]
> **No existing tool in the market specifically solves "Which FSM software does this home service SMB use?"**
>
> - **BuiltWith/Wappalyzer** only work IF the prospect has a website AND embeds a detectable widget
> - **ZoomInfo/Clearbit/Apollo** focus on contact data, not FSM technographics
> - **6sense/Bombora** focus on intent signals, not current tool usage
> - **Klue/Crayon** monitor your competitors' movements, not which competitor your prospect uses
>
> **This is genuine whitespace.** HCP can build a purpose-built competitive intelligence engine for the home service vertical that doesn't exist elsewhere.

---

## 4. Signal Taxonomy: The Complete Detection Playbook

### 4.1 Tier 1 — Direct Deterministic Signals (Rules-Based, No AI Needed)

Binary and conclusive. If found → ~100% confidence.

| # | Signal | Source | Detection Method |
|---|---|---|---|
| 1 | ServiceTitan scheduling widget | Website HTML | Regex: `embed.scheduler.servicetitan.com` or `embed.scheduleengine.net` |
| 2 | ServiceTitan CSS classes | Website HTML | Class match: `se-booking-show`, `st-booking-show` |
| 3 | Jobber Client Hub link | Website or GBP | URL: `clienthub.getjobber.com` |
| 4 | Workiz booking iframe | Website HTML | URL: `online-booking.workiz.com` |
| 5 | HCP booking widget | Website HTML | URL: `housecallpro.com` embed |
| 6 | GBP "Reserve with Google" provider | Google Business Profile | Booking provider attribution |

### 4.2 Tier 2 — Strong Indirect Signals (Structured Extraction)

Require extraction + interpretation. Fairly reliable.

| # | Signal | Source | Detection Method |
|---|---|---|---|
| 7 | Job postings mentioning FSM tool | Indeed, LinkedIn, ZipRecruiter | Keyword search in job descriptions |
| 8 | App store reviews by the prospect | Google Play, Apple App Store | Search reviews for business name + FSM app |
| 9 | Branded customer portal in reviews | Google/Yelp reviews | NLP: "I booked through their online portal" |
| 10 | Social media mentions of tools | Facebook, Instagram, LinkedIn | NLP: business posts about their tools |

### 4.3 Tier 3 — Inferential Signals (AI Required)

Require reasoning across multiple weak signals. This is where AI is essential.

| # | Signal Combination | What AI Infers |
|---|---|---|
| 11 | HVAC + 50+ reviews + Major metro + Sophisticated website | Likely ServiceTitan |
| 12 | Landscaping + Solo + Booking page + Modern website | Likely Jobber |
| 13 | Locksmith + Built-in phone system + Lead tracking | Likely Workiz |
| 14 | Flat-rate "Good/Better/Best" pricing + QuickBooks mentions | Likely FieldEdge |
| 15 | No website + Few reviews + Recently established | Likely no FSM tool (greenfield) |
| 16 | Multiple conflicting signals | Low confidence → flag for manual research |

---

## 5. Synthesis — Key Insights

### 5.1 Why AI Is the Right Lever (Hybrid Architecture)

| Layer | Rules-Based | AI-Powered |
|---|---|---|
| **Tier 1: Direct widget detection** | ✅ Perfect — AI adds no value | ❌ Unnecessary |
| **Tier 2: Structured extraction** | ⚠️ Partial — fragile regex | ✅ Better — NLP handles variations |
| **Tier 3: Multi-signal inference** | ❌ Impossible | ✅ Essential — LLM excels here |
| **Confidence scoring** | ⚠️ Manual thresholds | ✅ Calibrated probability |
| **Maintenance** | ❌ Each competitor = new rule | ✅ New competitor = prompt update |

**Verdict: Hybrid architecture.** Rules for Tier 1 (deterministic), AI for Tiers 2+3 (extraction + inference).

### 5.2 The Competitive Positioning Playbook

| Prospect's Current Tool | HCP's Angle | Key Message |
|---|---|---|
| **ServiceTitan** | Cost savings + simplicity | "You're paying $250+/tech/mo. HCP gives 80% of the value at a fraction of the cost." |
| **Jobber** | Feature upgrade | "Love Jobber's simplicity? HCP matches it and adds payments, marketing, and coaching." |
| **Workiz** | Platform breadth | "Workiz does comms well. HCP does comms + scheduling + invoicing + payments + marketing." |
| **FieldEdge** | Modern UX + mobile | "FieldEdge's QB sync is great. HCP does that too — plus a mobile-first experience your techs will love." |
| **No software** | Digital transformation | "Still on paper? HCP is the easiest way to go digital — most Pros are up in one day." |
| **Unknown** | Discovery approach | "We'd love to understand how you manage your business today." |
