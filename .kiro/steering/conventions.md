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
# {Topic} — Discovery Session

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

{Why this matters for our deliverables — link to specific DLV/OBJ}
```

---

## ADR format

```markdown
# {Decision Title}

## Status

{Proposed / Accepted / Superseded} — YYYY-MM-DD

## Context

{What situation prompted this decision? 2–4 sentences.}

## Decision

{What we decided. One sentence.}

## Rationale

{Why this option over alternatives. Keep brief — a table comparing options is fine.}

## Consequences

{What changes as a result of this decision — for other artefacts, teams, or processes.}
```

---

## Slide deck drafting (markdown-first)

```markdown
## Slide N — {Title}

{Bullet content — this becomes the slide body}

| Column 1 | Column 2 |
|---|---|
| data | data |

> Notes: {Speaker notes — what to say, what questions to expect, how to adapt to the room}
```

- One H2 per slide
- Body content = what appears on the slide
- `> Notes:` block = speaker notes (not rendered on slide)
- Translate to pptx only for final visual polish
