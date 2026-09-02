# DW-3 - Risk Requirements Discovery Workshop

**ClickUp:** 86d46975d | **Priority:** Core | **Duration:** 60-90 min
**Owner:** Cevo (discovery) + Novartis | **Feeds:** `22_constraints/`, risk register

---

## Objective

Discover the client's risk requirements and posture for an AI PoC so that risks are identified, owned, and controlled - and so the Go/No-Go decision (OBJ.007) has clear sign-off gates.

## Story

As the BA, I want to discover the client's risk requirements and posture for an AI PoC so that risks are identified, owned, and controlled.

## Attendees

- Cevo: BA (facilitator), SA, delivery lead
- Novartis: risk owner, sponsor or delegate, compliance representative (for cross-over topics), delivery/PMO contact

## Pre-reads

- Tensions (`21.01g`) and Assumptions Register (`21.01h`)
- Scope Boundary (`21.01d`) - grey zone items, especially "source data not API-ready"
- Success Criteria (`21.01e`)

---

## Opening frame

> "This is a proof-of-concept, so some risk is expected and acceptable - but we want to be explicit about which risks Novartis is comfortable carrying, which need controls, and who owns each one. We also want to agree what would make this a No-Go at the end. This session builds the risk register and the sign-off gates, not a solution."

---

## Question blocks

Prompts in the section intros are conversation openers; work through the table questions to land the specifics. Fill Response and Status live.
Status legend: Open / Answered / Parked.

### 1. Risk appetite for generative AI

*How much tolerance is there for imperfect AI output in a PoC context?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW3-01 | What is the risk appetite for generative AI outputs - conservative, moderate, exploratory? | Set the tolerance the design must respect | | Open |
| DW3-02 | Does appetite differ between internal-only use and anything customer/HCP-facing? | Distinguish tolerance by exposure | | Open |
| DW3-03 | What kind of AI error is simply unacceptable versus tolerable-and-corrected? | Define the line between fatal and recoverable errors | | Open |
| DW3-04 | Is there an existing enterprise risk appetite statement we should align to? | Reuse an existing standard rather than invent one | | Open |

### 2. Risk register, ownership and escalation

*How does Novartis expect risks to be tracked and escalated on this engagement?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW3-05 | Do you have a standard risk register format we should use or feed? | Align to existing tracking | | Open |
| DW3-06 | Who owns the risk register - Cevo, Novartis, or shared? | Assign clear register ownership | | Open |
| DW3-07 | What are the escalation thresholds and paths (when does a risk go to the sponsor)? | Define escalation triggers | | Open |
| DW3-08 | How often should risks be reviewed during the PoC? | Set a review cadence | | Open |

### 3. Accuracy and hallucination controls

*Regulated content can't tolerate confident-but-wrong output - how do we control that?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW3-09 | What accuracy bar must AI-generated content meet before a human sees it? | Set a pre-review quality threshold | | Open |
| DW3-10 | What controls are expected against hallucination (grounding/RAG, citations, confidence signals)? | Capture required accuracy controls | | Open |
| DW3-11 | Where is human-in-the-loop mandatory versus optional? (cross-ref DW-6, DW-9) | Define mandatory review points | | Open |
| DW3-12 | How should the system behave when it is uncertain? | Define fallback behaviour under uncertainty | | Open |

### 4. Risk categories to log

*Let's walk the risk landscape beyond just the AI output.*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW3-13 | Reputational risks (brand, HCP trust, regulatory perception)? | Log reputational exposure | | Open |
| DW3-14 | Operational risks (system access, integration failure, data quality - link to "not API-ready")? | Log operational exposure | | Open |
| DW3-15 | Delivery risks (timeline, capacity, dependency on Novartis access provisioning)? | Log delivery exposure | | Open |
| DW3-16 | Data/privacy risks? | Log data and privacy exposure | | Open |

### 5. Go/No-Go sign-off gates

*What has to be true at the end for this to be judged a success worth scaling?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW3-17 | Who are the decision-makers for the Go/No-Go (OBJ.007)? | Identify who signs off | | Open |
| DW3-18 | What risk-related criteria must be satisfied to get sign-off? | Define the risk gate for Go/No-Go | | Open |
| DW3-19 | Are there any hard stops - findings that would end the PoC early? | Surface early-termination triggers | | Open |

---

## What we must leave with (acceptance criteria)

- [ ] Risk appetite/tolerance for generative AI outputs captured
- [ ] Risk register expectations, ownership, and escalation defined
- [ ] Accuracy/hallucination controls for regulated content identified
- [ ] Reputational, operational, and delivery risks logged
- [ ] Risk sign-off gates for Go/No-Go (OBJ.007) understood

---

## Capture template

| Risk ID | Description | Category | Likelihood | Impact | Owner | Control / mitigation | Escalation trigger |
|---|---|---|---|---|---|---|---|
| | | | | | | | |

Plus: risk appetite statement, register ownership/cadence decision (`DEC-nnn`), Go/No-Go criteria list.

---

*Status: Draft facilitation guide*
*Last updated: 2026-08-31*
