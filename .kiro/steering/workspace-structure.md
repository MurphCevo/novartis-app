# BA workspace structure
> Steering doc — Kiro reads this to understand how this engagement is organised.

## Identity

This is a consulting Business Analyst / Solution Architect engagement workspace.
The practitioner works across cloud consulting, enterprise architecture, and IT service management.
All artefacts produced here are either internal working material or client-facing deliverables.

---

## Folder structure

This workspace uses a sub-Johnny Decimal system (sub-JDEX) nested inside a parent JD address.

```
10-19_projects/
  11_client-projects/
    11.{ID}_{client}-{engagement}/   ← you are here
      README.md
      .kiro/
      10-19_engagement/
        11_governance/
        12_comms/
        14_close-out/
        19_inbox/
      20-29_problem/
        21_context/
        22_constraints/
      30-39_solution/
        31_reqs/
        32_design/
        33_decisions/
      40-49_deliverables/
        41_draft/
        42_issued/
        43_outcomes/
      50-59_archive/
        51_superseded/
```

---

## What each area means

### 10-19_engagement — how we are running this
Anything about the engagement itself, not the client's problem.

- **11_governance/** — decisions log, RACI, change control, assumptions register
- **12_comms/** — meeting notes (YYYY-MM-DD-topic.md), action items, stakeholder log
- **14_close-out/** — internal, non-client-facing wrap-up material drafted while the engagement is still open — most notably a masked Precise.io profile write-up and lessons-learnt notes. This is not a client deliverable (see the 40-49_deliverables rule below) — it's collateral for the firm's own use. Files here carry a date prefix (YYYY.MM.DD-{client-slug}-precise-writeup.md) rather than the sub-JDEX {category}.{seq}-{slug} pattern, matching the naming used in the central library they'll eventually join. This folder is a staging area, not the permanent home — see "Close-out library (external)" below: at engagement close, its contents are moved (not copied) into the central library.
- **19_inbox/** — raw capture: daily notes, half-formed thoughts, unprocessed workshop outputs. This is intentional and AI-readable. Content here is unverified input, not settled project knowledge. Anything older than one week that has not been promoted should be filed properly or deleted.

### 20-29_problem — what we are solving
The client's situation as we understand it. Source material and constraints.

- **21_context/** — as-is state, pain points, discovery outputs, workshop readouts, interview notes
- **22_constraints/** — policies, standards, regulatory requirements, assumptions, risks

### 30-39_solution — what we are building
Our response to the problem.

- **31_reqs/** — user stories, acceptance criteria, BDD, functional and non-functional requirements
- **32_design/** — process maps, context diagrams, data flows, architecture diagrams
- **33_decisions/** — Architecture Decision Records (ADRs), options analysis, trade-off documentation. This is where "why" is recorded, not just "what".

### 40-49_deliverables — what we hand over
Client-facing outputs only. Nothing goes here that a client couldn't read.

- **41_draft/** — in-review versions, documents under active feedback
- **42_issued/** — signed-off, versioned, frozen artefacts. Do not overwrite; create a new version.
- **43_outcomes/** — metrics, client feedback, before/after comparisons, retrospective notes. This is the evidence base for case studies.

### 50-59_archive — superseded material
- **51_superseded/** — old versions and closed threads. Move here; do not delete. Preserve original filenames.

---

## Numbering rules

| Level | Convention | Example |
|---|---|---|
| Engagement (parent JD) | `11.{ID}` | `11.003` |
| Area (inside engagement) | Range prefix | `20-29_problem` |
| Category (inside area) | Two-digit prefix only | `21_context` |
| File | `{category}.{seq}-{slug}.{ext}` | `31.04-sftp-acceptance-criteria.md` |

- The engagement folder (`11.{ID}`) is the **only JD ID in the path**
- Sub-JDEX numbers are local — `11` inside this engagement has no relationship to `11` in the parent system
- IDs reappear only at file level
- `.kiro/` is Kiro-managed and is not numbered
- 14_close-out/ is the one category with an exception to the file-naming rule above — see its description, it uses a date prefix instead of {category}.{seq}
- 11.005-close-out/ is the one exception to "the engagement folder is the only JD ID in the path" — it borrows an 11.{ID} slot for the central close-out library rather than a client engagement. See "Close-out library (external)" below.

---

## Close-Out library (external)

Precise.io write-ups and lessons-learnt notes are meant to accumulate across all engagements, so their permanent home is a shared archive outside any single engagement folder:

```
C:\Users\iluva\OneDrive - Cevo\10-19-projects\11-client-projects\11.005-close-out\
  precise-writeups\
    YYYY.MM.DD-{client-slug}-precise-writeup.md
  lessons-learnt\
    YYYY.MM.DD-{client-slug}-lessons.md
```

This sits alongside individual client engagement folders in 11-client-projects\ and uses the same 11.{ID} addressing (ID 005) even though it isn't itself a client engagement — it's the practitioner's own cross-engagement record. Because it occupies 005 in that sequence, that ID should not be reused for an actual client engagement.

Workflow: while an engagement is open, drafts live locally in that engagement's 10-19_engagement/14_close-out/ (see above). At engagement close, the finished write-up and lessons-learnt notes are moved — not copied — into 11.005-close-out/, the same move-don't-duplicate pattern already used for 51_superseded/. A closed engagement folder should end up with an empty 14_close-out/, with the authoritative copy living centrally.

This path is not copied in via the primer — it's a standing reference Kiro should know about for every engagement, precisely because copying the whole back-catalogue into each new engagement folder would defeat the point of keeping it central.

---

## Filing decisions

**When classifying a document, ask "what is this about?" not "what was I doing when I made it?"**

| Content                                                       | Where it goes                                | Why                                                                  |
| ------------------------------------------------------------- | -------------------------------------------- | -------------------------------------------------------------------- |
| Workshop readout                                              | `21_context/`                                | It is about the problem, not the meeting                             |
| Decision made in a meeting                                    | `33_decisions/`                              | It is about the solution, not the meeting                            |
| Action items from a meeting                                   | `12_comms/`                                  | It is about running the engagement                                   |
| Half-formed thought                                           | `19_inbox/`                                  | Unprocessed; promote when clear                                      |
| Issued report                                                 | `42_issued/`                                 | Frozen client-facing output                                          |
| Client feedback on a report                                   | `43_outcomes/`                               | Outcome evidence                                                     |
| Precise.io profile write-up (drafting, engagement still open) | `14_close-out/`                              | Internal wrap-up collateral, not a client deliverable                |
| Precise.io profile write-up (final, engagement closed)        | central `11.005-close-out/precise-writeups/` | Builds the cross-engagement library - see above                      |
| Lessons learnt                                                | central `11.005-close-out/lessons-learnt`    | Internal retrospective; valuable to future engagements, not this one |
|                                                               |                                              |                                                                      |


---

## Case study readiness

Every engagement should be case study-ready at close. The framework is **Challenge / Approach / Outcome**.

| Case study element | Source in this workspace |
|---|---|
| Challenge | `21_context/` + `22_constraints/` |
| Approach | `33_decisions/` + `32_design/` |
| Outcome | `43_outcomes/` |

A `cs-frame.md` file should be created in `19_inbox/` at engagement start with three headings: Challenge, Approach, Outcome. Populate it throughout the engagement. Promote to `43_outcomes/` at close.

---

## Tone and voice

When producing content for this workspace:
- Use a BA/SA consulting voice — precise, structured, evidence-led
- Distinguish clearly between confirmed facts, working assumptions, and open questions
- Label draft material explicitly
- Treat `19_inbox/` content as unverified unless it has been promoted
- Treat `42_issued/` content as the authoritative record
