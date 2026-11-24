---
type: Article Summary
title: When Transformation Succeeds but the Enterprise Fails
description: Successful transformations can still fail the enterprise; effective change needs architecture-led governance
  tying delivery to strategy, stakeholders and measured value.
resource: https://www.architectureandgovernance.com/applications-technology/when-transformation-succeeds-but-the-enterprise-fails
tags:
- architecture
- enterprise-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.architectureandgovernance.com/applications-technology/when-transformation-succeeds-but-the-enterprise-fails
  title: When Transformation Succeeds but the Enterprise Fails
  author: Holt Hackney
status: stable
source_url: https://www.architectureandgovernance.com/applications-technology/when-transformation-succeeds-but-the-enterprise-fails
source: Architecture & Governance Magazine
source_type: blog
published: '2026-11-23'
domain: architecture
subdomain: enterprise-architecture
concepts: []
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d815bb4eec91f34ca4958
foundation_candidates:
- architecture-governance
- technical-debt
- gap-analysis
---

## 🎯 Core thesis
A transformation can meet every delivery target and still leave the enterprise less coherent and harder to change, so the real test is whether the change was effective, and enterprise architecture is the structure that makes it so.

## 🧭 Why the author wrote it
The article starts from the "quiet failure" of many transformations: the programme is on time, on budget and to quality, yet the enterprise still struggles to scale and further change gets harder. If the thesis is right, success should be judged by whether the change was effective, with governance applied to the change itself, and that changes how transformations are governed and measured.

## 1. Context
### The quiet failure of transformation
The article's term for a programme that succeeds on paper, in reports and metrics, but not in the enterprise. Every programme runs under constant pressure to deliver faster, spend less and raise quality, and every stakeholder checks whether their requirements are met. Success is judged on those terms, so the programme looks finished even when the enterprise has not changed for the better. The damage does not show in execution reports; it surfaces later, when the next programme inherits the bill.

> ❌ **Anti-pattern — Success judged on delivery alone:** a change is accepted because targets were hit, with no question about how it was governed. **Why people do it:** the pressure to deliver faster and cheaper is constant, and the reports track exactly those targets. **Consequence:** duplicated components, siloed processes and compounding technical debt build up unseen. **Fix:** ask what governance was applied to the change and whether it was effective, not only whether it was delivered.

### Governance of the change itself
The question the article says is rarely asked is "What governance was applied to the change itself?" Governance here means the decisions that shape a change: whether it fits strategy, whether it duplicates what exists, and who holds the whole picture. The article's point is that governance can be absent even when a programme is well run. Without it, every target can be hit and the enterprise still gets worse.

> 🔎 Added context: "Technical debt" is the accumulated cost of shortcuts and inconsistency that makes later change slower and more expensive.

## 2. The Role of Enterprise Architecture in Driving Effective Change
### Enterprise architecture (EA)
The article defines EA as the discipline that exists to guide organisations towards effective change. It optimises systems and processes across the whole enterprise so they form one integrated environment that responds to change and supports business strategy. It also gives strategic context for how digital capability evolves. Its aim is to balance transformation with continuous operational efficiency, so business units can innovate safely inside an integrated strategy.

## 3. What We Mean by Effective Change
### Effective change
Effective change is not simply change that is delivered. It achieves the desired business outcome without creating disproportionate cost, risk or complexity elsewhere in the enterprise. The definition moves the test from output (what shipped) to outcome (what it did to the whole enterprise). The next four properties describe what meeting that test looks like.

### Purposeful
Every change must trace to a business driver or strategic objective. The article's rule is direct: if a change cannot be traced, it cannot be justified. This turns the purpose of each change into something that can be checked, not assumed.

### Coherent
A coherent change fits the capabilities, data, applications and technology already in place, and the other changes in flight. The article treats coherence as a property of the whole portfolio, not of one project in isolation. A change can be sound on its own and still be incoherent with what is running around it.

### Controlled
The right changes are made at the right level of governance. A minor adjustment is handled quickly, while a change that reshapes the enterprise is treated as an architectural event. The article's point is proportionality: the same process should not govern a tweak and a restructuring.

### Value-realising
The outcome is measured after delivery and judged by the benefits achieved, not by the milestones passed. This is the property that most often goes missing, because delivery reporting stops at go-live. The article makes value realisation an explicit, later check rather than a closing formality.

## 4. Why It Matters
### Strategic drift
Without a managed link to strategy, delivery follows its own direction, and the enterprise drifts away from its goals even while each project succeeds. The article names this as the first cost of ineffective change. The fix it implies is to keep a live connection between each change and the strategy it serves.

### Compounding complexity
Duplicated capabilities and inconsistent data definitions raise the cost of every later change. Each one is small when it is made, but the costs add up in the budgets of future programmes. This is the mechanism behind the "quiet failure" in Section 1.

> ❌ **Anti-pattern — Defensible local fixes:** each tactical fix or workaround is accepted because it is defensible in isolation. **Why people do it:** each one solves a real problem for the team that raised it, and no one sees the whole. **Consequence:** together they become the complexity that consumes future budgets. **Fix:** keep a view of the whole picture so that local decisions are tested against enterprise effects.

### Hidden risk
When no one holds the whole picture, regulatory, security and resilience exposure stays invisible until it becomes an incident. The article's claim is that the risk is not absent, only unseen. Architecture's role here is to make it visible before it becomes an incident.

### The ability to change well
The article concludes that the ability to change well is itself a competitive capability. This reframes architecture from a cost of governance into a source of advantage. An organisation that changes with purpose and coherence can respond faster, and one that does not carries its drift and debt with it.

## 5. How the Enterprise Architect Guides Effective Change
Enterprise architects do not make the decisions about change, and they do not deliver it. They create the framework, visibility and discipline within which change can be decided and delivered well. The article illustrates this with a global automotive manufacturer using PLM, ERP and MES platforms across digital engineering and manufacturing.

### The Architecture Development Method (ADM)
The TOGAF Standard describes how to develop architecture through the ADM, and the article's Figure 1 is a simplified view of its key steps, with security and risk integrated throughout. The figure itself is not in the extracted text, so the seven steps below are taken from the body text. The steps move from vision to roadmap to delivery and then to managing change after go-live.

> 🔎 Added context: The ADM is the iterative method in the TOGAF Standard for developing an enterprise architecture, and the article applies its steps to one change programme.

### Step 1. Develop the Architecture Vision
The architect anchors the change to business strategy, establishing principles and business drivers, and tests readiness with a business transformation readiness assessment. Security principles and risk appetite are agreed at this point, not retrofitted later. The example is a manufacturer that wants to cut time to market by 30% by replacing localised tools and processes with one global process. The vision is a modernised digital platform connecting PLM, ERP, MES and other tools in a digital thread.

> ❌ **Anti-pattern — Retrofitted security:** security principles and risk appetite are left until after the design is set. **Why people do it:** the article does not say; it implies security is treated as a later addition. **Consequence:** the risk posture is decided by default rather than by sponsors. **Fix:** agree security principles and risk appetite in the vision, before the design hardens.

> 🔎 Added context: A digital thread is the connected flow of product data across engineering and manufacturing tools, so that one piece of data serves each stage.

### Step 2. Identify Stakeholders and Their Concerns
The architect identifies who is affected by the change and records what each one cares about, including security and data-protection stakeholders. The example lists four concerns. Engineering needs the correct version of data and workflows that do not reduce productivity. Infrastructure wants cloud used wherever possible. Finance needs the programme to stay within the approved budget. Information Security needs intellectual property protected across regions and suppliers.

### Step 3. Develop Current and Target States
The architect develops the baseline and the target across business, data, application and technology layers. Gap analysis shows what must be created, changed, retired or retained, and the gap is the real scope of the change. A security risk assessment shapes the target security architecture. In the example, regional PLM instances, local BOM processes and point-to-point interfaces give way to one global PLM backbone integrated with ERP and MES.

### Step 4. Provide the Roadmap
The architect turns the gap into work packages, transition architectures and a prioritised migration plan. Risks are assessed, mitigation is built into the sequence, and residual risk is made visible to sponsors. In the example, critical product lines roll out first, then legacy systems are retired. The highest data-protection risks are addressed before engineering data crosses borders.

### Step 5. Address Stakeholder Concerns
The roadmap returns to each stakeholder through architecture views built for their concerns. Trade-offs are made explicit, and concerns that cannot be fully met become informed decisions for sponsors rather than surprises in delivery. Security views show residual risk. The example gives Engineering performance and usability, Infrastructure hosting options, Finance cost and benefit by phase, and Security design compliance.

### Step 6. Guide Programme Delivery
The architect keeps delivery aligned with the architecture that justified the change. Two mechanisms do this. The first is an Architecture Contract with delivery teams, which makes the intent explicit. The second is an Architecture Board, which reviews compliance and approves justified deviations. With agile delivery, the architecture sets guardrails, teams iterate within them, and every backlog item traces to the roadmap. Security compliance checks and audits run alongside.

### Architecture Contract
An explicit agreement between the architecture and the delivery teams, stating the intent the teams must build to. In the article it is the first mechanism that keeps delivery aligned with the architecture that justified it. It makes deviations visible because the intent is written down.

> 🔎 Added context: The article does not define the Architecture Board's membership. In TOGAF terms it is a governance body that oversees architecture compliance, which is how the article uses it.

> ❌ **Anti-pattern — Unreviewed bespoke integration (as the example shows):** a team proposes a bespoke ERP interface outside the standard pattern. **Why people do it:** the article does not say; the example shows a team proposing one. **Consequence:** by the article's own logic it would reproduce the point-to-point interfaces the global backbone is meant to replace. **Fix:** the Board either aligns it to the standard integration pattern or grants a time-limited deviation.

### Step 7. Manage Architecture Change
After go-live, the architect confirms the architecture remains fit for purpose, monitors value realisation, reviews risk and controls, and decides, for each change request, whether it warrants a new cycle. In the example, the business reviews the delivered solution and proposes improvements, which become change requests that start a new cycle. Requirements management keeps every change traceable to a need, and alignment is retested at every step.

### The recurring question
The article asks one question in different forms at every step: does this still serve the outcome the business is trying to achieve? It is the thread that connects the vision, the stakeholders, the roadmap, delivery and the post-go-live review.

## 6. Conclusion
Change is inevitable, and effective change is a choice made through structure rather than good intentions. Enterprise architecture provides that structure: it ties change to strategy, hears stakeholders, describes the journey from current to target state, governs delivery with security and risk integrated, and checks that the promised value has arrived. The article's closing measure is whether the organisation changes with purpose and ends each cycle more coherent than it began.

> ❌ **Anti-pattern — Measuring architecture by its artefacts:** the function is judged by the number of models it produces or standards it enforces. **Why people do it:** the article does not say; artefacts are the visible output of the work. **Consequence:** the article says these are not the measure, so output can rise while coherence does not. **Fix:** judge the function by whether the organisation changes with purpose and becomes more coherent.

## 🧠 Mental models
### Proportional governance
**The model:** Governance should match the reach of the change. A minor adjustment is handled quickly, while a change that reshapes the enterprise is treated as an architectural event.
**How to apply it:** Before a change starts, decide whether it is minor or enterprise-shaping, and route it to the matching level of review.

### The recurring question
**The model:** At every stage of the cycle, ask whether the change still serves the outcome the business is trying to achieve.
**How to apply it:** Ask it at vision, stakeholder, roadmap, delivery and post-go-live reviews, and send any change that no longer serves the outcome into a change request for a new cycle.

## 🔑 Key takeaways
- Delivery on time, on budget and to quality is not the test; the article asks whether the change was effective.
- Effective change is purposeful, coherent, controlled and value-realising, and value is judged by benefits achieved, not milestones passed.
- Ineffective change compounds through strategic drift, duplicated capabilities, inconsistent data definitions and risk nobody sees.
- Enterprise architects create the framework, visibility and discipline for change; they do not decide or deliver it.
- Security principles and risk appetite are agreed at the Architecture Vision, not retrofitted later.
- Deviations from architecture are made through review: the Architecture Board aligns them or grants time-limited deviations.
- The measure of an architecture function is whether the organisation ends each cycle more coherent, not how many models or standards it produces.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Business (strategy, drivers, readiness, business layer in Step 3); Data (data definitions, digital thread, BOM, data protection); Application (PLM, ERP, MES, integration, point-to-point interfaces); Technology (cloud hosting, infrastructure). |
| Scope | Enterprise: the example replaces regional tools with one global process and platform across the manufacturer. A solution-level element (the ERP interface) appears only as an example in Step 6. |
| Stakeholders and concerns | Head of Engineering (correct data, productive workflows); Head of Infrastructure (cloud where possible); Head of Finance (budget held); Information Security (IP protected across regions and suppliers); sponsors (residual risk and trade-offs); Architecture Board (compliance and deviations). |
| Frameworks and notations | TOGAF Standard (ADM) and the TOGAF Series Guide: Integrating Risk and Security within a TOGAF Enterprise Architecture (named in the Figure 1 caption). No other notation named. |

### Decisions and trade-offs
- **One global process and platform versus localised tools:** the manufacturer replaces regional PLM instances, local BOM processes and point-to-point interfaces with one global PLM backbone integrated with ERP and MES, aiming to cut time to market by 30%. The article does not quantify the migration cost; its mitigation is the sequencing in Step 4.
- **Rollout sequence:** critical product lines first, then retirement of legacy systems, with the highest data-protection risks tackled before engineering data crosses borders. The cost is that the old and new systems must coexist during the rollout, which the article implies but does not spell out.
- **Bespoke ERP interface versus the standard integration pattern:** the Board either aligns the proposal to the standard pattern or grants a time-limited deviation. The cost of a deviation is a non-standard interface that lives until its time limit; the article does not say what happens at expiry.
- **Agile delivery within architectural guardrails versus team freedom:** teams iterate inside the guardrails, and every backlog item traces to the roadmap. The cost is traceability and compliance checks running alongside delivery, which the article presents as necessary.
- **Security agreed in the vision versus retrofitted later:** the article chooses early agreement of principles and risk appetite. The article states no cost; the implied cost is up-front effort in the vision phase.
- **Concerns that cannot be fully met:** surfaced as informed decisions for sponsors rather than delivery surprises. The cost is that sponsors must accept and own the residual trade-offs.

### Foundations it rests on
- [Enterprise Architecture](/foundations/enterprise-architecture.md) — the article uses it as the structure that aligns processes, information and technology with strategy and steers change, consistent with the working definition.
- [Architecture Domain](/foundations/architecture-domain.md) — the article applies the four TOGAF domains (business, data, application, technology) across its seven steps.
- [Architecture](/foundations/architecture.md) — the article stretches this: it treats architecture largely as governance and discipline ("structure rather than good intentions"), not only as elements and design principles.
- [Business Architecture](/foundations/business-architecture.md) — the business layer appears in Steps 1 and 3, but the article does not describe capabilities or value delivery in depth.

## 🏛️ For Kenji
Your AI governance background maps directly onto this article: the same stakeholder concerns and risk-appetite questions apply when an AI platform is delivered on time but adds duplicated models, data and interfaces. For your MBA, the "value-realising" property is a strong governance argument, since it asks for benefits to be measured after delivery. This is your interpretation, not the author's. Question it raises: how would you measure whether an AI platform rollout was effective, rather than merely delivered?

## 📚 Reading plan
### From the article
- [TOGAF Standard, Architecture Development Method](https://pubs.opengroup.org/togaf-standard/adm/chap12.html) — the primary source for the ADM that the seven steps are derived from.
### To go further
- TOGAF Series Guide: Integrating Risk and Security within a TOGAF Enterprise Architecture — the guide the article cites for integrating security and risk into the ADM, as in Figure 1.
- "The Alignment Gap: Why It Exists, and How Enterprise Architecture Closes It" (Architecture & Governance Magazine) — the article's related piece on the alignment problem EA addresses.
