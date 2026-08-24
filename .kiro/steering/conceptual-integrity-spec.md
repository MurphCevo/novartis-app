# Conceptual Integrity  - Artifact System Spec

## Purpose

Defines the Conceptual Integrity (CI) knowledge structure for BA discovery work: one synthesizing hub note, backed by atomic notes that can each be created, versioned, and revisited independently without requiring a full rewrite of the hub.

The hub note is what gets shown to a client or used as the litmus test in the moment. The atomic notes underneath are what actually get edited as discovery progresses. The hub stays a generated/synthesized index over the atoms  - it is not independently maintained.

---

## Structure

```
Conceptual Integrity (hub note)
├── Problem Statement       [stable]
├── The Why                 [stable]
├── Success Criteria        [stable, checkpoint-revisited]
├── Guiding Principles       [stable, checkpoint-revisited]
├── Scope Boundary           [living]
├── Decision Record          [living, append-only]
├── Tensions                 [living]
├── Assumptions Register     [living, append-only]
└── Glossary                 [living, append-only]
```

**Stable** = expected to change rarely; a change here is a signal worth flagging explicitly.
**Living** = expected to be added to and revised regularly as discovery proceeds.
**Append-only** = individual entries get superseded, not deleted or silently edited  - history stays visible.

---

## Atomic note types

### 1. Problem Statement
**Definition:** The operational pain  - what's actually observed today. Bottlenecks, cycle time, rework, where effort and delay concretely occur.
**Distinct from "The Why":** this is the *symptom*, observed and evidence-based. Keep it free of strategic framing.
**Template fields:**
- Current state observation (what's happening, in concrete terms)
- Evidence / source (interview, data, ticket analysis  - cite it)
- Who is affected and how
- Cost of inaction (time, risk, opportunity)

### 2. The Why
**Definition:** The strategic mandate  - why leadership is funding this now, what's driving urgency.
**Distinct from Problem Statement:** this is the *motivation*, not the symptom. Two different authors answering "why are we doing this" should land here vs. Problem Statement respectively, not duplicate each other.
**Template fields:**
- Leadership mandate / driving pressure
- What "doing nothing" costs strategically
- Timing  - why now, not later

### 3. Success Criteria
**Definition:** How you'll know, concretely, whether the eventual solution honored the CI. Checkable, not vague.
**Template fields:**
- Criterion (stated so a skeptic could verify it)
- How it will be measured
- Who owns confirming it

### 4. Guiding Principles
**Definition:** Statements that resolve tensions, not restate goals. A good principle tells you what to do when two good things conflict.
**Template fields:**
- Principle statement
- The tension it resolves (link to a Tension note)
- Non-negotiable or negotiable (tag)

### 5. Scope Boundary
**Definition:** What's explicitly in and out. Out-of-scope is usually the more valuable half  - it's what stops scope creep from eroding the CI quietly.
**Template fields:**
- In scope (list)
- Out of scope (list)
- Adjacent-but-excluded (things people will assume are in scope  - name them explicitly)

### 6. Decision Record
**Definition:** ADR-style atomic entries. Each decision is something you've settled, with a trail back to why.
**Template fields:**
- Decision
- Context / options considered
- Rationale
- Status: proposed / accepted / superseded
- Date + owner
- Linked principle or tension (if applicable)

### 7. Tensions
**Definition:** Real trade-offs the project holds a position on (e.g. Speed vs. Control). Not silently dropped  - held explicitly, revisited at checkpoints.
**Template fields:**
- The two competing goods
- Current position (and why)
- Non-negotiable or negotiable (tag)
- Linked principle (if one resolves it)

### 8. Assumptions Register
**Definition:** Things currently treated as true but not yet validated  - bets, not decisions. Distinct from Decision Record: a decision is settled, an assumption is a checkable guess still in play.
**Template fields:**
- Assumption
- Why it's being made (what's it standing in for)
- How/when it gets validated
- Status: open / confirmed / broken
- Impact if broken (what else depends on this)

### 9. Glossary
**Definition:** Ubiquitous language  - terms that mean different things to different stakeholders (e.g. "approval" to Marketing vs. Regulatory). Prevents silent misalignment that survives every other note intact.
**Template fields:**
- Term
- Definition (as agreed for this project)
- Where it diverges from common/departmental usage
- Source stakeholder group(s)

---

## Versioning & checkpoint practice

- CI starts as a draft (v1.0) after the initial vision/principles work.
- At each discovery checkpoint (process mapping, data discovery, compliance discovery  - see BA Discovery Approach backlog), ask explicitly: *does what we just learned confirm the CI, or does it force a revision?*
- Stable notes (Problem Statement, The Why) changing is a signal worth flagging to the client directly  - it usually means the original framing was incomplete, not just refined.
- Living notes (Scope Boundary, Tensions, Decision Record, Assumptions, Glossary) are expected to grow  - additions don't need special flagging, but superseded entries stay visible rather than being deleted.
- Compliance discovery is the checkpoint most likely to force a real Guiding Principles or Scope Boundary revision, since regulatory findings are often hard boundaries rather than preferences to weigh.

## Tie-in to sprint showcases

Each showcase is a natural moment to surface: what atomic notes were added or superseded this sprint, and whether the hub-level checkpoint question needs asking. The showcase artifact for a CI-related sprint can simply be the diff  - which atoms changed and why  - rather than a full hub re-presentation every time.
