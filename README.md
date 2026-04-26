# Agentic AI Decision Intelligence Platform

5-layer autonomous AI platform that enriches, scores, and routes 
B2B leads using multi-agent orchestration and AI reasoning with 
deterministic guardrails. Built for reliability, transparency, 
and production deployment.

![Architecture](GTM%20AI%20AGENTIC%20ENGINE.png)

[View PRD](#) ← replace with Notion link

---

## The Problem

B2B SaaS SDRs spend 13 to 21 minutes per lead on manual research 
across Salesforce, Gong, and spreadsheets before making a priority 
decision. At a 10-person sales team that is over 150 hours of 
weekly capacity lost to work that does not require human judgment.

The root cause is fragmented data. Buying signals live across CRMs, 
call transcripts, and intent platforms with no single system 
connecting them into a routing decision.

This platform automates that workflow end to end in under 30 seconds.

---

## The User

Primary user is an SDR at a B2B SaaS company managing 30 to 100 
leads per week. Before this platform they open Salesforce, pull 
CRM data manually, open Gong for transcript context, cross-reference 
ICP criteria, make a gut-call on priority, update the CRM stage, 
and assign to a rep. Every step is manual. Every step is a source 
of error.

After this platform they receive a Slack alert with lead score, 
priority tier, rep assignment, and recommended next action. The 
entire research and routing workflow is gone.

Secondary user is a RevOps manager who needs full auditability on 
every routing decision for pipeline reporting and leadership reviews.

---

## Product Decisions

**Enrichment before scoring before routing**

Each layer is a dependency for the next. Dirty data produces wrong 
scores. Wrong scores produce wrong routing. Wrong routing loses 
revenue. The sequencing was a product decision about reliability, 
not a technical preference.

**Hybrid AI reasoning with deterministic guardrails**

Pure LLM scoring hallucinates in revenue contexts. Pure rules 
engines miss nuanced signals in call transcripts. The hybrid 
approach gives you AI pattern recognition with deterministic 
reliability. Hard caps, floors, and overrides ensure no lead is 
catastrophically misdirected. Every score delta between the AI 
output and the final rules-adjusted score is recorded for full 
auditability.

**Three specialized agents over one general agent**

Asking one agent to simultaneously analyse signals, reason about 
routing, and format structured output produces non-deterministic 
results. Separating concerns across three sequential agents makes 
each step reliable, testable, and debuggable independently.

**Constrained MVP first**

I scoped to 20 leads and 3 data sources to validate the 
architecture end to end before investing in production 
integrations. The production upgrade path was designed upfront 
so going live requires zero code changes.

---

## Trade-offs

**Reliability over speed**

Removing the deterministic guardrails would cut latency from 
30 seconds to under 10. In a revenue routing context a faster 
wrong answer is worse than a slower right one.

**Consistency over maximum flexibility**

LLM temperature set to 0.2 deliberately. Default temperature 
produces inconsistent routing on identical leads across runs 
which destroys rep trust. Zero temperature makes agents too 
rigid for nuanced transcript reasoning. 0.2 is the deliberate 
middle ground between routing consistency and reasoning 
flexibility.

**Transparency over full automation**

Reps receive Slack alerts they can act on or override. Full 
automation without human visibility kills adoption regardless 
of accuracy. A system reps cannot understand will not be used.

---

## Success Metrics

**System level**

Pipeline latency under 30 seconds. 100% routing coverage with 
no leads falling through. Full score delta audit trail on every 
decision.

**Behavioral level**

SDR adoption rate. Override rate as a proxy for trust. Research 
time per lead trending toward zero.

**Business level**

SDR capacity freed for selling. MQL to SQL conversion 
improvement. Pipeline velocity improvement from faster triage.

A system with high technical accuracy that reps do not trust 
and do not use is a failed product. Adoption is the 
first-order metric. Technical accuracy is second-order.

---

## Iteration Roadmap

**v2.0** Replace mock components with production equivalents. 
DuckDB to BigQuery. Local CSV to Fivetran pulling from 
Salesforce. ChromaDB to Pinecone. Zero code changes required.

**v2.1** SDR feedback loop. Thumbs up or down on each Slack 
alert becomes a retraining signal. The system improves as 
reps use it.

**v2.2** Explainability layer. Full signal breakdown on every 
routing decision so reps understand why a lead was scored and 
routed the way it was. Explainability drives trust. Trust 
drives adoption.

**v3.0** Multi-tenant ICP configuration. Each sales team 
defines their own scoring criteria and routing rules without 
code changes.

---

## Technical Implementation

| Component | Technology | Why |
|-----------|------------|-----|
| Data warehouse | DuckDB | Zero latency querying. Maps to BigQuery or Snowflake in production with no architecture changes |
| Vector search | ChromaDB | Semantic retrieval over call transcripts finds buying signals regardless of exact keyword match |
| AI reasoning | Gemini 2.5 Flash via CrewAI | Sequential multi-agent orchestration with each agent building on the previous output |
| Guardrails | Deterministic rules engine | Hard caps and floors ensure AI reasoning cannot produce catastrophically wrong routing decisions |
| API layer | FastAPI | Production-ready REST endpoints. Any external platform can trigger the full pipeline via webhook |
| Data transformation | dbt | Version-controlled SQL models with full lineage and auditability |

**Five layer architecture**

Layer 1  Data ingestion. CRM data and call transcripts into DuckDB.
Layer 2  Context layer. Transcript embeddings into ChromaDB.
Layer 3  Lead scoring. AI reasoning plus deterministic rules override.
Layer 4  Agentic routing. Three sequential CrewAI agents.
Layer 5  Automation. Rep assignment, CRM update, Slack alert via REST API.


**Tech stack**

Python · DuckDB · ChromaDB · CrewAI · Gemini 2.5 Flash · 
FastAPI · sentence-transformers · dbt · HMAC-SHA256

---

## Feature Prioritization

I used RICE scoring to decide what to build in v1.0 vs. defer.
RICE = Reach x Impact x Confidence divided by Effort.
Each dimension scored 1 to 3. Higher score = higher priority.

| Feature | Reach | Impact | Confidence | Effort | RICE Score | Decision |
|---------|-------|--------|------------|--------|------------|----------|
| Semantic enrichment and retrieval | 3 | 3 | 3 | 2 | 90 | v1.0 |
| Hybrid AI and deterministic scoring | 3 | 3 | 3 | 2 | 88 | v1.0 |
| REST API and webhook endpoint | 2 | 3 | 3 | 1 | 85 | v1.0 |
| Three-agent routing architecture | 3 | 3 | 2 | 3 | 75 | v1.0 |
| Real-time CRM API integration | 3 | 3 | 2 | 3 | 65 | v2.0 |
| SDR feedback loop | 2 | 3 | 2 | 2 | 58 | v2.1 |
| Explainability layer | 3 | 3 | 1 | 2 | 52 | v2.2 |
| Multi-tenant ICP configuration | 1 | 2 | 2 | 3 | 28 | v3.0 |

**Why enrichment came first**

Data quality is the dependency for everything downstream. A 
scoring model fed dirty data produces wrong scores. Wrong 
scores produce wrong routing. Wrong routing loses revenue and 
erodes rep trust. There was no logical starting point other 
than getting the data layer right first.

**Why real-time CRM API was deferred**

High effort and the entire architecture could be validated 
with CSV input first. Failing fast on controlled data is 
cheap. Failing on live Salesforce data is not.

**Why the feedback loop was deferred**

You cannot build a meaningful retraining loop until you have 
real user behavior data. Building it before shipping means 
optimizing for a signal that does not yet exist.

**Why explainability was deferred**

Ship the core routing first, observe which decisions reps 
override and why, then design the explainability layer around 
the specific questions reps are actually asking. Building it 
before you have user feedback means guessing at what reps 
need to see.

---

## Retrospective

**Talk to SDRs before defining scoring criteria**

I made assumptions about what high intent looks like in a 
call transcript. In a real product cycle I would interview 
5 to 10 SDRs first to validate that the signals I am 
detecting actually predict conversion in their specific 
context. Assumptions are a starting point. User research 
is how you validate them.

**Add observability from day one**

I instrumented logging after the core architecture was built. 
The right approach is structured observability from the first 
commit so you can measure latency per layer, score 
distributions, and override rates before you have users, 
not after. You cannot improve what you cannot measure.
