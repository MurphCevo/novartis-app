# AI / Agentic App — Project Readiness Checklist

**Status:** DRAFT
**Owner:** Cloud Evolution Practice
**Purpose:** Reusable checklist for AI/agentic engagements. Pull a copy at engagement start, work through with the client, and track what's confirmed vs. outstanding.

---

## How to Use This

1. Copy this template into the engagement workspace at kickoff
2. Walk through each section with the client's platform/infra team in the first week
3. Mark items as confirmed (✅), outstanding (⏳), or blocked (🚫)
4. Items marked "Day One" are pre-requisites — the delivery team cannot start meaningful work without them
5. Items marked "Week 2+" can be resolved in parallel with early discovery/design work

---

## 1. Infrastructure Prerequisites & Provisioning

### 1.1 AWS Services

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 1.1.1 | Non-prod AWS account provisioned within client org | Day One | | |
| 1.1.2 | Amazon Bedrock enabled in target region | Day One | | |
| 1.1.3 | AgentCore / Step Functions / orchestration service enabled | Week 2+ | | |
| 1.1.4 | Storage: S3 buckets, DynamoDB tables, or vector store (OpenSearch Serverless, Aurora pgvector) | Week 2+ | | |
| 1.1.5 | Compute: Lambda, ECS/Fargate, or equivalent provisioned | Week 2+ | | |
| 1.1.6 | Notification services: SES, SNS, or equivalent (if comms channels in scope) | Week 2+ | | |
| 1.1.7 | Observability: CloudWatch, X-Ray, and/or LLM-specific tooling (Langfuse, LangSmith) | Week 2+ | | |

> **Practitioner note:** Confirm the full list of required services early — SCP restrictions often block services silently, and unblocking takes time through client change processes.

### 1.2 Model Access (Marketplace & Provisioning)

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 1.2.1 | Foundation model(s) activated in Bedrock console | Day One | | |
| 1.2.2 | First-time-use (FTU) acceptance completed for each model | Day One | | |
| 1.2.3 | Model governance / AI ethics approval obtained (if required by client) | Day One | | |
| 1.2.4 | Provisioned throughput requested (if latency-sensitive workloads expected) | Week 2+ | | |
| 1.2.5 | Approved model list documented (which models, which versions, any restrictions) | Day One | | |
| 1.2.6 | Process for requesting new/additional models documented | Week 2+ | | |

> **Practitioner note:** Model activation is the single most common Day One blocker. Some orgs require multiple approvals (security, AI governance, procurement) before a model is usable. Start this process before the engagement officially begins if possible.

### 1.3 Cross-Region Inference & Data Residency

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 1.3.1 | Data residency requirements confirmed (single region, or multi-region acceptable?) | Day One | | |
| 1.3.2 | Cross-region inference behaviour understood (Bedrock may route to secondary region for HA) | Day One | | |
| 1.3.3 | If single-region required: enforcement mechanism confirmed (inference profiles, IAM policy, or SCP) | Day One | | |
| 1.3.4 | Regions identified: primary + failover (if applicable) | Day One | | |

> **Practitioner note:** Bedrock cross-region inference is enabled by default and may not be disableable at the model level. If the client has hard data residency constraints (e.g. data must not leave ap-southeast-2), confirm enforcement early. This is a common surprise for regulated industries.

### 1.4 Org, Accounts, SCPs & Permissions

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 1.4.1 | Account structure understood (standalone, or within an org with multiple OUs?) | Day One | | |
| 1.4.2 | SCPs reviewed — known service/region restrictions documented | Day One | | |
| 1.4.3 | Process and SLA for SCP exceptions / whitelisting documented | Day One | | |
| 1.4.4 | IAM roles for delivery team provisioned (federated SSO or direct) | Day One | | |
| 1.4.5 | Service roles for workloads (Lambda execution role, ECS task role, Bedrock invoke role) | Week 2+ | | |
| 1.4.6 | Cross-account access policies (if AI Shared Services model) | Week 2+ | | |

### 1.5 Connectivity & Routing

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 1.5.1 | VPC provisioned with private subnets and NAT gateway | Day One | | |
| 1.5.2 | Transit gateway / peering to shared services and data sources | Week 2+ | | |
| 1.5.3 | VPC endpoints for Bedrock, S3, Secrets Manager (avoid public internet path) | Week 2+ | | |
| 1.5.4 | Firewall rules / security groups for integration targets (APIs, databases) | Week 2+ | | |
| 1.5.5 | DNS resolution for internal services confirmed | Week 2+ | | |
| 1.5.6 | Outbound internet access (if needed for external APIs, package registries) | Day One | | |

### 1.6 UI / Front-End Infrastructure

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 1.6.1 | Internal or external application? (Determines auth and network exposure) | Day One | | |
| 1.6.2 | CloudFront distribution required? | Week 2+ | | |
| 1.6.3 | WAF required? (Note: must be provisioned in us-east-1 for CloudFront association) | Week 2+ | | |
| 1.6.4 | Integration with existing client UIs or standalone? | Day One | | |
| 1.6.5 | Auth integration: Cognito, Entra ID, client IdP, or other? | Week 2+ | | |

---

## 2. Platform Decisions (Confirm Early)

These are architectural decisions that should be resolved in the first 1–2 weeks. Deferring them creates rework.

| # | Decision | Options to Consider | Status | Decision |
|---|----------|---------------------|--------|----------|
| 2.1 | Bedrock isolation model | Dedicated AI Shared Services account vs. project-scoped account | | |
| 2.2 | AI Gateway pattern | Centralised gateway for routing, rate limiting, logging, cost attribution — or direct invocation? | | |
| 2.3 | Multi-agent orchestration | Bedrock Agents, AgentCore, LangGraph, CrewAI, custom, or other? | | |
| 2.4 | LLM observability approach | Native (CloudWatch + X-Ray), Langfuse, LangSmith, or custom? | | |
| 2.5 | IaC approach | CDK, Terraform, CloudFormation, or client-mandated? | | |
| 2.6 | Vector store selection | OpenSearch Serverless, Aurora pgvector, Pinecone, or other? | | |

> **Practitioner note:** Decision 2.1 (isolation model) has downstream impact on networking, IAM, cost attribution, and blast radius. Don't default to "we'll just put it in the project account" without considering whether the client wants to reuse the AI platform for other workloads later.

---

## 3. SOP & Developer Tooling

### 3.1 Developer Access

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 3.1.1 | Access method: client laptop, virtual desktop, or BYO + VPN? | Day One | | |
| 3.1.2 | VPN / network access provisioned and tested | Day One | | |
| 3.1.3 | AWS console + CLI access confirmed (SSO role assumption working) | Day One | | |
| 3.1.4 | Access to client documentation (Confluence, Sharepoint, internal wikis) | Day One | | |
| 3.1.5 | Access to client comms (Teams, Slack, or equivalent) | Day One | | |

### 3.2 Development Tooling

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 3.2.1 | IDE: Kiro, Claude Code, VS Code, or client-mandated? | Day One | | |
| 3.2.2 | Git repository provisioned (GitHub, GitLab, CodeCommit, Bitbucket) | Day One | | |
| 3.2.3 | CI/CD pipeline: templates, artifact store, deployment targets | Week 2+ | | |
| 3.2.4 | Secrets management: Secrets Manager, Parameter Store, or other | Week 2+ | | |
| 3.2.5 | Package registries: can team access PyPI, npm, or equivalent? (Proxy/mirror?) | Day One | | |

---

## 4. Data & Integration Readiness

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 4.1 | Data classification confirmed (what level applies to data the agents will process) | Day One | | |
| 4.2 | API specs / schemas available for at least one integration target | Day One | | |
| 4.3 | Sample or synthetic data available for development | Day One | | |
| 4.4 | Integration team contacts identified (per target system) | Day One | | |
| 4.5 | Integration SLA understood (how long to get API access, test credentials) | Day One | | |
| 4.6 | Data format: MCP schema, OpenAPI spec, GraphQL, or custom? | Week 2+ | | |
| 4.7 | PII/PHI handling requirements (masking, tokenisation, DLP) | Day One | | |
| 4.8 | Encryption requirements: at rest and in transit (CMK vs. AWS-managed) | Week 2+ | | |

> **Practitioner note:** "Sample data" doesn't need to be perfect — even a few representative records or a schema document is enough to start building. The alternative is building blind and reworking when real data arrives.

---

## 5. Alignment of People

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 5.1 | Client-side infra staff: dedicated to project, or requests go to BAU queue? | Day One | | |
| 5.2 | If BAU queue: documented SLA for infra requests | Day One | | |
| 5.3 | Integration / API owners: identified and introduced (with timezone noted) | Day One | | |
| 5.4 | Security / governance contact: who approves model usage, data classification, network changes? | Day One | | |
| 5.5 | Escalation path: when requests are blocked or SLA missed, who do we go to? | Day One | | |
| 5.6 | Product owner / business stakeholder available for regular showcases? | Day One | | |

> **Practitioner note:** The single biggest velocity risk is not technical — it's whether infra requests sit in a BAU queue with a 2-week SLA while sprint commitments assume 2-day turnaround. Surface this in week one and agree a working model.

---

## 6. Security & Compliance

| # | Item | Priority | Status | Notes |
|---|------|----------|--------|-------|
| 6.1 | AI governance framework: does the client have one? What approvals are needed? | Day One | | |
| 6.2 | Data must not be used to train base models — confirmed and enforceable? | Day One | | |
| 6.3 | Audit logging requirements: query/response logging for LLM interactions | Week 2+ | | |
| 6.4 | Role-based access control: admin vs. developer vs. end-user boundaries | Week 2+ | | |
| 6.5 | Penetration testing / security review required before go-live? | Week 2+ | | |
| 6.6 | Compliance frameworks applicable (SOC2, HIPAA, industry-specific)? | Day One | | |

---

## 7. Client "Ready Day One" Summary

Send this to the client ahead of engagement start as the minimum set:

| # | We need this before the team starts | Why |
|---|--------------------------------------|-----|
| 1 | Non-prod account with network connectivity | Can't deploy anything without it |
| 2 | At least one foundation model activated + FTU done | Can't develop agents without model access |
| 3 | Developer access method provisioned and tested | Team is blocked until they can log in |
| 4 | Data residency and classification confirmed | Affects region, model selection, and logging design |
| 5 | Sample data or API specs for ≥1 integration target | Need something to build against |
| 6 | Named contacts: platform, security, integration | Unblocks requests and decisions |
| 7 | Documented process for service/change requests + realistic SLA | Sets expectations on turnaround |

---

## Revision History

| Version | Date | Author | Change |
|---------|------|--------|--------|
| 0.1 | 2026-08-21 | — | Initial draft from ECH review and planning session notes |
