# DW-1 - Source Data Product Owner Discovery Workshop

**ClickUp:** 86d469738 | **Priority:** Core | **Duration:** 60 min per system (or a 90-min round-robin)
**Owner:** Cevo (discovery) + Novartis | **Feeds:** RACI, `21_context/`, OBJ.005

---

## Objective

Identify and interview the Product Owner for each source system so we know who owns the data, the access, and the API provision for integration. This is a "who and how" session, not a "what's in the data" session - the data itself is covered in DW-7.

## Story

As the BA, I want to identify and interview the Product Owner for each source system so that we know who owns the data, access, and API provision for integration.

## Systems in scope

Veeva Vault PromoMats (VVPM), Aprimo / Fuse, SharePoint (post G-Drive migration), Sherlock, Customer Data Hub / Snowflake, Salesforce Marketing Cloud (SFMC), Google Analytics 4 (GA4), Veeva Oncore CRM, ShamanGo / Anthill Activator.

## Attendees

- Cevo: BA (facilitator), SA (technical scribe)
- Novartis: system/platform owner or delegate per system, plus whoever approves access requests

## Pre-reads

- Scope Boundary (`21.01d`) - integration targets list
- Objectives summary (`11.01`) - OBJ.005

---

## Opening frame

> "We're mapping who owns each system Autopilot may need to connect to. For each system we want three things: who the product owner is, where their responsibility starts and stops, and how we'd actually request access. We are not asking you to grant anything today - we're building the contact map and understanding the process."

---

## Question blocks

Prompts in the section intros are conversation openers; work through the table questions to land the specifics. Fill Response and Status live.
Status legend: Open / Answered / Parked.

### 1. Ownership

*Walk me through who is accountable for this system day to day.*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW1-01 | Who is the named product owner or system owner for [system]? | Establish the single accountable contact | | Open |
| DW1-02 | Who is the technical/platform contact (distinct from the business owner) if different? | Separate business ownership from technical ownership | | Open |
| DW1-03 | Who is the escalation contact when the primary owner is unavailable? | Ensure continuity for access requests | | Open |
| DW1-04 | Is ownership internal to Novartis AU, global, or held by a vendor? | Understand where authority actually sits | | Open |

### 2. Ownership boundaries

*Where does your responsibility for this system begin and end?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW1-05 | Do you own the data, the access provisioning, the API layer, or some subset? | Pin down the exact scope of this owner's remit | | Open |
| DW1-06 | If not you, who owns the parts you don't? | Close gaps in the ownership map | | Open |
| DW1-07 | Are there shared-responsibility boundaries with a vendor or a global team we should know about? | Surface split ownership that slows access | | Open |

### 3. Access and API provision

*If we needed a non-production integration to this system, how would that actually happen?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW1-08 | Is there an existing API or integration surface today? (yes/no/unsure - deep detail is DW-7) | Early read on integration feasibility | | Open |
| DW1-09 | What is the process to request access or a new integration? | Map the real access path | | Open |
| DW1-10 | Who approves it, and how is it prioritised against other demands? | Identify approver and prioritisation risk | | Open |
| DW1-11 | What is the typical SLA or lead time for such a request? | Feed realistic timelines into the plan | | Open |
| DW1-12 | Are there forms, tickets, or teams (e.g. a service desk) we must go through? | Uncover process overhead to schedule for | | Open |

### 4. Constraints and known issues

*What should we know before we assume we can connect to this?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW1-13 | Any data residency, contractual, or licensing constraints on integration? | Flag hard constraints before design | | Open |
| DW1-14 | Any planned migrations, replacements, or freezes affecting this system during the PoC window (Apr-Aug)? | Avoid building against a moving target | | Open |
| DW1-15 | Has anyone integrated with this system before? What did they learn? | Reuse prior lessons, avoid known traps | | Open |

---

## What we must leave with (acceptance criteria)

- [ ] Named Product Owner + escalation contact captured for each source system
- [ ] Ownership boundaries documented (data, access, API provision) per system
- [ ] Access-request approval/prioritisation process understood per system

---

## Capture template (per system)

| Field | Capture |
|---|---|
| System | |
| Product owner (name, role, contact) | |
| Technical contact | |
| Escalation contact | |
| Ownership scope (data / access / API) | |
| Access-request process | |
| Approver + prioritisation | |
| Typical lead time / SLA | |
| Known constraints / migrations | |
| Open questions (`Q-nnn`) | |
| Assumptions (`ASM-nnn`) | |

---

*Status: Draft facilitation guide*
*Last updated: 2026-08-31*
