# Project Readiness Checklist  - Working Notes

**Date:** 2026-08-21
**Author:** (not captured)
**Topic:** Drafting the AI Project Readiness Checklist structure and content

---

## Summary

Working notes toward the Project Readiness Checklist artefact (to be published on Confluence). Captures the categories of information a client needs to prepare before an Agentic AI engagement can begin: infrastructure provisioning, SOPs and tooling, platform architecture decisions, and people alignment. Also includes notes on model governance (supported models, whitelisting process).

---

## Checklist Structure

### 1. Infrastructure Prerequisites & Provisioning

| Category | Detail |
|----------|--------|
| **Services** | Bedrock, AgentCore, etc. |
| **Marketplace** | Model provisioning  - including documentation link, FTU, etc. |
| **Cross-region inference** | Transparent HA with Melbourne (from Sydney); varies by region and model. Can it be turned off? (Appears not.) See also enforcing data residency to a single region. |
| **Org / Account / SCPs / Permissions** | Account structure and guardrails |
| **Connectivity & routing** | Consider additional channels: email, etc. |
| **UI** | CloudFront + WAF? Internal or external app? Integrate with existing UIs? WAF is cross-region (us-east-1). |

### 2. SOPs & Tools

- Client laptop provision
- AWS account access
- Tooling: Kiro, Claude Code

### 3. Platform Architecture Decisions

- Do we need to isolate Bedrock into an AI Shared Services account, or just a project-specific implementation?
- Do we need an AI Gateway pattern?

### 4. Alignment of People

- Are client-side infrastructure staff **dedicated** to the project, or will they prioritise from a BAU queue?
- What is the expected SLA for infrastructure requests?

---

## Notes from Reading

- **Supported models:** What does the organisation have in the way of deciding which models / companies are supported?
- **Model whitelisting:** What do we need to do to get a model included in the whitelist?

---

## Open Questions

| # | Question | Owner | Status |
|---|----------|-------|--------|
| 1 | Can cross-region inference be disabled to enforce data residency? | TBD | Open |
| 2 | AI Shared Services account vs. project-specific  - which pattern? | TBD | Open |
| 3 | Do we need an AI Gateway? | TBD | Open |
| 4 | Client infra staff: dedicated or BAU queue? | TBD | Open |
| 5 | Model governance: what's the org's current model whitelist process? | TBD | Open |

---

## References

- Slack thread on cross-region inference & data residency: [link](https://cevoteam.slack.com/archives/C057A0RUF28/p1786033725442329)
- Good example format: *Resilience Review  - Cloud Evolution Practice* (Confluence)
