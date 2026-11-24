---
type: Article Summary
title: The FAA outage is a warning about technical debt in the AI era
description: AI-assisted coding speeds delivery, but its gains survive only with sustained investment in retiring technical
  debt.
resource: https://www.cio.com/article/4251637/the-faa-outage-is-a-warning-about-technical-debt-in-the-ai-era.html
tags:
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4251637/the-faa-outage-is-a-warning-about-technical-debt-in-the-ai-era.html
  title: The FAA outage is a warning about technical debt in the AI era
  author: Edward Anderson
status: stable
source_url: https://www.cio.com/article/4251637/the-faa-outage-is-a-warning-about-technical-debt-in-the-ai-era.html
source: CIO.com
source_type: blog
published: '2026-11-23'
domain: software-engineering
concepts:
- cascading-failure
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d8187911ffccf3c865aba
---

## 🎯 Core thesis
Generating code faster with AI does not make a system stronger, so businesses keep AI's gains only by investing in retiring the technical debt that new code adds to legacy systems.

## 🧭 Why the author wrote it
The author starts from the November 13 nationwide FAA ground stop and the November 16 Chase banking disruptions, and argues that naming one culpable company misses why the wider system was so vulnerable. If the thesis is right, leaders should judge AI-assisted development by the cost of operating, changing and integrating software, not only by how fast it is written.

## Opening: two November outages
> 🔎 Added context: the article has no heading over its opening paragraphs. This title is a navigation label added for this note.

The author asks whether AI-written code that lands on legacy systems nobody fully understands really helps. Investigators of the November 13 ground stop are examining a Juniper Networks router installed by FAA contractor L3Harris, which says it is cooperating. As of November 19 the precise cause was still unresolved. The author separates two questions: who triggered the disruption, and why the system was so vulnerable. Accountability matters, but the first answer "will not, by itself, explain" the second.

According to the FAA, a flood of network connection attempts destabilized legacy equipment, disrupting communications and radar feeds. Blocking traffic from two facilities stabilized the network. In the author's view this bears the hallmarks of accumulated technical debt. The investigation, he argues, must explain both what initiated the failure and what allowed it to spread.

The November 16 banking disruptions raise the same second question for businesses. Chase attributed its difficulties to a software bug, and customers at other banks saw delays on transactions involving Chase even where their own systems worked normally. Whether technical debt contributed is still to be established.

### Cascading failure
The key mechanism here is spread. A problem inside one organization or component travels along the connections that others depend on. In the FAA case, a flood of connection attempts reached legacy equipment that could not absorb it. In the Chase case, customers elsewhere were delayed because their transactions touched Chase's systems. Isolating the traffic from two facilities stabilized the FAA network, which shows that containing the spread matters as much as finding the first cause.

## Technical debt makes legacy systems increasingly fragile
The author defines technical debt as what builds up when teams take shortcuts, patch things together, rely on outdated components and defer improvements. It occurs in software, electronic hardware and the connections between them. Things still work, but they become harder to maintain and upgrade.

### Technical debt
Debt is a hidden burden: systems keep functioning while it grows, and its cost surfaces only when something changes. "Change one part, and you're not quite sure what else might break." The author calls the ongoing cost of that uncertainty "the interest we pay on the debt." Businesses add to the debt whenever the next feature takes priority over fixing what already exists.

> ❌ **Anti-pattern — Shipping over repairing:** Letting the next feature outrank fixing what already exists. **Why people do it:** The article says businesses do this when getting the next feature out the door takes priority. **Consequence:** Interest accrues, so each later change is riskier. **Fix:** Give debt reduction dedicated resources, as the author recommends in the final section.

### Legacy systems
The author's analogy is a house built in the 1970s. Owners add appliances and a bedroom air conditioner; running the microwave and air conditioner together trips a breaker, so everyone learns to avoid it. A licensed contractor adds a second floor, straining the electrical system and adding roof seams that leak onto the attic wiring, so the family watches for leaks whenever it rains. The unresolved problems and workarounds are the house's debt, and the family's habits are the knowledge needed to keep it running.

## AI can accelerate development — and deepen technical debt
The author's research with Geoffrey Parker and Burcu Tan combines interviews with software professionals and a dynamic simulation model. The interviews identified duplicated code, dependency conflicts, outdated components and architectural mismatches. Code "can perform its immediate task while making the broader system harder to maintain and change."

### AI-assisted coding
AI-assisted coding can accelerate development, but the added code can carry these problems into the wider system. The analogy continues: imagine AI-generated plans and thirty neighbors building two more wings over a weekend. It looks fine on a pleasant spring day, but would you trust it on a 110-degree afternoon in Austin with every air conditioner running, or during a downpour? The author states that the analogy does not claim AI is causing the recent outages.

> ❌ **Anti-pattern — Rapid AI-generated additions without oversight:** Adding AI-generated code quickly to legacy systems with no experienced engineering oversight. **Why people do it:** The article says businesses use AI to expand systems whose limits they may already struggle to understand. **Consequence:** Additions may look sound on a calm day but be untrustworthy under stress, the author's concern. **Fix:** Keep experienced engineering oversight over AI output.

### Simulation findings
The simulations suggest growing technical debt can swallow AI's early productivity gains, especially when new code interacts with legacy software. The interviews suggest electronic hardware makes these problems harder to manage. Each new layer of complexity makes the next change harder, until developers spend so much time keeping things working that little time remains to fix the causes. The article reports no simulation figures.

### Balance depends on the environment
The author does not argue against AI-assisted development. The model points to a balance in which AI improves performance without debt overwhelming the gains. A short-lived prototype built from scratch poses a different problem from software that must run for decades alongside older systems.

## Preserving AI's gains requires investing in engineering discipline
### Measuring cost, not only speed
"For business leaders, coding speed alone is an inadequate measure of success." A team can deliver features faster while increasing future maintenance obligations. Managers need to assess the cost of operating, modifying and integrating software alongside the speed of producing it.

> ❌ **Anti-pattern — Measuring only coding speed:** Judging success by how fast code is written. **Why people do it:** Not stated in the article. **Consequence:** Future maintenance obligations grow unseen. **Fix:** Assess operating, modifying and integration costs alongside production speed.

### Dedicated debt-reduction resources
Leaders should give technical-debt reduction dedicated resources. Sustained investment in retiring debt can help preserve AI's benefits. "If maintenance is always postponed until after the next release, the backlog can become a constraint on every future release."

> ❌ **Anti-pattern — Always postponing maintenance:** Deferring maintenance until after every next release. **Why people do it:** Not stated in the article. **Consequence:** The backlog constrains every future release. **Fix:** Sustained, dedicated investment in retiring debt.

### Experienced engineers
Experienced engineers recognize when apparently functional code conflicts with the architecture around it. The author says businesses should preserve that expertise, train junior developers to evaluate AI output, and document older systems before institutional knowledge disappears.

### Testing failure interactions
Testing must examine interactions and failure conditions, including whether one component's abnormal behavior can disrupt others. This returns to the opening question of what allowed a failure to spread.

The FAA investigation may find an equipment defect, an installation error or something else, and those findings will matter. "Replacing a component and strengthening a system are different tasks."

> ❌ **Anti-pattern — Treating replacement as strengthening:** Assuming that swapping a component makes the system stronger. **Why people do it:** Not stated in the article. **Consequence:** The underlying fragility remains, the author argues. **Fix:** Invest in understanding, repairing and testing the infrastructure, not only in replacing parts.

## 🧠 Mental models
### Technical debt as interest
**The model:** Shortcuts and deferred fixes are debt, and the ongoing cost of working around them is its interest.
**How to apply it:** Before a change, ask what else might break. If you are not sure, the debt is already being paid.

### The patched house
**The model:** A house extended without a plan, or built on AI-generated plans with many helpers, works on a calm day and fails under stress.
**How to apply it:** Test changes under peak load, heat and failure conditions, not only the pleasant-day case.

### Replacing versus strengthening
**The model:** Swapping a component is a different task from making the system stronger.
**How to apply it:** Judge modernization by whether the system became more understandable, repairable and testable.

## 🔑 Key takeaways
- Technical debt is the accumulated burden of shortcuts, outdated components and deferred fixes, and it is paid as interest on every later change.
- A failure can spread through the connections other systems depend on, as in the FAA ground stop and the Chase disruption.
- AI-assisted coding can speed development while adding duplicated code, dependency conflicts, outdated components and architectural mismatches.
- The author's simulations suggest growing debt can consume AI's early productivity gains, especially when new code meets legacy software.
- Leaders should measure the cost of operating, modifying and integrating software alongside how fast it is produced.
- Keeping AI's benefits requires dedicated debt-reduction resources, experienced engineers, documented older systems and tests of failure interactions.

## 🏛️ For Kenji
This is your interpretation, not the author's. In architecture work, the warning about spreading failure applies to integrations, shared network paths and dependencies between business units, where one team's change can disrupt another's services. A current-state review could ask which legacy components carry the most debt and which connections would carry a failure from one to another. The open question is how you would show a business leader that accumulated debt is slowing the next change before a failure makes it visible.

## 📚 Reading plan
### From the article
- The article cites no links.

### To go further
- Cascading failures in distributed systems — how one overloaded component can shift load onto its neighbours until the system as a whole fails.
- Chaos engineering — deliberately injecting faults to see how a running system behaves, matching the article's call to test failure conditions.
- Architecture decision records — a way to capture why older systems were built as they were, before institutional knowledge is lost.
- The author's research with Geoffrey Parker and Burcu Tan on AI-assisted coding and technical debt — interviews and a simulation model; the article gives no title, so search by the authors' names.

## 🔗 Concepts
- [cascading-failure](/concepts/cascading-failure.md)
