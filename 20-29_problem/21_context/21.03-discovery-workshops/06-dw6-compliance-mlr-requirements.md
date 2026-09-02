# DW-6 - Compliance / MLR Requirements Discovery Workshop [Gap-fill]

**ClickUp:** 86d46978z | **Priority:** Gap-fill | **Duration:** 90 min
**Owner:** Cevo (discovery) + Novartis MLR | **Feeds:** `22_constraints/`, CI review, OBJ.006

> **Note:** Recommended gap-fill (not in the original request). The CI spec flags compliance discovery as the checkpoint most likely to force a Guiding Principles / Scope Boundary revision, because regulatory findings tend to be hard boundaries rather than preferences. Drop this session if MLR is handled outside the BA track. Treat any finding here as a flag to raise with the client, not a quiet edit.

---

## Objective

Discover Medical, Legal & Regulatory (MLR) compliance requirements so the governance-first architecture (OBJ.006) satisfies pharma compliance obligations for AI-generated promotional content.

## Story

As the BA/SA, I want to discover MLR compliance requirements so that the governance-first architecture satisfies pharma compliance obligations.

## Attendees

- Cevo: BA (facilitator), SA
- Novartis: MLR reviewers (medical, legal, regulatory), compliance lead, promotional review owner

## Pre-reads

- Journey map (`21.02`) - the COMPLIANCE phase and the "compliance as a bottleneck" pain
- Scope Boundary (`21.01d`) - no base-model training on Novartis data
- Problem statement (`21.01a`) - compliance reviewer persona

---

## Opening frame

> "Compliance is where a lot of the current pain sits - review lands late and rework stretches timelines. We want to understand the MLR process precisely so the agents help rather than create new compliance exposure. We're especially interested in where AI-generated content changes your review, and what has to be non-negotiable. If something we're planning would cross a regulatory line, this is the session to tell us."

---

## Question blocks

Prompts in the section intros are conversation openers; work through the table questions to land the specifics. Fill Response and Status live.
Status legend: Open / Answered / Parked.

### 1. MLR review and approval workflow

*Walk me through the MLR review as it works today, stage by stage.*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW6-01 | What are the stage-gates a piece of promotional content passes through? | Map the MLR workflow | | Open |
| DW6-02 | Who reviews at each gate (medical / legal / regulatory) and in what order? | Identify reviewers and sequence | | Open |
| DW6-03 | What triggers rework, and how often does it happen? | Locate rework drivers | | Open |
| DW6-04 | Where in the cycle does review currently land, and why is that a problem? | Understand the late-review bottleneck | | Open |

### 2. Regulatory obligations for AI-generated content

*What changes when the content was drafted by an AI rather than a person or agency?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW6-05 | Are there specific obligations or restrictions on AI-generated promotional content? | Capture AI-specific regulatory rules | | Open |
| DW6-06 | Does AI involvement need to be disclosed or recorded anywhere? | Determine disclosure requirements | | Open |
| DW6-07 | Are there content categories where AI drafting is not acceptable at all? | Identify hard exclusions | | Open |
| DW6-08 | What codes/regulations apply (e.g. local promotional codes, therapeutic advertising rules)? | Source the governing regulations | | Open |

### 3. Auditability and record-keeping

*What has to be logged and kept, and for how long?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW6-09 | What must be recorded about a query, a generation, and an approval (who, what, when)? | Define audit record content | | Open |
| DW6-10 | Are query and result logs a hard requirement? Retention period? | Confirm logging obligation and retention | | Open |
| DW6-11 | What audit evidence would a regulator or internal audit expect to see? | Define the audit evidence bar | | Open |

### 4. Human-in-the-loop review

*Where must a qualified human review before anything moves forward?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW6-12 | Which steps require mandatory human sign-off? | Locate mandatory review points | | Open |
| DW6-13 | What must the human be able to see to sign off responsibly (sources, citations, change history)? | Define reviewer information needs | | Open |
| DW6-14 | Can AI pre-check content against rules before human review to shift-left the effort? | Explore shift-left opportunity | | Open |

### 5. Constraints on training and data use

*What are the hard lines on using Novartis data with these models?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW6-15 | Confirm: no base-model training on Novartis data (fine-tuning on approved copy in scope)? | Confirm the data-use boundary | | Open |
| DW6-16 | Any constraints on which data can be sent to which model or region? | Capture data-routing constraints | | Open |
| DW6-17 | Any constraints on retaining prompts/outputs? | Capture retention constraints | | Open |

---

## What we must leave with (acceptance criteria)

- [ ] MLR review/approval workflow and stage-gates documented
- [ ] Regulatory obligations for AI-generated promotional content captured
- [ ] Auditability & record-keeping (query/result logging) requirements defined
- [ ] Human-in-the-loop review requirements identified
- [ ] Constraints on training/fine-tuning with Novartis data confirmed

---

## CI checkpoint

At the end, ask explicitly: *does anything we heard force a revision to Guiding Principles (`21.01c`) or Scope Boundary (`21.01d`)?* Flag any hard boundary to the client directly.

## Capture template

| Area | Requirement / obligation | Hard boundary? | Owner | Q/ASM/DEC |
|---|---|---|---|---|
| MLR workflow / gates | | | | |
| AI-content obligations | | | | |
| Auditability / logging | | | | |
| Human-in-the-loop | | | | |
| Data / training constraints | | | | |

---

*Status: Draft facilitation guide*
*Last updated: 2026-08-31*
