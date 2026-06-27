<h1 align="center">Hi, I'm Raju Singh 👋</h1>

<p align="center">
  <b>Principal Engineer — GenAI · Agentic AI · RAG · ML Systems</b><br>
  I architect and lead production LLM systems: enterprise RAG over fragmented data,
  document-intelligence pipelines, and autonomous agents — with the evals, guardrails,
  and resiliency to run them reliably.
</p>

<p align="center">
  <a href="mailto:rajuslaw@gmail.com">Email</a>
  <!-- Add when ready: LinkedIn, personal site/blog -->
</p>

---

## 🧭 About

Principal/Staff-level engineer **leading** GenAI systems in enterprise supply-chain and finance
operations. I work end to end — data ingestion, retrieval architecture, agent design, guardrails,
failover, and cost/latency optimization — and I lead the teams that ship them.

- 🔭 Leading: an AI contract-intelligence agent giving procurement teams a single source of truth
- 🛠️ Focus: RAG, agentic workflows, document AI (OCR + LLM), evals, guardrails, LLM infra
- 🧠 I optimize for **trade-offs and measured results**, not demos

---

## 🚀 Selected work

> Sanitized summaries of production systems I've led. Internal/vendor names generalized; the
> architecture and engineering decisions are my own. Click through for full case studies with
> architecture diagrams.

### 🔍 [Contract Intelligence System](./contract-intelligence.md) — Enterprise RAG agent *(Lead)*
A natural-language agent that unifies contract data fragmented across four enterprise systems
(contracts, ERP/payments, invoicing, regional contracts) so procurement can ask *"What discount
does Vendor A offer on electronics?"* and get cited answers in seconds.

- **Two-flow design** — batch ingestion (raw landing zone → ETL) separated from the query pipeline.
- **ETL:** metadata extraction → PII scan/mask (tokenization) → classification → content-hash dedup → change-data-capture that keeps the vector store in sync on update/delete.
- **Hybrid retrieval in parallel:** keyword (Postgres metadata, exact) + semantic (vector DB with **RBAC/region/vendor metadata pre-filtering at query time**), merged through a **self-hosted cross-encoder reranker** (10–50ms, no API cost).
- **Guardrails:** input PII/security checks; **conditional context-grounding** (cosine-sim hallucination check that runs only when reranker confidence < 95%, saving 200–400ms on confident answers); output PII + secret-leak validation.
- **LLM routing via LiteLLM:** primary model with same-provider and cross-provider failover.
- **Resiliency:** cache fallback (hash-based invalidation), weekly data-quality reconciliation, full audit logging & telemetry.

`Python` · `RAG` · `hybrid retrieval` · `cross-encoder reranking` · `vector DB` · `Postgres` · `LiteLLM` · `Presidio` · `Kubernetes`

### 🧾 [AI-Powered Invoice Extraction](./invoice-extraction.md) — Document intelligence *(Lead)*
OCR + LLM pipeline that auto-populates invoice fields in an internal invoicing app, replacing
slow, error-prone manual entry. LLMs (not regex) handle the wide variety of vendor formats.

- **Phase 1:** user uploads invoice → API extracts invoice #, date, amount, PO date → auto-fills form for verification.
- **Phase 2:** background job monitors a shared mailbox → extracts attachments → creates draft records → routes to the team for review.
- **Targets:** ≥90% auto-fill accuracy · ≥60% faster record creation · ≥50% fewer rejections.

`OCR` · `LLM extraction` · `document AI` · `human-in-the-loop` · `Python`

### 🤖 [Solution Center Automation](./solution-center-automation.md) — Multi-system agent *(Lead)*
An automation agent that reads inbound employee emails, detects intent, routes to the right
backend (travel/expense, corporate card, invoicing), executes the action via API or UI
automation, and replies with a professional, generated response.

- Email + intent detection & entity extraction → LLM/embedding-based **routing** → action execution (REST APIs where available, headless browser automation where not) → LLM-composed reply.
- Designed for secure delegated access (service accounts, secrets management, SSO).

`agents` · `tool-use / routing` · `LangChain` · `Playwright` · `Microsoft Graph` · `Python`

---

## 🧰 Tech

**GenAI/LLM:** RAG (hybrid + reranking) · agents & tool-use · document AI (OCR+LLM) · evals · guardrails (Presidio) · prompt/context engineering
**Infra:** LiteLLM routing/failover · vector DBs · Postgres · caching/CDC · Kubernetes
**Lang/Tools:** Python · LangChain · Playwright · Microsoft Graph · Docker · CI/CD · observability

---

## 🌱 Open source

_Targeting contributions to the production tools I work with — LiteLLM, Microsoft Presidio,
RAGAS. Merged PRs will be linked here._

## 📝 Writing

Technical deep-dives in progress, drawn from the systems above:

- Conditional grounding: cutting hallucination-check latency without losing safety
- Hybrid RAG with query-time RBAC filtering (why post-retrieval filtering is wrong)
- LLM router failover patterns with LiteLLM

---

<p align="center"><i>Open to Principal / Staff AI Engineering roles · rajuslaw@gmail.com</i></p>
