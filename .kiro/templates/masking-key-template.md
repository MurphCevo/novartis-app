# Masking Key

> Internal reference only. Do NOT share externally.
> Maps masked terms in portfolio/case-study versions back to real engagement entities.

---

## Engagement

| Masked Term | Real Entity |
|---|---|
| {Industry descriptor, e.g. "ASX-listed energy infrastructure client"} | {Client name} |
| The organisation | {Client name} |
| Engagement sponsor | {Name and role} |
| The consulting team | Cevo Consulting |

---

## Workloads / Systems

| Masked Term | Real Entity | Notes |
|---|---|---|
| Workload A | {name} | {context} |
| Workload B | {name} | {context} |

---

## Teams & People

| Masked Term | Real Entity | Notes |
|---|---|---|
| {Role title} | {Person name} | {context} |

---

## Dates & Timelines

| Masked Term | Real Date |
|---|---|
| Month 1 | {real date and milestone} |
| Month 3 | {real date and milestone} |

---

## Masking Rules

1. Client name → generic industry descriptor
2. Workload/system names → sequential letters (A, B, C) ordered by criticality
3. People names → role titles only
4. Specific dates → relative month offsets
5. Consulting firm name → removed entirely
6. Internal file references → removed
7. Tool names → keep where generic (AWS, GitLab); remove where they form an identifying fingerprint
8. Quotes from staff → kept (not attributable without names)
