# Planning Session

**Date:** 2026-08-17
**Attendees:** (not captured)
**Topic:** Start date, data sources, ECH blockers, shared services architecture

---

## Summary

The September 1 start date is being pushed out due to delays in finalising the MSA. Discussion focused on understanding what data sources are available, how to interface with them, and what sample data or APIs exist. The team also reviewed blockers within ECH (Bedrock provisioning, marketplace approval, model dependencies) and discussed the landing zone architecture including AI Shared Services, transit gateway, and Kade's AI Gateway pattern.

---

## Key Discussion Points

### Start Date
- September 1 start is being pushed out due to MSA delays.

### Data Sources & Integration
- What systems will provide data?
- How do we interface with those systems?
- Questions to resolve:
  - What data sources are available?
  - What do we expect from them?
  - Is there an API / MCP format we can inspect?
  - Any sample data we can look at?

### ECH Blockers
- Getting Bedrock up and running
- Models not provisioned  - requires marketplace approval
- Model dependency issues
- Denny has written this up (reference his document)

### Landing Zone & Shared Services Architecture
- AI Shared Services Account to provide:
  - Bedrock
  - Basic AI capabilities
  - Transit Gateway
  - Vector storage
  - General storage
- Kade's pattern for an AI Gateway (to be reviewed)

---

## Open Questions

| # | Question | Owner | Status |
|---|----------|-------|--------|
| 1 | What data sources are available and in what format? | TBD | Open |
| 2 | Is there an API / MCP interface we can inspect? | TBD | Open |
| 3 | Any sample data available? | TBD | Open |
| 4 | What is the revised start date post-MSA? | TBD | Open |

---

## Action Items

| # | Action | Owner | Due |
|---|--------|-------|-----|
| 1 | Obtain Denny's ECH write-up and circulate | TBD | TBD |
| 2 | Review Kade's AI Gateway pattern for applicability | TBD | TBD |
