# 🧾 AI-Powered Invoice Extraction — Document Intelligence

![Role](https://img.shields.io/badge/Role-Technical%20Lead%20%2F%20Principal-0A66C2)
![Domain](https://img.shields.io/badge/Domain-Finance%20Ops-444)
![Pattern](https://img.shields.io/badge/Pattern-OCR%20%2B%20LLM%20Extraction-1f6feb)
![HITL](https://img.shields.io/badge/Design-Human--in--the--Loop-2da44e)

> An OCR + LLM pipeline that **auto-populates invoice fields** in an internal invoicing
> application, replacing slow, error-prone manual data entry. Because vendors use wildly different
> invoice formats, LLMs — not brittle regex — interpret the fields.
>
> *Sanitized case study. Internal application and vendor-service names are generalized; the
> architecture and engineering decisions are my own.*

[← Back to profile](./README.md)

---

## Contents

- [1. The problem](#1-the-problem)
- [2. Why AI instead of traditional OCR](#2-why-ai-instead-of-traditional-ocr)
- [3. Architecture](#3-architecture)
- [4. Functional highlights](#4-functional-highlights)
- [5. Success metrics](#5-success-metrics)
- [6. Design decisions & trade-offs](#6-design-decisions--trade-offs)
- [7. Design FAQ](#7-design-faq)
- [My role](#my-role)

---

## 1. The problem

Manual entry of invoice metadata into the payment-request workflow was slow and mistake-prone:

- ~**30% of monthly payment-request-form (PRF) rejections** came from data mismatches.
- Rejections caused **payment delays and vendor escalations**.
- The team needed automation that improved **both accuracy and speed**.

---

## 2. Why AI instead of traditional OCR

- Vendors use varied invoice formats and inconsistent field labels.
- Plain OCR extracts text but **can't reliably identify which value belongs to which field** using
  regex.
- An **LLM understands context** and extracts structured data even from formats it hasn't seen
  before.

The pipeline therefore pairs OCR (read the pixels) with an LLM (interpret the fields), delivered
through a **third-party OCR + LLM extraction service**.

---

## 3. Architecture

```mermaid
flowchart TB
    subgraph Phase1[Phase 1 — Inline auto-fill]
        U1[User uploads invoice<br/>PDF / image] --> EX1[OCR + LLM<br/>extraction service]
        EX1 --> F1[Structured fields:<br/>invoice no, date, amount, PO date]
        F1 --> FORM[Auto-fill PRF form]
        FORM --> V1[User verifies & submits]
    end
    subgraph Phase2[Phase 2 — Email-based draft]
        MBX[Shared mailbox] --> JOB[Background job<br/>monitors inbox]
        JOB --> AT[Extract invoice attachment]
        AT --> EX2[OCR + LLM<br/>extraction service]
        EX2 --> DRAFT[Create draft PRF]
        DRAFT --> NOTIFY[Notify invoicing team for review]
    end
```

### Phase 1 — Inline PRF auto-fill
1. The user uploads an invoice PDF/image in the app.
2. The file is sent to the extraction service (OCR + LLM).
3. The service returns structured fields: invoice number, date, amount, PO date.
4. Fields are auto-filled into the PRF form **for human verification** before submission.

### Phase 2 — Email-based draft PRF
1. A background job monitors a shared mailbox.
2. It extracts the invoice attachment and sends it to the extraction service.
3. A **draft PRF** is created in the invoicing application.
4. A notification email routes it to the invoicing team for review.

Both phases keep a **human in the loop** — the AI accelerates, the human approves. Invoice errors
are expensive, so approval authority stays with people.

---

## 4. Functional highlights

- PRF auto-fill from an uploaded invoice
- Draft PRF creation from inbound email
- Invoicing-team review workflow built in
- Model handles diverse vendor formats without per-vendor rules
- Improves both processing speed and data accuracy

---

## 5. Success metrics

| Metric | Target |
|---|---|
| Auto-fill accuracy | ≥ 90% |
| Reduction in PRF creation time | ≥ 60% |
| Drop in rejections | ≥ 50% |
| Draft PRFs created from email | ≥ 30% |

> These were the program's success criteria. *(Replace with measured production numbers once
> cleared to share — a real figure here is worth a lot.)*

---

## 6. Design decisions & trade-offs

| Decision | Choice | Why |
|---|---|---|
| Extraction approach | OCR + LLM (not regex) | Generalizes across unseen vendor formats |
| Human review | Always-on, both phases | Invoice errors are costly; keep approval with humans |
| Two entry points | Upload + shared mailbox | Meets users where they already work |
| Output | Structured fields into the existing PRF form | Drops into the current workflow; no user retraining |

---

## 7. Design FAQ

<details>
<summary><b>Why not just train a template per vendor?</b></summary>

Per-vendor templates don't scale — every new vendor or format change means new rules and ongoing
maintenance. An LLM generalizes across formats from context, so onboarding a new vendor needs no
new code.
</details>

<details>
<summary><b>How do you prevent bad extractions from causing wrong payments?</b></summary>

Human-in-the-loop by design. Phase 1 auto-fills but requires the user to verify and submit; Phase 2
creates only a *draft* PRF that the invoicing team reviews. The AI never finalizes a payment record
on its own.
</details>

<details>
<summary><b>How is accuracy measured?</b></summary>

Against the success criteria above — auto-fill accuracy, PRF creation time, and rejection rate —
tracked over the rollout and compared to the manual-entry baseline.
</details>

---

## My role

I **led** the design and delivery: the OCR + LLM extraction approach, the two-phase rollout (inline
auto-fill, then email-driven drafts), the human-in-the-loop review workflow, and the success-metric
framework.

**Stack:** `OCR` · `LLM extraction` · `document AI` · `human-in-the-loop` · `email automation` ·
`Python`

[← Back to profile](./README.md)
