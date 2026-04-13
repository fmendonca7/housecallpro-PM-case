# 🎯 HCP Case Study — Deliverables & Execution Roadmap

## Overdelivery Strategy for Staff PM Differentiation

---

## Part I — Cirurgical Analysis: O Que EXATAMENTE Foi Pedido

### Entrega Final = 4 itens, enviados via link ou zip, 24h antes da sessão

---

### 📦 DELIVERABLE 1: Working Prototype

**O que eles disseram:**
> "A working prototype — functional, demoable, and built using AI coding tools. Not a wireframe or a deck. Something that runs."
> "The prototype can be lightweight and partially mocked (e.g., stubbed outputs) as long as the core interaction, failure handling, and eval plan are clearly demoable."

**O que PRECISA conter (mínimo):**
- [ ] Algo que **roda** — não é Figma, não é slide, não é wireframe
- [ ] Construído com **AI coding tools** (isso é avaliado)
- [ ] **Core interaction** demoável — o fluxo principal funciona
- [ ] **Failure handling** demoável — o que acontece quando o sistema erra/não sabe
- [ ] **Eval plan** demoável — como se testa a qualidade

**O que o case EXPLICITAMENTE quer que o protótipo endereça:**
1. ✅ Detecção de competitor tools que um prospect usa
2. ✅ Signal enrichment para prospects de home service
3. ✅ Sales reps podem **tailor outreach** com base nos dados
4. ✅ Sales reps podem **priorizar high-fit leads**
5. ✅ Resultado: **acelerar pipeline conversion**

**O que a live iteration session exige do protótipo:**
> "The panel will give you feedback as if we are customers. Your job is to act on that feedback live — using AI tools, in the session, in front of us."

- [ ] Código **flexível** o suficiente para ser modificado ao vivo em 30min
- [ ] Stack que você domina e consegue iterar rapidamente
- [ ] Estrutura modular (componentes isolados, fácil de trocar/adicionar)

> [!IMPORTANT]
> **NOSSA ESTRATÉGIA: Não fazer "apenas" um protótipo.**
> Vamos construir um **MVP funcional real** — com pipeline AI que de fato roda, dados reais de businesses, e uma interface que parece produto, não hack. A diferença entre "protótipo que roda" e "MVP que impressiona" é o que vai te destacar.

**Staff-level overdelivery:**

| Aspecto | "Meets Expectations" | "Staff PM — Contratação Óbvia" |
|---|---|---|
| Interface | Streamlit/basic UI | Dashboard polido com design system profissional |
| Dados | Mocked JSON | Mix de dados reais (scraped) + mocked, claramente documentado |
| AI Pipeline | Um prompt para LLM | Pipeline multi-stage com confidence scoring real |
| Failure handling | Mensagem de erro | Framework de confidence tiers com evidence trail + feedback loop |
| Iterabilidade | Difícil de mudar | Componentes modulares, config-driven, fácil de pivotar ao vivo |
| Deployment | Roda local | Deployed com link público (Vercel/Railway) |

---

### 📦 DELIVERABLE 2: Process Folder (Artefatos de Processo)

**O que eles disseram:**
> "The files that show how you got there. This should include, at minimum:"
> - **a problem framing doc**
> - **any research or competitive reference you pulled**
> - **a requirements or PRD document (even if brief)**
> - **evidence of how you iterated with an LLM to refine your approach before building**
> "Organized files in a folder or repo."

**Sub-artefatos obrigatórios:**

#### 📄 2A. Problem Framing Doc

**O que o case pede que esteja aqui (seção "Your prototype and process artifacts should address"):**

> 1. **Problem framing** — What specific pain point are you targeting and why? What does "good" look like and what does failure look like?

Precisa responder:
- [ ] Qual **pain point específico** estamos atacando? (não genérico)
- [ ] **Por que** esse pain point e não outro?
- [ ] O que **"good" looks like**? (definição clara de sucesso)
- [ ] O que **"failure" looks like**? (definição clara de fracasso)
- [ ] Contexto do problema no workflow atual dos sales reps
- [ ] Impacto quantificável (mesmo que estimado)

> 2. **Solution design** — What approach are you taking and why? What does the AI do autonomously versus hand back to a human? Describe one viable non-AI alternative you considered and explain why AI is the better choice here.

Precisa responder:
- [ ] Qual abordagem e **por que essa**?
- [ ] O que o **AI faz sozinho** vs. o que **devolve para o humano**?
- [ ] **Uma alternativa não-AI viável** que você considerou
- [ ] **Por que AI é a melhor escolha** (argumento fundamentado)

> 3. **Trust and failure** — How does the customer interact with this when the system is wrong or uncertain? How does trust build over time?

Precisa responder:
- [ ] Como o usuário **interage com o sistema quando ele erra**?
- [ ] Como o usuário **interage quando o sistema está incerto**?
- [ ] Como a **confiança se constrói ao longo do tempo**?

> 4. **Measurement** — How would you know if this is working? What outcome metrics matter, and if AI is involved, what quality bar would you set before shipping?

Precisa responder:
- [ ] Como saber se está **funcionando**?
- [ ] Quais **outcome metrics** importam?
- [ ] Qual **quality bar para AI** antes de fazer ship?

#### 📄 2B. Research / Competitive References

Precisa conter:
- [ ] Pesquisa sobre os competidores mencionados (ServiceTitan, Jobber, Workiz, FieldEdge)
- [ ] Como eles se diferenciam, pricing, segmentos, sinais detectáveis
- [ ] Benchmarks de mercado (se existem ferramentas similares)
- [ ] Referências de como empresas SaaS B2B fazem competitive intelligence

#### 📄 2C. Requirements / PRD Document

Precisa conter:
- [ ] User stories / Jobs to be Done
- [ ] Requisitos funcionais do MVP
- [ ] Requisitos não-funcionais (performance, accuracy thresholds)
- [ ] Scope: o que está IN vs OUT
- [ ] Priorização e justificativa

#### 📄 2D. Evidence of LLM Iteration

> "evidence of how you iterated with an LLM to refine your approach before building"

Precisa conter:
- [ ] Prompts usados durante a fase de **planejamento** (não só coding)
- [ ] Como o output do LLM influenciou decisões
- [ ] Onde você discordou do LLM e por quê
- [ ] Evolução do pensamento ao longo das iterações

> [!WARNING]
> **Este item é um trap test.** Eles querem ver que você PENSA antes de construir, e que usa AI como parceiro de pensamento — não como máquina de código. A evidência de iteração com LLM na fase de PLANEJAMENTO é diferente da evidência de uso de AI para CODAR.

---

### 📦 DELIVERABLE 3: Eval Spec (1 página max)

**O que eles disseram:**
> "Before you built, you defined what 'good' looks like."
> Include:
> - **Success criteria**
> - **An error taxonomy** (what types of failures matter and why)
> - **A mini test set of 10–20 examples**
> "If AI is used, focus on AI output quality"
> "This doesn't need to be exhaustive — it needs to be specific enough that a team member could run it next week."

Precisa conter:
- [ ] **Success criteria** — métricas quantitativas claras
- [ ] **Error taxonomy** — tipos de falha categorizados por severidade + justificativa
- [ ] **Mini test set** de 10-20 exemplos com:
  - Input (business info)
  - Expected output (qual tool, que confiança)
  - Ground truth (como você sabe que é verdade)
- [ ] **Foco em AI output quality** — precision, recall, confidence calibration
- [ ] **Reproduzível** — alguém da equipe consegue rodar na semana seguinte

> [!IMPORTANT]
> **Constraint: 1 página max.** Isso é intencional — testa sua capacidade de ser conciso e priorizar. Não é lugar para dissertação. É lugar para clareza cirúrgica.

**Staff-level overdelivery:**

| Aspecto | "Meets Expectations" | "Staff PM — Contratação Óbvia" |
|---|---|---|
| Test set | 10 exemplos hipotéticos | 20 exemplos com **businesses reais**, ground truth verificável |
| Error taxonomy | Lista de erros possíveis | Tabela com severity, acceptable rate, mitigation strategy |
| Success criteria | "Accuracy > 80%" | Métricas por tier de confiança, por competidor, com thresholds operacionais |
| Reproduzibilidade | "Rode o script" | Script automatizado que roda o eval end-to-end e gera report |

---

### 📦 DELIVERABLE 4: AI Usage Log

**O que eles disseram:**
> "This is an evaluated part of your submission, not a formality."
> Include:
> - **Which tools you used and for what**
> - **Your top 2–3 prompts or workflows that shaped the outcome**
> - **At least one place where AI output was wrong, misleading, or off — and what you did about it**
> - **At least one thing you deliberately chose to do without AI, and why**
> "We're looking for judgment and real practice — not a tool list."

Precisa conter:
- [ ] **Tools usadas** + para que (não lista — contexto)
- [ ] **Top 2-3 prompts/workflows** que moldaram o resultado
- [ ] **≥1 erro do AI** + o que você fez a respeito
- [ ] **≥1 decisão sem AI** + por que deliberadamente
- [ ] Tom: **julgamento e prática real**, não virtue signaling

> [!TIP]
> Este log vai ser construído **organicamente** durante nosso trabalho. Mas já sabemos que precisamos capturar: momentos de iteração, momentos de divergência, e momentos de decisão humana pura. Vou te ajudar a documentar isso ao longo do caminho.

---

## Part II — Checklist Consolidado de Entregas

```
📁 hcp-case-study/
├── 📁 process/
│   ├── 01_problem_framing.md          ← Pain point, definição de bom/ruim
│   ├── 02_research_competitive.md     ← Pesquisa de mercado + competidores
│   ├── 03_solution_design.md          ← Abordagem, AI vs humano, alternativa não-AI
│   ├── 04_trust_and_failure.md        ← Tratamento de erro, incerteza, trust building
│   ├── 05_measurement.md             ← Métricas, quality bar
│   ├── 06_prd.md                      ← Requirements, scope, priorização
│   └── 07_llm_iteration_evidence.md   ← Prompts, evolução do pensamento
├── 📄 eval_spec.md                    ← 1 página: criteria, error taxonomy, test set
├── 📄 ai_usage_log.md                 ← Disclosure avaliada
├── 📁 prototype/                      ← O MVP funcional
│   ├── ...código...
│   └── README.md                      ← Como rodar
└── 📄 README.md                       ← Índice + contexto geral
```

---

## Part III — Workflow de Prompts: A Sequência Exata

> [!IMPORTANT]
> Cada prompt abaixo é uma **fase de trabalho**. Você me envia o prompt, nós trabalhamos juntos, documentamos o output, e passamos para o próximo. **Essa sequência É a evidence of LLM iteration.**

### Fase 0: Research Foundation
**Objetivo:** Construir base de conhecimento que informa tudo depois.

```
PROMPT 0.1 — Competitive Intelligence Deep Dive
"Pesquise profundamente os 4 competidores mencionados no case (ServiceTitan, 
Jobber, Workiz, FieldEdge). Para cada um, documento: segmento-alvo, pricing, 
features principais, sinais detectáveis online (widgets, integrations, job 
postings, review patterns). Inclua também outros competidores relevantes que 
o case não mencionou mas que são significativos no mercado."

PROMPT 0.2 — Sales Workflow Research  
"Analise como funciona o workflow típico de um SDR/BDR fazendo outbound 
para SMBs de home service. Quais são as etapas de research, quanto tempo 
gastam, que ferramentas usam, quais são as maiores frustrações. Use 
referências de blogs de sales ops, podcasts, e conteúdo de vendedores 
de FSM software."

PROMPT 0.3 — Competitive Intelligence Tools Benchmark
"Pesquise as soluções existentes de competitive intelligence e technographics 
(BuiltWith, Wappalyzer, ZoomInfo, Clearbit, etc.). Como elas detectam tech 
stack? Quais são as limitações para SMBs de home service especificamente? 
Existe alguma solução já focada nesse nicho?"
```

---

### Fase 1: Problem Framing
**Objetivo:** Produzir o Problem Framing Doc + início do Solution Design.

```
PROMPT 1.1 — Problem Framing
"Com base na pesquisa que fizemos, me ajude a escrever o Problem Framing 
Doc. Quero que ele responda: (1) Qual é o pain point específico que estamos 
atacando e por que ele é o de maior alavancagem? (2) Quem é o usuário 
principal e qual é o JTBD? (3) O que 'good' looks like — com cenários 
concretos? (4) O que 'failure' looks like — com cenários concretos e 
consequências reais? Não quero genérico — quero specificity."

PROMPT 1.2 — Solution Architecture  
"Agora vamos desenhar a solução. Preciso responder: (1) Qual é a abordagem 
e por quê? (2) O que o AI faz sozinho vs. devolve para humano — com a 
linha clara de onde corta? (3) Uma alternativa não-AI viável + por que AI 
é melhor. Quero que a solução seja real — com pipeline stages, data sources, 
confidence model, e output format."
```

---

### Fase 2: Trust, Measurement & Requirements
**Objetivo:** Produzir Trust & Failure doc, Measurement doc, PRD.

```
PROMPT 2.1 — Trust & Failure Framework
"Desenhe o framework de trust e failure para nossa solução. Preciso de: 
(1) Confidence tiers com regras claras de classificação, (2) UX para 
cada tier — o que o rep vê, o que pode/deve fazer, (3) Feedback loop 
— como o rep reporta erro e como isso melhora o sistema, (4) Como 
trust se constrói ao longo do tempo — progressive disclosure, accuracy 
track record visible to user."

PROMPT 2.2 — Measurement Framework
"Defina o framework de medição. Preciso de: (1) Outcome metrics que 
importam para o business (pipeline metrics), (2) AI quality metrics 
(precision, recall, confidence calibration), (3) Quality bar antes 
de shipping — thresholds específicos, (4) Como monitorar em produção."

PROMPT 2.3 — PRD
"Escreva o PRD para o MVP. User stories, requisitos funcionais e 
não-funcionais, scope in/out, priorização com justificativa. Foco 
no que vamos construir no protótipo, com notas sobre o que seria V2."
```

---

### Fase 3: Eval Spec
**Objetivo:** Produzir o Eval Spec de 1 página.

```
PROMPT 3.1 — Eval Spec
"Construa o eval spec em 1 página. Preciso de: (1) Success criteria 
quantitativos, (2) Error taxonomy com severity e acceptable rates, 
(3) Mini test set de 20 exemplos com businesses REAIS que eu possa 
verificar — input, expected output, ground truth source. O teste 
precisa ser reproduzível por alguém do time na semana seguinte."
```

---

### Fase 4: Build the MVP
**Objetivo:** Construir a solução funcional real.

```
PROMPT 4.1 — Architecture & Stack Decision
"Vamos definir a stack técnica do MVP. Critérios: (1) Preciso iterar 
ao vivo em 30min durante a sessão, (2) Precisa parecer profissional 
— não pode parecer hack, (3) AI pipeline real que funcione, (4) Deploy 
com link público. Proponha a arquitetura e justifique."

PROMPT 4.2 — Build Core Pipeline
"Construa o pipeline AI de enrichment. [Detalhes baseados na 
arquitetura definida em 4.1]"

PROMPT 4.3 — Build Interface
"Construa a interface do dashboard. [Detalhes baseados no PRD]"

PROMPT 4.4 — Wire Pipeline + Interface
"Conecte o pipeline à interface. Teste end-to-end."

PROMPT 4.5 — Failure States & Edge Cases
"Implemente todos os failure states, empty states, loading states, 
e confidence UI."

PROMPT 4.6 — Deploy
"Deploy para [plataforma] com link público."
```

---

### Fase 5: Polish & Documentation
**Objetivo:** AI Usage Log, README, empacotamento final.

```
PROMPT 5.1 — AI Usage Log
"Me ajude a montar o AI Usage Log baseado em todo o nosso histórico 
de trabalho. Vamos selecionar os momentos mais relevantes."

PROMPT 5.2 — Final Package
"Organize tudo no formato final para entrega."
```

---

## Part IV — Estratégia de Overdelivery por Segmento da Sessão

### Part 1 — Process Walkthrough (10 min)

| O que esperam | O que vamos entregar |
|---|---|
| Problem framing doc | Problem framing doc + **user research synthesis** com quotes simuladas de SDR interviews |
| Research | Competitive intelligence detalhada + **benchmark de ferramentas existentes** |
| PRD breve | PRD completo com **prioritization matrix** |
| Evidence de LLM | **Narrative de iteração** mostrando evolução do pensamento, não só prompts |

### Part 2 — Demo (15 min)

| O que esperam | O que vamos entregar |
|---|---|
| Protótipo que roda | **MVP deployed** com link público, pipeline AI real |
| Stubbed outputs | **Mix de dados reais + enrichment AI real** |
| Core interaction funciona | **Fluxo completo**: list → enrich → detail → feedback |
| Failure handling | **Confidence tiers visuais** + feedback loop funcional |

### Part 3 — Live Iteration (30 min)

| O que esperam | O que vamos entregar |
|---|---|
| Consegue mudar coisas ao vivo | **Arquitetura modular** — configs, componentes separados, prompt engineering fácil |
| Usa AI tools em tempo real | **Workflow praticado** — sabemos exatamente como iterar cada parte |
| Reage bem a feedback | **Preparar 5-7 cenários de feedback provável** e ter respostas planejadas |

### Part 4 — Debrief (5 min)

| O que esperam | O que vamos entregar |
|---|---|
| "Próxima versão" verbal | **V2 roadmap escrito** com 3-month vision no doc de solution design |
| Learnings | Reflexão genuína sobre trade-offs feitos |

---

## Part V — Cenários de Feedback Provável na Live Session

Baseado no prompt e no contexto de Internal Tooling + Sales:

| # | Feedback provável do painel | Preparação |
|---|---|---|
| 1 | "E se o prospect não tem website?" | Ter fallback flow para no-website prospects |
| 2 | "Como isso se integra no Salesforce?" | Mock de Salesforce integration ou wire diagram pronto |
| 3 | "Qual é o custo de API por lead enriquecido?" | Ter estimativa de custo por call pronta |
| 4 | "E se o prospect usa software que não conhecemos?" | "Other/Unknown" detection + learning loop |
| 5 | "Como batch processing funcionaria?" | Ter pelo menos o design de batch mode |
| 6 | "E se o confidence score está errado?" | Demo do feedback loop + retrain cycle |
| 7 | "Isso escala para 10K leads por dia?" | Ter resposta sobre architecture scaling |

---

## Part VI — Decisões Técnicas Antecipadas

### Stack para o MVP (recomendação a validar na Fase 4)

```
Frontend:  Next.js ou HTML/JS vanilla (o que você iterar melhor ao vivo)
Backend:   Python FastAPI (pipeline AI + API)
AI:        OpenAI GPT-4o (inference) + regras determinísticas (tech stack detection)
Database:  SQLite ou JSON (simplicidade para MVP)
Deploy:    Vercel (frontend) + Railway/Render (backend)
```

### Por que essa stack:
1. **Python para AI** — ecossistema mais maduro para LLM
2. **FastAPI** — rápido de construir, fácil de modificar ao vivo
3. **Deploy cloud** — link público = profissionalismo
4. **SQLite/JSON** — zero overhead de infra

---

## Próximos Passos

> [!IMPORTANT]
> **A sequência é clara:**
> 1. Você revisa e aprova este plano
> 2. Começamos pela **Fase 0: Research** — os prompts 0.1, 0.2, 0.3
> 3. Cada fase produz um artefato. Cada artefato é entregável.
> 4. Na Fase 4, construímos o MVP real.
> 5. Na Fase 5, empacotamos tudo.
>
> **Cada interação nossa está sendo documentada e será parte do deliverable "Evidence of LLM Iteration".**
