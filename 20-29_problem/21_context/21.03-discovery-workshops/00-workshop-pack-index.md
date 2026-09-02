# Discovery Workshop Pack - Index

**Engagement:** Project Autopilot (Novartis multi-agentic AI PoC)
**Epic:** Discovery Workshops - Requirements Elicitation (ClickUp DW-1 to DW-9)
**Owner:** Cevo (BA/SA) + Novartis stakeholders
**Status:** Draft facilitation instruments - unverified until run and promoted to readouts

---

## Purpose

This pack contains the facilitation guides for the requirements-elicitation workshops that establish the discovery baseline for Project Autopilot. Each guide provides conversation prompts, structured questions, and a capture template so any facilitator can run the session consistently and produce a comparable readout.

These are working instruments, not deliverables. Outputs from each session are written up as discovery readouts and filed against the destinations noted below (`21_context/`, `22_constraints/`, `31_reqs/`), then fed into the Conceptual Integrity (CI) notes (`21.01x`).

---

## How to use these guides

1. **Before the session** - read the linked context (problem statement, scope boundary, CI notes) and pre-fill the attendee list. Send the objective and the "what we need to leave with" list to attendees in advance.
2. **During the session** - use the opening frame verbatim to set context. Work through the question blocks. Each block opens with a conversation prompt in *italics*; the questions underneath sit in a table (ID, Question, Purpose, Response, Status). Fill Response and Status live - Status is Open / Answered / Parked. Question IDs are per-workshop (e.g. `DW1-01`, `DW5-12`) so they can be referenced across the pack. Capture assumptions (`ASM`), open questions (`Q`), and decisions (`DEC`) as you go using the workspace numbering convention.
3. **After the session** - write a discovery readout (use the discovery-note format from conventions once ~3 sessions are done), file it to the destination noted, and flag whether anything forces a CI revision.

---

## Workshop set

| # | Workshop | ClickUp | Priority | Owner (Novartis side) | Primary feed |
|---|----------|---------|----------|-----------------------|--------------|
| DW-1 | Source Data Product Owner | 86d469738 | Core | System product owners | RACI, `21_context/`, OBJ.005 |
| DW-2 | Security Requirements | 86d46974g | Core | Security / IAM | `22_constraints/`, OBJ.006 |
| DW-3 | Risk Requirements | 86d46975d | Core | Risk / delivery lead | `22_constraints/`, risk register |
| DW-4 | Change Management Requirements | 86d46976c | Core | Ops / platform / marketing leads | `11_governance/`, `22_constraints/` |
| DW-5 | Marketing Requirements | 86d46977v | Core | Marketing team | `31_reqs/`, UC-4/5/10 |
| DW-6 | Compliance / MLR Requirements | 86d46978z | Gap-fill | MLR reviewers | `22_constraints/`, CI review, OBJ.006 |
| DW-7 | Source Data Deep-Dive | 86d46979y | Gap-fill | System SMEs / data eng | `21_context/`, `22_constraints/`, OBJ.005/DLV.005 |
| DW-8 | Process, Cost & Cycle-Time Baseline | 86d4697av | Gap-fill | Marketing ops | `21_context/`, `43_outcomes/`, SC (21.01e) |
| DW-9 | AI Governance & Responsible AI | 86d4697bp | Gap-fill | AI governance / architecture | `22_constraints/`, CI Guiding Principles, OBJ.006/007 |

Gap-fill workshops (DW-6 to DW-9) are recommended additions beyond the original request. Drop or fold into an adjacent session if handled elsewhere - see each guide's note.

---

## Suggested sequencing

Discovery has natural dependencies. A workable order:

1. **DW-5 Marketing** and **DW-8 Process Baseline** first - they frame the real user need and the as-is baseline everything else is measured against. Can run back to back with the same audience.
2. **DW-1 Source Data Product Owner** next - identifies who owns each system, which unlocks scheduling for the deep-dive.
3. **DW-7 Source Data Deep-Dive** - run per system once the PO is known. Can be run jointly with DW-1 where the PO is also the SME.
4. **DW-2 Security**, **DW-9 AI Governance**, **DW-6 Compliance/MLR** - the governance-first cluster. Run these once scope and data flows are understood so the constraints attach to something concrete.
5. **DW-3 Risk** and **DW-4 Change Management** - later, once enough is known to log real risks and design an adoption/change approach.

CI checkpoint note: DW-6 (Compliance/MLR) is the session most likely to force a Guiding Principles or Scope Boundary revision, because regulatory findings tend to be hard boundaries rather than preferences. Treat any such finding as a flag to the client, not a quiet edit.

---

## Shared facilitation kit

**Standard opening frame (adapt per session):**

> "Thanks for making time. Cevo is building a proof-of-concept for Project Autopilot - a set of AI agents that help the marketing team discover knowledge, generate compliant content, and tag assets. This session is about [TOPIC]. We're here to understand how things work today and what constraints we need to design around, not to propose a solution yet. There are no wrong answers - if something is messy or manual today, that's exactly what we need to hear. We'll capture assumptions and open questions as we go and share the write-up back with you."

**Standard closing frame:**

> "Before we wrap: what did we not ask about that we should have? Who else should we be talking to on this topic? And what's the one thing that would most worry you if this went live?"

**Capture legend (workspace convention):**

| Tag | Meaning |
|---|---|
| `Q-nnn` | Open question raised, needs an answer |
| `ASM-nnn` | Assumption we're carrying until validated |
| `DEC-nnn` | Decision settled in the room |
| `RISK` | Risk to log in the register (DW-3 owns consolidation) |

---

*Status: Draft*
*Last updated: 2026-08-31*
