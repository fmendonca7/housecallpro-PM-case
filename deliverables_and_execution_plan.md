# 🎯 HCP Case Study — Deliverables & Execution Roadmap

## Overdelivery Strategy for Staff PM Differentiation

---

> [!IMPORTANT]
> **OUR STRATEGY: Don't build "just" a prototype.**
> We're building a **real, functional MVP** — with an AI pipeline that actually runs, real business data, and an interface that looks like a product, not a hack. The gap between "prototype that runs" and "MVP that impresses" is what differentiates.

**Staff-level overdelivery:**

| Aspect | "Meets Expectations" | "Staff PM — Obvious Hire" |
|---|---|---|
| Interface | Streamlit / basic UI | Polished dashboard with a professional design system |
| Data | Mocked JSON | Mix of real (scraped) + mocked data, clearly labeled |
| AI pipeline | Single LLM prompt | Multi-stage pipeline with real confidence scoring |
| Failure handling | Error message | Confidence-tier framework with evidence trail + feedback loop |
| Iterability | Hard to change | Modular components, config-driven, easy to pivot live |
| Deployment | Runs locally | Deployed with public link (Vercel / Railway) |

---

### 📦 DELIVERABLE 1: Process Folder (Process Artifacts)

**What they said:**
> "The files that show how you got there. This should include, at minimum:"
> - **a problem framing doc**
> - **any research or competitive reference you pulled**
> - **a requirements or PRD document (even if brief)**
> - **evidence of how you iterated with an LLM to refine your approach before building**
> "Organized files in a folder or repo."

**Required sub-artifacts:**

#### 📄 2A. Problem Framing Doc

> 1. **Problem framing** — What specific pain point are you targeting and why? What does "good" look like and what does failure look like?

Must answer:
- [ ] Which **specific pain point** are we attacking? (not generic)
- [ ] **Why** this pain point and not another?
- [ ] What does **"good" look like**? (clear definition of success)
- [ ] What does **"failure" look like**? (clear definition of failure)
- [ ] Context of the problem in the current sales rep workflow
- [ ] Quantifiable impact (even if estimated)

> 2. **Solution design** — What approach are you taking and why? What does the AI do autonomously versus hand back to a human? Describe one viable non-AI alternative you considered and explain why AI is the better choice here.

Must answer:
- [ ] Which approach and **why this one**?
- [ ] What does **AI do on its own** vs. what does it **hand back to a human**?
- [ ] **One viable non-AI alternative** you considered
- [ ] **Why AI is the better choice** (well-grounded argument)

> 3. **Trust and failure** — How does the customer interact with this when the system is wrong or uncertain? How does trust build over time?

Must answer:
- [ ] How does the user **interact with the system when it's wrong**?
- [ ] How does the user **interact when the system is uncertain**?
- [ ] How does **trust build over time**?

> 4. **Measurement** — How would you know if this is working? What outcome metrics matter, and if AI is involved, what quality bar would you set before shipping?

Must answer:
- [ ] How to know if it's **working**?
- [ ] Which **outcome metrics** matter?
- [ ] What **quality bar for AI** before shipping?

#### 📄 2B. Research / Competitive References

Must contain:
- [ ] Research on the competitors named in the case (ServiceTitan, Jobber, Workiz, FieldEdge)
- [ ] How they differentiate, pricing, segments, detectable signals
- [ ] Market benchmarks (if similar tools exist)
- [ ] References on how B2B SaaS companies do competitive intelligence

#### 📄 2C. Requirements / PRD Document

Must contain:
- [ ] User stories / Jobs to be Done
- [ ] MVP functional requirements
- [ ] Non-functional requirements (performance, accuracy thresholds)
- [ ] Scope: what's IN vs. OUT
- [ ] Prioritization and rationale

#### 📄 2D. Evidence of LLM Iteration

> "evidence of how you iterated with an LLM to refine your approach before building"

Must contain:
- [ ] Prompts used during the **planning** phase (not just coding)
- [ ] How LLM output influenced decisions
- [ ] Where you disagreed with the LLM and why
- [ ] Evolution of thinking across iterations

---

### 📦 DELIVERABLE 2: Eval Spec (1 page max)

Must contain:
- [ ] **Success criteria** — clear quantitative metrics
- [ ] **Error taxonomy** — failure types categorized by severity + rationale
- [ ] **Mini test set** of 10–20 examples with:
  - Input (business info)
  - Expected output (which tool, what confidence)
  - Ground truth (how you know it's true)
- [ ] **Focus on AI output quality** — precision, recall, confidence calibration
- [ ] **Reproducible** — a teammate can run it next week

> [!IMPORTANT]
> **Constraint: 1 page max.** 


**Staff-level overdelivery:**

| Aspect | "Meets Expectations" | "Staff PM — Obvious Hire" |
|---|---|---|
| Test set | 10 hypothetical examples | 20 examples with **real businesses**, verifiable ground truth |
| Error taxonomy | List of possible errors | Table with severity, acceptable rate, mitigation strategy |
| Success criteria | "Accuracy > 80%" | Metrics per confidence tier, per competitor, with operational thresholds |
| Reproducibility | "Run the script" | Automated script that runs the eval end-to-end and generates a report |

---

### 📦 DELIVERABLE 3: AI Usage Log

Must contain:
- [ ] **Tools used** + what for (not a list — context)
- [ ] **Top 2–3 prompts/workflows** that shaped the outcome
- [ ] **≥1 AI failure** + what you did about it
- [ ] **≥1 decision made without AI** + why deliberately
- [ ] Tone: **judgment and real practice**, not virtue signaling

> [!TIP]
> This log will be built **organically** during the work. But we already know we need to capture: moments of iteration, moments of divergence, and moments of purely human decision. I'll help you document this along the way.

---

## Part II — Consolidated Delivery Checklist

```
📁 hcp-case-study/
├── 📁 process/
│   ├── 01_problem_framing.md          ← Pain point, definition of good/bad
│   ├── 02_research_competitive.md     ← Market research + competitors
│   ├── 03_solution_design.md          ← Approach, AI vs. human, non-AI alternative
│   ├── 04_trust_and_failure.md        ← Error handling, uncertainty, trust building
│   ├── 05_measurement.md              ← Metrics, quality bar
│   ├── 06_prd.md                      ← Requirements, scope, prioritization
│   └── 07_llm_iteration_evidence.md   ← Prompts, evolution of thinking
├── 📄 eval_spec.md                    ← 1 page: criteria, error taxonomy, test set
├── 📄 ai_usage_log.md                 ← Evaluated disclosure
├── 📁 prototype/                      ← The functional MVP
│   ├── ...code...
│   └── README.md                      ← How to run
└── 📄 README.md                       ← Index + general context
```

---

## Part III — Prompt Workflow: The Exact Sequence

> [!IMPORTANT]
> Each prompt below is a **phase of work**. You send me the prompt, we work together, we document the output, and we move to the next. **This sequence IS the evidence of LLM iteration.**

### Phase 0: Research Foundation
**Objective:** Build the knowledge base that informs everything that follows.

```
PROMPT 0.1 — Competitive Intelligence Deep Dive
"Research the 4 competitors named in the case in depth (ServiceTitan,
Jobber, Workiz, FieldEdge). For each: target segment, pricing, core
features, online detectable signals (widgets, integrations, job
postings, review patterns). Include other relevant competitors the
case didn't name but are significant in the market."

PROMPT 0.2 — Sales Workflow Research
"Analyze the typical workflow of an SDR/BDR doing outbound to home
service SMBs. Research stages, time spent, tools used, biggest
frustrations. Pull from sales ops blogs, podcasts, and content
from FSM software sellers."

PROMPT 0.3 — Competitive Intelligence Tools Benchmark
"Research existing competitive intelligence and technographics tools
(BuiltWith, Wappalyzer, ZoomInfo, Clearbit, etc.). How do they detect
tech stacks? What are the limitations for home service SMBs
specifically? Is there any solution already focused on this niche?"
```

---

### Phase 1: Problem Framing
**Objective:** Produce the Problem Framing Doc + start of Solution Design.

```
PROMPT 1.1 — Problem Framing
"Based on the research we've done, help me write the Problem Framing
Doc. I want it to answer: (1) What's the specific pain point we're
attacking and why is it the highest-leverage one? (2) Who's the main
user and what's the JTBD? (3) What does 'good' look like — with
concrete scenarios? (4) What does 'failure' look like — with concrete
scenarios and real consequences? No generic — I want specificity."

PROMPT 1.2 — Solution Architecture
"Now let's design the solution. I need to answer: (1) What's the
approach and why? (2) What does AI do on its own vs. hand back to
a human — with the clear line where it cuts? (3) A viable non-AI
alternative + why AI is better. I want the solution to be real —
with pipeline stages, data sources, confidence model, and output
format."
```

---

### Phase 2: Trust, Measurement & Requirements
**Objective:** Produce Trust & Failure doc, Measurement doc, PRD.

```
PROMPT 2.1 — Trust & Failure Framework
"Design the trust and failure framework for our solution. I need:
(1) Confidence tiers with clear classification rules, (2) UX for
each tier — what the rep sees, what they can/should do, (3) Feedback
loop — how the rep reports an error and how that improves the
system, (4) How trust builds over time — progressive disclosure,
accuracy track record visible to the user."

PROMPT 2.2 — Measurement Framework
"Define the measurement framework. I need: (1) Outcome metrics
that matter for the business (pipeline metrics), (2) AI quality
metrics (precision, recall, confidence calibration), (3) Quality
bar before shipping — specific thresholds, (4) How to monitor in
production."

PROMPT 2.3 — PRD
"Write the PRD for the MVP. User stories, functional and
non-functional requirements, scope in/out, prioritization with
rationale. Focus on what we'll build in the prototype, with notes
on what would be V2."
```

---

### Phase 3: Eval Spec
**Objective:** Produce the 1-page Eval Spec.

```
PROMPT 3.1 — Eval Spec
"Build the eval spec in 1 page. I need: (1) Quantitative success
criteria, (2) Error taxonomy with severity and acceptable rates,
(3) Mini test set of 20 examples with REAL businesses I can verify —
input, expected output, ground truth source. The test needs to be
reproducible by someone on the team next week."
```

---

### Phase 4: Build the MVP
**Objective:** Build the real, functional solution.

```
PROMPT 4.1 — Architecture & Stack Decision
"Let's define the technical stack for the MVP. Criteria: (1) I
need to iterate live in 30 min during the session, (2) It needs
to look professional — can't look like a hack, (3) Real AI
pipeline that works, (4) Deploy with public link. Propose the
architecture and justify."

PROMPT 4.2 — Build Core Pipeline
"Build the AI enrichment pipeline. [Details based on the
architecture defined in 4.1]"

PROMPT 4.3 — Build Interface
"Build the dashboard interface. [Details based on the PRD]"

PROMPT 4.4 — Wire Pipeline + Interface
"Connect the pipeline to the interface. Test end-to-end."

PROMPT 4.5 — Failure States & Edge Cases
"Implement all failure states, empty states, loading states,
and confidence UI."

PROMPT 4.6 — Deploy
"Deploy to [platform] with public link."
```

---

### Phase 5: Polish & Documentation
**Objective:** AI Usage Log, README, final packaging.

```
PROMPT 5.1 — AI Usage Log
"Help me assemble the AI Usage Log based on all of our work
history. Let's pick the most relevant moments."

PROMPT 5.2 — Final Package
"Organize everything in the final format for submission."
```

---

## Part IV — Overdelivery Strategy by Session Segment

### Part 1 — Process Walkthrough (10 min)

| What they expect | What we'll deliver |
|---|---|
| Problem framing doc | Problem framing doc + **user research synthesis** with simulated quotes from SDR interviews |
| Research | Detailed competitive intelligence + **benchmark of existing tools** |
| Brief PRD | Complete PRD with **prioritization matrix** |
| Evidence of LLM use | **Iteration narrative** showing evolution of thinking, not just prompts |

### Part 2 — Demo (15 min)

| What they expect | What we'll deliver |
|---|---|
| Prototype that runs | **Deployed MVP** with public link, real AI pipeline |
| Stubbed outputs | **Mix of real data + real AI enrichment** |
| Core interaction works | **Full flow**: list → enrich → detail → feedback |
| Failure handling | **Visual confidence tiers** + functional feedback loop |

### Part 3 — Live Iteration (30 min)

| What they expect | What we'll deliver |
|---|---|
| Can change things live | **Modular architecture** — configs, separated components, easy prompt engineering |
| Uses AI tools in real time | **Practiced workflow** — we know exactly how to iterate each part |
| Reacts well to feedback | **5–7 likely feedback scenarios prepared** with planned responses |

### Part 4 — Debrief (5 min)

| What they expect | What we'll deliver |
|---|---|
| Verbal "next version" | **Written V2 roadmap** with 3-month vision in the solution design doc |
| Learnings | Genuine reflection on trade-offs made |

---

## Part V — Likely Feedback Scenarios in the Live Session

Based on the prompt and the Internal Tooling + Sales context:

| # | Likely panel feedback | Preparation |
|---|---|---|
| 1 | "What if the prospect has no website?" | Have a fallback flow for no-website prospects |
| 2 | "How does this integrate with Salesforce?" | Mocked Salesforce integration or a wire diagram ready |
| 3 | "What's the API cost per enriched lead?" | Have a per-call cost estimate ready |
| 4 | "What if the prospect uses software we don't know?" | "Other/Unknown" detection + learning loop |
| 5 | "How would batch processing work?" | Have at least the design for batch mode |
| 6 | "What if the confidence score is wrong?" | Demo the feedback loop + retrain cycle |
| 7 | "Does this scale to 10K leads per day?" | Have an answer on architecture scaling |

---

## Part VI — Anticipated Technical Decisions

### MVP stack (recommendation to validate in Phase 4)

```
Frontend:  Next.js or vanilla HTML/JS (whichever you iterate faster on live)
Backend:   Python FastAPI (AI pipeline + API)
AI:        OpenAI GPT-4o (inference) + deterministic rules (tech stack detection)
Database:  SQLite or JSON (simplicity for MVP)
Deploy:    Vercel (frontend) + Railway / Render (backend)
```

### Why this stack:
1. **Python for AI** — most mature ecosystem for LLMs
2. **FastAPI** — fast to build, easy to modify live
3. **Cloud deploy** — public link = professionalism
4. **SQLite / JSON** — zero infra overhead

---

## Next Steps

> [!IMPORTANT]
> **The sequence is clear:**
> 1. You review and approve this plan
> 2. We start with **Phase 0: Research** — prompts 0.1, 0.2, 0.3
> 3. Each phase produces an artifact. Each artifact is a deliverable.
> 4. In Phase 4, we build the real MVP.
> 5. In Phase 5, we package everything.
>
> **Every interaction of ours is being documented and will be part of the "Evidence of LLM Iteration" deliverable.**
