---
type: Article Summary
title: 'More control, fewer dependencies: Why DORA is forcing a connectivity rethink'
description: Several cloud providers don't add resilience if they share network paths; trace critical services down to connectivity
  to find shared failure points.
resource: https://www.cio.com/article/4252226/more-control-fewer-dependencies-why-dora-is-forcing-a-connectivity-rethink.html
tags:
- architecture
- technology-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4252226/more-control-fewer-dependencies-why-dora-is-forcing-a-connectivity-rethink.html
  title: 'More control, fewer dependencies: Why DORA is forcing a connectivity rethink'
  author: Thomas King
status: stable
source_url: https://www.cio.com/article/4252226/more-control-fewer-dependencies-why-dora-is-forcing-a-connectivity-rethink.html
source: CIO.com
source_type: blog
published: '2026-11-24'
domain: architecture
subdomain: technology-architecture
concepts:
- correlated-failure
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81869ea5eb4fa3fa2281
foundation_candidates:
- business-continuity
- resilience
- single-point-of-failure
---

## 🎯 Core thesis
Several clouds and providers do not guarantee resilience if they depend on the same network connections, so CIOs need to know what happens when those connections fail.

## 🧭 Why the author wrote it
The author, Thomas King, opens with a bank whose investments were spread across several funds that were all exposed to the same company, and whose risk team would have hard questions to answer. He argues CIOs should bring the same skepticism to network architecture. If he is right, connectivity has to be part of investment and continuity decisions before a cloud service is chosen, not after it is running.

## Opening: resilience depends on independence
The article opens with the claim that spreading applications across providers does not help if the paths to them converge. The author says that "when we distribute applications across different providers, we need to know where their apparent 'independence' actually ends."

### Independence of a fallback
A fallback only protects you if it is not exposed to the same variables as the primary. The author states it plainly: "a plan B only works if it isn't exposed to the same set of variables as Plan A." A second cloud can open up more options, but its value as a fallback depends on whether people can still reach it when something fails. If the applications on both clouds keep running while the path to them runs through the same infrastructure, a single disruption can still leave customers stranded. Redundancy is therefore a question about shared paths, not a count of providers.

### Business continuity at board level
The author calls the lack of business continuity "a difficult thing for a CIO and their team to explain to the board", especially when the company has invested heavily in resilience. That is the reason he argues that connectivity "deserves a seat at the top table." The concern is less about technical elegance than about whether leadership can explain how customers keep access during a disruption.

### Shared exposure and the bank analogy
The bank example carries the argument. A bank that holds several funds that all own the same company is not diversified, and its risk team would ask about that exposure. The author asks CIOs to examine their network the same way, so they can see "where their apparent 'independence' actually ends." The analogy is how the article teaches the rest of its points, and it reappears in the final section.

## A European regulation, a familiar American problem
The author argues that the EU's Digital Operational Resilience Act (DORA) raises questions that US CIOs should also ask.

### Digital Operational Resilience Act (DORA)
DORA forces financial institutions to examine the dependencies behind their digital services, and much of that traces back to network architecture. Its requirements run from managing technology risk to testing resilience, with a mandate to report major incidents and to oversee external providers. The author says what sets DORA apart from other regulations is that it pushes institutions to examine how much their ability to serve customers depends on decisions and infrastructure largely outside their control. He adds that US CIOs "ought to pay attention", since "a US bank's customers expect to reach their money regardless of where its technology suppliers are based." DORA gives CIOs a reason to ask the question he puts at the center of the article: "What can fail here? And what will still work when it does?"

### Third-party risk and the incident figures
The first joint report from the European Supervisory Authorities found that 29% of major technology incidents reported across the EU financial sector in 2025 originated with third parties. Around a third had effects across borders, which the author links to the shared infrastructure and services that connect financial institutions. He notes that these figures cover a wide range of failures, so they do not prove a network cause, but they give CIOs everywhere a compelling reason to check where their own dependencies sit. He also points to the OCC, a US federal agency that supervises national banks, which in its Spring 2025 risk report highlighted operational resilience and third-party risk management, pointing to disruptions across financial services and other sectors.

### Supplier evaluation and recovery assurance
The first step the author proposes is to change how suppliers are evaluated. A provider with strong recovery processes for its own services is not thereby immune from disruption. Firms need to understand where the provider's dependencies sit, including at the infrastructure level. That requires a conversation among the people buying services, the engineers connecting them, and the leaders responsible for business continuity, and the author wants that conversation to follow a critical service far enough to expose the dependencies beneath it.

> ❌ **Anti-pattern — accepting recovery claims at face value:** treating a provider's own recovery story as proof that the service will survive a disruption. **Why people do it:** the article does not say; the provider's recovery documentation is the easiest evidence to obtain. **Consequence:** "we risk approving a recovery plan whose success rests on assumptions nobody has tested." **Fix:** trace the provider's dependencies to the infrastructure level and test the recovery plan against them.

## Following the dependencies beneath the cloud
This section is the method. The author starts from a customer-facing service and works downward.

### Tracing a critical service from the top down
The author would start with a service customers depend on, such as making a payment through an online banking app, and trace what has to work for the payment to go through. The app might run in one cloud while drawing on a fraud detection service in another and a core banking system elsewhere. Each part could have its own backup arrangements, but whether the payment can still proceed requires looking deeper than those arrangements. The method is to pick a service, list its dependencies, and continue down to the infrastructure beneath each one.

### Correlated failure through shared routes
The author shows how separate-looking connections can share a single point. Two connections bought from different carriers may use the same underlying fiber route. Entirely separate routes may enter a building through the same conduit, or converge at a facility that both depend on. A failure at that shared point can take away the primary connection and its backup together, which leads to his question: "is it really a backup at all?"

🔎 Added context: A fiber cut is a physical break in an optical cable. A conduit is a protective duct that can carry many cables, so cables sharing one conduit can be damaged by the same event.

> ❌ **Anti-pattern — counting carriers as independence:** assuming two connections from different carriers are separate paths. **Why people do it:** the article does not say; different carriers look like different routes on a contract. **Consequence:** one fiber cut, conduit break or shared facility failure removes both primary and backup. **Fix:** ask for the actual route of each connection and the evidence that the paths are separate.

### Procurement questions and red flags
The author wants this discussion to reach procurement. A CIO should ask suppliers to explain how their proposed routes differ and what evidence supports that separation. If that detail is hard to obtain, or the provider is not forthcoming, that is "your first red flag." Once dependencies are visible, the business can decide where alternative connections help. He would prioritize the services that most closely affect customers, giving them a viable route through a disruption, "with enough capacity to easily support those services."

### Internet exchange (IX)
An Internet Exchange brings networks together so they can exchange traffic directly. Where the relevant networks are present and agree to interconnect, traffic can follow a shorter path with fewer intermediary networks. Exchanging traffic locally also avoids unnecessary journeys to distant hubs, which makes performance more predictable. The author is clear that an IX is not mandated by DORA.

### Private cloud connectivity and interconnection platforms
Through an interconnection platform, an institution can establish dedicated connections to cloud providers, which gives it greater control over how traffic reaches those services. Private connections between networks and private connections between clouds serve different needs. The author's point is that both help engineers build paths that are easier to understand and manage.

> ❌ **Anti-pattern — rebuilding the dependency through the new platform:** moving to an interconnection facility while every route to it still shares one vulnerable link. **Why people do it:** the article does not say; the new platform is the part that gets attention and budget. **Consequence:** "an institution can inadvertently recreate its original dependency." **Fix:** apply the same scrutiny to the access connections to the facility as to the infrastructure inside it, and check that they take physically diverse routes.

### Testing the surviving path
The author judges designs through testing. If the primary connection goes down, the surviving path needs to carry the required traffic while keeping the service usable. He stresses what "usable" means: "even a payment that eventually completes after repeated timeouts is still a serious disruption in the eyes of the customer."

> ❌ **Anti-pattern — counting eventual success as recovery:** treating a transaction that completes after repeated timeouts as a successful failover. **Why people do it:** the article does not say; the transaction still finishes, so the outage looks resolved in logs. **Consequence:** the customer experiences a serious disruption that the recovery plan records as a pass. **Fix:** test whether the service stays usable on the surviving path, not only whether it eventually completes.

### Digital sovereignty
The author sees interconnection as a way to exercise greater control over the infrastructure a business depends on. An institution meeting its DORA obligations can still strengthen its Internet connectivity through more independent routes and direct connections to the online services it uses. He says this is what digital sovereignty means here: "having more say over how traffic travels and more options when a provider or path becomes unavailable." The option is only useful if the connections "are designed and tested with the same care."

🔎 Added context: The author uses the term narrowly, for control over traffic paths and fallback options. Broader meanings, such as data residency law, are not discussed in the article.

## Giving connectivity a seat in the boardroom
The last section returns to the bank analogy and turns it into a decision rule.

### Look-through diversification
A bank would not consider its investments diversified just because it held several funds; it would examine what the funds own and whether they share exposure to the same company. For CIOs, choosing several providers is a start, but the connections need to be followed far enough to see whether one failure could leave them all unreachable.

### Connectivity in investment decisions
The author argues that connectivity has to enter the conversation while investment decisions are still being made. If a new cloud service is meant to support an essential business function, its connections belong in the discussion about how that function will survive a disruption. The cost of an independent route can then be weighed against the consequences of losing access, and business leaders should be involved in deciding what level of risk the business will accept.

### DORA as an opportunity for US technology leaders
The author sees DORA's scrutiny as a chance for US technology leaders to examine their own choices, ask better questions of suppliers, and make recovery plans more credible. His closing test is whether one can explain "how a customer will still reach an essential service when its usual connection disappears." He describes that question, followed down through the architecture, as where "the real work begins."

## 🧠 Mental models
### Independence test
**The model:** A fallback is real only if it is not exposed to the same variables as the primary, as in "a plan B only works if it isn't exposed to the same set of variables as Plan A."
**How to apply it:** For each backup, ask "what can fail here? And what will still work when it does?" and trace shared fiber, conduits and facilities until the two paths no longer converge.

### Look-through diversification
**The model:** A set of holdings is diversified only when you look through to what each one owns. Several funds that share one underlying company are concentrated, whatever their count.
**How to apply it:** Count shared dependencies rather than providers. List each provider's connections and the physical and operational points they share.

### Trace the critical service
**The model:** Start from a service customers depend on, such as a payment, and follow what must work for it to succeed, down to the infrastructure under each dependency.
**How to apply it:** Prioritize the services that most affect customers, then check whether the payment still proceeds when any one primary path fails.

### Test the surviving path
**The model:** A backup is proven only when the surviving path carries the required traffic and the service stays usable, not merely when it eventually completes.
**How to apply it:** Run failover tests against the real traffic profile and judge the result by the customer experience, including repeated timeouts.

## 🔑 Key takeaways
- A second cloud is no real backup if its path runs through the same infrastructure as the first, because a single disruption can strand customers while the applications keep running.
- Two connections from different carriers can share one fiber route, conduit or facility, so a single failure can remove the primary and the backup together.
- The author's test case is a customer payment, traced through the app, a fraud detection service, a core banking system, and the network beneath each.
- A provider's strong recovery processes do not prove it is immune to disruption; the author treats evasive or missing route evidence as "your first red flag."
- The first joint report from the European Supervisory Authorities found 29% of major technology incidents in the EU financial sector in 2025 originated with third parties, and around a third had cross-border effects.
- Direct interconnection through internet exchanges and private cloud links can shorten paths and add control, but the access links to those facilities need the same scrutiny as everything inside them.
- Connectivity belongs in investment decisions, where the cost of an independent route is weighed against the consequences of losing access, with business leaders deciding the acceptable risk.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Technology (primary: carriers, fiber routes, conduits, cloud connections, IX and interconnection platforms). Application (a payment app that depends on a fraud detection service and a core banking system). Business (business continuity, board risk appetite, customer access to money). Data: not covered. |
| Scope | Segment or capability: the path of one critical customer service, a payment, across several providers and the infrastructure under them. It is not enterprise-wide and not a single project. |
| Stakeholders and concerns | CIOs and their teams (continuity, explaining exposure to the board). Board and business leaders (acceptable risk, cost of an independent route versus losing access). Procurement (supplier evidence on route separation). Engineers (paths that are easy to understand and manage). Regulators, through DORA and the OCC (third-party risk, incident reporting). Customers (reaching their money when a path fails). |
| Frameworks and notations | None named. DORA and the OCC risk report are regulatory references, not architecture frameworks. |

### Decisions and trade-offs
- **How deep supplier evaluation goes:** the options are accepting a provider's recovery processes or tracing its dependencies down to infrastructure. The author recommends tracing, following a critical service far enough to expose shared dependencies. The cost is a longer conversation between buyers, engineers and continuity leaders, and suppliers may need to disclose route detail.
- **Where alternative connections go:** the author recommends prioritizing the services that most closely affect customers, with a viable route through disruption and enough capacity. The cost is the capacity and expense of independent routes, which he says must be weighed against the consequences of losing access.
- **Direct interconnection versus transit through intermediary networks:** the author recommends direct exchange where networks agree to interconnect, and dedicated cloud connections where greater control is needed. The benefits are shorter paths, more predictable performance and more control. The costs are that access connections need the same scrutiny, the design must be tested, and an IX is not mandated by DORA.
- **When connectivity is decided:** the author recommends bringing connectivity into investment decisions for new cloud services, not after they are running. The cost is earlier involvement from networking and business leaders, and a harder conversation about risk acceptance.

### Foundations it rests on
- [Enterprise Architecture](/foundations/enterprise-architecture.md) — the article uses it to bring connectivity into investment decisions and risk choices, which stretches "steer change" toward decisions made by business leaders.
- [Technology Architecture](/foundations/technology-architecture.md) — the article traces structure beneath a service down to carriers, fiber and conduits, which is finer-grained than "technology services and components" and centers on paths.
- [Solution Architecture](/foundations/solution-architecture.md) — the article applies one critical service across several providers, which is broader than the single project scope in the working definition.
- [Architecture domain](/foundations/architecture-domain.md) — the article spans technology, application and business domains, and does not cover data.

## 🏛️ For Kenji
This is your interpretation, not the author's. For a Staff Architect, the practical step is to make supplier and route dependencies visible in the architecture repository and link them to each service's recovery objectives. In the MBA architecture work, the boardroom section is a risk-appetite question: business leaders should decide how much route risk to accept, with the architecture making the trade-off visible. The question it raises for you: for one of your critical services, can you name the shared physical point, such as a carrier, conduit or facility, whose single failure would take out both the primary and the backup?

## 📚 Reading plan
### From the article
- [More control, fewer dependencies: Why DORA is forcing a connectivity rethink](https://www.cio.com/article/4252226/more-control-fewer-dependencies-why-dora-is-forcing-a-connectivity-rethink.html) — the full original, since the article cites no links and the note leaves out nothing from it.

### To go further
- Digital Operational Resilience Act (DORA) — the EU regulation the article is built around, with its requirements for testing, incident reporting and third-party oversight.
- The first joint report from the European Supervisory Authorities on major technology incidents — the source of the 29% third-party figure.
- The OCC's Spring 2025 risk report — the US view of operational resilience and third-party risk management.
- Internet exchanges and interconnection platforms — how direct peering shortens paths and where access links fit.
- Business continuity testing for critical services — how to test failover against real traffic and usability targets.

## 🔗 Concepts
- [correlated-failure](/concepts/backend-systems/correlated-failure.md)
