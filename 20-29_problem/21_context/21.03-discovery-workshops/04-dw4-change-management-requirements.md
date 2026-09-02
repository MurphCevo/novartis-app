# DW-4 - Change Management Requirements Discovery Workshop

**ClickUp:** 86d46976c | **Priority:** Core | **Duration:** 60-90 min
**Owner:** Cevo (discovery) + Novartis | **Feeds:** `11_governance/`, `22_constraints/`

---

## Objective

Discover change management requirements across two dimensions: organisational adoption (will marketing users actually use this?) and technical change control (how are agent changes governed and deployed?). The aim is a PoC that is adoptable and whose changes are governed.

## Story

As the BA, I want to discover change management requirements (org adoption and technical change control) so that the PoC is adoptable and changes are governed.

## Attendees

- Cevo: BA (facilitator), SA (for CI/CD topics)
- Novartis: marketing team lead (adoption), platform/DevOps owner (technical change control), support/service owner, training/enablement contact

## Pre-reads

- Problem statement (`21.01a`) - personas and their pain
- Objectives summary (`11.01`) - OBJ.008 knowledge transfer
- Kickoff/readiness notes (`12_comms/2026-08-25-kickoff-and-readiness.md`)

---

## Opening frame

> "Two sides to this conversation. First, the human side - what will it take for the marketing team to trust and adopt these agents. Second, the technical side - how changes to the agents get reviewed, approved, and deployed safely. We want the PoC to land well with users and to have a clean change process behind it."

---

## Question blocks

Prompts in the section intros are conversation openers; work through the table questions to land the specifics. Fill Response and Status live.
Status legend: Open / Answered / Parked.

### 1. Support model and change control

*Once something is running, who looks after it and how do changes get made?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW4-01 | What is the expected support model for a PoC (who fields issues, what hours)? | Define who supports and when | | Open |
| DW4-02 | What is the existing change-control process for new tooling in marketing operations? | Map the current process to fit into | | Open |
| DW4-03 | Does the PoC need to fit an existing ITSM/change process, or can it run lighter as a PoC? | Determine governance weight | | Open |

### 2. Change approval and prioritisation

*When we want to change an agent's behaviour, how does that get approved?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW4-04 | Who approves changes to agent behaviour, prompts, or scope during the PoC? | Identify the change approver | | Open |
| DW4-05 | How is change prioritised when there are competing requests? | Understand prioritisation mechanism | | Open |
| DW4-06 | Is there a change advisory board or equivalent we need to engage? | Uncover formal governance bodies | | Open |

### 3. Adoption and readiness (marketing users)

*What makes the difference between marketers using this and quietly ignoring it?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW4-07 | How do the 2 brand managers and wider marketing team feel about AI in their workflow today? | Gauge baseline sentiment | | Open |
| DW4-08 | What has adoption of past tools looked like - what worked, what didn't? | Learn from prior adoption attempts | | Open |
| DW4-09 | What would build trust in AI-generated content among these users? | Identify trust levers | | Open |
| DW4-10 | Who are the likely champions and the likely sceptics? | Map the adoption stakeholder landscape | | Open |
| DW4-11 | How should we involve users during the PoC (demos, pilots, feedback loops)? | Plan the user-engagement cadence | | Open |

### 4. Training and knowledge transfer (OBJ.008)

*How do we leave Novartis able to run and understand this?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW4-12 | What training expectations exist for end users of the Autopilot Assistant? | Scope user training needs | | Open |
| DW4-13 | What technical knowledge transfer do internal teams need (architecture, operations, runbooks)? | Scope technical KT needs | | Open |
| DW4-14 | What format works best (docs, sessions, shadowing)? | Match delivery to audience preference | | Open |
| DW4-15 | Who receives the knowledge transfer, and when should it start? | Identify recipients and timing | | Open |

### 5. CI/CD for agent deployment

*How should agent changes flow from build to running safely?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW4-16 | Is there a standard CI/CD toolchain the PoC should use? | Fix the deployment toolchain | | Open |
| DW4-17 | What environments exist (dev/test/prod) and what gates sit between them? | Map the environment topology | | Open |
| DW4-18 | What testing/approval is required before an agent change is deployed? | Define pre-deployment gates | | Open |
| DW4-19 | Any rollback expectations? | Capture rollback requirements | | Open |

---

## What we must leave with (acceptance criteria)

- [ ] Support model & change-control process documented
- [ ] Change approval/prioritisation process captured
- [ ] Adoption & readiness needs for marketing users identified
- [ ] Training & knowledge-transfer expectations (OBJ.008) recorded
- [ ] CI/CD change process for agent deployment understood

---

## Capture template

| Area | Current state | PoC expectation | Owner | Q/ASM/DEC |
|---|---|---|---|---|
| Support model | | | | |
| Change approval | | | | |
| User adoption | | | | |
| Training / KT | | | | |
| CI/CD | | | | |

---

*Status: Draft facilitation guide*
*Last updated: 2026-08-31*
