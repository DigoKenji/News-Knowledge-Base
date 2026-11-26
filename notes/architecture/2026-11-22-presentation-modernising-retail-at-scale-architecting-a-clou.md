---
type: Article Summary
title: 'Presentation: Modernising Retail at Scale: Architecting a Cloud‑to‑Edge Platform for a Global Enterprise'
description: Tesco's transformation shows large-enterprise change needs talent, process and platform change together, delivered
  in staggered steps.
resource: https://www.infoq.com/presentations/tesco-enterprise-tech-transformation
tags:
- architecture
- enterprise-architecture
- ai-agent
- model-context-protocol
- bounded-context
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/presentations/tesco-enterprise-tech-transformation
  title: 'Presentation: Modernising Retail at Scale: Architecting a Cloud‑to‑Edge Platform for a Global Enterprise'
  author: Jayesh Bhayani
status: stable
source_url: https://www.infoq.com/presentations/tesco-enterprise-tech-transformation
source: InfoQ
source_type: blog
published: '2026-11-20'
domain: architecture
subdomain: enterprise-architecture
concepts:
- ai-agent
- model-context-protocol
- bounded-context
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d815d89a2ff94819bc62a
foundation_candidates:
- system of record
- build versus buy
- workload placement
---

## 🎯 Core thesis
Large-enterprise technology transformation only works when system change is paired with talent, process and cultural change, and is delivered in small, staggered steps tied to real business demand.

## 🧭 Why the author wrote it
Jayesh Bhayani, who spent ten years at Tesco, starts from the state he found: fragmented, siloed, vendor-dependent technology that could not deliver omnichannel retail at scale. He shares what it took to modernise a retailer this size, and argues the lessons apply to any business with physical estate, digital channels and real-time operations. If the thesis is right, a new platform alone will not deliver the outcome, because the operating model has to change with it.

## Opening (before the first heading)
> 🔎 Added context: the transcript has an untitled lead-in before its first heading. It is kept here as its own part.

Bhayani describes joining Tesco after about 25 years in Silicon Valley. He first saw the website as "pretty basic" and thought the grocer had little technology, but found a large, siloed estate that needed transformation at scale. He presents the talk as a compressed account of ten years of learning, covering the platform, the culture change behind it and where the company is heading.

## Context: Huge Scale of Our Business
### Operating scale
Tesco runs about 5,000 stores of varying size across five countries, after divesting its Thai and Malaysian businesses. It has 20 distribution centres, 25 million customers (almost one third of the UK population) and 350,000 colleagues, of whom about 15,000 work in head office and about 6,000 in technology. It handles around 50 million baskets, 1.3 million online orders a week, 50 million cases through its supply chain and 4.5 billion calls a day, with 98% UK home-delivery coverage. Technology decisions carry weight at this size.

### Mission-critical estate
Almost 50,000 point-of-sale (POS) devices, called tills in the UK, run in stores, along with thousands of handheld devices on the same platform. Failures are therefore customer-facing and immediate. The author uses this to explain why transformation here is harder than at a start-up or mid-size firm.

## Why This Matters Beyond Retail?
### Efficiency and effectiveness
Efficiency means doing things well and fast. Effectiveness means doing the right things. The author stresses that the two differ: a team can be fast on everything and still chase the wrong goals. He claims the lesson applies to any industry with physical estate, digital operations and real-time demand.

> ❌ **Anti-pattern — Efficiency without effectiveness:** Doing many things quickly without checking that they are the right things. **Why people do it:** not given in the article. **Consequence:** the organisation is "chasing wrong goals". **Fix:** check the goal first, then pursue speed: "do the right things and do things the right way".

## The Starting Point - The Big WHY
### Siloed stacks and fragmented identity
Technology existed but was siloed by historical business structure. Stores and e-commerce ran different systems, and e-commerce itself ran several stacks for grocery, marketplace, clothing (F&F) and wine, including Microsoft .NET and Oracle. A customer had a different login for each, with no single customer view, data platform, product management or single source of truth for stock or price.

### Monolithic, batch and on-premise estate
Systems ran as monoliths in private data centres, because public cloud was not yet in use. Getting a server could take about two months. Store POS software was about 20 years old, batch-oriented and hard to connect to. Engineers deployed it into stores by carrying floppy disks.

### Customised vendor software
Tesco's scale let it demand bespoke changes from vendors. That left it locked into heavily customised versions for years, some at least a decade old and impossible to upgrade. Heavy outsourcing to consultancies added to the problem, which the author describes as poor culture and poor quality.

> ❌ **Anti-pattern — Customising vendor products to your own shape:** Demanding bespoke changes because your scale lets you. **Why people do it:** the author says it "sounds good initially". **Consequence:** years of lock-in to a customised product, and systems that cannot be upgraded. **Fix:** the article's later answer is to decide build or buy by differentiation, and to keep bought platforms flexible (see the framework and cloud sections).

## Strategic Imperative for Change
### Omnichannel and global coherence
Customer expectations were moving fast, and the old way of doing business was not working. Business wanted to change while technology was stuck. Tesco needed omnichannel experience, global coherence across physical and digital estates, rapid change cycles, higher reliability, security, capability and capacity. The response was to move from fragmented stacks to unified platforms serving all channels and markets, built as a multi-tenant "retail platform" (the author stresses this is more than a tech platform).

### Change lead time
A minor change to POS software took about six months from the vendor. Regression testing then broke three other things, and the vendor charged again to fix them. Anything reaching the tills took six to nine months.

> ❌ **Anti-pattern — Accepting vendor change cycles:** Treating a six-to-nine-month release cycle as normal. **Why people do it:** the article does not say; the vendor model was the only option. **Consequence:** regressions, repeated charges for bug fixes, and no store-level change for months. **Fix:** the article's answer is platforms the company controls, which later let teams go live in weeks.

## Where Are We Now?
### Platform coverage and trust
All UK and Republic of Ireland business now runs on the platform, and Central European businesses are being added. Trust in technology is higher there, and technology strategy is integrated with business strategy. The author contrasts this with the earlier service mindset ("we just need three things, can you build it?"), which created the silos. Technology is now "an integral part of the entire business ecosystem", not a unit in a corner.

## Ecosystem of Transformation
### Talent transformation
Most development had been outsourced with a service mindset. Tesco redefined career paths, titles and job definitions, and set out to insource the work. It started in the UK and India, then added Hungary and Poland to scale and to find talent. Career frameworks were redefined across product, engineering and programme, and recruitment changed too.

### Product, not programme
Product management did not exist at the start. Programme managers and service engineers ran work, and a business person's favourite contact in technology would "get it done" through outsourcing. The shift was to an agile, product-driven company that builds products, not one-off solutions. The author notes that programme managers remain but do a different job.

### Organisation that mirrors the business
Tesco has four major business units: supply chain and distribution; commercial, which decides ranging across about 40,000 to 100,000 SKUs, price, promotion, store design and aisles; channels, meaning online and stores; and customer, covering loyalty. Technology was reorganised to mirror this structure, because "you can't run your own ship in a large company". The third pillar is process transformation, since unsuitable processes defeat talent and system change.

> ❌ **Anti-pattern — Running your own ship:** Technology organised as a separate unit that claims to be different from the rest of the company. **Why people do it:** not given in the article. **Consequence:** the author says such transformation is not sustainable. **Fix:** align the organisation to the business domains and the business structure.

## System Transformation: Strategic Framework
### Build and buy
Tesco builds what is a core differentiator and buys what is not. The retail platform is a differentiator, including the POS software on every till, scan-as-you-shop handhelds, and cameras. Enabling back-office functions such as people and finance systems are bought. Bought products must still integrate with the platform.

### Domain boundaries and guardrails
Centralised change control and architecture boards, where designs and code queue for review, no longer fit the pace of delivery, and the author says they will fit even less with an agentic workforce. Tesco decentralised with clear domain boundaries, shared standards and "freedom within the guardrails".

> ❌ **Anti-pattern — Central review queues:** Every design and code change waits for a board. **Why people do it:** not stated in the article. **Consequence:** the author implies queues cannot keep pace with delivery. **Fix:** set domain boundaries and standards, then let teams decide how, inside those guardrails.

### Tech excellence and total loss recovery
Tech excellence is proactive. It covers more than bugs and OS patching, and asks how the company avoids the same problems again. Total loss recovery is a different problem from disaster recovery (DR), which the author calls basic. Business leaders need education on this, and it needs dedicated time and focus. The result is faster delivery with clear ownership and long-term resilience.

> ❌ **Anti-pattern — Resilience left to chance:** Assuming resilience will happen because engineers will handle it on the side. **Why people do it:** the author says even technology staff assume this. **Consequence:** no dedicated time or focus, so total loss is unplanned. **Fix:** plan total loss recovery as a funded capability under tech excellence.

## The Retail and Tech Platform
### Platform tenets and principles
The tenets go beyond customers. Colleague experience matters for the operational systems in stores, and supplier experience matters because suppliers rated Tesco poorly. Other tenets are reuse across markets and business units, offline operation, cyber risk and compliance built in, and cost as a feature. The principles are: composable services; domain design; clear data pathways and data quality; layered security from perimeter to core; multi-tenancy by default, with one deployment serving several countries and businesses; building product capabilities rather than point solutions; and abstractions.

### Offline operations at the edge
Thousands of edge devices run in stores where the network can be flaky. Replacing the network would cost hundreds of millions, and grocery margins are about 4% (3% to 4.5%). The same microservices run in the cloud and at the edge, with abstractions that keep business logic and data consistent. If the network fails, the store still identifies loyalty customers, takes card and contactless payments, and handles shelf-edge price updates. The design fails in the customer's favour.

### Cost as a feature
Public cloud spend was GBP 50 million to 60 million a year four or five years earlier and growing exponentially. For a GBP 50 billion business at about 4% margin, roughly GBP 2 billion, a GBP 100 million cloud bill is a large P&L hit. The author says technology must keep cost a feature, not a constraint, and must know what value it buys.

### Abstractions
Adding an item to a basket calls at least 40 microservices: identity token, product ID, customer UUID, personalisation, price, description and more. Without abstractions, a path from a phone to the data centre would take about 40 network round trips, which the author calls extremely slow. Abstractions hide complexity, and at this scale they keep digital and physical estates cohesive without becoming a bottleneck.

> ❌ **Anti-pattern — Exposing every service to the client:** Clients call each microservice directly across the network. **Why people do it:** not stated in the article. **Consequence:** about 40 network round trips for one basket add, which the author calls "extremely slow". **Fix:** put abstractions between clients and services.

### Domains and the cloud detour
The retail platform has more than 60 domains and hundreds of microservices across private and public cloud. Early on, Tesco had only data-centre servers with no elasticity. It then handed developers credit cards to sign up for public cloud in a rush, which brought high cost and unclear value, followed by repatriation and re-migration. The lesson is to build centralised capabilities first, which takes longer but avoids costly migration.

> ❌ **Anti-pattern — Credit-card cloud adoption:** Letting teams adopt public cloud ad hoc to move quickly. **Why people do it:** speed and the need for elasticity. **Consequence:** high cost, no clear value, then repatriation and re-migration. **Fix:** build central capabilities first, so migration is planned once.

### Workload placement
Services that need true elasticity and scale go to public cloud, and the author favours portable PaaS. Large systems such as supply-chain and distribution-centre technology run in private cloud, which also offers the same developer experience. Presence is about 80% private and 20% public, but cost is the reverse: about 80% in public cloud. An equivalent public setup costs 8 to 10 times more. Proprietary PaaS locks you in, and the author warns that licence fees rise exponentially. He also notes that a developer could spend almost GBP 500 a day on agentic code development without clarity on the value created.

> ❌ **Anti-pattern — Easy to enter, hard to justify:** Using public cloud without knowing what value it creates. **Why people do it:** public cloud is "easy to jump in". **Consequence:** costs grow exponentially, and the author cites GBP 500 a day of agentic development spend. **Fix:** choose portable PaaS and place each workload by value and flexibility.

### One team owns each platform
One team owns the edge platform, the retail cloud platform and, during migration, the legacy platform. The author says splitting a problem between a team building new services and a team running old ones "never works", and motivation drops quickly.

> ❌ **Anti-pattern — Split ownership of old and new:** One team builds new services while another keeps the legacy systems. **Why people do it:** not stated in the article. **Consequence:** in the author's experience, it never works. **Fix:** keep one team accountable for both during migration.

### Shared services
Engineers at first enjoyed running Cassandra, Kafka and protocols themselves. As production workloads grew, Tesco had no separate QA or support team, so engineers were fully responsible for production. Tesco then invested in shared services so engineers no longer run their own infrastructure.

## Beyond AI in Operations - Agentic Commerce
### AI in forecasting and logistics
Tesco has used AI for a long time. Forecasting is the hardest supply problem: if forecasts are wrong, shelves empty or food is wasted. The author's example is 100 kg of tomatoes. Forecasts depend on lead times, which vary from four deliveries a day for tomatoes to six months for jeans, and on shelf life. Distribution uses stock layout, truck routing and load-filling across thousands of miles a day. Personalisation and pricing are also AI-driven.

### Agentic commerce is a business decision
Customers increasingly spend time in ChatGPT and Gemini and may buy groceries through agents. The author argues that not taking part risks marginalisation, but taking part also risks losing the first customer touch. So it is "a business decision", not just a technology one.

### Machine-readable data and agent gateways
Search optimisation has to change. Crawlers for agents need machine-readable data, which the author calls AEO (agentic understanding). Identity, consent and policy frameworks must define what agents may do on a website or through APIs. Gateways for agents, with sub-agents and agent-to-agent protocols, will sit beside existing channels, and MCP will help agents discover services and data. Commerce protocols such as UCP and ACP are emerging, but the author says they are not ready for production scale. UK rules do not allow agents to make payments for customers, and rules differ across countries.

> 🔎 Added context: the article refers to an architecture diagram that is not in the transcript, so this description relies on the spoken text only.

### From system of record to system of delegation
The platform changes from a system of record to a system of delegation, because agents now work with the company's data and services and make decisions for customers. Existing experience APIs, such as add item to basket, may need extra endpoints for agents, but they still run on the same retail platform.

## What Could Have Been Better?
### One central tech platform, built early
Tesco moved to public cloud early, and each team built its own CI/CD, observability and other tooling. Every team claimed to have the best pipeline, but the author wanted one CI/CD pipeline, not 50. Portfolio management came late. With about 6,000 technology staff and hundreds of products, Tesco now uses a product maturity model to place each product on a maturity curve.

> ❌ **Anti-pattern — A pipeline per team:** Each team builds its own CI/CD and observability. **Why people do it:** each team believes its own pipeline is the best. **Consequence:** duplicated effort across many pipelines. **Fix:** build one central technology platform early.

### Location strategy
Engineering can sit in several locations. Product management, engineering and programme work must not sit in different places, or the teams will not be self-sufficient. The author says that thinking harder about these early avoids costly re-implementation and migration.

## Tips for Any Large Enterprise Transformation
### Change processes, not only systems
System transformation is not enough. Business processes also need change, because they develop organically, like systems. The author asks why four countries should have different operating processes, and recommends simplifying operations.

### Build core capabilities early for throughput
Core capabilities such as CI/CD and observability should be built early. With the platform in place, Tesco can now go live in two or three weeks, where the same work might once have taken six months. Without those capabilities, the company would only have duplicated effort on each new change.

### Evolve, avoid tech for tech, and communicate value
The platform was horizontally organised. With platforms mature, Tesco is becoming more vertical, forming partnerships between business domains and technology. Value must be communicated early, because it is hard to explain outside technology and even within it. The author warns against "tech for tech".

### Protect IP and avoid vendor lock-in
Tesco is careful about IP leakage when using agentic code development, and asks SaaS vendors for data-sharing and IP protections to keep its differentiation. The author ends by saying that technology keeps changing, from blockchain to public cloud to agents, and that without the right culture, talent and trust in the company, the transformation "won't work".

## 🧠 Mental models
### Efficient versus effective
**The model:** Efficiency is doing things well and fast. Effectiveness is doing the right things. Speed on the wrong goals is still waste.
**How to apply it:** Before you measure delivery speed, check that the goal is right.

### Freedom within guardrails
**The model:** Decisions are decentralised inside clear domain boundaries, shared standards and guardrails, instead of being queued through central boards.
**How to apply it:** Write the guardrails first, then let domain teams choose the how.

### Build what differentiates, buy the rest
**The model:** Build core differentiators, here the retail platform and its POS software. Buy enabling back-office functions, and integrate everything bought into the platform.
**How to apply it:** For each capability, ask whether it differentiates the business. If it does not, buy it and integrate it.

### Fail in favour of the customer
**The model:** When the network fails, store operations degrade toward the customer. Loyalty is still identified and payments still taken.
**How to apply it:** Decide in advance which operations must continue offline and which way each should fail.

## 🔑 Key takeaways
- System change was not enough at Tesco. Talent, process and organisation had to change too, which the author says took about ten years.
- One retail platform now runs 100% of UK and Republic of Ireland business, is extending to Central Europe, and is multi-tenant by default.
- Build core differentiators such as POS and buy enabling functions, and integrate every bought product into the platform.
- About 80% of Tesco's presence is in private cloud but about 80% of its cost is in public cloud. An equivalent public setup costs 8 to 10 times more, so portable PaaS and deliberate workload placement matter.
- Stores keep trading when the network fails: they still identify loyalty customers and take payment, and one team owns the edge and cloud platforms.
- Agentic commerce is a business decision. It needs machine-readable data, consent policies and agent gateways, and moves the platform from system of record to system of delegation.
- Building central CI/CD and observability early is what lets Tesco go live in two or three weeks where it once took about six months.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Business (capabilities, business units, org structure, product model); Application (60-plus domains, microservices, abstractions); Technology (private and public cloud, edge devices, POS); Data (lightly: data pathways, data quality, single customer view) |
| Scope | Enterprise: one retail platform across five countries and four business units. The subdomain is enterprise-architecture, and the article also goes into solution-level detail such as the basket call path |
| Stakeholders and concerns | Business leaders (trust, demand, value); customers (experience); colleagues in stores (operational systems); suppliers (experience); engineers (production support burden); finance (cost and P&L); regulators (agent payment rules); security (cyber risk, IP leakage) |
| Frameworks and notations | None named (no TOGAF, ArchiMate, BPMN or similar). Protocols named: MCP, ACP, UCP, agent-to-agent |

### Decisions and trade-offs
- **Build versus buy:** Build core differentiators, buy enabling functions, and integrate bought products. Cost: integration effort and dependence on vendors for the bought parts.
- **Centralise versus decentralise:** Decentralise inside domain boundaries and guardrails instead of central boards. Cost: discipline in standards and guardrails.
- **Public versus private cloud placement:** Put elastic, scale-driven services in public cloud and large systems in private cloud. Cost: public cloud is about 8 to 10 times dearer for an equivalent setup and costs rise exponentially if unmanaged.
- **Portable versus proprietary PaaS:** Use portable PaaS only, to keep the option to move. Cost: the article admits this is "not perfect" and gives up some managed-service convenience.
- **Offline failure mode:** Fail in favour of the customer at the edge. Cost: the article does not state the cost; it is implied by the need for consistent data when the network returns.
- **Staggered versus big-bang change:** Sequence change in small steps, because a 50 billion business cannot pause. Cost: slower overall change and a long, uneven journey.
- **Team ownership across legacy and new:** One team owns old and new platforms during migration. Cost: the same team carries both loads.
- **Shared services versus self-service infrastructure:** Centralise shared services so engineers stop running their own Cassandra and Kafka. Cost: investment in a shared platform team.
- **Central versus per-team CI/CD:** Build one central pipeline. The author says the team should have built it earlier. Cost: upfront investment before product teams need it.
- **Abstraction layer versus direct service calls:** Add abstractions so clients avoid about 40 round trips per basket add. Cost: an extra layer to design and run.
- **Participating in agentic commerce:** Take part but add gateways and policies. Cost: the first customer touch may move to another company, and UCP and ACP are not production-ready.

### Foundations it rests on
- [Enterprise Architecture](/foundations/enterprise-architecture.md): the article treats the whole group as one architecture, but uses it to steer a multi-year change programme, which the definition covers.
- [Business Architecture](/foundations/business-architecture.md): the author maps technology teams to the four business units, which is the business-structure view the definition describes.
- [Business capability](/foundations/business-capability.md): the article uses "capability" for reusable platform products, such as "build product capabilities, not solutions". This stretches the definition toward delivery units, so the term is used differently here.
- [Architecture Domain](/foundations/architecture-domain.md): the article covers all four TOGAF domains, with Data lightly.
- [Application Architecture](/foundations/application-architecture.md): the 60-plus domains and microservices form the application structure.
- [Technology Architecture](/foundations/technology-architecture.md): the private and public cloud and edge services form the technology structure.
- [Enterprise](/foundations/enterprise.md): Tesco is a single enterprise across five countries and four business units.

## 🏛️ For Kenji
Your interpretation, not the author's: this matters for any enterprise architect who must decide how much to build versus buy, and where to put workloads, while keeping cost visible. It also bears on the MBA's architecture side, because the article ties technology choices to P&L and business structure. One question it raises: for a platform you work on, which capabilities truly differentiate the business, and how would you show that to a sceptical CFO?

## 📚 Reading plan
### From the article
- [InfoQ presentation: Modernising Retail at Scale](https://www.infoq.com/presentations/tesco-enterprise-tech-transformation) — the source talk and its full transcript; read it for the Q&A and any slides not reproduced here.
### To go further
- TOGAF 9.2 architecture domains (Business, Data, Application, Technology) — to place the article's examples in the standard model.
- Product management and portfolio maturity models in large IT organisations — the article uses a maturity curve for portfolio decisions but does not explain it.
- Agentic commerce protocols and MCP — the article is cautious about UCP and ACP, so the current state is worth checking yourself.

## Concepts
- [AI agent](/concepts/ai-ml/ai-agent.md)
- [Model Context Protocol (MCP)](/concepts/ai-ml/model-context-protocol.md)
- [Bounded context](/concepts/architecture/bounded-context.md)
