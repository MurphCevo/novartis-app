# RFP Appendix A  - Project Autopilot Vision & Context

---

## The Challenge

Novartis' marketing operations for its long tail of 70+ brands are critically constrained by limited marketing capacity, manual workflows, and fragmented technology.

| Challenge | Impact |
|---|---|
| **Limited Marketing Capacity** | 2 brand managers for 70+ brands → burnout risk, reactive planning |
| **Fragmented Processes** | Manual, fragmented workflows → duplication, slow turnaround |
| **Compliance Complexity** | Complex validation → compliance delays, non-compliance risk |
| **Data Silos** | Scattered data → poor visibility, weak personalisation |
| **High Vendor Dependence** | Reliance on external agencies & reviewers → high cost, slow delivery |

---

## Service Blueprint

A cross-functional service blueprint mapping the end-to-end content supply chain journey.

**Stages:** Planning → Copy → Design → Compliance → Publishing → Reporting

**Actors:** Vendors, Creative Agency, Marketing & Execution, Reviewers, Tools, KPIs/Metrics

**Research Phase (highlighted):**

| Actor | Activity |
|---|---|
| Marketing Manager | Conduct desktop research to inform campaign strategy and identify target audience / associations |
| Marketing Manager | Gather existing materials |
| Marketing Manager | Generate content ideas or draft initial content outline using AI |

**Pain Points identified:**

- Time-consuming to review new trials/info; lack of TA insights
- Manual processes; disconnected platforms; planning is short-term

> See full version: Artefacts/Novartis-ProjectAutopilot_Service-Blueprint

---

## User Journey

**Total end-to-end duration: 14 weeks**

| Step | Activity | Duration | Teams |
|---|---|---|---|
| 1 | Research Internal + External Sources | 4 weeks | MKTO, Agency |
| 2 | Content + Channel Planning | 2 weeks | MKTO, BASE, MLR, Agency |
| 3 | Writing + Reference Packing | 2 weeks | MKTO, Agency, MLR, SME |
| 4 | Creative + Technical Design | 1 week | MKTO, Agency |
| 5 | Compliance Review | 2 weeks | MKTO, Agency, MLR |
| 6 | Publishing (Internal) | 3 hours | MKTO, BASE |
| 7 | Reporting (External) | 2 weeks | MKTO, Agency, Distributor |
|  - | Reporting (Internal) | 1 week | MKTO |
|  - | Publishing (External) | 1 week | MKTO, Agency, Distributor |

**Pain Points (by step):**

1. **Research**  - No unified search. Relevant materials may get missed.
2. **Content + Channel Planning**  - Planning and reporting are disconnected. Manual segmentation.
3. **Writing**  - Limited Copilot queries. Generic agency and Content Engine outputs.
4. **Creative + Technical Design**  - No in-house capability for design. GenAI outputs prohibited. Lack of templates and limited tools.
5. **Compliance Review**  - Referencing/anchoring is manual and slow. Issues detected late in Veeva. Slow review cycles and content rework. Reviewers overloaded.
6. **Publishing (Internal)**  - Platform issues delay go-live. No AI-generated journeys; campaigns built from scratch. Multiple disconnected publishing platforms.
7. **Reporting (External)**  - Manual compilation; no deep insights. Campaigns not personalised beyond simple segments.

**Systemic issues:**

- Disjointed workflows. Siloed tools. Governance bottlenecks. Fragmented data and weak integration.
- Tools not fit for purpose fueling reliance on agencies and external reviewers.

> See slide 20 for the full service blueprint.

---

## Employee Impact

Behind every inefficiency are real employees spending more time managing processes than creating value.

| Persona | Archetype | Role | Quote |
|---|---|---|---|
| **Marketing Manager** | The Orchestrator | Drives campaign strategy, develops briefs, coordinates agency and internal teams. | "I spend more time reviewing sub optimal content creation than on strategy development." |
| **CRM Analyst** | The Optimiser | Owns CRM database health, manages customer segmentation, builds lifecycle campaigns, and tracks CRM performance. | "I need a single source of truth for customer interactions." |
| **Reviewer** | Medical/Legal/Regulatory | Reviews and approves content to ensure compliance with the Medicines Australia Code of Conduct, accuracy, and regulatory fit. | "I can't afford mistakes, but everything lands on my desk at once." |
| **Digital Marketing Specialist** | The Executor | Builds and publishes campaigns across digital platforms. | "Juggling multiple platforms is overwhelming. Everything feels slow and tedious." |

> See final report for the full list of archetypes.

---

## The Vision: Project Autopilot

| Today | Tomorrow |
|---|---|
| Content delivery relies on manual coordination across disconnected systems and vendors. | AI coordinates the workflow, so humans can focus on strategy, creativity, and oversight. |

**Today:** Marketer manually connects to Research Tools, Design Tools, Compliance Tools, Publishing Tools, Reporting Tools, and Vendors.

**Tomorrow:** Project Autopilot (Agentic AI) sits between the marketer and all tools, coordinating via AI agents. Vendor dependency is reduced (dashed line).

---

## What Is Autopilot: The AI Partner for Modern Marketing

**Project Autopilot** connects tools, data, and insights to simplify marketing and help teams work smarter while humans stay in control.

| Component | Form Factor | Description |
|---|---|---|
| **A. The Autopilot Assistant** | AI Agents + Chat Interface | An AI co-pilot that connects to marketing tools, surfaces insights, and handles routine tasks. More than a chatbot, it helps marketers act faster and focus on strategy. |
| **B. The Autopilot Workbench** | AI Agents + Web App | A central hub that brings together tools, data, and workflows. Gives teams real-time visibility and AI guidance, reducing manual coordination while they keep working in their existing tools. |

---

## Tangible Use Cases

**Readiness:** 🟢 High | 🟡 Medium | 🔴 Low

| # | Use Case | Description | Readiness |
|---|---|---|---|
| 1 | Insight Synthesis | AI summarises data into actionable insights | 🟡 |
| 2 | Predictive Planning | AI forecasts campaign results & resource needs | 🟡 |
| 3 | Audience Segmentation | AI clusters audiences by behaviour and engagement | 🟡 |
| **4** | **Knowledge Discovery** | **AI retrieves approved assets and insights fast** | **🟢** |
| **5** | **Content Generation** | **AI drafts compliant copy and creative variants** | **🟢** |
| 6 | Video Generation | AI generates short-form, compliant videos | 🔴 |
| 7 | Creative Personalisation | AI tailors content by persona, channel, and tone | 🔴 |
| 8 | Reference Tagging | AI links every claim to its approved source | 🟢 |
| 9 | Compliance Intelligence | AI checks claims and links evidence automatically | 🟢 |
| **10** | **Asset Tagging** | **AI auto-tags assets for easier search and reuse** | **🟢** |
| 11 | CX Journey Automation & Optimisation | AI generates campaign journeys | 🟡 |
| 12 | Performance Optimisation | AI monitors results and suggests improvements | 🟡 |
| 13 | Workflow Orchestration | AI automates task routing and approvals | 🟡 |

> See slide 27 for fully fleshed out use cases and tech approach.
>
> Note: Salesforce CDP is accelerated (some of these use cases might turn to yellow).

---

## Use Case #4  - Knowledge Discovery

| | |
|---|---|
| **Team** | Marketers & Medical |
| **# of Users** | 25 |
| **Stage** | Planning & Creation |
| **Readiness** | 🟢 High |
| **Solution** | AI Agent Skill |
| **Potential Impact** | Reduces duplication, improves quality, and accelerates compliant asset development |

**User Story:** As a Marketing Manager, I want AI to surface relevant approved assets, insights, and templates through intelligent search, so that I can quickly reuse existing materials and ensure consistency and compliance across campaigns.

### Our Current Process

- Teams rely on keyword searches or manual browsing in DAM systems to locate assets and templates.
- Results depend on exact tags or file names, making discovery inconsistent and time-consuming.
- Valuable materials are often overlooked, leading to rework, duplication, and inconsistent messaging across markets.
- Only ICE brands have access to human support from Production Hub to uncover new/soon to be released materials.

### Our Future

- Marketing teams want to find relevant, approved assets quickly, regardless of how they're titled or tagged.
- They want a search experience that understands their intent and retrieves the right materials even when phrasing varies (e.g. searching "HCP adherence campaigns" or "congress summary templates" yields the same relevant content).
- Teams also want the ability to easily reuse or adapt approved assets within their authoring tools, improving both speed and compliance in campaign development.
- This would help reduce duplication, ensure consistency across markets, and enable more efficient content creation.

### Expected Benefits

- Saves time by surfacing relevant, approved content instantly
- Reduces rework and ensures consistent messaging
- Promotes compliant reuse of verified materials
- Improves efficiency across planning and creation teams

### Pull Data From

- Digital Asset Management systems and repositories (VVPM, Aprimo (Fuse), SharePoint)
- Strategy & Journey Docs (Sherlock, SharePoint)
- Modular templates and content blocks (future state  - ShamanGo/Anthill Activator)
- Campaign performance and engagement data (CDH, SFMC, GA4, Veeva Oncore CRM, 3rd Party Publishers)
- Approved copy and claim libraries (future state  - VVPM)

### Dependencies

- Migration of data to SharePoint from G Drive
- Existing metadata frameworks and DAM infrastructure
- Semantic indexing of content repositories
- Integration with authoring and compliance tools
- Governance for tagging, version control, and expiry

---

## Use Case #5  - Content Generation

| | |
|---|---|
| **Team** | Marketers |
| **# of Users** | 20 |
| **Stage** | Creation |
| **Readiness** | 🟢 High |
| **Solution** | AI Agent Skill |
| **Potential Impact** | Reduce agency reliance. Savings from $5,000 to $15,000 for simple webpages, emails, and presentations. |

**User Story:** As a Marketing Manager, I want AI to generate compliant, high-quality copy and creative assets from existing templates and approved content, so that I can accelerate content development and reduce dependency on agencies while maintaining compliance and brand consistency.

### Our Current Process

- Content creation is largely outsourced to agencies and production hub. Internally, the IMB team has been experimenting with Copilot Researcher for copy ideation and early drafting, which has produced positive results for content quality and speed. Also, there is no approved image generation tool.
- However, once the draft is produced, there is no direct link to authoring tools such as Shaman Go, BeeFree, PowerPoint or internal content management systems like Drupal, SFMC. Manual copy-paste, reformatting, and compliance checks are required, extending production timelines.
- The Content Engine can technically generate content into templates, but adoption is low due to variable output quality and limited integration with production workflows.

### Our Future

- Marketing teams want to quickly generate high-quality, compliant draft content  - such as copy, imagery, or variants  - directly from approved templates and content blocks.
- They want to be able to prompt the AI with campaign objectives or audience details and receive well-structured drafts aligned with therapy area, tone, and compliance standards.
- Human reviewers will refine and approve the outputs within existing workflows, ensuring consistency and quality.
- Teams also want seamless integration between AI generation and authoring tools, allowing them to edit, version, and publish without manual transfer  - accelerating delivery across markets.

### Expected Benefits

- Reduces time to produce draft content and variants
- Improves consistency, tone, and compliance through fine-tuned AI models
- Decreases reliance on external agencies for routine copy creation and simple designs
- Enables faster adaptation of global materials for local markets
- Prepares the foundation for future workflow and authoring integration

### Pull Data From

- Approved content repositories (VVPM, Aprimo - Fuse)
- Modular templates and content blocks (ShamanGo/Anthill Activator)
- Product and claim databases for compliant reference (VVPM)
- Campaign performance and engagement data to guide tone and relevance (CDH, SFMC, GA4, Veeva Oncore CRM, 3rd Party Publishers)

### Dependencies

- Fine-tuning of AI content models on approved copy and brand tone
- Development of reusable modular templates for copy and assets
- Integration between AI tools and content management or authoring environments
- Governance for human review and compliance oversight

---

## Use Case #10  - Asset Tagging

| | |
|---|---|
| **Team** | Marketers |
| **# of Users** | 20 |
| **Stage** | Publishing |
| **Readiness** | 🟢 High |
| **Solution** | AI Agent Skill |
| **Potential Impact** | Save 20+ hours per campaign |

**User Story:** As a Marketing Manager or Production Hub/Agency employee, I want AI to automatically tag and classify content, so that assets are easy to find, track, and reuse globally.

### Our Current Process

- Asset tagging and metadata entry are largely manual (e.g. 30+ fields for each asset or page within an asset).
- Quality and consistency depend on the uploader, leading to incomplete or inconsistent tags.
- This makes it difficult to find approved materials or track where content has been reused.
- It also leads to incomplete content effectiveness insights (e.g. only ICE assets are completely tagged).

### Our Future

- Teams want new assets to be automatically tagged with consistent, meaningful metadata based on content type, therapy area, audience, key message, and usage context.
- They want the system to recognise visual, textual, and contextual cues so that relevant tags are suggested or applied automatically, aligned with the global metadata model.
- This would make assets instantly searchable, traceable, and ready for reuse  - without additional manual effort or risk of inconsistency.
- This would also ensure actionable insights are generated for future campaigns or campaign optimisation.

### Expected Benefits

- Improves asset discoverability and reuse
- Ensures metadata consistency across global systems
- Reduces manual tagging effort and errors
- Strengthens audit traceability and lifecycle control
- Be able to generate more actionable insights

### Pull Data From

- Digital Asset Management systems (VVPM, Aprimo)
- Metadata models and taxonomies (Excel)
- Historical usage and publication logs (CDH, Veeva Oncore CRM, Veeva Oncore Production)

### Dependencies

- Integration with DAM platforms and metadata models
- AI model training on existing taxonomies and content sets
- Governance for tag approval and change control

---

## Our Phased Path to Future

| | Phase 1: Efficiency & Foundation | Phase 2: Intelligence & Coordination | Phase 3: Transformation & Experience |
|---|---|---|---|
| **Theme** | Integrate & Optimise | AI Agents Implementation | Smart Workbench |
| **Focus** | Audit and connect existing tools; activate modular content and adopt reusable templates; develop pilot AI agents and integration capability. | Build AI Agents for identified use cases and centralise workflows and tool connectivity. | Build unified workspace integrating research, creation, compliance, publishing & reporting. Existing agents can be activated on demand to support campaign execution. |
| **Value** | Streamlined delivery and standardised data enabling early AI readiness. | Smarter coordination, data-driven insights, and early ROI. | Unified experience with embedded intelligence, compliance, and scalability. |
| **Expected Outcome** | A clean and connected data foundation that prepares the organisation for AI interfaces in later phases. | AI-assisted workflows with **The Autopilot Assistant (Chat)** interface, enabling marketers to interact with AI through chat. | A single **The Autopilot Workbench** interface that gives marketers a connected, visual workspace across all tools and data. |

> See slide 57 or the final report for more details.

---

## Next Steps: Phase 1

### Step 1  - Pilot AI Agent POCs

Build 2 -3 agent "skills" to validate integration logic.

- **Mobilise Autopilot Project Manager**  - Formalise initiative under a named PM, translate discovery insights into RFP brief, align stakeholders (S)
- **Design and Run Targeted AI Pilot POCs**  - Select 2 -3 AI Agent POCs (Knowledge Discovery, Asset Tagging, Content Generation), test pull/push flows (M)

### Step 2  - Tech Deep Dive

Understanding the target systems and integration capabilities.

- **Conduct Deep Technical Discovery & Integration Mapping**  - Perform technical assessment after pilot learnings: APIs, auth layers, tool readiness, integration constraints. (L)

### Step 3  - Full Roadmap

Mapping the systems to the use cases to build the roadmap.

- **Build the Full Roadmap & Implementation Plan**  - Consolidate business, pilot, and technical insights; define phased rollout plan and Go/No-Go criteria. (L)
- **Dependencies:** Several tools in the marketing stack are being replaced or upgraded. Integration readiness will influence roadmap sequencing and timing.

> **Important:** Timelines will be defined after POC scoping and technical dependency mapping. Current sizing reflects **relative effort**, not duration.

> See final report for more details.
