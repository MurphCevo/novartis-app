# Conventions

## Diagram references in Markdown

When a markdown document references a diagram, use an HTML comment with the following format:

```markdown
<!-- DIAGRAM {filename}.drawio/{tab-name} -->
```

### Structure

| Part | Meaning |
|---|---|
| `DIAGRAM` | Keyword identifying this as a diagram reference |
| `{filename}.drawio` | The Draw.io source file, located in `32_design/` |
| `/{tab-name}` | The specific tab/page within the Draw.io file |

### Example

```markdown
<!-- DIAGRAM 32.03-diagrams.drawio/context-diagram -->
```

This tells Kiro: "look at the named tab in `30-39_solution/32_design/{filename}.drawio` for visual context relevant to this section."

### Behaviour

- The comment is invisible in rendered markdown output
- When reviewing a document, Kiro should read the referenced `.drawio` file and locate the named tab to understand the diagram context
- Multiple diagram references can appear in a single document, each scoped to the section they appear in

---

## Draw.io file organisation

### Approach: group by deliverable or theme, use tabs for individual diagrams

- Use **one `.drawio` file with multiple named tabs** as the default. Related diagrams stay together and the `DIAGRAM` reference convention handles tab-level linking.
- Create a **dedicated `.drawio` file** when a deliverable or theme has ~4 or more diagrams.
- A general-purpose file (e.g. `32.03-diagrams.drawio`) is fine for diagrams that don't belong to a single deliverable.

### Naming

| Level | Convention | Example |
|---|---|---|
| File | `{category}.{seq}-{slug}.drawio` | `32.03-diagrams.drawio` |
| Tab | Lowercase, hyphenated, descriptive | `context-diagram`, `triage-flow` |

### Rules

- Tab names must be unique within a file and descriptive enough to identify the diagram without opening it.
- All `.drawio` files live in `30-39_solution/32_design/`.
- Do not create one file per diagram unless there is a specific reason (e.g. handing a single file to a third party).

---

## Discovery note format (use after ~3 sessions)

```markdown
# {Topic}  - Discovery Session

**Date:** YYYY-MM-DD
**Attendees:** {names and roles}
**Topic:** {one-line description}

---

## Key Findings

{Structured findings, organised by sub-topic}

---

## Open Questions

| # | Question | Status |
|---|----------|--------|
| 1 | {question} | Open / Answered / Parked |

---

## Relevance to Engagement

{Why this matters for our deliverables  - link to specific DLV/OBJ}
```

---

## ADR format

```markdown
# {Decision Title}

## Status

{Proposed / Accepted / Superseded}  - YYYY-MM-DD

## Context

{What situation prompted this decision? 2 -4 sentences.}

## Decision

{What we decided. One sentence.}

## Rationale

{Why this option over alternatives. Keep brief  - a table comparing options is fine.}

## Consequences

{What changes as a result of this decision  - for other artefacts, teams, or processes.}
```

---

## Slide deck drafting (markdown-first)

```markdown
## Slide N  - {Title}

{Bullet content  - this becomes the slide body}

| Column 1 | Column 2 |
|---|---|
| data | data |

> Notes: {Speaker notes  - what to say, what questions to expect, how to adapt to the room}
```

- One H2 per slide
- Body content = what appears on the slide
- `> Notes:` block = speaker notes (not rendered on slide)
- Translate to pptx only for final visual polish

---

## Character encoding

- **Never use em dashes (` -`) or en dashes (` -`) anywhere in this workspace.** Use a plain hyphen (`-`) or spaced hyphen (` - `) instead.
- Reason: em/en dashes cause encoding artefacts (`â€"`) when files are opened in tools that assume Windows-1252 (e.g. Excel opening a CSV). Plain hyphens are universally safe.
- This applies to all file types: markdown, CSV, drawio labels, and any other text content.

---

## Conceptual Integrity Artifact Numbering

### Purpose

In addition to the requirements-level prefixes above, the Conceptual Integrity (CI) artifact set uses its own numbering for cross-cutting concerns that sit above individual requirements — success criteria, guiding principles, and tensions. These are referenced throughout deliverables and trace back to the CI atomic notes in `21_context/`.

### Prefix Reference

| Artifact Type | Prefix | Example | Notes |
|---|---|---|---|
| Success Criteria | `SC` | `SC-01` | A verifiable criterion for project success. Defined in CI Success Criteria (21.10d). Numbered sequentially. |
| Guiding Principle | `GP` | `GP-01` | A design principle that resolves a tension. Defined in CI Guiding Principles (21.10e). |
| Tension | `T` | `T-01` | A named trade-off the project holds an explicit position on. Defined in CI Tensions (21.10h). |

### Relationship to Requirements Prefixes

These CI-level prefixes are **upstream** of requirement-level artifacts:

- A **Business Requirement** (`BR`) often traces to a **Success Criterion** (`SC`) — the BR states the need; the SC states how we'll know it's met.
- A **Functional/Non-Functional Requirement** (`FR`/`NFR`) may reference a **Guiding Principle** (`GP`) as rationale for a design choice.
- A **Decision** (`DEC`) may reference a **Tension** (`T`) it resolves or a **Guiding Principle** (`GP`) it applies.

### Quick Reference (CI-level)

```
SC-00     Success Criteria
GP-00     Guiding Principle
T-00      Tension
```

---

## Requirements & BA Artifact Numbering Conventions

### Purpose

This section defines the ID numbering convention used to tag and track business analysis artifacts across a project — questions, assumptions, decisions, requirements, business rules, and business requirements. Consistent IDs make artifacts easy to reference in documents, easy to trace across a requirements traceability matrix (RTM), and easy to search for in tooling (Jira, Confluence, Excel, etc.).

### Format

```
<PREFIX>-<NNN>
```

- Prefix and number are separated by a hyphen (`-`), not a dot. Hyphens avoid ambiguity with decimal numbers or version numbers, and behave more predictably in filenames, spreadsheets, and tools that auto-format dotted numeric strings.
- The numeric portion is **3 digits, fixed-width, zero-padded** (`001`, `002`, ... `999`).
- Numbers are assigned sequentially within each prefix, in the order artifacts are raised. They are never reused, even if an artifact is later deleted or superseded — deprecate it instead (see Lifecycle below).
- If a category exceeds 999 items, extend to 4 digits (`FR-1000`) rather than restarting or reusing an existing prefix.

### Prefix Reference

| Artifact Type | Prefix | Example | Notes |
|---|---|---|---|
| Question | `Q` | `Q-001` | An open question raised during elicitation that needs an answer before requirements can be finalized. |
| Assumption | `ASM` | `ASM-001` | A statement taken as true without proof, used to fill a gap where confirmed information isn't yet available. |
| Decision | `DEC` | `DEC-001` | A recorded decision, typically made to resolve a Question or formalize an Assumption. |
| Functional Requirement | `FR` | `FR-001` | Describes what the system must do — a behavior, feature, or function. |
| Non-Functional Requirement | `NFR` | `NFR-001` | Describes a quality attribute or constraint — performance, security, usability, availability, etc. |
| Business Rule | `RULE` | `RULE-001` | A constraint or policy that governs behavior or data, independent of any single system (e.g. eligibility criteria, calculation logic, compliance mandates). |
| Business Requirement | `BR` | `BR-001` | A high-level business need or objective that justifies the initiative — the "why," which functional/non-functional requirements exist to satisfy. |

#### Why `RULE` instead of `BR` for Business Rule

`BR` and `BRQ` are visually close and easy to misread or mistype in a fast-moving table, especially since Business Rule and Business Requirement serve different purposes (a rule *constrains*; a requirement *specifies a need*). Using `RULE` for Business Rule and reserving `BR` for Business Requirement removes that ambiguity at a glance and keeps the two concepts visually distinct.

### Optional: Domain/Module Qualifier

For larger projects with multiple workstreams or system modules, insert a short domain tag between the prefix and the number to keep related items grouped and searchable:

```
<PREFIX>-<DOMAIN>-<NNN>
```

Example: `FR-PAY-001` (Payments), `NFR-AUTH-002` (Authentication).

This is optional and should only be adopted if the register is expected to grow large enough that a flat list becomes hard to scan. Domain tags should be short (2–5 characters), fixed once chosen, and documented in a lookup table alongside this convention.

### Lifecycle & Status

Every artifact should carry a status alongside its ID, tracked in the register (not encoded in the ID itself):

- **Draft** — captured but not yet reviewed or confirmed.
- **Open** — active and unresolved (mainly for Questions).
- **Approved / Confirmed** — reviewed and accepted.
- **Deprecated / Superseded** — no longer active; reference the superseding ID if applicable.
- **Rejected** — considered and explicitly not progressed.

IDs are permanent once assigned. If an item is deprecated or rejected, retire the ID rather than reassigning it to a new item.

### Traceability

Where possible, link related artifacts explicitly rather than relying on proximity in a document:

- A **Decision** should reference the **Question** it resolves (e.g. `DEC-004 resolves Q-004`).
- A **Functional/Non-Functional Requirement** should reference the **Business Requirement** it supports (e.g. `FR-012 traces to BR-002`).
- A **Business Rule** should be referenced by any Functional Requirement whose behavior it governs (e.g. `FR-012 implements RULE-003`).

This mapping is best maintained in a Requirements Traceability Matrix (RTM), with one column per relationship.

### Example Register Snippet

| ID | Title | Type | Status | Traces To |
|---|---|---|---|---|
| `BR-001` | Reduce manual invoice processing time | Business Requirement | Approved | — |
| `Q-001` | Which ERP system holds the source invoice data? | Question | Open | — |
| `ASM-001` | Source data will be available via API, not flat file | Assumption | Draft | Q-001 |
| `DEC-001` | Confirmed: integrate via ERP REST API | Decision | Approved | Q-001, ASM-001 |
| `RULE-001` | Invoices over $10,000 require dual approval | Business Rule | Approved | BR-001 |
| `FR-001` | System shall route invoices >$10,000 to a second approver | Functional Requirement | Approved | BR-001, RULE-001 |
| `NFR-001` | Invoice approval routing shall complete within 2 seconds | Non-Functional Requirement | Approved | FR-001 |

### Quick Reference

```
Q-000     Question
ASM-000   Assumption
DEC-000   Decision
FR-000    Functional Requirement
NFR-000   Non-Functional Requirement
RULE-000  Business Rule
BR-000    Business Requirement
```
---
