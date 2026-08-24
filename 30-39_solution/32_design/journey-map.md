# Marketing Content Supply Chain — Journey Map

**Status:** Draft
**Source:** journey-map.png (10-19_engagement/11_governance/)

---

## OKR

| # | Objective | Key Result |
|---|---|---|
| 1 | Streamline the E2E content supply chain | Reduce new asset/campaign creation from 12 weeks to 6 weeks |
| 2 | Reduce reliance on 3rd parties for campaign and asset creation | Reduce external production spend by 50% YoY OR >50% of all campaigns/assets created using generative AI |
| 3 | Sustain or better quality of content | Increase average engagement rates (CTR/time on site, depending on channel) by 5% YoY |
| 4 | Improve employee experience across the content supply chain | ESAT >4 out of 5 measured annually |

---

## Process Stages

| Stage | Description |
|---|---|
| **Planning** | Defining campaign objectives, target audiences, and overall strategy. Creating briefs and aligning stakeholders before content work begins. |
| **Copy** | Drafting and refining campaign messaging, adapting content for different channels, and preparing references for Compliance. |
| **Design** | Designing visual assets (emails, CRMs, banners, print), formatting content for different platforms, and preparing files for submission. |
| **Compliance** | Reviewing content for medical, legal, and regulatory accuracy. Submitting reference packs, managing revisions, and ensuring approvals. |
| **Publishing** | Releasing approved assets across channels (e.g. SFMC, Oncore, publisher portals). Coordinating with vendors and ensuring correct formatting. |
| **Reporting** | Tracking campaign performance using dashboards, analytics, and engagement data. Feeding insights back into planning for future campaigns. |

---

## Swim Lanes (Actors)

### 1. Vendors

Vendors support the final step of the workflow: distributing approved content across channels such as media placements. Their focus is on execution rather than creation, but they depend heavily on smooth handoffs from upstream teams, and last-minute changes often slow them down.

### 2. Creative Agency

Agencies handle creative execution when work is outsourced. They produce copy, designs, and assets based on briefs they often receive incomplete. Agencies navigate brand guidelines, compliance needs, and require clear guidance to deliver on brand outputs.

### 3. Marketing & Execution

The marketing team is the central orchestrator, pulling together to plan, create, and deliver campaigns. Marketing develops strategy, briefs agencies (or produces content directly), and balances compliance requirements. Execution ensures content goes live by managing systems, publishing pages, and scheduling delivery. Their constant challenge is balancing time, cost, and quality while dealing with fragmented tools, manual processes, and limited resources.

### 4. Reviewers

Medical, Legal, and Regulatory (MLR) reviewers evaluate all content to ensure it meets compliance standards. Their work is essential but often manual, repetitive, and overloaded as content grows, leading to bottlenecks and rework.

### 5. Tools

The technology layer across each stage — the systems used by each actor to perform their activities.

### 6. KPI (Metrics)

Key performance indicators measured at each stage of the supply chain.

---

## Detailed Flow by Stage

### Planning

| Actor | Activities |
|---|---|
| **Marketing & Execution** | Define campaign objectives → Identify target audience → Conduct desktop research → Review past campaign performance → Gather existing materials → Generate content ideas / draft initial outline using AI → Create campaign brief → Align stakeholders on brief |
| **Creative Agency** | — (not yet engaged) |
| **Reviewers** | — (not yet engaged) |
| **Vendors** | — (not yet engaged) |

**Tools:** Sherlock, SharePoint, Snowflake/CDH, Google Analytics 4, Copilot Researcher

**KPIs:**
- % strategic alignment of briefs
- Brief quality: % of briefs complete on first submission
- Time to brief approval

---

### Copy

| Actor | Activities |
|---|---|
| **Marketing & Execution** | Write/edit copy internally OR brief agency → Draft campaign messaging → Adapt copy for channels → Prepare reference pack for compliance |
| **Creative Agency** | Receive brief → Develop copy concepts → Produce copy drafts → Iterate based on feedback |
| **Reviewers** | — (not yet engaged at this stage, but references being prepared) |

**Tools:** Copilot Researcher, Content Engine, Microsoft 365, BeeFree

**KPIs:**
- Number of revision cycles
- Copy approval time
- % of copy generated using AI vs. manual/agency

---

### Design

| Actor | Activities |
|---|---|
| **Marketing & Execution** | Brief design requirements → Review creative outputs → Approve or request revisions → Prepare files for compliance submission |
| **Creative Agency** | Receive design brief → Create visual assets (emails, banners, print, CRMs) → Adapt templates → Format for platforms → Deliver final assets |
| **Reviewers** | — (not yet engaged) |

**Tools:** Canva, Figma, PowerPoint, ShamanGo, Anthill Activator, Adobe Creative Suite

**KPIs:**
- Design delivery time vs. SLA
- % assets built from templates
- Number of design revision rounds

---

### Compliance

| Actor | Activities |
|---|---|
| **Marketing & Execution** | Submit content + reference pack to Veeva Vault PromoMats → Respond to reviewer queries → Manage revisions → Resubmit |
| **Creative Agency** | Respond to compliance feedback → Make required copy/design changes → Resubmit updated assets |
| **Reviewers** | Review content for medical accuracy → Check legal compliance → Validate regulatory requirements → Approve / request changes → Final sign-off |

**Tools:** Veeva Vault PromoMats (VVPM)

**KPIs:**
- Approval cycle time
- % first-time approval rate
- Number of review rounds per asset
- Reviewer workload (items per reviewer)

---

### Publishing

| Actor | Activities |
|---|---|
| **Marketing & Execution** | Schedule and publish approved content → Configure journeys/campaigns in platforms → QA published assets → Coordinate vendor distribution |
| **Creative Agency** | — (limited involvement; may support technical publishing) |
| **Reviewers** | — (post-approval, no involvement) |
| **Vendors** | Receive approved assets → Distribute across media channels → Confirm placement → Report delivery |

**Tools:** Salesforce Marketing Cloud (SFMC), Veeva Oncore CRM, Drupal, Publisher portals, Aprimo/Fuse

**KPIs:**
- Publishing lead time (approval to live)
- % content published on schedule
- Number of platform issues causing delay

---

### Reporting

| Actor | Activities |
|---|---|
| **Marketing & Execution** | Monitor campaign performance → Compile reports → Analyse engagement data → Feed insights back into planning for next campaign |
| **Vendors** | Provide publisher/media performance reports |

**Tools:** Google Analytics 4, Snowflake/CDH, Salesforce Marketing Cloud, Veeva Oncore CRM, 3rd party publisher dashboards

**KPIs:**
- Campaign engagement rate (CTR, time on site)
- Content effectiveness score
- ROI per campaign
- Insights actioned in next campaign cycle

---

## Archetypes (Personas)

| Persona | Role | Archetype | Key Pain |
|---|---|---|---|
| **Marketing Manager** | Drives campaign strategy, develops briefs, coordinates agency and internal teams | The Orchestrator | Spends more time reviewing sub-optimal content than on strategy |
| **CRM Analyst** | Owns CRM database health, manages segmentation, builds lifecycle campaigns | The Optimiser | Needs a single source of truth for customer interactions |
| **Reviewer (MLR)** | Reviews and approves content for compliance | Medical/Legal/Regulatory | Everything lands on their desk at once |
| **Digital Marketing Specialist** | Builds and publishes campaigns across digital platforms | The Executor | Juggling multiple platforms; everything feels slow |

---

## Deliverables & Content Types

### Email
| Type | Example |
|---|---|
| EDM/Triggered emails | Newsletters, invitations, event follow-ups |
| CRM/Journey emails | Onboarding, nurture sequences |

### Digital
| Type | Example |
|---|---|
| Owned digital | HCP portal content, microsites |
| Conference/Events | Digital event assets, registrations |

### Web & Digital
| Type | Example |
|---|---|
| Webpages | Landing pages, content hubs |
| Social | Paid and organic social assets |

### Print
| Type | Example |
|---|---|
| Detail aids | Leave-behinds, sales materials |
| Conference assets | Booth displays, congress materials |

---

## Tools Overview

### Planning, Communications & Insights
- Sherlock (insights & strategy)
- Snowflake / CDH (data warehouse)
- Google Analytics 4
- Copilot Researcher
- SharePoint

### Content Creation & Design
- Content Engine
- Canva
- Figma
- PowerPoint
- BeeFree
- ShamanGo
- Anthill Activator
- Adobe Creative Suite

### Content Management & Review
- Veeva Vault PromoMats (VVPM)
- Aprimo / Fuse
- SharePoint (DAM)

### Publishing & Distribution
- Salesforce Marketing Cloud (SFMC)
- Veeva Oncore CRM
- Drupal
- Publisher portals

### Data & Analysis Infrastructure
- Snowflake / CDH
- Google Analytics 4

---

## Pain Points Summary (Cross-Stage)

| Stage | Key Pain Points |
|---|---|
| Planning | No unified search for past assets; planning disconnected from reporting; manual audience segmentation |
| Copy | Limited AI drafting capability; generic agency outputs; manual reference packing |
| Design | No in-house capability; GenAI outputs prohibited; lack of templates; limited tools |
| Compliance | Manual referencing/anchoring; issues detected late; slow review cycles; reviewer overload |
| Publishing | Platform issues delay go-live; no AI-generated journeys; campaigns built from scratch; multiple disconnected platforms |
| Reporting | Manual compilation; no deep insights; campaigns not personalised beyond simple segments |

**Systemic Issues:**
- Disjointed workflows
- Siloed tools
- Governance bottlenecks
- Fragmented data and weak integration
- Tools not fit for purpose fueling reliance on agencies and external reviewers

---

## Current Duration: 14 Weeks End-to-End

| Step | Duration |
|---|---|
| Research + Planning | 4 weeks |
| Content + Channel Planning | 2 weeks |
| Writing + Reference Packing | 2 weeks |
| Creative + Technical Design | 1 week |
| Compliance Review | 2 weeks |
| Publishing (Internal) | 3 hours |
| Reporting (External) | 2 weeks |
| Reporting (Internal) | 1 week |
| Publishing (External) | 1 week |

**Target (OKR 1):** Reduce from 12 weeks to 6 weeks.
