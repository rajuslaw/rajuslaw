<!--
HOW TO USE
1. Create a PUBLIC repo named EXACTLY your GitHub username → it renders on your profile.
2. Put this README.md at the root. Replace every <PLACEHOLDER>.
3. SANITIZE: these are employer projects. Before publishing, remove internal/vendor
   names (SAP Ariba, PeopleSoft, INVTR, the specific vendor APIs) and confirm nothing
   breaches your NDA. The architecture and your decisions are yours to talk about;
   proprietary identifiers are not. Numbers below are from your decks — keep only what
   you're cleared to share, mark estimates as "target" if not yet measured in prod.
-->

<h1 align="center">Hi, I'm <NAME> 👋</h1>

<p align="center">
  <b>Principal Engineer — GenAI · Agentic AI · RAG · ML Systems</b><br>
  I architect and lead production LLM systems: enterprise RAG over fragmented data,
  document-intelligence pipelines, and autonomous agents — with the evals, guardrails,
  and resiliency to run them reliably.
</p>

<p align="center">
  <a href="https://linkedin.com/in/<HANDLE>">LinkedIn</a> ·
  <a href="<BLOG_OR_SITE_URL>">Writing</a> ·
  <a href="mailto:rajuslaw@gmail.com">Email</a>
</p>

---

## 🧭 About

Principal/Staff-level engineer currently **leading** GenAI systems in enterprise supply-chain
and finance operations. I work end to end — data ingestion, retrieval architecture, agent
design, guardrails, failover, and cost/latency optimization — and I lead the teams that ship them.

- 🔭 Leading: an AI contract-intelligence agent giving procurement teams a single source of truth
- 🛠️ Focus: RAG, agentic workflows, document AI (OCR + LLM), evals, guardrails, LLM infra
- 🧠 I optimize for **trade-offs and measured results**, not demos

---

## 🚀 Selected work

> Case studies below are sanitized summaries of production systems I've led. Internal/vendor
> names removed. Architecture diagrams and writeups: see the linked repos/posts.

### 🔍 [Contract Intelligence System](./contract-intelligence.md) — Enterprise RAG agent *(Lead)*
A natural-language agent that unifies contract data fragmented across four enterprise systems
(contracts, ERP/payments, invoicing, regional contracts) so procurement can ask *"What discount
does Vendor A offer on electronics?"* and get cited answers in seconds.

**Architecture I designed & led:**
- **Two-flow design** — batch ingestion (S3 raw zone → ETL) separated from the query pipeline.
- **ETL:** metadata extraction → PII scan/mask (tokenization) → classification → MD5-hash dedup → change-data-capture that keeps the vector store in sync on update/delete.
- **Hybrid retrieval in parallel:** keyword (Postgres metadata, exact) + semantic (vector DB with **RBAC/region/vendor metadata pre-filtering at query time**), merged through a **self-hosted cross-encoder reranker** (10–50ms, no API cost).
- **Guardrails:** input PII/security checks; **conditional context-grounding** (cosine-sim hallucination check that runs only when reranker confidence < 95%, saving 200–400ms on confident answers); output PII + secret-leak validation.
- **LLM routing via LiteLLM:** primary model with same-provider and cross-provider failover; tested across models on an eval set.
- **Resiliency:** cache fallback (hash-based invalidation, TTL), weekly data-quality reconciliation across store/index/metadata, full audit logging & telemetry.

`Python` · `RAG` · `Postgres` · `Pinecone` · `cross-encoder reranking` · `LiteLLM` · `Presidio` · `Kubernetes`

### 🧾 [AI-Powered Invoice Extraction](./invoice-extraction.md) — Document intelligence *(Lead)*
OCR + LLM pipeline that auto-populates invoice fields in an internal invoicing app, replacing
slow, error-prone manual entry. LLMs (not regex) handle the wide variety of vendor formats.

- **Phase 1:** user uploads invoice → API extracts invoice #, date, amount, PO date → auto-fills form for verification.
- **Phase 2:** background job monitors a shared mailbox → extracts attachments → creates draft records → routes to the team for review.
- **Targets:** ≥90% auto-fill accuracy · ≥60% faster record creation · ≥50% fewer rejections · ≥30% of records drafted from email.

`OCR` · `LLM extraction` · `document AI` · `human-in-the-loop` · `Python`

### 🤖 [Solution Center Automation](./solution-center-automation.md) — Multi-system agent *(Lead)*
An automation agent that reads inbound employee emails, detects intent, routes to the right
backend (travel/expense, corporate card, invoicing), executes the action via API or UI
automation, and replies with a professional, generated response.

- Email + intent detection & entity extraction → LLM/embedding-based **routing** → action execution (REST APIs where available, headless browser automation where not) → LLM-composed reply.
- Designed for secure delegated access (service accounts, secrets management, SSO).

`agents` · `tool-use / routing` · `LangChain` · `Playwright` · `Microsoft Graph` · `Python`

---

## 🌱 Open source
<!-- Land 2–3 real merged PRs (LangChain, LlamaIndex, RAGAS, Presidio, LiteLLM are natural fits given your stack). Delete this section until you have one. -->
- [<repo> #<PR>](<URL>) — <one-line description>

## 📝 Writing
<!-- Turn each case study's hardest problem into one post. Strong principal-level talking points. -->
- [Conditional grounding: cutting hallucination-check latency without losing safety](<URL>)
- [Designing hybrid RAG with query-time RBAC filtering](<URL>)
- [LLM router failover patterns with LiteLLM](<URL>)

---

## 🧰 Tech

**GenAI/LLM:** RAG (hybrid + reranking) · agents & tool-use · document AI (OCR+LLM) · evals · guardrails (Presidio) · prompt/context engineering
**Infra:** LiteLLM routing/failover · vector DBs (Pinecone) · Postgres · caching/CDC · Kubernetes
**Lang/Tools:** Python · LangChain · Playwright · Microsoft Graph · Docker · CI/CD · observability

---

<p align="center"><i>Open to Principal / Staff AI Engineering roles · rajuslaw@gmail.com</i></p>
