# [Challenge] Senior AI Engineer — Industrial Document Intelligence

## 📄 Context

Maintenance technicians work from equipment manuals: long PDFs full of tables, diagrams and text in several languages. Some of them were produced by tools that don't follow the rules. When a technician is standing next to a machine, a wrong answer is worse than no answer.

We want a service that lets teams **upload their manuals** and **ask questions about them**, getting answers they can **trust and verify**.

This is not a feature list. It's a product problem. We describe **who uses the system and what they need**. Turning that into a production-minded architecture, including the concerns nobody wrote down for you, is the job. The architecture is yours: components, data flow, API shape, storage, models, and libraries. **We care as much about *why* you chose something as about whether it works.**

**Build an MVP**: a working system that gets the core right. Then explain your **architecture choices**, and **what is missing and what you would do next** to take it to production. Deciding what matters most is part of the test.

**Expected effort: ~8 hours. Please don't go beyond that.** You won't be able to do everything, and we don't expect you to. When you hit the limit, stop. We grade your decisions, not the volume of code.

You may use any AI coding assistant. In the follow-up interview, you will walk us through your solution, your architecture decisions, and your next steps to make it production-ready.

**This test must be coded in Python and its libraries.** Do not use tools such as N8N, LangChain, LangGraph, Snowflake, or Databricks. Any LLM / embedding provider is fine (list the env vars and we'll provide keys).

---

## 📁 Provided material

| Path | Description |
|---|---|
| `case_files/*.pdf` | 4 real equipment manuals (EN / PT / ES). Treat them as representative of production data. |
| `challenge/eval/public_questions.jsonl` | Sample questions with expected answers and sources. We will grade with a larger hidden question set and additional documents of the same kind. |

---

## 👥 Use cases

**UC1 — Onboarding a plant's documentation.**
A reliability engineer onboards a new site and uploads its documentation in one go: dozens of manuals, from a 2-page leaflet to a 500-page catalog. Some are scans. Some may be corrupted, password-protected or not PDFs at all. One broken file must not ruin the rest of the upload.

**UC2 — Asking a question in the field.**
A technician asks a question in their own language, e.g. *"How much grease does a frame 324 motor take?"*. The manual may be written in another language. They need:
- a direct answer
- the exact source (which document, which page, what it literally says) so they can double-check it
- an honest *"the documents don't answer this question"* when that's the case. Never a plausible guess from general knowledge.
- when the answer depends on something they didn't say (which model? which speed?), the system must make that clear instead of picking one

**UC3 — Operating it.**
When something goes wrong (a document failed, an answer was wrong, the LLM provider is down, latency spiked), an on-call engineer needs to find out what happened for that specific request or document without guessing.

---

## 🚀 Part A — Coding

Build a working system that serves the use cases above. Some minimum requirements:

1. **An HTTP API** for UC1 and UC2. You design the endpoints, payloads, status codes and error format.
2. **Answers** carry citations (document, page, verbatim quote) and make it clear whether the system answered, couldn't find the answer, or needs clarification.
3. **An eval harness** (`make eval` or equivalent) that takes a folder of PDFs and a JSONL file in the format of `public_questions.jsonl`, ingests the PDFs through your API, runs the questions and prints a report. You decide which metrics matter and how to compute them reliably. Adding your own questions is optional. Commit your latest report with comments on the failures you didn't fix.

Everything else (internal architecture, components, how work flows, what gets stored where) is your decision.

---

## 🧠 Part B — Plan the system design (written)

Consider the following scenario for taking your MVP to production scale.

**Scenario:** the service becomes the document assistant for **2,000 industrial customers** (multi-tenant). Every day it ingests **~40,000 PDFs** (5–500 pages each, ~15% scanned or broken). Uploads come in bursts when a customer onboards a whole plant. Questions peak at **~300/s**, and users expect an answer in **under 4 s (p95)**.

What would it take to extend your application to handle it? Start with what is missing from your MVP, **in priority order and why that order**. Then explain how the system handles the scenario. Cover at least ingestion at scale, reliability, cost, and security.

Answer in `DESIGN.md`. Diagrams are welcome. Aim for ~1–2 pages. Depth beats length.

---

## 📦 Deliverables

A Git repository with:

- the implementation, with `docker compose up` bringing up everything needed and `make eval` working, both from a clean clone
- `README.md`: setup, env vars, example requests/responses, and an **architecture overview with a diagram**
- `DECISIONS.md`:
  - the decisions that matter. For each one: the problem, the options you considered, what you chose and **why**, and what would make you change your mind
  - what you deliberately cut, and the known limitations
- `DESIGN.md`: Part B, your prioritized next steps and your plan for production scale
- your eval report (Part A, item 3)

---

## ✅ Evaluation criteria

| Area | Weight | What we look for |
|---|---|---|
| **Answer quality** | 30% | Correct answers, valid citations, honest "not found" / "needs clarification", cross-language questions |
| **Document understanding** | 20% | Messy data detected and handled, tables kept usable, the right content retrieved |
| **Evaluation** | 10% | Meaningful metrics, honest reporting |
| **Architecture & reliability** | 20% | How well the system holds up under real production conditions you identified yourself, and clarity of the *why* |
| **Design under load (Part B)** | 15% | System design aware of tradeoffs and failure modes |
| **Engineering craft** | 5% | Code quality, API design, error semantics, observability, developer experience |

A **30-minute follow-up interview** follows: an architecture walkthrough and a deep-dive on your decisions.

