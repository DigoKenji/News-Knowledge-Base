---
type: Article Summary
title: Open Questions in Pattern Application
description: A pattern is only applied when a recorded decision, a record on design elements, and a conformance check exist together.
resource: https://www.architectureandgovernance.com/uncategorized/open-questions-in-pattern-application
tags:
- architecture
- integration-and-software-architecture
- adr
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://www.architectureandgovernance.com/uncategorized/open-questions-in-pattern-application
  title: Open Questions in Pattern Application
  author: Architecture & Governance Magazine
status: stable
source_url: https://www.architectureandgovernance.com/uncategorized/open-questions-in-pattern-application
source: Architecture & Governance Magazine
source_type: blog
published: '2026-11-18'
domain: architecture
subdomain: integration-and-software-architecture
concepts:
- adr
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d8105950edfa52c54f634
---

## 🎯 Core thesis
A pattern is only applied when a decision, the pattern recorded on the actual design elements it touches, and a check that the design matches the pattern all exist together.

## 🧭 Why the author wrote it
Architects say they "apply patterns" and "use decision records", but when asked to show where, the answer is usually missing; the author says it is repeatable in roughly 1 in 500 interviews or mentoring sessions. If the author's thesis holds, a pattern could be traced from library to decision to design to code and checked, instead of argued from memory. The author states that PAL, the tool he describes, is designed but not running yet.

## Opening: the repeatability problem and the research effort

The article opens with questions: when to apply a pattern and when not, how a practitioner learned that, and which design artifact shows which pattern applies. The author says the field is full of claims that patterns and decisions should be "awesome", and of many stories of them not working.

### A 200-pattern reference library
The author's group launched a library of 200 common and experimental industry patterns for architecture. He says it is not nearly complete but is an excellent start, and that the work took a year of his life. Before it, there was no common reference point, and the vendor repositories disagreed with each other. The library is the source PAL draws on for its recommendations and checks.

### Repeatability as the research question
The author wants scientific or semi-scientific rigor in architecture methods: reusable techniques that others can succeed with, built from talking to many practitioners. He describes the effort as a partnership with Shawn McCarthy, chief architect at Manulife and chairman of the Chief Architect Forum, plus four students and a professor from UC Denver. The gap he is attacking is that repeatable success is rare.

> ❌ **Anti-pattern — Marketing-led pattern adoption:** Slogans such as "Apply Patterns! Use Decision Records! Three easy steps." **Why people do it:** not stated in the article. **Consequence:** the author reports repeatability is "absolutely miserable". **Fix:** build techniques from evidence gathered across many practitioners, which is the author's stated approach.

## Ergane and what is being built

The extracted text includes the caption "Context View of Ergane in Ergane"; the image itself is not in the text.

### Ergane, the BTABoK-based design tool
Ergane is the author's BTABoK-based design tool for understanding, testing and experimenting with architecture. PAL was designed on an Ergane board, so Ergane is both the tool used to design PAL and the environment PAL sits in. The article describes PAL as called only by the board service.

### PAL, designed but not yet running
The author is explicit that PAL is not running yet. What exists is a design, and he says he would rather show the design than sell a demo. The rest of the article therefore describes intended behavior and design artifacts, not measured results.

## What PAL does

PAL is a service that holds the BTABoK pattern library and does two jobs. The first recommends patterns that fit a design card the architect is working on. The second, which the author says he cares about most, checks whether patterns already chosen actually appear in the design the way the library describes.

### Pattern recommendation
The first job takes a design card and suggests patterns that might fit it. The article presents this as the first job but says little about how fit is judged.

### Pattern conformance checking
The second job checks chosen patterns against the design. The author notes that almost nobody does this: ask where a pattern is in the design and in the code, and the conversation goes quiet. This is the link that turns "we apply patterns" into something inspectable.

> ❌ **Anti-pattern — Pattern claimed but not shown:** Saying a pattern is used without pointing to where it sits in the design or code. **Why people do it:** not stated in the article; the author observes that people could name the patterns they "used" but almost nobody could show where. **Consequence:** the pattern "lived in someone's head", so no review can check it. **Fix:** record the pattern on the design elements it touches and check it against them, which is what PAL is designed to do.

## We designed it using the method

Every part of PAL was designed on an Ergane board using BTABoK viewpoints, as seven cards, each answering a different question.

### BTABoK viewpoints
Each card answers its question from one viewpoint, and no single card gives the whole answer. The author's answer to "which design artifact shows which pattern applies" is several of them, each from a different angle.

### Primary context view
This card shows where PAL sits among everything else in Ergane. PAL is a domain service on the private network, called only by the board service. The article says the reader has already seen this card.

### Architecturally significant requirement
An architecturally significant requirement is one that changes the shape of the system if it is gotten wrong. PAL's requirement is that it must recommend and check patterns from the current published library without slowing the board. The author says this requirement drives half the design.

### Quality attribute card with six scenarios
A scenario is a specific situation with a measurable response, and this card holds six of them. The extracted text does not list the six. It does show one of the card's tactics: an answer cache keyed by card version and library version.

### Interaction model (usability viewpoint)
This card sets out exactly what the architect sees and does, step by step. The article gives the viewpoint's purpose but does not reproduce its steps.

### REST interface (integration viewpoint)
This card is the contract between the board service and PAL, covering every call, every error and every timeout. The article does not give endpoints, error codes or timeout values.

### Latency budget (performance viewpoint)
This card states how long each step is allowed to take. The Cache Aside sequence appears here: the board service looks in the answer cache, goes to PAL on a miss, then stores the answer. The article gives no budget figures.

### Deployment model
This card says where PAL runs and who can reach it. In the article, the deployment view leaves open where the cache lives.

The author's point is that a pattern does not show up on one card. Asking which artifact shows which pattern applies gives several answers, and designing PAL is an attempt to solve that problem.

## A Cache-Related Pattern?

The author notices caching in the PAL design and asks the questions a team would ask: is it Cache Aside, where is it, is it Redis, and how would you show it visually and in code.

### Cache Aside
Cache Aside is the worked example. You check the cache first; if the item is not there, you go to the source and then put the answer in the cache for next time. On a whiteboard it is simple, but in a real design the pattern shows up in pieces across a whole set of views.

### Partial match on the PAL board
Cache Aside appears in the latency budget sequence, in the answer-cache tactic on the quality attribute card, and partly in the deployment view, where the cache location is still open. The context view has Redis in it, and its decisions list still says "Caching … undecided". The author's verdict is "kind of": it is a partial match, visible only by looking at all the diagrams together.

> ❌ **Anti-pattern — Pattern shape present with no decision:** Cache Aside appears in sequences and tactics, but no decision record exists for it. **Why people do it:** not stated in the article. **Consequence:** the article says it has "no decision behind it at all", so the match is partial at best. **Fix:** record a decision with its trade-offs, then record the pattern on the elements it touches.

## When do you apply a pattern, and when not?

The author restates the two jobs: suggest patterns for a design, and check patterns already chosen against the design.

### Four-part definition of an applied pattern
The author's answer to "what does it mean for a pattern to be applied" has four parts. There must be a decision (someone chose it, why, and the trade-offs). The pattern must be recorded on the actual design elements it applies to. There must be a check that the design really matches the pattern. The fourth part, applying it, is the hardest. Remove any one and you are back to opinions.

### Draft, then accept
In PAL, applying a pattern changes nothing on its own. PAL drafts a decision record with the pattern, the elements it touches, and the forces and trade-offs taken from the library. The architect edits, argues and decides. Only accepting the draft records the pattern on the design, and only then can PAL check it.

### Conforms, partial, missing, not evaluated
Once a pattern is recorded, PAL reports that the design conforms, is partial (and says what is missing), or is missing completely. If it cannot check something, it says "not evaluated". The author's rule is that PAL never says you passed when nobody looked.

> ❌ **Anti-pattern — Silent pass:** Reporting a pattern as satisfied when nobody checked it. **Why people do it:** not stated in the article. **Consequence:** an unchecked claim passes as verified, which the author says would change half the architecture reviews he has sat in. **Fix:** report unchecked items as "not evaluated".

### Drift checking into code
The drift work carries the same check into the code, so a pattern can be followed from the library, to a decision, to the design, to what was built. The author says he has wanted that chain for about 20 years.

## Where the science comes in

A pattern does not live on one diagram. The Cache Aside example shows the same pattern split across a whole set of views, which is why a single diagram cannot settle whether it is applied.

### Structured model elements
Each Ergane model is structured: every element has an ID, every relationship has a type, and every card knows its viewpoint. This lets PAL compare a pattern against the actual elements and relationships across the cards and return conforms, partial or missing, along with what it found. Each result points at specific elements, so the architect can look and argue.

### Pattern descriptions as pictures
Vendor repositories describe patterns as a picture and a paragraph. The author says that is fine for learning a pattern but useless for checking one.

> ❌ **Anti-pattern — Pattern as picture and paragraph:** Defining a pattern only by a sketch and prose. **Why people do it:** not stated in the article; the author says it suits learning. **Consequence:** there is nothing to compare a design against, so no check is possible. **Fix:** describe each pattern per view (sequence, deployment, context) with the decisions and quality attributes it touches.

### Open questions in pattern matching
The author lists questions nobody has answered: how to judge a match, which parts of a pattern must appear in which view, what counts as partial, how to tell real Cache Aside from a look-alike, and whether a pattern missing from the deployment view means the design or the diagram is wrong.

### Growing the library
Each of the 200 patterns would need to say what it looks like in a sequence, a deployment view and a context view, and which decisions and quality attributes it touches. The author closes by asking for a pattern lead to help turn the library into something the profession can prove works.

## 🧠 Mental models

### Four-part test for an applied pattern
**The model:** A pattern is applied only when a decision exists, the pattern is recorded on the design elements it touches, a check compares design with pattern, and the application is carried through. Remove any one part and what remains is opinion.
**How to apply it:** When someone says "we use Cache Aside", ask for the decision, the elements it touches and the check result.

### Matching across views
**The model:** A pattern is recognized by its parts across several views, such as sequence, tactics, deployment and context, not by one diagram. A match is partial when some views show the pattern and others do not.
**How to apply it:** List the parts each viewpoint should show, then check each view. A gap in one view is either a design gap or a diagram gap, and someone has to decide which.

### Honest status: conforms, partial, missing, not evaluated
**The model:** A verification result has four states, and "not evaluated" is a legitimate answer rather than a pass.
**How to apply it:** Report anything unchecked as "not evaluated" so that no one reads it as passing.

## 🔑 Key takeaways
- A pattern counts as applied only with a recorded decision, the pattern recorded on specific design elements, and a check against the design.
- PAL is designed but not yet running, so the article describes intended behavior and not measured results.
- Cache Aside on PAL's board appears in the latency sequence, the answer-cache tactic and partly in deployment, yet has no decision behind it, which makes it a partial match.
- PAL drafts decision records from the library and changes nothing until the architect accepts the draft.
- PAL's check reports conforms, partial (with what is missing), missing or not evaluated, and never a pass when nobody checked.
- Pattern library entries need per-view descriptions and the decisions and quality attributes they touch, or they cannot be checked.
- The author reports roughly one in 500 interviews or mentoring sessions showed someone applying a pattern repeatably.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Application (PAL as a domain service with a REST interface to the board service) and Technology (deployment model, Redis, latency budget). Data is touched only lightly through the answer-cache key. Business is not covered. |
| Scope | Solution: the article concerns one service (PAL) designed on one Ergane board, although the 200-pattern library is meant as a shared reference. |
| Stakeholders and concerns | Architects, who must see where patterns sit and argue with drafts; architecture reviewers, who need evidence rather than claims; the board service, which needs the contract, timeouts and latency it depends on; the pattern lead role the author wants filled; the author, who is concerned with repeatability and rigor. The stated requirement is that PAL must not slow the board. |
| Frameworks and notations | BTABoK (named as the source of the viewpoints, the pattern library and the pattern lead role). No TOGAF, ArchiMate, BPMN or C4 is named. REST and Redis are named as technologies, not notations. |

### Decisions and trade-offs
- **Draft, then the architect accepts:** The options were to record a pattern on the design automatically or to draft a decision for the architect to edit and accept. The author chose the draft. The article states no cost; the implied cost is that every applied pattern needs human review before it is recorded.
- **Checking over recommending:** The author calls checking the job he cares about most, while recommending is the first job. The article does not describe a cost of this focus.
- **"Not evaluated" over a pass:** Unchecked items are reported as not evaluated. The article states no cost; the implied cost is more results that look incomplete.
- **Private domain service, called only by the board service:** The article gives this placement and does not discuss alternatives or costs.
- **Answer cache keyed by card version and library version, with Redis:** The tactic and technology are described, but the caching decision is still recorded as "undecided" in the context view and the cache location is open in the deployment view.

### Foundations it rests on
- [Architecture](/foundations/architecture.md) — "architecturally significant requirement" is used as a requirement that changes the system's shape, which fits the working definition of properties and principles governing how a system is built.
- [Application Architecture](/foundations/application-architecture.md) — PAL is described as a domain service with a defined interaction with the board service, which fits the structure and interaction of applications; the article does not address application structure across the enterprise.
- [Solution Architecture](/foundations/solution-architecture.md) — the work is scoped to one service designed on one board, which matches the single-solution scope, though the shared pattern library stretches it.
- [Architecture domain](/foundations/architecture-domain.md) — the article never names the four TOGAF domains and organizes its design by viewpoints instead, so it uses the idea differently from the working definition.

## 🏛️ For Kenji
This is your interpretation, not the author's. In architecture governance, the article suggests that a pattern decision could be reviewed against its recorded decision, the design elements it touches and a check result, instead of being asserted in a meeting. For your MBA work, it links to evidence-based decision-making. Which of your current design reviews would fail the "show me where" test, and what evidence would replace it?

## 📚 Reading plan
### From the article
- Patterns and Reference Models (link text only; the extracted article gives no URL, so none is written) — the author's 200-pattern library, the source PAL draws on.
### To go further
- Cache Aside — the worked example; read how it appears in a sequence and in a cache-backed deployment.
- Architecture decision records — the article's decision record is the unit PAL drafts and an architect accepts.
- BTABoK viewpoints and pattern library — the body of knowledge the article's method is built on.
- Quality attribute scenarios — the article's six-scenario card; read how a measurable response is specified.

## 🔗 Concepts
- [adr](/concepts/adr.md)
