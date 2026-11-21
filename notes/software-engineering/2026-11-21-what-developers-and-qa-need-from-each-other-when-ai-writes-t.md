---
type: Article Summary
title: What developers and QA need from each other when AI writes the code
description: AI speeds up code change, so proof becomes the bottleneck and the Development-to-QA handoff must carry a shared
  evidence contract.
resource: https://www.cio.com/article/4251514/what-developers-and-qa-need-from-each-other-when-ai-writes-the-code.html
tags:
- software-engineering
- verification-tax
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4251514/what-developers-and-qa-need-from-each-other-when-ai-writes-the-code.html
  title: What developers and QA need from each other when AI writes the code
  author: CIO.com
status: stable
source_url: https://www.cio.com/article/4251514/what-developers-and-qa-need-from-each-other-when-ai-writes-the-code.html
source: CIO.com
source_type: blog
published: '2026-11-20'
domain: software-engineering
concepts:
- verification-tax
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81ec8788c10b9dd22f33
---

## 🎯 Core thesis
AI makes code changes cheap enough that proof becomes the bottleneck, so teams must design the Development-to-QA handoff around a shared contract of evidence before they accelerate delivery.

## 🧭 Why the author wrote it
The article starts from a small, passing change to a shared component that broke a paying customer's access, and only a live-system alert caught it. If the thesis is right, teams should judge AI-assisted delivery by whether each release is proven from outside the change, not by how fast code is produced or whether the build is green.

## The change that passed every check
A small edit to a shared component passed code review and QA. The tests were green and the code did what the ticket asked. Normally, when a customer bought a subscription and closed the browser, the system turned that order into working access in the background. The change broke that step, and an alert from the live system's logs caught the failure that QA and the reviewers missed. The author's diagnosis is that the tests were not what failed. The handoff was: nobody had documented which other services used the component, or the order in which they had to change. Developer work and QA work meet at that handoff, and AI strains it by multiplying small edits to shared code faster than anyone documents their dependencies.

### Green build
A green build shows only that the code passed its own tests. The article's warning is that those tests were usually written from the same reading of the ticket as the code, so they agree with it by design. A release that can be defended to the business therefore needs proof from outside the change, and the handoff decides whether that proof arrives.

### Delivery stability
Delivery stability means how often changes fail in production or need unplanned fixes. DORA's 2025 study of nearly 5,000 technology professionals found that teams adopting AI delivered more while this stability got worse. The finding matters because it shows that higher output alone does not indicate healthy delivery.

> 🔎 Added context: DORA is the DevOps Research and Assessment program, and change failure rate, which the article reuses later, is one of its standard delivery metrics.

## Design the handoff before starting delivery
In most delivery teams the author works with, the handoff from Development to QA is a ticket changing status and not much else. The proposed fix is agreement among Architecture, Development, QA and Product on the minimum context and evidence every change must carry. That agreement becomes the handoff and sign-off contract.

### Handoff and sign-off contract
The contract defines what the change should do, in pass-or-fail terms. It names the affected components, dependent services and changed interfaces or data contracts. It lists open items, such as unresolved questions and unfinished agent work, and defines the evidence required at each gate. It also states who can accept the residual risk. The purpose is that each stage receives what it would otherwise have to rebuild.

### AI requirements reviewer
During review, an AI requirements reviewer reads the ticket and checks that the code meets its promises. QA asks a different question: whether what the ticket asked for is safe to release. That includes checking whether the specifications were right and whether the system around the change still works. Because the two questions differ, a pass on the first does not answer the second.

### Reciprocal release signal
The communication runs both ways. QA gives Development a release signal that separates product defects from noise in tests and the environment. QA also calls out missing evidence and says which parts of the handoff helped. That feedback feeds into the contract, the test strategy and the automation rules for the next change.

## What QA needs from the ticket and from the change
QA has to know what "correct" means before it looks at the code. That definition comes from the ticket, as approved scope and acceptance criteria that can each be marked pass or fail.

### Requirements-first testing
The tests that come with a change usually rest on the same reading of the requirements as the code, so they confirm the code's own story. The article's example is a ticket stating that a subscription is canceled at the end of the paid period. If QA writes tests from that statement, an implementation that cancels immediately fails them. If QA writes tests from the finished code instead, the same immediate cancellation passes, and the tests certify it as correct. Testing from the requirements therefore has to come first.

> ❌ **Anti-pattern — Writing tests from the finished code:** the tests describe whatever the code already does. **Why people do it:** 🔎 the implementation is the most visible artifact when testing starts; the article gives no reason. **Consequence:** the tests pass and confirm a wrong behavior as correct. **Fix:** write tests from the ticket's acceptance criteria before reading the implementation.

### Two maps: requested and touched
QA compares two maps. One shows what the ticket requested. The other shows what the handoff says was touched. Even when the ticket does not mention it, a redeployed shared service belongs on the second map. If dependent services were not updated, or extra work was never declared, the change is incomplete. Services that are just along for the ride widen the test plan. Reading the code still matters later, for diagnosis, such as stabilizing a test that keeps failing with no product defect behind it.

## Match assurance and evidence to the change
Three things make one change harder to prove than another: whether the code is new or old, how much of the feature the AI generated, and how much freedom the tools were given. Real tasks involve a mix of all three.

### Difficulty axes
The easy side is a minor new feature where the engineer still controls the tools and the quality checks are already in place. The hard side is a major change to an older system built with very little supervision. There the context must be rebuilt, so the same change costs more to prove. Proof may then require contract tests, integration checks, security or performance evidence, and explicit human sign-off.

### Blast radius
How much proof a change needs depends on its business impact and blast radius, meaning how much else the change can take down. The small edit illustrates this, because it broke the step that gives a paying customer working access.

### Evidence checkpoints
Teams collect proof at different checkpoints, such as finished tests, completed reviews and checked requirements. The closer a change gets to release, the higher the bar. The handoff carries what the next stage would otherwise have to rebuild.

### Four result states
Each test is logged as pass or fail. A test that was started but could not be finished is marked blocked. A test that was never run is left empty. Blocked and empty show where proof is missing, so a green build cannot pretend that every test passed.

## Measure confidence per unit of time
Confidence per unit of time is how quickly a team can turn a change into a release decision backed by proof. The article defines it as evidence completeness divided by validation lead time, counted only after completeness clears the threshold set for that risk class. The article's rule is that faster validation is not an improvement if completeness falls.

### Evidence completeness
Completeness is the share of required evidence items recorded as passed or failed for that risk class. Blocked and empty items do not count. The article's own change shows the effect: the build was green, but the checks on dependent services were empty. That meant low completeness and an unfinished release. The author notes that empty checks are common.

> ❌ **Anti-pattern — Treating a green build as complete proof:** the build status looks finished while required checks are still empty. **Why people do it:** 🔎 a single green status is the most visible signal; the article gives no reason. **Consequence:** a release is approved on partial evidence. **Fix:** count only passed or failed items against the required set for the risk class.

### Validation lead time
Validation lead time runs from the moment a change is ready for review to the recorded release decision. It is a segment of DORA's lead time for changes.

### Seven signals and three comparisons
The article says seven signals are enough to start. Change failure rate stays as it is. The other six are read as rates or durations:

- Escaped defect rate: production defects as a share of all defects found.
- Critical escape share: the share of those escapes rated critical.
- Validation lead time, as defined above.
- Non-product failure rate: test or environment failures as a share of all failures.
- Rework on generated code inside a fixed window, such as two weeks after acceptance.
- Evidence completeness, the one measure with no parent in the standard set.

Add two pieces of context: build speed, and how often a person has to step in. Then compare build speed against validation lead time, escaped defects against their severity and change failure rate, and rework against non-product failures and human intervention.

The article warns that raw counts mislead. If throughput doubles and escaped defects go up by half, quality improved, even though a raw count reports a regression. 🔎 Per change, that is 0.75 of the earlier escape rate.

> ❌ **Anti-pattern — Reading raw counts as quality:** a rising count of defects is taken as a regression with no reference to volume. **Why people do it:** 🔎 counts are the simplest figures to report; the article gives no reason. **Consequence:** a team that is improving looks like it is getting worse. **Fix:** normalize by volume and track rates.

## Expect a dip at first.
Early adoption is not a straight line up, according to the article.

### J-curve
DORA's ROI of AI-assisted software development research describes the early path as a J-curve. Teams see a temporary productivity dip and a period of instability while they adjust. 🔎 Added context: a J-curve is a performance pattern that falls before it rises, so early results can look worse than the baseline.

### Verification tax
The dip comes from what the research calls the verification tax: the extra time spent reviewing generated code as its volume rises. The tax grows with volume, so checking capacity has to keep pace with generating capacity. The article's test for this is whether checking keeps up with generating.

## Start with one delivery path
McKinsey's 2026 State of AI survey found that nearly three-quarters of AI high performers had fundamentally redesigned their workflows, compared with roughly a quarter of other respondents. The article notes that redesign does not have to start everywhere. One real delivery path is enough.

### One real delivery path
Before the next AI-generated change reaches that path, the four functions need the handoff and sign-off contract in place. They should measure the handoff against its own baseline, and then watch whether checking keeps up with generating.

### Earned tool freedom
AI tools can be given more freedom only when confidence per unit of time improves and delivery stability holds. Freedom is therefore earned from measured proof, not granted at the start.

### Role evaluation
QA is moving from re-running the obvious checks to designing the proof. Development is producing changes the next stage can confirm on its own. Neither role should be evaluated by the number of bugs found. The article closes with the point that if you accelerate your coding without accelerating your proof, AI will just help your team reach its next bottleneck faster.

## 🧠 Mental models
### Two maps
**The model:** The ticket defines the requested scope, and the handoff defines the touched scope. A change is checked against both.
**How to apply it:** Before QA runs anything, list the requested scope and the touched scope side by side. Any touched service with no test is a gap.

### Blast radius sets the proof bar
**The model:** The proof a change needs scales with its business impact and blast radius, not with whether the code is new.
**How to apply it:** Rate each change by what else it can take down, then set the required evidence items and sign-off level from that rating.

### Proof must keep pace with generation
**The model:** Generating faster only helps if checking keeps up. Otherwise the bottleneck moves to proof.
**How to apply it:** Track confidence per unit of time and evidence completeness together. Do not accept a lead-time gain that lowers completeness.

## 🔑 Key takeaways
- A green build shows that code passed tests written from the same reading of the ticket. It does not show the change is safe to release.
- The Development-to-QA handoff should be a written contract that names pass-or-fail criteria, affected components, dependent services, open items and the evidence each gate requires.
- QA should write tests from acceptance criteria before reading the implementation, then compare the requested scope with the touched scope.
- Blocked and empty results mark where proof is missing, and they do not count toward evidence completeness.
- Measure confidence per unit of time as evidence completeness divided by validation lead time, and only after completeness clears the threshold for the risk class.
- Expect a J-curve: a temporary productivity dip while verification work rises with the volume of generated code.
- Give AI tools more freedom only when confidence per unit of time improves and delivery stability holds.

## 🏛️ For Kenji
This is your interpretation, not the author's. For a Staff Architect, the handoff and sign-off contract looks like an architecture artifact in its own right, since it defines which dependencies and interfaces a change must declare before it ships. That is close to the governance work you already do in AI governance. The question it raises: which dependency and interface map would your team need before a change to a shared component could ship with a documented order of rollout?

## 📚 Reading plan
### From the article
- None. The article cites no links, so none are listed here.

### To go further
- DORA's 2025 study of AI adoption and delivery stability (named without a title in the article): the source of the nearly 5,000-professional finding.
- Tricentis 2026 Quality Transformation survey: the source of the 60% untested-to-production figure.
- DORA's ROI of AI-assisted software development research: the source of the J-curve and verification tax.
- McKinsey 2026 State of AI survey: the source of the workflow redesign comparison.

## Concepts
- [verification-tax](/concepts/verification-tax.md)
