---
type: Article Summary
title: Telecom sob pressão de custos
description: Telecom operators deliver more data for less revenue, so AI's payoff lies in network operations and needs governance
  first.
resource: https://mitsloanreview.com.br/telecom-sob-pressao-de-custos/
tags:
- architecture
- business-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://mitsloanreview.com.br/telecom-sob-pressao-de-custos/
  title: Telecom sob pressão de custos
  author: MIT Sloan Management Review Brasil
status: stable
source_url: https://mitsloanreview.com.br/telecom-sob-pressao-de-custos/
source: MIT Sloan Management Review Brasil
source_type: blog
published: '2026-11-16'
domain: architecture
subdomain: business-architecture
concepts: []
selected_by: architecture-lane (PC run)
content: pc-run
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81fe948dd7f8c07737f9
---

## 🎯 Core thesis
Telecom operators now deliver more data for less revenue per unit, so the real payoff of AI lies in network operations, and it has to rest on governance and observability before agents can deliver it.

## 🧭 Why the author wrote it
The article starts from a squeeze: in Q2 2026 the average price per gigabyte fell 13.4% year on year while data use per user rose 16.8%, and capex is contracting worldwide. If the thesis holds, AI investment should be justified by operational results such as repair time, escalations and alert volume, not mainly by customer-service chatbots. The byline marks the piece as sponsored or cobranded content with e-Core, and its main voice is an e-Core executive, so the vendor-sourced figures should be read with that in mind.

## Opening: a shrinking price per gigabyte
The original has no heading for its first four paragraphs; this section covers them.

### Price per gigabyte against data volume
According to Anatel's Panorama Econômico-Financeiro, the average price per gigabyte in mobile telephony fell from R$ 6.19 to R$ 5.36 between Q2 2025 and Q2 2026, a drop of 13.4%. Over the same period average data use per user rose from 5.82 GB to 6.80 GB, up 16.8%. The author's point is that operators carry more traffic while earning less for each unit carried.

> 🔎 Added context: Price multiplied by volume gives a rough spend per user. By my arithmetic, R$ 6.19 × 5.82 GB is about R$ 36.0 and R$ 5.36 × 6.80 GB is about R$ 36.5, so total spend per user is roughly flat (about +1%). The article's "receive less" claim is about unit price, not total revenue per customer. This is my calculation, not the author's.

### Revenue grows while investment stays flat
Conexis Brasil Digital reports 2025 gross revenue of R$ 338 billion for Brazilian telecoms, above inflation for the third year running. Investment reached R$ 36.3 billion and, adjusted for inflation, was practically stable at +0.2% against the prior period. The sector earns more while its network spending barely moves.

### Global cost containment
Omdia reports that global connectivity revenue grew 4% in 2025 to US$ 1.3 trillion, while capex, the operators' investment, fell 2% to US$ 303 billion. Dell'Oro Group projects another 2% fall in global telecom capex in 2026 and average growth of only 1% a year through 2030. The author presents this as a sector-wide cost-containment cycle, not a Brazilian peculiarity.

### Legacy and commoditization
Douglas Pereira, a telecom engineer with experience at Vivo, Ericsson and TIM and now head of enterprise transformation sales LATAM at e-Core, argues that the operators' history of many acquisitions lets legacy and culture weigh on innovation, and that efficiency and time to market remain hard. He adds that "traditional telecom is becoming a commodity", so the challenge is to monetize the existing base with digital products, using upsell and cross-sell across a fixed and mobile footprint already present throughout Brazil.

## AI stops being a project and becomes infrastructure
### AI investment forecasts
IDC expects telecom operators to invest US$ 617 million in AI in Latin America in 2026. Brazil accounts for almost half of that total, and spending is forecast to grow 37% a year between 2024 and 2029.

### Infrastructure and intent-based networks
IDC expects AI to become the central infrastructure of telecoms, moving networks from static to dynamic, where business objectives turn into network behavior without human intervention. The article calls this the intent-based model.

> 🔎 Added context: In intent-based networking, an operator states a goal, such as a service level for a region, and the system translates it into configuration. Engineers set the intent instead of changing each device by hand.

### Adoption signal
In the Telecom DX Survey 2025, 67% of Brazilian operators named AI among the most important technologies for their digital transformation.

### The bottleneck is not the technology
Pereira says AI and agents are on every executive agenda, but many leaders are still unsure how to apply them, how to measure ROI and how to prove the investment. The constraint the article identifies is organizational and measurement capability.

## Where is the gain?
### Customer service is where most AI sits today
Anatel recorded 1,354,791 complaints in 2025, 6.9% more than the year before, breaking a run of declines. Billing and cancellation grew the most, while complaints about technical quality and repairs fell 9.3%.

### The hidden cost of unresolved contacts
Pereira describes a customer who tries the phone, a chatbot, WhatsApp, email and the ombudsman without a response. From the company's side, one customer generated five attempted complaints, so a single customer is loading every service channel. The article's implied lesson is that a company learning about faults only from customers pays for them several times.

> ❌ **Anti-pattern — Detecting faults through customer complaints:** Operators wait for customers to report outages, who then contact several channels, and neighbours call the operator to say the problem is serious. **Why people do it:** the article does not say; complaint data is the easiest signal to get. **Consequence:** a hidden cost the author calls "gigantic", spread across channels and repeated contacts. **Fix:** detect deviations in the network itself, using monitoring and AI agents, and open tickets before customers call.

### Where AI should act: the network
GSMA's report "A Economia Móvel na América Latina 2026" (May 2026) finds that 85% of AI deployments by regional operators are already in operation, but 78% concentrate on customer service and only 8% on networks.

Pereira's example: AI can assess several characteristics at once, without waiting for ten complaints in the same region. With governance, observability and AI agents that analyse deviations, the system produces a diagnosis for field teams and opens tickets. After a storm, for instance, the system cross-references the region's wind, lightning and equipment-offline data to find what was damaged, so a prepared team arrives with what is needed.

### Layered networks and alert noise
Networks overlap in layers (fixed, 2G, 3G, 4G, 5G and Wi-Fi), and a fault in one layer propagates. As demand grows, monitoring tools generate more alerts, and much of this becomes noise that delays the response to what matters.

> 🔎 Added context: MTTR (mean time to repair) is the average time from a fault being detected to its restoration. The article uses it as the main yardstick for AI's operational value.

### How the gain is measured
IBM's estimates suggest that a well-designed generative AI strategy for network operations could cut MTTR by 30% to 40%. It could also reduce, by a similar proportion, the time operations-centre technicians spend answering field questions, and cut escalations to higher support tiers by 20% to 30%. These are projections for a well-designed strategy, not measured results from the article's subject.

### A vendor case: fewer alerts, faster detection
An e-Core client that received about 100,000 alerts a month handled 27,000 in the period, and its incident detection time fell 57%. The author concludes that teams can focus on decisions that need judgment without growing headcount in line with traffic, which matters in a sector with no cost slack. This is a single vendor-reported case.

### Governance and visibility first
Pereira concludes that the starting point is governance and visibility. AI works only when the business and its destination are understood, with defined processes, mapped databases and assets, security, clear governance and proactive escalation, all focused on the business and empowering staff. He closes by saying telecom is a very lively market, and everyone needs the speed and flexibility that well-applied technology brings.

## 🧠 Mental models
### Commodity pressure
**The model:** When the carried product becomes a commodity, value shifts from the capacity itself to services built on the existing base, such as digital products sold through upsell and cross-sell.
**How to apply it:** For any utility-like business, ask whether revenue still grows with volume or only with services layered on the same footprint.

### Governance before agents
**The model:** AI agents add value only on top of governance, observability, defined processes and mapped data assets.
**How to apply it:** Before approving an AI agent, check that its inputs, decisions and results are observable and that the process it acts on is defined.

### Correlate, do not count
**The model:** An alert is evidence only in context. Cross-referencing weather, equipment status and regional data turns alert volume into a diagnosis.
**How to apply it:** Measure alert systems by the decisions they enable, not by the number of alerts they produce.

## 🔑 Key takeaways
- The average price per gigabyte fell 13.4% year on year in Q2 2026, while data use per user rose 16.8%.
- Brazilian telecom revenue grew above inflation in 2025, while investment was practically stable at +0.2% in real terms.
- Of regional operators' AI deployments, 78% focus on customer service and 8% on networks, a balance the author implies is under-used for networks.
- Complaints about billing and cancellation grew in 2025, while technical and repair complaints fell 9.3%.
- The author judges network AI by repair time, technician time, escalations and alert volume, citing IBM ranges.
- According to Pereira, governance, observability and mapped data come before AI agents.
- The e-Core case (100,000 to 27,000 alerts; detection time down 57%) is vendor-reported.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Business (cost pressure, monetization of the base, customer-service operations); Data (mapped data assets, cross-referenced weather, equipment and alert data); Application (AI agents, monitoring tools, chatbots, ticketing); Technology (layered fixed and mobile networks from 2G to 5G and Wi-Fi, intent-based network control) |
| Scope | Segment or capability: network operations and customer service inside one operator, with sector-level market figures used as context. Not an enterprise-wide programme and not a single solution. |
| Stakeholders and concerns | Executives (ROI, proof of investment, cost); network operations centre and field teams (alert noise, repair time); customers (billing, cancellation, repeated contacts); regulator Anatel (complaint trends); staff and culture (legacy from acquisitions) |
| Frameworks and notations | None named. The article cites sources (Anatel, Conexis, Omdia, Dell'Oro, IDC, GSMA, IBM) and an "intent-based" network model, but no architecture framework. |

### Decisions and trade-offs
- **Where to apply AI first:** Customer service, where most deployments are, versus network operations, which the author recommends. Customer service is the current, measurable use; network operations needs data maturity and monitoring to be credible, and its ROI is harder to prove.
- **Governance and visibility before agents:** The author recommends it. The cost is a slower start, with investment in processes, databases and asset mapping before visible results.
- **Monetize the existing base with digital services:** Recommended over relying on connectivity volume. The cost is the upsell and cross-sell capability the operator must build, which legacy and culture slow.
- **Move toward intent-based networks:** Presented as where IDC expects AI to take telecoms. The article does not discuss the costs or risks of autonomous network behavior.

### Foundations it rests on
- [Architecture domain](/foundations/architecture-domain.md) — the four areas the article touches map onto TOGAF's four primary domains, though the article does not use TOGAF.
- [Business architecture](/foundations/business-architecture.md) — cost, monetization and customer-service operations fall here, but the article does not use the term.
- [Business capability](/foundations/business-capability.md) — the article's "capilaridade" (network reach) is treated as an asset, not as a capability, so the link is loose.
- [Technology architecture](/foundations/technology-architecture.md) — the layered networks (fixed, 2G to 5G, Wi-Fi) are the technology components whose interaction spreads faults.
- [Data architecture](/foundations/data-architecture.md) — mapped databases and cross-referenced data are the article's precondition for AI.
- [Application architecture](/foundations/application-architecture.md) — AI agents, chatbots and monitoring tools are the applications; the article does not describe how they group into capabilities.

## 🏛️ For Kenji
This is a useful model for building an AI business case at a service provider: value shows up in repair time, escalations and alert volume, and the data foundation is a precondition. Check the source first, because it is cobranded with a vendor, so the e-Core and IBM figures are vendor-sourced. This is your interpretation, not the author's: how would you prove ROI for an alert-correlation platform when baseline MTTR and alert volumes are not consistently measured, and which metric would a CFO accept?

## 📚 Reading plan
### From the article
The article cites no links. The sources it names are listed below.
### To go further
- Anatel, Panorama Econômico-Financeiro — source of the per-gigabyte price and complaint figures; check the latest quarter yourself.
- GSMA, "A Economia Móvel na América Latina 2026" (May 2026) — source of the 85%, 78% and 8% deployment split.
- IBM, generative AI in network operations — source of the MTTR, technician and escalation ranges.
- Intent-based networking — the target model behind IDC's view of AI as telecom infrastructure.
- Alert correlation and AIOps — the technique behind the article's point that alert volume is noise without context.
