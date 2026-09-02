# DW-9 - AI Governance & Responsible AI Discovery Workshop [Gap-fill]

**ClickUp:** 86d4697bp | **Priority:** Gap-fill | **Duration:** 90 min
**Owner:** Cevo (discovery) + Novartis | **Feeds:** `22_constraints/`, CI Guiding Principles, OBJ.006 / OBJ.007

> **Note:** Recommended gap-fill (not in the original request). This is AI-specific governance, distinct from generic security (DW-2) and MLR compliance (DW-6). If Novartis treats it as part of Security or Compliance, fold the questions into those sessions and drop this one.

---

## Objective

Discover AI-specific governance requirements so the PoC meets Novartis's Responsible AI expectations - model governance, human-in-the-loop, observability, and content-safety for regulated content.

## Story

As the SA, I want to discover AI-specific governance requirements (distinct from generic security) so that the PoC meets Novartis's Responsible AI expectations.

## Attendees

- Cevo: SA (facilitator), BA
- Novartis: AI governance / Responsible AI lead, enterprise architecture, data science/ML contact if one exists, risk representative

## Pre-reads

- Scope Boundary (`21.01d`) - no base-model training on Novartis data; fine-tuning on approved copy in scope
- Guiding Principles (`21.01c`) and Tensions (`21.01g`)
- DW-2 Security and DW-6 Compliance readouts if already run (avoid re-asking)

---

## Opening frame

> "This session is specifically about Responsible AI - the governance that applies because there are models in the loop, on top of your normal security and compliance rules. We want to understand which models are acceptable, where humans must stay in control, how we prove the AI is behaving, and what content-safety expectations apply given this is regulated pharma content."

---

## Question blocks

Prompts in the section intros are conversation openers; work through the table questions to land the specifics. Fill Response and Status live.
Status legend: Open / Answered / Parked.

### 1. Model governance and approved model set

*Which models are we allowed to use, and how is that decided?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW9-01 | Is there an approved/allow-listed set of models (e.g. Bedrock / Claude)? | Fix the permitted model set | | Open |
| DW9-02 | Who approves a new model for use, and how long does that take? | Understand model approval process and lead time | | Open |
| DW9-03 | Are there constraints on model hosting/region? | Capture hosting/residency constraints | | Open |
| DW9-04 | Any prohibited model providers or deployment patterns? | Identify hard exclusions | | Open |

### 2. Data-use boundary

*Let's nail down exactly what the models can and can't do with Novartis data.*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW9-05 | Confirm the boundary: no training of the base model on Novartis data. | Confirm the core governance boundary | | Open |
| DW9-06 | Confirm what is in scope: fine-tuning on approved copy - any conditions on that? | Confirm permitted fine-tuning scope | | Open |
| DW9-07 | Constraints on prompts/outputs being retained or used for model improvement? | Capture retention/reuse constraints | | Open |
| DW9-08 | Any constraint on sending data to a third-party model endpoint at all? | Test the external-endpoint boundary | | Open |

### 3. Human-in-the-loop approval points

*Where must a human approve before the AI's work proceeds?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW9-09 | Which agent actions require human approval before they take effect (especially UC-5 content generation)? | Locate mandatory approval points | | Open |
| DW9-10 | Who is the accountable human at each point? | Assign accountability | | Open |
| DW9-11 | What must they see to approve responsibly (sources, confidence, diffs)? | Define reviewer information needs | | Open |
| DW9-12 | Do these align with the MLR human-in-the-loop points from DW-6? | Reconcile with compliance to avoid duplication | | Open |

### 4. Observability, evaluation and acceptance thresholds

*How do we prove the AI is doing what it should, and know when it isn't?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW9-13 | What LLM observability is expected (logging of prompts, responses, tokens, latency)? | Define observability requirements | | Open |
| DW9-14 | What evaluation metrics matter (accuracy, groundedness, relevance)? | Choose evaluation metrics | | Open |
| DW9-15 | What acceptance thresholds must the PoC hit to pass (feeds OBJ.007 Go/No-Go)? | Set pass/fail thresholds | | Open |
| DW9-16 | How should the system flag low-confidence or out-of-policy output? | Define flagging behaviour | | Open |

### 5. Bias, fairness and content safety

*What are the expectations for safe, appropriate output in a regulated context?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW9-17 | What content-safety guardrails are required (toxicity, off-label claims, unapproved indications)? | Define required guardrails | | Open |
| DW9-18 | Any bias/fairness expectations relevant to the content and audiences? | Capture bias/fairness expectations | | Open |
| DW9-19 | What must never appear in generated content? | Define absolute content exclusions | | Open |
| DW9-20 | How should the system behave at the edge of acceptable content? | Define edge-case behaviour | | Open |

---

## What we must leave with (acceptance criteria)

- [ ] Model governance & approved model set documented (Bedrock/Claude)
- [ ] "No base-model training on Novartis data" boundary confirmed (fine-tuning on approved copy in scope)
- [ ] Human-in-the-loop approval points identified
- [ ] LLM observability, evaluation metrics, and acceptance thresholds defined
- [ ] Bias, fairness, and content-safety expectations for regulated content captured

---

## CI checkpoint

Ask explicitly: *does anything here update a Guiding Principle (`21.01c`) or a Tension (`21.01g`)?* Responsible-AI positions often resolve a Speed-vs-Control style tension - capture the position taken.

## Capture template

| Area | Requirement / position | Threshold (if any) | Owner | Q/ASM/DEC |
|---|---|---|---|---|
| Model governance | | | | |
| Data-use boundary | | | | |
| Human-in-the-loop | | | | |
| Observability / eval | | | | |
| Bias / content safety | | | | |

---

*Status: Draft facilitation guide*
*Last updated: 2026-08-31*
