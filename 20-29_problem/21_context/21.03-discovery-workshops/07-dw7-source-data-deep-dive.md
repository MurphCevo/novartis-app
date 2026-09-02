# DW-7 - Source Data Deep-Dive Discovery Workshop [Gap-fill]

**ClickUp:** 86d46979y | **Priority:** Gap-fill | **Duration:** 60-90 min per system
**Owner:** Cevo (discovery) + Novartis | **Feeds:** `21_context/`, `22_constraints/`, OBJ.005 / DLV.005

> **Note:** Recommended gap-fill (not in the original request). The Scope Boundary flags "source data not API-ready" as a top risk with timeline impact. Can be run jointly with DW-1 where the product owner is also the data SME. DW-1 answers "who owns it"; this session answers "what is the data and can we actually connect to it".

---

## Objective

Understand the data itself in each source system so we can confirm integration feasibility and de-risk the "not API-ready" scenario before it hits the build timeline.

## Story

As the SA, I want to understand the data itself in each source system (distinct from who owns it) so that we can confirm integration feasibility and de-risk the "not API-ready" scenario.

## Systems in scope

Same list as DW-1: VVPM, Aprimo/Fuse, SharePoint, Sherlock, Customer Data Hub/Snowflake, SFMC, GA4, Veeva Oncore CRM, ShamanGo/Anthill Activator. Prioritise systems feeding UC-4/5/10.

## Attendees

- Cevo: SA (facilitator), BA
- Novartis: system SME, data engineer / integration contact for the system

## Pre-reads

- DW-1 readout (product owners identified)
- Scope Boundary (`21.01d`) - integration targets and the "not API-ready" grey-zone item

---

## Opening frame

> "This is the technical counterpart to the ownership conversation. For this system we want to see what the data actually looks like, how good it is, and whether we can connect to it today. Honest answers about gaps help us more than optimistic ones - if it's not API-ready, we'd much rather know now than in build."

---

## Question blocks

Prompts in the section intros are conversation openers; work through the table questions to land the specifics. Fill Response and Status live.
Status legend: Open / Answered / Parked.

### 1. Data format and schema

*Show me a representative sample of what lives in this system.*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW7-01 | What are the key entities/objects and their fields? | Understand the data model | | Open |
| DW7-02 | What formats (structured records, documents, binaries, assets)? | Determine handling approach per format | | Open |
| DW7-03 | Is there a schema or data dictionary we can have? | Obtain authoritative schema reference | | Open |
| DW7-04 | What identifiers link this data to other systems? | Enable cross-system joins | | Open |

### 2. Data quality, completeness and freshness

*How much would you trust this data if we fed it straight into an agent?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW7-05 | How complete is the data - known gaps or sparse fields? | Assess completeness risk | | Open |
| DW7-06 | How clean is it - duplicates, inconsistent tagging, free-text where structure is expected? | Assess cleanliness risk | | Open |
| DW7-07 | How fresh - real-time, daily, or stale? How is it updated? | Understand freshness and update cadence | | Open |
| DW7-08 | Any quality issues we should flag now? (cleansing/migration is client responsibility - we consume clean APIs) | Log quality gaps as risks/assumptions | | Open |

### 3. API-readiness today

*Can we connect to this system programmatically right now?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW7-09 | Is there a documented API today? REST, GraphQL, file export, other? | Confirm current integration surface | | Open |
| DW7-10 | Is it available in a non-prod environment for the PoC? | Confirm PoC access | | Open |
| DW7-11 | If no API today, what's the realistic path and timeline to one? | Surface the key "not API-ready" risk | | Open |
| DW7-12 | Who would build/expose it - Novartis, vendor, us? (We don't build source-system APIs) | Assign API provisioning ownership | | Open |

### 4. Connectivity and access mechanism

*Assuming access is granted, how would data physically flow?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW7-13 | Direct API call, an S3 drop / file handoff, or an MCP server? | Choose the connectivity pattern | | Open |
| DW7-14 | Auth mechanism (keys, OAuth, service account)? | Define authentication approach | | Open |
| DW7-15 | Rate limits, payload limits, or throttling to design around? | Capture technical constraints | | Open |

### 5. Vector DB / embedding needs (hybrid RAG)

*For knowledge-retrieval use cases, what needs to be embedded and searchable?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW7-16 | Which content should be embedded for semantic search (UC-4)? | Scope embedding targets | | Open |
| DW7-17 | Volume and update frequency of that content? | Size the embedding workload | | Open |
| DW7-18 | Any constraints on where embeddings can be stored/processed (residency)? | Capture residency constraints on embeddings | | Open |

---

## What we must leave with (acceptance criteria)

- [ ] Sample data format & schema captured per source
- [ ] Data quality, completeness, and freshness assessed
- [ ] API-readiness today confirmed (or gap flagged) per source
- [ ] Connectivity/access mechanism identified (direct API, S3 drop, MCP)
- [ ] Vector DB / embedding requirements captured (hybrid RAG)

---

## Capture template (per system)

| Field | Capture |
|---|---|
| System | |
| Key entities / schema | |
| Data quality / completeness / freshness | |
| API today? (Y/N/partial) | |
| If no API: path + timeline (RISK) | |
| Connectivity mechanism | |
| Auth + limits | |
| Embedding / vector needs | |
| Q/ASM/DEC | |

---

*Status: Draft facilitation guide*
*Last updated: 2026-08-31*
