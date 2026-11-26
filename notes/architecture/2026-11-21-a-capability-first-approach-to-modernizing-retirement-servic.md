---
type: Article Summary
title: A Capability-First Approach to Modernizing Retirement Services Technology Platforms
description: Modernization should start from a business capability assessment, ranking capabilities by maturity, criticality
  and strategic value before choosing platforms.
resource: https://www.architectureandgovernance.com/uncategorized/a-capability-first-approach-to-modernizing-retirement-services-technology-platforms
tags:
- architecture
- business-architecture
- bounded-context
- ai-agent
- strangler-fig
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.architectureandgovernance.com/uncategorized/a-capability-first-approach-to-modernizing-retirement-services-technology-platforms
  title: A Capability-First Approach to Modernizing Retirement Services Technology Platforms
  author: Karthikeyan Marappan
status: stable
source_url: https://www.architectureandgovernance.com/uncategorized/a-capability-first-approach-to-modernizing-retirement-services-technology-platforms
source: Architecture & Governance Magazine
source_type: blog
published: '2026-11-20'
domain: architecture
subdomain: business-architecture
concepts:
- bounded-context
- ai-agent
- strangler-fig
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81daa9ffc65a10c7e8f6
foundation_candidates:
- domain-driven design
- capability maturity model
- orchestration layer
---

## 🎯 Core thesis
Retirement services providers should assess their business capabilities, ranking them by maturity, criticality and strategic value, before they choose the platforms and architecture that will support them.

## 🧭 Why the author wrote it
Replatforming programs often start from the existing applications and end up reproducing the same operating limits in a new system. If the thesis is right, modernization is sequenced by business need, and each technology decision can be traced back to the capability it is meant to improve.

## Opening: Business Context Before Replatforming
> 🔎 Added context: The article has no heading over its opening paragraphs, so this title is added for reference. The standfirst states the thesis: "A capability-based assessment can help retirement services organizations identify modernization priorities, align technology investments with business needs and sequence change according to operational and regulatory risk."

### Replatforming Without Business Context
Replatforming discussions succeed most when they are aligned at the outset with how the business intends to grow, which products it will support and how it wants to serve its markets. Without that context, vendor evaluation, integration planning and migration decisions rest on an incomplete picture, so the new platform is likely to inherit the limitations the program was meant to resolve. The author's point is that alignment has to come before the platform choice, not after it.

> ❌ **Anti-pattern — Like-for-like replacement:** replacing aging technology and improving maintainability without addressing the operating limitations that prompted the investment. **Why people do it:** the article describes programs framed around aging technology, which makes the technology the starting point. **Consequence:** the new platforms "often struggle to support plan sponsors, serve participants, introduce products or manage regulatory requirements efficiently," and the business works around the same constraints. **Fix:** define the operating limitations and business direction first, then decide what the replacement must do.

### Client and Market Strategy as Requirements
Client strategy creates its own requirements. A provider serving small and midsize employers needs a different enrollment and servicing model from one supporting large plan sponsors through dedicated relationship teams. K-12 education and healthcare clients bring different plan structures, contribution requirements and distribution relationships. Once the direction is clear, modernization can be grounded in the capabilities needed to execute it: where the organization performs well, where existing systems limit it, and which gaps carry enough business or regulatory significance to justify investment.

## Start With What the Business Must Be Able to Do

### Business Capabilities
A business capability describes what an organization needs to be able to do, independent of the applications or systems that support it. Participant enrollment stays a core capability whether it runs on a legacy recordkeeping platform or on technology introduced by a modernization program. Calculating a required minimum distribution works the same way: the business obligation stays constant even when the calculation engine changes. Separating the capability from its technology is the first step of the analysis.

### Capabilities That Span Several Systems
The separation matters in retirement services because one activity often extends across several systems and functions. A hardship withdrawal may involve participant servicing, eligibility and compliance rules, payment processing and data held in several applications, all serving one obligation to the participant. Looking at each application on its own makes these dependencies hard to see, while treating the functions as one shared capability shows how they work together.

> ❌ **Anti-pattern — Application-by-application review:** assessing each application independently, even when several of them serve the same business activity. **Why people do it:** application inventories are the easiest starting point, and the article notes later that they do not reveal how work actually happens. **Consequence:** the dependencies among functions stay hidden. **Fix:** group applications under the shared business capability they serve.

### Mapping the Broader Operation
A recordkeeping platform may support much of plan and participant servicing, transactions, claims, investments and finance. Capabilities such as pre-sales and request for proposal (RFP) support, or new-business onboarding, are usually handled by CRM, sales-enablement and other systems. Mapping by capability makes these relationships visible and gives a more complete view of the technology that delivers them.

The article refers to Figure 1, which shows the broader capability landscape and the portion typically supported by the recordkeeping platform. Only its caption survives in the extracted text:

> Figure 1. A retirement services capability map shows how the typical recordkeeping platform supports only part of the broader business capability landscape.

### Seeing the Full Landscape Before Replacement
A capability map lets teams see the full operating landscape before they evaluate replacement platforms, which helps prevent the program from reproducing existing limitations. In a like-for-like replacement, newer technology can preserve operating gaps if the existing application structure becomes the basis for defining what the replacement should support. Understanding those gaps at the capability level gives teams a clearer basis for deciding which applications to retain, extend or replace.

> 🔎 Added context: "Like-for-like" means replacing a system with one that does the same job in the same shape. The article uses the term to describe a replacement that copies the existing structure rather than the business need.

## Invest According to Business Value

### Differentiating and Commodity Capabilities
Capability mapping is more useful when it reflects how the organization competes, because modernization funds and engineering capacity should not be spread evenly across every function. Some capabilities directly support the products, services or client experience that set the company apart. Others are essential to operations but can be handled well by standard solutions. The classification varies across providers because their product mix, client base and growth strategy differ.

The article gives two contrasting examples. An annuity provider may need to invest heavily in product pricing and payout administration, because those capabilities shape its competitive position. A company whose growth depends on K-12 or healthcare plan sponsors may place more value on sponsor servicing and participant communications, reflecting what those markets require. Essential functions such as general ledger accounting and document imaging can often be handled through standard solutions.

### Investment Shapes the Architecture
Investment decisions made at the capability level shape both the architecture and the modernization roadmap. An outsized investment in a commodity function takes resources away from capabilities that support growth or differentiate the business. Insufficient investment in a high-value capability leaves the organization constrained by the same limitations it set out to address. Product direction, distribution, client segments and growth priorities give technology teams the basis to decide where specialized investment is warranted and where standard solutions are enough.

> ❌ **Anti-pattern — Even distribution of investment:** spreading modernization dollars and engineering resources evenly across every function. **Why people do it:** the article does not say why, but evenly spread effort is the default when no capability classification exists. **Consequence:** high-value capabilities are underfunded and commodity functions absorb capacity. **Fix:** classify capabilities by how they support competitive position, then fund them accordingly.

> ❌ **Anti-pattern — Customizing commodity functions:** extensive customization of standard functions such as general ledger accounting or document imaging. **Why people do it:** the article does not say. **Consequence:** the customization adds cost and consumes engineering capacity without strengthening what distinguishes the business. **Fix:** use standard solutions for commodity functions.

## Turning Capability Analysis Into a Modernization Roadmap
The author uses a Capability-Application Alignment Model built around five related areas: capability inventory, maturity, criticality, application alignment and sequencing. The inventory needs enough detail to expose how work actually happens, and enough structure to guide investment decisions.

### Capability-Application Alignment Model
The five areas form one chain. The inventory defines the capabilities. Maturity shows how well each one runs today. Criticality shows how much a weakness matters. Application alignment links each capability to the systems that support it, and sequencing turns those findings into an order of work. The model is presented as the author's own practice ("In practice, I use..."), so it is one architect's method rather than a published standard.

### Building the Capability Inventory
Building the inventory takes structured work between business and technology teams. The author says that, in the author's experience, Domain-Driven Design workshops are an effective way to bring subject-matter experts together around the operating environment as it works in practice. The workshops examine business processes, people, applications and the upstream and downstream dependencies behind each capability. This often uncovers details that application inventories do not reveal. A process that looks automated may still depend on spreadsheets, manual reconciliation or the knowledge of a few employees. Several applications may support parts of one capability, and one application may hold responsibilities that belong to several business domains.

> ❌ **Anti-pattern — Inventory built from applications alone:** treating the application list as a description of the business. **Why people do it:** the application list is the artifact that already exists. **Consequence:** manual workarounds and shared responsibilities stay invisible. **Fix:** run structured workshops that walk through how each capability is actually delivered.

### Business Reference Architecture
A business reference architecture adds the technology view by mapping existing assets to the capabilities they support. The result is a shared picture of the current state that shows business and technology teams where the gaps are.

### Capability Maturity
Maturity can be assessed along a practical progression: manual, standardized, automated and eventually adaptive or AI-augmented. The author puts accuracy ahead of aspiration. A process that runs through the experience of employees who know how to work around system limitations is fundamentally different from a genuinely automated capability, even when both look successful on an executive dashboard.

> ❌ **Anti-pattern — Aspirational maturity scores:** recording the maturity a capability should have, not the maturity it has. **Why people do it:** the article does not say, though it contrasts such scores with what executives see on dashboards. **Consequence:** workaround-dependent processes look automated, and modernization is planned from a false baseline. **Fix:** score maturity from how the work actually runs.

### Criticality
Maturity is meaningful for planning only alongside criticality, because an immature capability may carry little business risk while another can directly affect regulatory compliance, participant experience or revenue. The author evaluates criticality by regulatory exposure, participant impact and cost or revenue sensitivity. Distribution processing shows the relationship well. An error can create compliance exposure, affect a participant directly and generate substantial remediation costs, while a temporary limitation in an internal reporting function carries considerably less operational risk. Evaluating maturity and criticality together gives a stronger basis for prioritization than application age alone.

### From Gaps to Application Decisions
Once the assessments are in place, capability gaps can be mapped to application decisions based on what the business actually needs to improve. The response may be to extend an existing application, introduce an integration layer, replace part of the legacy environment or move the capability onto a new platform. Taken together, these decisions establish the modernization sequence.

> 🔎 Added context: An integration layer is a shared set of components that mediates connections between systems, so that they do not have to connect to each other directly.

### Practical Constraints and Priority
Contract expirations, vendor availability and implementation windows will inevitably influence the roadmap. The article says they should not become the primary basis for deciding what is modernized first. The greater priority goes to capabilities with significant business exposure and large maturity gaps, where improvement reduces meaningful risk or creates operating value. That keeps the roadmap tied to business need rather than to the lifecycle of existing applications.

> ❌ **Anti-pattern — Sequencing by lifecycle:** ordering modernization by contract dates, vendor availability, implementation windows or application age. **Why people do it:** these pressures are real and visible, and the article acknowledges they will influence the roadmap. **Consequence:** the program may modernize convenient systems and leave high-exposure capabilities untouched. **Fix:** rank by business exposure and maturity gap, and treat contract and vendor timing as constraints on that order.

## Choosing the Modernization Path After the Capability Work
Capability priorities give the business context for deciding how the supporting technology should change. The answer does not have to be the same across the organization. Some parts of the legacy environment may warrant incremental replacement. Others can keep operating behind APIs or an integration layer. Certain capabilities may justify migration to a modern or SaaS platform. Each path has different implications for cost, implementation risk, technical debt and the control the organization keeps.

### Incremental Replacement
The author has found incremental modernization particularly useful. Replacing the legacy environment in stages lets the business keep operating throughout the transition. Teams can validate individual capabilities as they move into the new architecture, which reduces the disruption of a large migration. Careful sequencing is essential wherever old and new systems have to coexist for an extended period.

> 🔎 Added context: The article does not name the strangler fig pattern, but describes the same idea: staged replacement in which old and new systems run side by side.

### Wrapping a Reliable Legacy Core
Wrapping the core with APIs or an integration layer is a different path. It suits cases where the more immediate constraints are access to existing functionality and interoperability with newer applications. The organization can expose existing functions to newer applications and digital experiences without committing immediately to a full replacement. The flexibility is real, but the technical debt inside the core still has to be addressed.

### Full Migration to a Modern or SaaS Platform
A full migration may make sense when a large part of the environment no longer supports the capabilities the business needs, or when the organization has a compelling reason to consolidate technology at scale.

> ❌ **Anti-pattern — Judging platforms by breadth:** choosing a platform because of how much it covers. **Why people do it:** the article does not say, but breadth is an easy headline measure. **Consequence:** "Platform breadth alone ... is a poor measure of fit," because configuration, extensions, integrations and operating processes must still support the provider's particular products, clients and servicing model. **Fix:** bring the decision back to the capability analysis.

### Standard Solutions and Custom Engineering
The right path can vary by capability. Standardized solutions often serve commodity functions well, while differentiating capabilities sometimes warrant more control or custom engineering. The article adds that modern developer tooling and AI-assisted engineering are changing the economics of these choices, giving organizations more flexibility in deciding where a third-party platform provides enough value and where more internal control is justified.

### Traceability
A modernization strategy is more defensible when each technology decision can be traced back to the business capability it is meant to improve. That link gives leadership a practical basis for choosing among incremental replacement, integration around the legacy core, platform migration or custom development, according to the value, maturity and criticality of each capability.

## Capability Boundaries Can Strengthen API Design
The capability analysis can also inform API architecture, particularly where integrations have built up around individual applications over time. APIs designed around the data or applications they expose tend to carry existing system boundaries into the integration layer. That creates dependencies on technology the organization may later replace. Domain-Driven Design organizes APIs around the more durable business domains identified during the capability work.

### Three Foundational Layers
In retirement services, the article proposes three foundational layers. Data APIs provide controlled access to the underlying data stores. Business Service, or Domain, APIs hold the rules and data for areas such as employer or sponsor, participant, investments and funding sources. Transactions that need information or logic from several domains are coordinated through an orchestration layer, so that each domain keeps responsibility for its own business rules.

> ❌ **Anti-pattern — Application-shaped APIs:** designing APIs according to the data owned or exposed by individual applications. **Why people do it:** the article suggests integrations accumulate around applications over time. **Consequence:** existing system boundaries carry into the integration layer and tie it to technology that may be replaced. **Fix:** organize APIs around the business domains identified in the capability work.

### Business Rules Stay in Their Domain
Participant eligibility and contribution processing illustrate the structure. The rules for these activities remain in the relevant domain and apply consistently wherever the capability is used, which avoids several versions of the same logic embedded in front-end applications. Changes to the underlying data platform can then happen without moving the domain's rules, and a participant or sponsor interface can evolve without rewriting them.

### Why Stable Boundaries Matter
Stable domain boundaries let each layer evolve independently, even when platforms, interfaces and regulations change on different schedules. They stop an update in one part of the environment from becoming a broad integration effort. They also create clearer points for security and compliance controls. Authentication, authorization and least-privilege access still have to operate throughout the architecture, especially where participant financial and personal data is involved. Domain ownership helps establish where access requirements belong and who is accountable for them.

### AI Agents and Permissions
The article expects AI agents to interact directly with retirement-services APIs. Their permissions should correspond to the business activities they are authorized to perform, with narrow access, appropriate auditability and controls that reflect the sensitivity of the underlying data.

> ❌ **Anti-pattern — Broad agent access:** giving an AI agent access to APIs beyond the activities it is meant to perform. **Why people do it:** the article does not say. **Consequence:** the article's concerns about accountability, auditability and participant data sensitivity are not addressed. **Fix:** scope agent permissions to specific business activities, and audit them.

The capability and domain structure created during modernization gives organizations a practical foundation for defining these boundaries as AI takes on a larger role in retirement servicing.

## Modernization Should Leave the Business More Adaptable
Retirement services organizations will keep modernizing aging recordkeeping and servicing environments as business requirements shift, regulatory demands grow and legacy technology becomes harder to support. Cost, implementation time and disruption will shape those programs, but the larger measure of success is whether the resulting architecture lets the business support its products, clients and participants as their needs change.

### Before the RFP
Before an RFP or a target platform is committed, leadership should have a clear view of the capabilities the business needs, where the most consequential maturity gaps are and which areas carry the greatest regulatory, participant or financial exposure. Connecting these priorities to the applications that support them gives a defensible basis for deciding what to retain, extend, replace, purchase or build, and a modernization sequence grounded in business need.

### Enduring Capabilities Outlast Platforms
Platforms and SaaS offerings will change, regulations will reshape retirement operations, and AI will influence both servicing models and software development. The underlying needs stay the same: enrolling and serving participants, administering plans, processing transactions and meeting regulatory obligations. Architecture organized around these enduring capabilities can absorb technology change without forcing the business to reconsider its operating model at every major platform change.

### Intelligent Agents and Trust
The article closes by noting that intelligent agents will play a larger role in retirement servicing. The opportunity to automate and adapt has to coexist with strict requirements for compliance, security and participant trust.

## About the Author
The author is Director of Technical Delivery at Corebridge Financial and has 25 years of experience in business transformation, application modernization and technical delivery. The biography says the author leads modernization across recordkeeping, data integration, cloud migration, API-driven architecture, analytics and AI, and is an IEEE Senior Member who has presented technical papers at IEEE conferences. The article ends with four tags (API Architecture, Application Modernization, Business Capability Mapping, digital transformation) and an advertising placeholder. None of these carries an argument of its own.

## 🧠 Mental models
### Capability before application
**The model:** The business capability is what the organization must be able to do. Applications are one of several things that support it, and they can change while the capability stays the same.
**How to apply it:** Before an RFP, list the capabilities and map each application onto them, then look for gaps and for capabilities that depend on several systems.

### Maturity and criticality together
**The model:** Maturity says how well a capability runs. Criticality says how much its weakness matters. A low-maturity capability with little exposure can wait, while a moderately mature one with high regulatory or participant exposure may need to move first.
**How to apply it:** Score each capability on maturity, then rate criticality by regulatory exposure, participant impact and cost or revenue sensitivity, and prioritize large gaps in high-exposure capabilities.

### Three layers of APIs
**The model:** Data APIs expose the data stores. Domain APIs hold each business area's rules. An orchestration layer coordinates the transactions that cross domains.
**How to apply it:** Put each business rule in the domain that owns it, and keep cross-domain coordination in the orchestration layer so that the rules do not get copied into front-end applications.

## 🔑 Key takeaways
- Set the business direction, products and client segments before any platform evaluation, because a platform chosen without them inherits the limitations the program is meant to fix.
- Map capabilities across the whole operation, including the CRM and sales systems outside the recordkeeping platform, since the recordkeeping platform typically supports only part of the landscape.
- Rank capabilities by maturity and criticality together, with criticality judged by regulatory exposure, participant impact and cost or revenue sensitivity, rather than by application age.
- Assess maturity by how the work actually runs, not by aspirational scores, because staff workarounds can make a manual process look automated.
- Choose the modernization path capability by capability, among incremental replacement, wrapping the core with APIs or an integration layer, SaaS or platform migration and custom engineering.
- Organize APIs around business domains with Data APIs, Domain APIs and an orchestration layer, so business rules stay in one place and the integration layer does not inherit legacy system boundaries.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Business (capabilities, capability map, business reference architecture, products and client segments); Application (application alignment, API architecture, integration layer, retain, extend, replace or buy decisions); Data (Data APIs over data stores, changes to the data platform); Technology (SaaS and modern platforms, mapping of technology assets to capabilities). |
| Scope | Enterprise: the analysis covers the whole retirement services operation, from recordkeeping to CRM and finance, though it works at capability level. |
| Stakeholders and concerns | Leadership teams (investment and sequencing before an RFP); technology leaders (platform and integration choices); business and product teams (fit with products and client segments); compliance and regulatory owners (regulatory exposure); participants and plan sponsors (service and data protection); engineering teams (capacity consumed by commodity customization); owners of AI agent access (permissions and auditability). |
| Frameworks and notations | None named. The article uses Domain-Driven Design and a "business reference architecture" but does not name TOGAF, ArchiMate, BPMN, BIZBOK or C4. |

### Decisions and trade-offs
- **Business-first versus platform-first sequencing:** the author starts with capabilities and business direction before any vendor evaluation. The cost is a slower start, and it requires the business context to be gathered first.
- **Modernization path per capability:** the options are incremental replacement, wrapping the legacy core with APIs or an integration layer, migration to a modern or SaaS platform, and custom engineering. Each differs in cost, implementation risk, technical debt and the control kept, and the article says the answer varies by capability.
- **Standard solution versus custom engineering:** commodity functions use standard solutions, and differentiating capabilities may justify custom work. The cost of custom work is engineering capacity; the cost of standard solutions is less control.
- **Prioritization basis:** the author ranks by business exposure and maturity gap and treats contract, vendor and window constraints as limits. The cost is that the roadmap may conflict with contract timing.
- **API organization:** APIs are organized by business domain and coordinated by an orchestration layer, rather than by the data or applications behind them. The article does not state the cost of this structure, but it implies extra design work and the workshops needed to identify domains.
- **AI agent permissions:** the article recommends narrow, auditable permissions tied to business activities. The cost is that agents can do less without wider access, which the article does not weigh.

### Foundations it rests on
- [Business Capability](/foundations/business-capability.md) — The article uses capabilities as the unit for maturity and criticality scoring. Its maturity scale measures how work is done, which the working definition's "independent of who does it or how" does not cover.
- [Business Architecture](/foundations/business-architecture.md) — The article's capability map and business reference architecture are business-architecture artifacts, used here to set modernization priorities.
- [Architecture domain](/foundations/architecture-domain.md) — The article covers business, application, data and technology concerns, but it never names the TOGAF domains. Its use of "domain" for business areas such as participant and sponsor follows Domain-Driven Design, which is a different meaning.
- [Application Architecture](/foundations/application-architecture.md) — The article treats applications as groups of capabilities, and its retain, extend, replace and integration-layer decisions are application-architecture decisions.
- [Data Architecture](/foundations/data-architecture.md) — Data APIs provide controlled access to data stores, and the article treats data platform changes as separate from business rules.
- [Technology Architecture](/foundations/technology-architecture.md) — The article uses technology architecture for SaaS and modern platform choices, and for mapping technology assets to capabilities.

## 🏛️ For Kenji
This applies directly to architecture work at the Staff level, because the article shows how to connect business capability, investment and platform decisions in one traceable chain. It also applies to MBA disciplines in business strategy and operations, where capability investment is a strategic choice. Given your work on AI governance, the most useful part is the section on AI agent permissions. The open question is how you would scope an agent's permissions when a single task needs data and rules from several business domains. This is my interpretation, not the author's.

## 📚 Reading plan
### From the article
- [A Capability-First Approach to Modernizing Retirement Services Technology Platforms](https://www.architectureandgovernance.com/uncategorized/a-capability-first-approach-to-modernizing-retirement-services-technology-platforms) — The full original, which includes Figure 1 and the author's bio, neither of which made it into the extracted text. The article cites no other links.

### To go further
- Domain-Driven Design, strategic design (bounded contexts and context mapping) — The article relies on domain boundaries to organize APIs, and this is the theory behind them.
- Capability-based planning in TOGAF 9.2 (business architecture) — It places capability work inside the enterprise architecture method that the article's terms come from.
- Capability maturity models, with their manual-to-adaptive progression — They show how to score maturity consistently across capabilities.
- Strangler fig migration — The staged replacement that the article describes with coexisting old and new systems.

## Concepts
- [bounded-context](/concepts/architecture/bounded-context.md)
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [strangler-fig](/concepts/architecture/strangler-fig.md)

