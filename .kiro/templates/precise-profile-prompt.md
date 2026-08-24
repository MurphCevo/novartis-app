# Precise.io Profile Generate - Prompt Template

Use this prompt at the close of any client engagement (go-live, handover, or offboarding) to turn the engagement workspace into a Precise.io-ready profile entry. Paste it into a new session with the engagement's 11.{ID} folder in context, filling in the bracketed fields first.

---

## The Prompt

An engagement has just closed. I need you to generate a Precise.io profile
entry from the workspace at 11.{ID}_{client}-{engagement}/, following the
BA workspace structure defined in workspace-structure.md.

Engagement folder: 11.{ID}_{client-slug}
Masking key (if one exists): {path to masking-key file, e.g. 11.{ID} masking-key.md}

Do this in two passes:

## Pass 1  - Gather and verify

Pull source material only from these locations, and nothing from
19_inbox/ unless it has already been promoted elsewhere:

- 21_context/  - as-is state, pain points, discovery/workshop outputs
- 22_constraints/  - regulatory, policy, risk constraints that shaped the work
- 33_decisions/  - ADRs, options analysis (this is the "why" and "approach")
- 32_design/  - what was actually built/designed
- 42_issued/  - frozen, client-facing deliverables (titles + one-line purpose)
- 43_outcomes/  - metrics, before/after, client feedback, retrospective notes
- 19_inbox/cs-frame.md if present  - Challenge / Approach / Outcome framing
- C:\Users\iluva\OneDrive - Cevo\10-19-projects\11-client-projects\11.005-close-out\precise-writeups\
   - the central library of previously accepted Precise.io write-ups from
  past engagements (if reachable in this session); skim 1-2 for voice and
  phrasing consistency, but do not reuse their facts

Before drafting, tell me:
- The date range of the engagement (start → last activity or go-live date)
- Which facts are confirmed vs. still working assumptions
- Any gaps where 43_outcomes/ has no metrics yet (flag rather than invent numbers)

## Pass 2  - Draft the entry

Produce TWO outputs:

### A) Long-form internal draft entry
Match the structure and level of detail used in
energy-infra-precise-profile-entry.md exactly:

- Project Title
- Client Industry (sector only  - never the client name)
- Role
- Duration
- Engagement Type
- Project Summary (2-3 sentences: situation → complication → why it mattered)
- Key Activities (bullets, past tense, action-first)
- Key Deliverables (bullets, nouns  - what was handed over)
- Skills & Technologies (table: Disciplines / Frameworks / Cloud / Tooling /
  Methods / Soft Skills  - omit rows with nothing genuine to put in them)
- Value Delivered (bullets  - outcomes and numbers from 43_outcomes/ only;
  no invented metrics)
- Notes (masking status, and a pointer to the masking key if one exists)

Mark it "Draft for precise.io project/experience entry  - promote to final
once reviewed" exactly as the existing example does.

### B) Precise.io-ready narrative entry
Condense (A) into the short narrative format already used in the live
profile (precise-profile-2026.05.01.md)  - a dated heading followed by 2-4
short paragraphs, each opening with a bolded theme phrase
(e.g. "Workshop Leadership & Deep Discovery:"), written in first person,
past tense, client-safe language. This is the version that gets pasted
directly into precise.io. Do not include a Skills & Technologies table or
internal notes in this version  - fold anything essential into the prose.

## Masking rules (apply to both outputs)

- No client name, no site/workload identifiers, no numbers that could
  identify the client (e.g. exact FUM, exact headcount) unless already used
  in a previous published entry for this client
- Use industry + engagement type instead of naming the organisation
  ("a major Australian energy infrastructure company", not the company name)
- If a masking key file exists for this engagement, use its substitutions
  and note that it was used; if none exists, tell me one should probably be
  created before this entry is finalised

## Voice

Follow the tone rules in workspace-structure.md: precise, structured,
evidence-led. Follow the intent in precise-profile-descriptor.md: clear,
professional, client-friendly, and it should reflect both what I did and
the value I brought  - not just a task list. Match the register already
established in precise-profile-2026.05.01.md (confident, specific,
outcome-first, no filler adjectives).

## Where this goes

While the engagement is still open, save the long-form draft (A) into
10-19_engagement/14_close-out/ within the engagement folder, NOT into
40-49_deliverables  - that area is client-facing only, and this write-up
isn't. Name the file with a date prefix so it's ready to join the central
library in date order: YYYY.MM.DD-{client-slug}-precise-writeup.md (masked
slug, matching the existing dated-file convention).

At engagement close, the finished file gets MOVED (not copied) out of
14_close-out/ into the central library at:
C:\Users\iluva\OneDrive - Cevo\10-19-projects\11-client-projects\11.005-close-out\precise-writeups\
 - if this session can't reach that path directly, tell me so I can move it
myself; don't leave a duplicate behind in 14_close-out/.

Give me the narrative version (B) directly in the chat so I can paste it
into Precise.io. Do not touch the live profile doc
(precise-profile-2026.05.01.md) yourself  - I'll fold the accepted entry
into it once I've reviewed it.

---
