# [Service Name] — Service Design Package

## Document Control

| Field            | Value                                  |
| ---------------- | -------------------------------------- |
| Document ID      |                                        |
| Version          | 1.0                                    |
| Status           | Draft / In Review / Approved / Retired |
| Service Owner    |                                        |
| Author(s)        |                                        |
| Created Date     |                                        |
| Last Reviewed    |                                        |
| Next Review Date |                                        |
| Classification   |                                        |

**Version History**

| Version | Date | Author | Description   |
| ------- | ---- | ------ | ------------- |
| 1.0     |      |        | Initial Draft |

**Approvals**

| Name / Role | Date |
| ----------- | ---- |
|             |      |

---

## Table of Contents

- [1. Purpose & Scope](#1-purpose--scope)
- [2. Service Overview](#2-service-overview)
- [3. Solution Architecture](#3-solution-architecture)
- [4. Service Level Agreements](#4-service-level-agreements)
- [5. Service Management Processes](#5-service-management-processes)
- [6. Service Support & Maintenance](#6-service-support--maintenance)
- [7. Business Continuity & Disaster Recovery](#7-business-continuity--disaster-recovery)
- [8. Identity, Access & Data Protection](#8-identity-access--data-protection)
- [9. Security & Compliance](#9-security--compliance)
- [10. Risk Register](#10-risk-register)
- [11. Service Transition](#11-service-transition)
- [12. Service Cost & Charging](#12-service-cost--charging)
- [13. Review & Continuous Improvement](#13-review--continuous-improvement)
- [Appendices](#appendices)

---

## 1. Purpose & Scope

<!-- Guidance: Explain why this document exists and what is in/out of scope. This is especially useful for handover scenarios where the receiving team needs boundaries. -->

**Purpose**

<!-- What is this document for? e.g. "This document defines the service design for [Service Name] to support operational handover and ongoing service management." -->

**In Scope**

- 

**Out of Scope**

- 

**Document Relationships**

| Related Document | Location / Link | Relationship |
| ---------------- | --------------- | ------------ |
|                  |                 |              |

---

## 2. Service Overview

<!-- Guidance: High-level description of the service, its purpose, and the business value it delivers. -->

| Field                    | Value |
| ------------------------ | ----- |
| Service Name             |       |
| Service ID / AppServ_ID  |       |
| Service Owner            |       |
| Technical Owner          |       |
| Business Owner           |       |
| Support Group            |       |
| Criticality              |       |
| Sensitivity              |       |
| Service Status           |       |

**Service Description**

<!-- What does this service do, and who does it serve? -->

**Business Justification & Value**

<!-- Business case and expected value outcomes. -->

**Dependencies**

| Service / Dependency | Type (upstream / downstream) | Owner | Notes |
| -------------------- | ---------------------------- | ----- | ----- |
|                      |                              |       |       |

---

## 3. Solution Architecture

<!-- Guidance: Complete this section to the level of detail appropriate for the service's criticality. For low-criticality services, a context diagram and interface inventory may suffice. -->

### System Context

<!-- DIAGRAM: {filename}.drawio/{tab-name} -->

**Business Context**

<!-- How does this service fit within the broader business landscape? -->

**Architectural Context**

<!-- DIAGRAM: {filename}.drawio/{tab-name} -->

### Infrastructure Components

| Component | Type | Specification | Location / Environment |
| --------- | ---- | ------------- | ---------------------- |
|           |      |               |                        |

### Integration Points

| Interface | System | Protocol / Method | Direction | Notes |
| --------- | ------ | ----------------- | --------- | ----- |
|           |        |                   |           |       |

### Data

<!-- Guidance: Complete if applicable — describe database technologies, key data entities, and how data moves through the system. -->

**Database Design**

**Data Flows**

<!-- DIAGRAM: {filename}.drawio/{tab-name} -->

### Configuration Items (CIs)

| CI Name | CI Type | CI Class | Owner | CMDB Reference |
| ------- | ------- | -------- | ----- | -------------- |
|         |         |          |       |                |

---

## 4. Service Level Agreements

<!-- Guidance: Define agreed service quality targets. If no formal SLA exists, document operational expectations. -->

| Metric                    | Target |
| ------------------------- | ------ |
| Availability Target       |        |
| Performance Target        |        |
| Support Hours             |        |
| Incident Response (P1)    |        |
| Incident Response (P2)    |        |
| Incident Response (P3/P4) |        |
| Resolution Time (P1)      |        |
| Resolution Time (P2)      |        |
| Peak Usage Times          |        |
| Critical Periods          |        |

---

## 5. Service Management Processes

<!-- Guidance: Define how each ITSM process applies to this service. Complete only sections relevant to the service's maturity and criticality. -->

### Incident Management

| Field                    | Value |
| ------------------------ | ----- |
| Incident Classification  |       |
| Escalation Path          |       |
| On-Call Roster           |       |
| Major Incident Process   |       |

### Request Fulfilment

| Field                 | Value |
| --------------------- | ----- |
| Service Request Types |       |
| Fulfilment Process    |       |
| Self-Service Portal   |       |

### Problem Management

| Field                      | Value |
| -------------------------- | ----- |
| Known Errors / Workarounds |       |
| Problem Record Location    |       |
| Root Cause Analysis Process|       |

### Change Management

| Field                    | Value    |
| ------------------------ | -------- |
| Change Category          |          |
| CAB Review Required      | Yes / No |
| Standard Change Templates|          |
| Emergency Change Process |          |

### Configuration Management

| Field                   | Value |
| ----------------------- | ----- |
| CMDB Reference          |       |
| CI Relationships        |       |
| Configuration Baseline  |       |

### Capacity & Performance Management

| Field                  | Value |
| ---------------------- | ----- |
| Current Capacity       |       |
| Growth Forecast        |       |
| Performance Thresholds |       |
| Capacity Review Frequency |    |

---

## 6. Service Support & Maintenance

### Support Model

| Tier                          | Detail |
| ----------------------------- | ------ |
| Tier 1 — Service Desk        |        |
| Tier 2 — Application Support |        |
| Tier 3 — Engineering / Vendor|        |
| Support Hours                 |        |
| After-Hours Support           |        |

### Escalation Path

| Tier 1 — Triage | Tier 2 — Support | Tier 3 — Engineering | Vendor / Specialist |
| ---------------- | ----------------- | -------------------- | ------------------- |
| →                | →                 | →                    | ✓                   |

### Maintenance Windows

| Field                        | Value |
| ---------------------------- | ----- |
| Scheduled Maintenance Window |       |
| Emergency Maintenance Process|       |
| Blackout Periods             |       |
| Communication Plan           |       |

### Monitoring & Alerting

| Field                          | Value |
| ------------------------------ | ----- |
| Infrastructure Health Monitoring|      |
| Application Health Monitoring  |       |
| Alerting Tool / Platform       |       |
| Alert Recipients               |       |
| Monitoring Dashboard Link      |       |

---

## 7. Business Continuity & Disaster Recovery

| Field                      | Value |
| -------------------------- | ----- |
| Recovery Time Objective (RTO) |    |
| Recovery Point Objective (RPO)|    |
| Business Impact Level      |       |
| Critical Periods           |       |

### Backup & Restoration

| Field                | Value |
| -------------------- | ----- |
| Backup Schedule      |       |
| Backup Location      |       |
| Retention Period     |       |
| Restoration Process  |       |
| Last Tested          |       |

### Disaster Recovery

| Field              | Value |
| ------------------ | ----- |
| DR Strategy        |       |
| DR Environment     |       |
| Failover Process   |       |
| DR Test Frequency  |       |
| Last DR Test Date  |       |
| DR Test Results    |       |

---

## 8. Identity, Access & Data Protection

<!-- Guidance: Complete if the service manages user identities, access controls, or sensitive data. -->

### User Access

| Field                   | Value |
| ----------------------- | ----- |
| Authentication Method   |       |
| Authorisation Model     |       |
| Access Request Process  |       |
| Access Review Frequency |       |

### Developer & Privileged Access

| Field                        | Value |
| ---------------------------- | ----- |
| Dev Environment Access       |       |
| Production Access Controls   |       |
| Privileged Access Management |       |

### Service Accounts

| Account Name | Purpose | Owner | Review Date |
| ------------ | ------- | ----- | ----------- |
|              |         |       |             |

### Data Protection

| Field                   | Value |
| ----------------------- | ----- |
| Retention Policy        |       |
| Retention Period        |       |
| Disposal Method         |       |
| Regulatory Requirements |       |
| Encryption at Rest      |       |
| Encryption in Transit   |       |
| Key Management System   |       |
| Key Rotation Schedule   |       |

---

## 9. Security & Compliance

<!-- Guidance: Document SSDLC controls and compliance requirements. For services without a CI/CD pipeline, focus on the regulatory and compliance sections. -->

### Regulatory & Compliance Requirements

| Requirement | Standard / Framework | Control Owner | Status |
| ----------- | -------------------- | ------------- | ------ |
|             |                      |               |        |

### Secure Development

| Field             | Value |
| ----------------- | ----- |
| SAST Tool         |       |
| DAST Tool         |       |
| Code Review Process |     |
| Security Champion |       |
| SBOM Location     |       |
| SBOM Format       |       |

### CI/CD Pipeline

| Field                | Value |
| -------------------- | ----- |
| CI Platform          |       |
| Build Triggers       |       |
| Automated Tests      |       |
| Security Gates       |       |
| CD Platform          |       |
| Deployment Strategy  |       |
| Rollback Procedure   |       |
| Environment Progression |    |

---

## 10. Risk Register

<!-- Guidance: Consolidate all identified risks here. Use the categories below or adapt to your organisation's risk taxonomy. -->

### Operational Risk

| ID  | Risk | Likelihood | Impact | Mitigation | Owner |
| --- | ---- | ---------- | ------ | ---------- | ----- |
|     |      |            |        |            |       |

### IT Risk

| ID  | Risk | Likelihood | Impact | Mitigation | Owner |
| --- | ---- | ---------- | ------ | ---------- | ----- |
|     |      |            |        |            |       |

### Vendor Risk

| ID  | Risk | Likelihood | Impact | Mitigation | Owner |
| --- | ---- | ---------- | ------ | ---------- | ----- |
|     |      |            |        |            |       |

### Security Risk

| ID  | Risk | Likelihood | Impact | Mitigation | Owner |
| --- | ---- | ---------- | ------ | ---------- | ----- |
|     |      |            |        |            |       |

---

## 11. Service Transition

<!-- Guidance: Complete when transitioning the service into live operation (new deployment or handover). Can be removed or marked N/A for services already in steady state. -->

### Deployment Approach

| Field               | Value |
| ------------------- | ----- |
| Deployment Strategy |       |
| Target Environment  |       |
| Deployment Lead     |       |
| Planned Go-Live Date|       |

### Testing & Validation

| Test Type | Scope | Owner | Pass Criteria | Status |
| --------- | ----- | ----- | ------------- | ------ |
|           |       |       |               |        |

### Training & Knowledge Transfer

| Field                    | Value    |
| ------------------------ | -------- |
| Training Required        | Yes / No |
| Target Audience          |          |
| Delivery Method          |          |
| Knowledge Base / Runbook Location | |

### Go-Live Criteria

| Criteria | Owner | Status | Notes |
| -------- | ----- | ------ | ----- |
|          |       |        |       |

### Early Life Support (ELS)

| Field            | Value |
| ---------------- | ----- |
| ELS Period       |       |
| ELS Owner        |       |
| ELS Exit Criteria|       |

---

## 12. Service Cost & Charging

<!-- Guidance: Complete if cost tracking or chargeback applies. -->

### Cost Model

| Field                       | Value |
| --------------------------- | ----- |
| Total Cost of Ownership (TCO)|      |
| Annual Run Cost             |       |
| Capital Cost                |       |
| Cost Centre                 |       |

### Charging Arrangements

| Field             | Value |
| ----------------- | ----- |
| Charging Model    |       |
| Billing Frequency |       |
| Chargeback Process|       |

---

## 13. Review & Continuous Improvement

### KPIs & Metrics

| KPI | Target | Measurement Method | Frequency | Owner |
| --- | ------ | ------------------ | --------- | ----- |
|     |        |                    |           |       |

### Review Schedule

| Field                        | Value |
| ---------------------------- | ----- |
| Operational Review Frequency |       |
| Strategic Review Frequency   |       |
| Next Scheduled Review        |       |
| Review Chair                 |       |

### Continuous Improvement Register

| Improvement Item | Priority | Owner | Target Date | Status |
| ---------------- | -------- | ----- | ----------- | ------ |
|                  |          |       |             |        |

---

## Appendices

### Appendix A — Glossary

| Term | Definition |
| ---- | ---------- |
|      |            |

### Appendix B — User & Account Register

| User Group | Role / Access Level | Volume | Operating Hours |
| ---------- | ------------------- | ------ | --------------- |
|            |                     |        |                 |

### Appendix C — Key Contacts

| Name | Role | Organisation | Contact Details |
| ---- | ---- | ------------ | --------------- |
|      |      |              |                 |

### Appendix D — References

- ITIL 4 Foundation
- ISO/IEC 20000
- Organisation IT Policy Framework
