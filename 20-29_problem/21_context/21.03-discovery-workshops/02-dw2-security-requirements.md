# DW-2 - Security Requirements Discovery Workshop

**ClickUp:** 86d46974g | **Priority:** Core | **Duration:** 90 min
**Owner:** Cevo (discovery) + Novartis | **Feeds:** `22_constraints/`, OBJ.006

---

## Objective

Discover Novartis security requirements so the PoC architecture is designed to meet them from the start, rather than retrofitted. This is the generic security session; AI-specific governance is DW-9, and MLR/regulatory is DW-6.

## Story

As the BA/SA, I want to discover Novartis security requirements so that the PoC architecture is designed to meet them from the start.

## Attendees

- Cevo: SA (facilitator), BA (scribe)
- Novartis: information security lead, IAM/Entra admin, network/cloud security, secrets/key management owner

## Pre-reads

- Scope Boundary (`21.01d`) - cross-cutting concerns (RBAC, logging, no training on Novartis data)
- Objectives summary (`11.01`) - OBJ.006 governance-first architecture
- AI infra prereqs note (`12_comms/2026-08-25-ai-infra-prereqs.md`)

---

## Opening frame

> "Autopilot will connect AI agents to Novartis systems and data. We need to design it to your security bar from day one. This session is about the rules the architecture has to live inside - data classification, identity and access, encryption, network, and secrets. Where you have an existing standard, point us to it; where the agent pattern is new territory, let's talk it through."

---

## Question blocks

Prompts in the section intros are conversation openers; work through the table questions to land the specifics. Fill Response and Status live.
Status legend: Open / Answered / Parked.

### 1. Data classification and handling

*How is the data these agents will touch classified, and what does that dictate?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW2-01 | What data classification scheme applies (public / internal / confidential / restricted)? | Establish the classification framework to design to | | Open |
| DW2-02 | Which classification do approved marketing assets, strategy docs, and campaign performance data fall under? | Classify the specific data the agents touch | | Open |
| DW2-03 | What handling rules attach to each level (storage location, encryption, who can see it)? | Derive concrete handling constraints | | Open |
| DW2-04 | Are there data residency requirements (must data stay in-region)? | Constrain hosting and model region choices | | Open |
| DW2-05 | Any data that is out of bounds for an AI system entirely? | Identify hard exclusions early | | Open |

### 2. Identity and access (IAM, Entra, RBAC)

*How should a person - and an agent - prove who they are and what they're allowed to do?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW2-06 | What is the identity provider (Entra ID / Azure AD)? Is SSO mandatory? | Fix the authentication foundation | | Open |
| DW2-07 | What RBAC model applies? How are roles defined and granted? | Understand authorisation model to reuse | | Open |
| DW2-08 | How should the Autopilot Assistant authenticate an end user? | Define the user auth flow for the interface | | Open |
| DW2-09 | Is there a least-privilege / just-in-time access expectation? | Capture access-minimisation requirements | | Open |
| DW2-10 | How are access reviews and de-provisioning handled? | Cover the full access lifecycle | | Open |

### 3. Agent identity and session/token management

*An agent acting on a user's behalf is a newer pattern - how should its identity work here?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW2-11 | Should agents have their own service identities, or act as the user (delegated/on-behalf-of)? | Decide the agent identity model | | Open |
| DW2-12 | How should tokens be issued, scoped, refreshed, and revoked? | Define token lifecycle rules | | Open |
| DW2-13 | What session lifetime and re-authentication rules apply? | Constrain session handling | | Open |
| DW2-14 | How do we audit "which identity did what" when an agent chains calls across systems? | Ensure traceability across agent call chains | | Open |

### 4. Encryption, network and connectivity

*What are the rules for data in transit and at rest, and how do we reach your systems?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW2-15 | Encryption standards required at rest and in transit? | Capture mandatory encryption baselines | | Open |
| DW2-16 | Network constraints - private endpoints, VPC/VNet peering, no public internet egress? | Define network design constraints | | Open |
| DW2-17 | Is connectivity to source systems allowed direct, or via a broker/gateway/MCP layer? | Determine the allowed connectivity pattern | | Open |
| DW2-18 | Any allow-listing / SCP change process we need to plan for (ref: infra prereqs note)? | Plan for network/policy change lead time | | Open |

### 5. Secrets management

*Where do credentials, keys, and tokens live?*

| ID | Question | Purpose | Response | Status |
|---|---|---|---|---|
| DW2-19 | What is the approved secrets manager (e.g. AWS Secrets Manager, Vault, Key Vault)? | Fix where secrets are stored | | Open |
| DW2-20 | Rotation policy and ownership? | Capture rotation and ownership rules | | Open |
| DW2-21 | How are secrets provisioned into a non-prod environment? | Enable secure PoC setup | | Open |

---

## What we must leave with (acceptance criteria)

- [ ] Data classification & handling rules documented
- [ ] Identity & access requirements captured (IAM, Entra, RBAC)
- [ ] Agent identity & session/token management approach identified
- [ ] Encryption, network security, and connectivity constraints recorded
- [ ] Secrets management approach agreed

---

## Capture template

| Area | Requirement / constraint | Standard reference | Owner | Q/ASM/DEC |
|---|---|---|---|---|
| Data classification | | | | |
| IAM / RBAC | | | | |
| Agent identity / tokens | | | | |
| Encryption | | | | |
| Network / connectivity | | | | |
| Secrets management | | | | |

---

*Status: Draft facilitation guide*
*Last updated: 2026-08-31*
