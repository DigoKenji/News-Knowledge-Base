---
type: Article Summary
title: 'Spec-Driven Development Frameworks a Year Later: Pattern Families, Measurements and a Choice Matrix'
description: No spec framework has won; the right pattern depends on the change type, and spec maintenance after a second
  change is unmeasured.
resource: https://medium.com/gitconnected/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64
tags:
- software-engineering
- adr
- agent-harness
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/gitconnected/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64
  title: 'Spec-Driven Development Frameworks a Year Later: Pattern Families, Measurements and a Choice Matrix'
  author: Level Up Coding
status: stable
source_url: https://medium.com/gitconnected/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64
source: Medium (Level Up Coding)
source_type: medium
published: '2026-10-30'
domain: software-engineering
concepts:
- adr
- agent-harness
- llm-as-a-judge
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81878993f04066a929ee
---

## 🎯 Core thesis
No spec-driven development framework has won a year after the field had fifteen contenders; the right pattern depends on the type of change, and the cost of keeping a spec true after a second requirement change is the part nobody has measured.

## 🧭 Why the author wrote it
The author starts from a team choosing a spec framework now, with one public per-change-type measurement in hand, in which four frameworks landed within the noise band on merged tickets at first delivery. If the thesis is right, the decision should be made per type of change (bug fix, feature in an existing codebase, change across components), and each option should be tested at the second requirement change, not chosen as a product.

> 🔎 Added context: "Spec" here means a structured description of intended behavior that an AI coding agent receives as the contract for a change.

## Quick Win — Core Insight
The lead of this section gives the practical advice in one line: don't pick a framework, pick a pattern for the change type you make most often, then check how it behaves at the second requirement change.

### Pattern by change type
The only per-change-type measurement the author knows shows that a bounded bug fix with a failing test gained nothing from a spec: the control without a spec merged 9 of 10 tickets, the same as the best configurations (Kiro with a dedicated Bugfix Spec, BMAD with a track matched to the task). For a feature in an existing system the advantage returns, with spec arms merging 2 to 3 more tickets out of 10. The author's recommendation is a bug-fix template with three headings plus a regression test, presented explicitly as a recommendation to try rather than a measured result. For a change across several repositories, OpenSpec's dedicated features are in beta and no comparison was found.

### Spec maintenance after rollout
This is the point the author calls most important and which no study found compares: the cost of keeping a spec current. In the studied public repositories, 81.5% of spec files have a single commit. The author's argument is that a pattern which does not force the spec to be reconciled with the code in the same pull request leaves the agent, after the second change, with an outdated rulebook as context.

> ❌ **Anti-pattern — Choosing one framework for every change:** adopting a single workflow for bug fixes, features and cross-component work alike. **Why people do it:** the article gives no reason; it is the natural default when a team adopts one tool. **Consequence:** the vendors themselves have split their paths (Kiro Bugfix Specs, Spec Kit's bug extension, BMAD Quick Flow), which the author reads as an admission that one workflow does not fit every change. **Fix:** pick a pattern per change type and test it at the second change.

## The Year the First Version Stopped Being a Promise
The section describes how the field matured in one year and what changed in how vendors present their tools.

### Spec Kit 1.0.0
Spec Kit shipped version 1.0.0 on August 21, 2026, a year after its first commit. The maintainer described 1.0.0 as a version number, not a maturity certificate, and admitted that entering an existing codebase could be smoother than it is, and that the trust boundary is guidance, not enforcement. In practice this means the agent is told what to do and nothing stops it from doing otherwise. For a reader evaluating the tool, the release is a sign of a settling API rather than a proven production track record.

### Platform primitives
Version 1.0 combines five primitives: extensions, presets, integrations, workflows and steps. The author's point is that the framework became a platform, so presets and extensions now have to be versioned and updated like any other software dependency. That turns a template choice into a dependency-management choice.

### Technology Radar placement
In the April Technology Radar vol. 34, Thoughtworks placed Spec Kit and OpenSpec in the "Assess" ring, meaning worth exploring but not yet worth broad adoption. The Spec Kit entry describes two broad camps: teams relying on increasingly capable agents with minimal structure, and teams that prefer defined flows. The author cautions that the Radar reports team experience, not measurements.

### Bugfix paths
The most important change, in the author's view, is that vendors now split their paths by type of change. Kiro has Bugfix Specs, Spec Kit has had a bug extension since version 0.9.5 (assess, fix, test, with no spec, plan or tasks), and BMAD has Quick Flow. The author's reading is that this is an admission that one workflow does not fit every change.

> 🔎 Added context: Kiro, Spec Kit, OpenSpec and BMAD are named as tools in the article; the article does not describe their internals beyond what is quoted here.

*(Figure in the article: the year of spec frameworks, from the first commit to a 1.0 that does not promise maturity.)*

## Four Pattern Families Instead of Fifteen Products
The section reduces the fifteen products to four pattern families, grouped by how each family represents a change and what it treats as the source of truth. The author states this as a hypothesis: how a family represents a change largely determines the cost of maintaining the spec, though the studies cited do not separate pattern, tool and team process.

### Spec as consolidated bylaws
The simplest model the author offers is company bylaws kept as one consolidated text. The constitution is the charter, the set of rules that every change inherits. Families differ in how each change is taken in and then merged into that text.

### Constitution / specify / plan / tasks family
This family, represented by Spec Kit and, in IDE form, by Kiro's three documents, files each change as a full application with three attachments: what, how, and a task list. The converge command compares the current code with the spec, plan and tasks and appends the missing tasks, without changing the spec or the code. Converge is therefore a gap detector that reads and appends tasks, not a decision maker about which requirement wins.

> 🔎 Added context: converge is the Spec Kit command named in the article; it is described here only as far as the article describes it.

### Propose / apply / archive family with spec delta
OpenSpec represents each change as a spec delta, an amendment that lists only added, modified and repealed provisions. Archiving merges the change into the current spec and files its folder with a date. The MODIFIED section carries a full replacement block, not a note about the difference. The delta is the carrier of the change; the consolidated text is the result of merging deltas.

### IDE family
Kiro and planning modes in editors keep requirements, design and tasks inside the editor. Kiro's Bugfix Spec uses a WHEN … THEN / SHALL / SHALL CONTINUE TO pattern, so the bug fix also describes the behavior that must survive the change. The IDE is a cross-cutting axis: Kiro belongs to this family, yet represents a change like the constitution family.

### Spec-as-source family
In this family the spec is the source code and the generated code is the compiler output. Tessl is the example, and it pivoted to a skills registry this year while the framework stayed in beta.

### Levels of spec, and nondeterminism
Birgitta Böckeler of Thoughtworks distinguished spec-first, spec-anchored and spec-as-source levels in a 2025 analysis of three tools. Her analysis also found the weak point of the last level: generating repeatedly from the same spec produced different code. The author treats this as a risk built into the approach, not a bug of one version.

> ❌ **Anti-pattern — Treating the spec as the source and regenerating on every change:** letting the spec be the only thing a human edits and regenerating code from it. **Why people do it:** the article gives no reason; it is the promise of the spec-as-source family. **Consequence:** repeated generation from the same spec produced different code in Böckeler's analysis. **Fix:** the article does not prescribe one.

The author's classification puts the cells on two axes and notes that BMAD is treated as a variant of the full application with tracks matched to scale. The second-change mechanisms reduce to two strategies: a full application or an amendment. The author flags this as their classification, not the result of a study of market convergence.

*(Figure in the article: four pattern families on two axes, how you represent a change and what the source of truth is.)*

## The Protocol Through Which I Read Every Measurement
The author states up front that the text contains no runs of their own, and that the measurements come from preprints, reports and practitioner accounts collected through the end of October 2026. The section sets out the protocol the author uses to read every number.

### Six conditions for a fair comparison
A fair comparison, in the author's terms, needs three task types plus a second requirement change, the same model, pinned versions, a time budget, acceptance criteria, and a separation of elapsed time from active human minutes. Without these, a difference between tools can reflect the setup rather than the pattern.

### The Uvik benchmark
The best available measurement is the Spec-Driven Development Benchmark 2026 published by Paul Francis of Uvik Software. It has 50 production tickets from five repositories, the same declared model, random arm order per ticket, and up to 45 minutes for the spec plus up to 45 for the build. That means the control had up to 45 minutes in total while the spec arms had up to 90. It breaks three conditions: some tool versions are described only as "build" and "6.x", Kiro ran in a different harness, and there is no second change, as the authors state: "One ticket at a time".

### Harness
A harness is the environment that drives the agent, meaning its prompts, tools and loop. The author's point is that when one tool runs in a different harness, the comparison is confounded, because the tool's results are not only a function of its spec pattern.

### Noise band
The noise band is the range in which a difference can come down to chance. The authors give ±10 percentage points, and with 10 tickets per cell a single ticket moves the result by that much. The author notes this is the resolution of the score, not a confidence interval. The differences fall within the authors' operational band, which does not prove the tools are equivalent. The report is from a commercial publisher and has not been independently reproduced.

```javascript
# Spec pattern evaluation protocol (one sprint)
tasks:   1. bug fix with a failing test  2. feature in an existing module
         3. change across two components  4. second requirement change for task 2
fixed:   model + version, tool versions, time budget for spec and build
record:  elapsed time | active human minutes | rounds of corrections
         spec lines | test results | who reconciled spec with code
```

**What this does:**
- Lists the four tasks, including the second requirement change on task 2, which is the case the existing benchmark leaves out.
- Fixes the variables (model and versions, tool versions, time budgets) so that the comparison isolates the pattern.
- Records elapsed time and active human minutes separately, so speed is not confused with attention.
- Records spec lines, test results and who reconciled the spec with the code, which makes spec maintenance a measured quantity.

*(Figure in the article: the evaluation protocol. The best available measurement meets some of the conditions and does not study the second change.)*

## Results: What Was Measured and What Was Only Promised
The section keeps each number tied to the question it answers, so the author does not stack them into a ranking.

### First delivery
In the Uvik benchmark, four frameworks merged 40 to 42 of 50 tickets, against 36 for the control without a spec. Median time to merge was 29 minutes for the control and 36 for the delta family, with 41 minutes for the IDE family, 44 for the full application and 55 for BMAD. The frameworks sit within noise of each other. The control has the lowest median time among its own merged changes, but each arm merged a different set of tickets, so this is not a clean speed comparison.

### Per change type
With 10 tickets per cell, the control scored 90% on a bug fix with a failing test against 80 to 90% for the spec arms. For a feature in FastAPI or Django the control scored 60% against 80 to 90%, and for a change touching six or more files the control scored 50% against 80%. The author says this is the only measurement they know that supports the thesis that the best pattern for a bug fix is not the best for a feature, directionally and not as a ranking. A change touching six or more files is not the same as a change across several services or repositories.

### Corrections and spec documents
Rounds of corrections after the completion report were 0.46 to 0.64 in the spec arms and 0.74 in the control. Median spec length ran from 93 lines in the delta family to 188 in BMAD. Spec–code mismatch in merged changes was 2.4% for deltas and 4.9% for IDE, with 12.5% for the full application and 12.2% for BMAD. The author notes that with about 40 merged changes per arm this is a few events (1, 2, 5 and 5 changes), a signal to investigate rather than a confirmed difference, and it is the largest point difference between families at first delivery.

### Quality by LLM judge
An April 2026 preprint with 128 runs on five repositories scored the authors' system with an LLM-as-a-judge, a model that grades the output instead of a human. The full Spec Kit flow scored 3.51 on a 1 to 5 scale, and the variant without artifacts 3.46. Repository grounding hooks gave a clearer gain of +0.15 points, which the authors describe as catching context errors earlier rather than raising the average. A small blinded human test did not confirm the direction: 19 votes for the full flow, 8 for the hooks variant, and 33 ties.

> 🔎 Added context: an LLM judge can have its own biases; the article does not discuss this, so the human test is the author's only cross-check.

### Time for completed runs
Completed runs took 14.4 minutes without artifacts and 24.0 with the full flow. The author points out these come from different time budgets (40 and 90 minutes), so they do not measure the cost of adding a spec.

### Reviewer pilot
A pilot with five reviewers from September 2026 adds a distinction between detection and accountability. With an approved spec, recall (the share of real discrepancies found) did not differ significantly: 0.525 against 0.518, p = 0.69, which does not prove there is no effect. Attributing a finding to a specific requirement rose from zero to 0.81, at the cost of a longer review. The zero is structural, since without an approved spec there is nothing to cite. The author's reading is that in this pilot the spec bought accountability, not detection.

### Practitioner cost tests
Two runs by one practitioner on Medium (IT Chronicles, GPT-5.2) showed Spec Kit using 97 to 109% more tokens than OpenSpec. Another practitioner (Mani, Medium) reported that the same feature in an existing codebase took about 5 hours with reviews using Spec Kit and under two with OpenSpec. The author labels both as signals from single runs, not norms.

### Documentation only
Several features are documented but unmeasured: drift detection in the OpenSpec cloud agent, the converge and artifact-reconciliation extensions in Spec Kit, tests of unchanged behavior in Bugfix Spec, and BMAD tracks.

### Light structure without a framework
Light structure has partial support. The control without spec artifacts was competitive on the bug fix and small changes, although the author did not test the template recommended earlier. In a September 2026 preregistered preprint, a 267-word spec frame had fewer defects flagged by checkers in 95 of 100 non-tied pairs (out of 250) on one-shot Python code generation. The author notes it is unknown how much of this comes from the format and how much from the extra reasoning, since in Garg's pilot reasoning before coding produced a similar effect on simple tasks. No study was found comparing a template with a framework across the full sequence including a second change.

*(Figure in the article: green cells were measured by someone, gray ones are promised by documentation, white ones are empty, including the entire second-change row.)*

## The Second Requirement Change: Who Breaks Down
The section opens with a quotation from Helmuth von Moltke: "No plan survives first contact with the enemy." The author applies it to a spec written before implementation.

### Implementation drift
Agarwal and coauthors from CMU and Google, in a 2026 study, separated two ways a spec breaks. Implementation drift is when the spec says a session expires after 30 minutes and the agent implements 60. A discrepancy like this shows up in the tests, provided the tests reflect the right version of the requirement.

### Spec decay
Spec decay is when the business deliberately changes the time to an hour and the spec still says half an hour. Here the code and tests may already match the new intent, so a green test suite passes despite the old spec. The studied repositories show 81.5% of spec files with a single commit, though the author notes many are archived change descriptions, so this is not the share of outdated specs. A stratified LLM assessment of 1,000 documents found 77.6% contained implementation details. The authors conclude from interviews that such specs age faster, a conclusion and not a measured rate.

> ❌ **Anti-pattern — Writing implementation details into the spec:** describing how the code works rather than the behavior it must have. **Why people do it:** the article does not say; it reports that 77.6% of the sampled documents do it. **Consequence:** according to the authors' interviews, such specs age faster. **Fix:** the article implies keeping the spec to behavior; it does not give a detailed rule.

### Scenarios and parallel deltas
A scenario is an example of behavior pinned to a requirement, like a test case written in prose. Issue #1246 showed that two changes to the same requirement, prepared against the same old base, could both archive correctly while the second silently removed the first one's scenarios. The mechanism is that the whole block is replaced by requirement name. A later guard aborts archiving when current scenarios are missing. Issue #1387 shows the remaining gap: preserved scenario names do not protect against overwriting newer content with older. The author reconciles this with the 2.4% mismatch from earlier: that measurement covered single tickets, while the issues describe overlapping deltas prepared on a stale base and archived one after another.

> ❌ **Anti-pattern — Parallel deltas on a stale base, archived in sequence:** two changes to one requirement, both written against an old version of the spec. **Why people do it:** the article does not say; parallel work on one requirement is the implied case. **Consequence:** the second archive can silently remove or overwrite the first change, even when scenario names are preserved. **Fix:** the article points to a drift check before archiving and to explicitly linked, versioned changes across repositories.

### Converge and the living spec
The full-application family has converge, which detects gaps between the code and the artifacts but does not decide whether a new requirement replaces the old one. The choice among the flow-back, flow-forward and living spec models, described in the project's guide, is left to the team. In the IDE family, synchronization after a design change has no published cost measurement. In spec-as-source, the second change means regenerating, with the nondeterminism risk.

### A team that dropped the workflow
A SaaS team in Japan (RevComm) dropped the delta workflow in the frontend after a few months, because UI behavior changed too often and spec updates did not pay back (engineering blog, July 2026). The author says this is not a verdict on every deployment.

*(Figure in the article: parallel deltas on an old base. The guard protects scenario names, but archiving can still overwrite newer requirement content.)*

### The author's conclusion
Whatever the family, a behavior change should point to the current contract and the related spec change. Ideally this happens in one repository, in the same pull request; across repositories, as explicitly linked, versioned changes. Before archiving, a drift check is worth running.

## Decision Tree: Legacy, Greenfield, Compliance, Team
The author first assumed the tree would start with a question about the tool. The evidence reverses the order: first the type of change, then compliance and team, the tool last. The outcome should be recorded as an ADR, a short architecture decision record.

### Legacy and bug fix
Use a bug-fix template, a regression test and the constitution's rules. The full feature flow is overkill here: Böckeler described a small bug that was turned into 16 acceptance criteria. Kiro's counter-argument is that a bug fix is where the agent breaks working code, which is why the template has an unchanged-behavior section.

> ❌ **Anti-pattern — Running the full feature flow on a bug fix:** applying the complete spec-plan-tasks process to a small defect. **Why people do it:** the article does not say; the full flow is the default in its family. **Consequence:** Böckeler's example grew a small bug into 16 acceptance criteria. **Fix:** a three-heading bug template, a failing regression test, and an explicit unchanged-behavior section.

```javascript
**Bugfix:** <short description>
Current behavior:    WHEN <condition> THEN system <wrong behavior>
Expected:            System SHALL <correct behavior>
Unchanged:           System SHALL CONTINUE TO <behavior that must survive>
Regression test:     <path to the test that fails today>
```

**What this does:**
- The WHEN … THEN line captures the current wrong behavior in the same form as a requirement, so the bug is stated as a testable condition.
- The SHALL line states the expected behavior as a requirement the fix must satisfy.
- The SHALL CONTINUE TO line is the section the author singles out: it names behavior that must survive the fix, which is where agents tend to break working code.
- The regression test line points to a test that fails today, which ties the spec to something executable.

### Feature in an existing codebase
Use a spec delta by default. With a full application, use a lean preset and, as the author's recommendation, a constitution rule: search for existing functionality first. Böckeler described an agent that found existing classes and generated them from scratch anyway, so the rule targets that failure.

> ❌ **Anti-pattern — Generating new code when a similar class exists:** the agent writes a new implementation without checking the codebase. **Why people do it:** the article does not say why the agent does it. **Consequence:** duplicated functionality, reported by Böckeler. **Fix:** a constitution rule to search for existing functionality first.

### Greenfield
A full application with a constitution makes sense when the architecture is fluid and the team wants gates. A Reddit team cited in the CMU and Google study reported 2 to 3 times more tokens and about twice the time. The author calls this one anecdote, not a norm, but a value to include in the budget.

### Compliance
An archive of dated deltas and the attribution of findings to requirements give an audit trail. Enforcement remains in tests and review.

### Team size
For a solo developer or a small team, the author recommends planning mode plus a single file, as a recommendation rather than a measured result. For parallel streams and several components, the author suggests shared context for the agent (Stores and Worksets in OpenSpec, in beta) plus a separate check for a stale base and delta conflicts, which these features do not provide on their own. No effectiveness comparisons were found.

The tool matters in three places: in delta merge semantics, in converge (which only reads and appends tasks), and in gates built into the IDE. The author describes these as operational differences, not interchangeable Markdown.

*(Figure in the article: the decision tree, with the type of change first, then compliance and team, and the tool last.)*

## Summary: The Choice Matrix
The summary says that a year ago the map of fifteen frameworks answered what exists, and today the more useful question is which pattern fits the change being made this week.

Legend used in the matrix: measured means someone measured it, docs means documentation only, and recommendation means the author's conclusion to test.

1. The tools from the old fifteen fall into four pattern families (recommendation). The operational differences between tools remain.
2. At first delivery, differences in merged tickets fall within the noise band (measured: Uvik, 10 tickets per cell). The type of change differentiates the result more.
3. Bug fix with a test: a three-heading template (recommendation; the control equals the arms, measured). Feature in an existing codebase: spec delta (recommendation; the arms' advantage over control, measured). Several repositories: beta features (docs), with no measurements.
4. The cost of maintaining a spec after the second change has not been compared in the studies found. Linking a behavior change to a spec change (recommendation) works in every family.
5. Read vendors' numbers through the protocol: model, versions, budget, criteria, noise.

The author closes by asking readers who run the protocol on three tickets to describe what happened at the second requirement change.

## 🧠 Mental models
### Spec as consolidated bylaws
**The model:** a spec is company bylaws kept as one consolidated text, and the constitution is the charter every change inherits.
**How to apply it:** before adding a change, ask which charter rules it must inherit and which part of the consolidated text it amends.

### Delta as amendment
**The model:** a spec delta is an amendment that lists only added, modified and repealed provisions, and archiving merges it into the consolidated text.
**How to apply it:** when two amendments touch one requirement, check that both were written against the current base before archiving either.

### Pattern follows the change type
**The model:** the best pattern is the one that fits the type of change, and its behavior must be checked at the second requirement change.
**How to apply it:** choose the pattern for your most frequent change type, then run one requirement change against it before adopting it broadly.

### Measurement versus promise
**The model:** a number answers one question under one setup; a feature documented but not measured is a promise, not a result.
**How to apply it:** before trusting a vendor's figure, check the model, versions, time budget, acceptance criteria and noise band.

## 🔑 Key takeaways
- No framework won; the article's measured results fall within a noise band of about ±10 percentage points at first delivery, so the type of change matters more than the tool.
- For a bounded bug fix with a failing test, the control without a spec merged 9 of 10 tickets, as good as the best configurations.
- For a feature in an existing codebase, the spec arms merged 2 to 3 more tickets out of 10 than the control, the one advantage the article calls measured.
- The cost of keeping a spec current after a second requirement change is unmeasured in every study the author found, and it is the most important open question.
- Spec files are rarely edited again: 81.5% have a single commit in the studied public repositories.
- Overlapping deltas prepared on a stale base can overwrite each other on archive, even when scenario names are preserved.

## 🏛️ For Kenji
This is your interpretation, not the author's. The article's decision order (type of change first, compliance and team second, tool last) maps well onto architecture work, where the choice of a pattern is a decision to record as an ADR. The question it raises for an architect is how a team would detect spec decay in a repository before a second requirement change, since the article's measurements do not answer it.

## 📚 Reading plan
### From the article
- [Original article on Medium](https://medium.com/gitconnected/spec-driven-development-frameworks-a-year-later-pattern-families-measurements-and-a-choice-matrix-125bcddf1e64) — the full text with figures and cited sources; the extracted text carries no link URLs, so the sources it cites are listed below by name only.

### To go further
- Spec-Driven Development Benchmark 2026 (Paul Francis, Uvik Software) — the main per-change-type measurement the article analyzes, with its noise band and 50-ticket design.
- Thoughtworks Technology Radar vol. 34 (April) — the Assess placement of Spec Kit and OpenSpec and the "two broad camps" framing.
- Birgitta Böckeler's 2025 analysis of three tools (Thoughtworks) — the spec-first, spec-anchored and spec-as-source levels and the nondeterminism finding.
- Agarwal and coauthors (CMU and Google), 2026 study — the implementation drift and spec decay distinction and the 81.5% and 77.6% figures.
- Garg's reviewer pilot (InfoQ, September 2026) — recall versus attribution when reviewing against an approved spec.
- OpenSpec issues #1246 and #1387, as cited in the article — the overlapping-deltas and stale-base problem and its remaining gap.
