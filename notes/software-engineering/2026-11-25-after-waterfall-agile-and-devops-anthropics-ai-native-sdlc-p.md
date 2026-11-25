---
type: Article Summary
title: After waterfall, agile, and DevOps — Anthropic’s AI-Native SDLC playbook
description: AI-native SDLC replaces handoff meetings with versioned artifacts and write-time governance, since faster coding
  alone leaves the pipeline slow.
resource: https://medium.com/@vateo/after-waterfall-agile-and-devops-anthropics-ai-native-sdlc-playbook-0131a3a9156e
tags:
- software-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-25T06:47:00-03:00'
sources:
- resource: https://medium.com/@vateo/after-waterfall-agile-and-devops-anthropics-ai-native-sdlc-playbook-0131a3a9156e
  title: After waterfall, agile, and DevOps — Anthropic’s AI-Native SDLC playbook
  author: vateo
status: stable
source_url: https://medium.com/@vateo/after-waterfall-agile-and-devops-anthropics-ai-native-sdlc-playbook-0131a3a9156e
source: Medium (vateo)
source_type: medium
published: '2026-09-22'
domain: software-engineering
concepts:
- sdlc
- devops
- agile-scrum
- ai-agent
- agent-harness
- context-engineering
- software-requirements
- software-testing
- version-control-code-review
- ci-cd
- observability
- incident-response
- spec-driven-development
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/423baf74172d81569a37c9fdaa50422a
---

## 🎯 Core thesis
Now that AI has made writing code fast, the remaining delay sits in the process around it, and the author argues the fix is an AI-native SDLC: a loop in which AI is embedded in every phase, phases hand off through versioned files instead of meetings and tickets, and governance is enforced when work is written rather than at a later review.

## 🧭 Why the author wrote it
The author has led an AI transformation in an engineering organization since last year. Their team sped up coding with tools such as Claude Code, but delivery did not speed up, because planning, review, deployment and approvals still run at human pace. If the thesis is right, the process and the org chart have to be redesigned around AI, not just given AI tools inside the existing pipeline.

## 1. The Problem the Playbook Solves
The article opens with an experience most teams share once AI coding takes hold: code is written much faster, but delivery does not get faster. Teams first blame themselves for using the tools badly, then blame the tools, and only then find that the process is what is stuck.

The author describes their own path. Step one was switching the coding tool to Claude Code, the easiest step with the fastest payoff. Step two was building on spec-driven development (SDD) to handle harder questions: how many humans and agents collaborate, how code stays consistent, and how this scales across versions. Their development work now runs in three modes, chosen by project complexity: AI-assisted, AI-collaborative and AI-autonomous. Product, design, QA and operations have adopted AI as well. The problems show up at the seams. The product-to-UI handoff still produces a Figma mock that an agent cannot consume directly, and the handoff between development and testing, and between deployment and operations, is still the old pipeline.

The author's opening diagnosis has three parts. Once code stops being the bottleneck:

- The bottleneck moves to either side of the build step (planning, review, deployment), which still runs at human speed.
- Agents write most of the diff, and line-by-line human review cannot keep up.
- Exceptions and approvals still route through weekly and monthly committees, so governance costs rise.

The summary line is "the faster you code, the longer you wait."

### Local optimization versus whole-pipeline autonomy
Each step gets faster, but the whole flow does not. The author's point is that local optimization cannot fix what is fundamentally a whole-pipeline autonomy problem. Solving it requires a methodology designed for AI from the start, which the author calls an AI-native methodology.

### The electrification analogy
The author recalls how factories adopted electricity. Owners first bolted electric motors onto the spot where the steam engine had been, in the same building and on the same shafts. The real payoff took about forty years, and only arrived once factories were redesigned around electricity and the assembly line emerged. The analogy carries the article's main warning: swapping the power source without changing the layout yields little.

🔎 Added context: "Whole-pipeline autonomy" here means an AI agent can carry work across planning, build, test, deploy and operations with limited human intervention, not just help with one step.

## 2. What Is an AI-Native SDLC: The Loop, and the Plays
The AI-native SDLC is presented as a reimagined process that keeps the control objectives of the traditional lifecycle but enforces them differently. The flow changes from linear to a loop, with AI embedded at each point. The process automates handovers and triggers the next play, which the author says addresses the manual and clumsy handoffs between phases in the traditional SDLC.

The six phases survive: plan, design, build, test, deploy and maintain. What changes is how they connect.

### The loop of files
Instead of meetings, tickets and sign-offs, each phase ends by committing a file to git, and the next phase starts by reading it. The chain in the article is:

intent.md → spec.md → plan.md → code and tests → PR with review findings → incident record → back to a fresh intent.md

The key idea is that humans and AI read the same file. A product owner can work with intent.md, and so can an agent. Documents stop being memos written for people and become the interface between humans and agents. The commit history also serves as the audit trail: who asked for what, what the AI did, and who approved it.

### Plays
The body of the playbook consists of sixteen plays, grouped into six non-linear stages. Each play covers five things: what changes, how to start, the concrete steps, governance, and how to measure the result.

### Four principles
The plays rest on four principles:

- Artifacts are the evidence: everything goes into git.
- Defense in depth: skills make violations rare, and hooks make them nearly impossible.
- Humans hold the judgment seats: intent, risk and authorization always belong to people.
- Coexist with what you have: Jira and Figma do not need to be torn down.

## 3. Plan: Requirements Start with intent.md
Whoever has the idea talks to the AI directly, in their own words, until the idea is clear. The AI shapes that conversation into an intent.md file, which records the problem, the desired outcome, the affected users, the constraints and the open questions.

The article's example was written by J. Ortiz from claims operations, not by a product manager or an engineer:

```markdown
# Intent: claims status self-service 
Author: J. Ortiz (claims operations). Status: draft. 

## Problem 
Customers phone the contact center to ask where their claim is. Handlers spend roughly a third of call time on status-only queries. 

## Proposed outcome 
Customers see claim status, next step and expected date in the portal. 

## Affected users and systems 
Claims handlers, portal team, claims-core API. 

## Constraints 
No new PII in the portal session. Existing authentication only. 

## Open questions 
Do third-party loss adjusters need access too?
```

> 🔎 Added context: the article labels this block as javascript, but it is a plain-text intent document, so the fence uses a markdown hint. The content is unchanged.

**What this does:**
- `# Intent: ...` and `Author` / `Status` lines identify the document and its owner, so the file works as a record of who asked for what.
- `## Problem` states the pain in measurable terms: a third of handler call time goes to status-only queries.
- `## Proposed outcome` describes what the customer will see, not how it will be built, which keeps the intent separate from design.
- `## Affected users and systems` names the people and systems an agent needs to know about before planning.
- `## Constraints` and `## Open questions` carry the security limit (no new PII, existing authentication only) and an unresolved scope question, so both are visible before design starts.

The product owner's role shifts from writing requirements to reviewing intent. Clarification and record-keeping are done by the agent. Compared with today's process, the author says requirements analysis becomes thinner and starts earlier, drawing on user pain and insight and on the MRD and BRD. The article is candid that the exact way to use intent.md still needs tuning to each team and its people.

## 4. Design: Policy Binds at Write Time, Not Review Time
Prototyping and requirements design merge into a single session. The intent.md goes to a design agent, which produces a mock or front-end page together with a spec.md.

Security, compliance and brand rules are written as skills, so the AI is constrained the moment it writes the spec. Where rules conflict, the AI flags the conflict as a risk item and routes it to the policy owner before engineering sees it.

> ❌ **Anti-pattern — policy checked at review time:** compliance, security and brand problems surface in a review meeting weeks after the spec was written. **Why people do it:** that is how review gates have always worked, and the rules live in people's heads or in documents nobody reads at design time. **Consequence:** rework costs an order of magnitude more than it would have at design time, as the article puts it. **Fix:** encode the rules as skills the agent reads while writing the spec, and route conflicts to the policy owner at that point.

The author notes that their current practice uses a prd-spec.md, but moving straight from requirements to designed pages still needs heavy designer involvement.

## 5. Build: Review the Plan, Not the Code
Engineers start in plan mode. They give Claude the intent and the spec, and it drafts an implementation plan covering which files change, in what order, what could break, and what proves the change is done.

🔎 Added context: plan mode is a Claude Code mode in which the agent proposes a plan for approval before it makes edits.

The bar is strict: an engineer who never sat in on the discussion should be able to implement the change from the plan alone. Once the plan passes, the implementation usually lands in one pass, because the directional questions were settled in the documents and execution is what AI does best.

The phase comes with three assets:

- **CLAUDE.md**: described as the new-hire manual for the AI, covering commands, conventions, architecture and the mistakes it keeps making. A mistake gets written in only the second time it happens, and the file stays under one page.
- **Skills**: turn organizational knowledge into soft constraints on code.
- **Hooks**: hard constraints. Blocked paths stay blocked, and credentials never enter a diff.

The summary rule: skills make compliance probable, and hooks make it certain. The author's current practice is a design and spec directory that records these rules by category and module, in real detail, and the author says this works quite well.

## 6. Test: The AI Checks Itself First
Before a session hands anything over, the AI runs the build, runs the tests and screenshots the UI, and fixes the problems until everything is green. "Done" includes the evidence.

The bug-fix discipline is the clever part. The AI first reproduces the bug as a failing test and commits that test. Then it fixes the code, with a hook locking the test file so it cannot be changed. A pre-existing test that the agent cannot rewrite is the only proof that the bug is actually gone.

> ❌ **Anti-pattern — the agent edits the test it must pass:** the agent adjusts or deletes the test so the suite goes green. **Why people do it:** a failing test blocks the session, and rewriting it is the quickest way forward. **Consequence:** the suite stops proving anything, and the bug can return unnoticed. **Fix:** commit the reproducing test first, and lock the test file with a hook before the fix is written.

### Evaluating the AI's own configuration
One layer further out, the AI's configuration is regression-tested like code. The author suggests choosing 20 to 50 real historical tasks as an eval suite. Any change to CLAUDE.md, skills or hooks reruns the whole set. Every production incident becomes a permanent eval case.

🔎 Added context: an eval suite here is a fixed set of tasks with known good outcomes, run against the agent setup to detect regressions after a change.

## 7. Deploy: The Agent Walks Up to the Production Gate — It Doesn't Walk Through
Review runs in both directions. The AI reviews other people's pull requests under one policy, and it works through the comments on its own pull requests. Humans answer exactly two questions:

> 📋 **Checklist — human review of AI changes**
> - [ ] Is this change what the plan called for?
> - [ ] Is the risk acceptable?

Governance does not run on review meetings. It runs on hooks as gates. Autonomy is tiered: the AI ships freely to development environments, staging sits in between, and production requires a named human's sign-off.

> ❌ **Anti-pattern — the agent passes the production gate itself:** the same agent that wrote the change also pushes it through to production. **Why people do it:** it removes the wait for a human and looks like automation. **Consequence:** the approval step disappears from the audit trail, so no named person owns the production risk. **Fix:** keep production sign-off with a named human, and make the gate a hook, not a meeting.

The author also gives a counterintuitive reminder: rollback should be the best-rehearsed path in the whole pipeline, because the agent in the maintain phase may trigger it at any hour.

## 8. Maintain: The Loop Closes
Monitoring does not use AI at all. Deterministic scripts watch metrics against statistical baselines. Only when a metric crosses a threshold is the AI woken, and its authority depends on how far the metric has moved:

- One standard deviation: log it.
- Two standard deviations: read-only diagnosis.
- Three standard deviations: act. Even then, only two moves are allowed: open a PR into review, or trigger a pre-approved runbook.

> ❌ **Anti-pattern — open-ended agent remediation:** an agent woken by an alert is allowed to take any corrective action it judges necessary. **Why people do it:** it is the fastest way to stop an incident at night. **Consequence:** the agent may change production in ways no one reviewed, and the change leaves no approved path behind. **Fix:** tie the agent's authority to the size of the deviation, and limit action to a reviewed PR or a pre-approved runbook.

The diagnosis is written as a new intent.md and flows back into the plan phase. Production issues become input to the next round of development, which the author notes traditional SDLC never had.

The playbook includes a real example from a Slack incident channel. At 22:04 someone @-mentioned Claude. Three minutes later it delivered a diagnosis against the release diff. A human replied "Go". Four minutes after that, the rollback was complete and the postmortem was filed. The request, the diagnosis, the authorization and the fix all stay in the channel, which serves as the audit log.

## 9. Why I Take It Seriously
The author gives three reasons. First, the playbook operates on the process layer. Using the pattern of tools, then process, then industry, the author argues that this is the step with which a paradigm shift usually begins. Second, it addresses governance directly: compliance, audit and security are the last mile of enterprise adoption, which the AI-coding conversation has mostly skipped. The author argues that what regulated companies can adopt is what tends to become the de facto standard. Third, the authors run the process themselves: 80% of the code merged at Anthropic is written by Claude, and the test suite is almost entirely Claude-written. The article repeats that over 80% figure as the company's own disclosure.

The author's caveat is that the de facto standard is still a frontrunner's draft, not a verdict. Agile took nearly a decade to move from manifesto to global default, and the author expects the AI-native process will need its own period of adoption, though probably not as long. The closing advice is to push further against this playbook, because in the AI era the goal is to be a player, not a spectator.

## 🧠 Mental models
### Tools, then process, then industry
**The model:** software paradigms have cycled through waterfall, agile and DevOps with the same order: tools shift first, process catches up, and then the industry reorganizes around them. Agile produced Jira and the Scrum Master, and DevOps produced Kubernetes and SRE.
**How to apply it:** when a new tool becomes standard in your teams, expect the process and roles to be the next thing that must change, and plan for that instead of stopping at the tool rollout.

### The factory motor
**The model:** adopting a powerful new technology without changing the surrounding layout captures very little of its value. The gain arrives when the layout is redesigned around the new source of power.
**How to apply it:** for each step you speed up with AI, check whether the handoffs before and after it still assume the old bottleneck.

### Skills make compliance probable, hooks make it certain
**The model:** soft constraints, such as skills and conventions in CLAUDE.md, make the right behaviour likely. Hard constraints, such as hooks that block paths or credential leaks, make the violation nearly impossible.
**How to apply it:** put rules that can tolerate exceptions into skills, and put rules that must never be broken into hooks.

### Graduated authority
**The model:** the AI's authority should scale with how severe the situation is: log for small deviations, diagnose for larger ones, and act only within pre-approved moves.
**How to apply it:** set explicit thresholds and the allowed actions for each level before an incident, not during one.

### Humans hold the judgment seats
**The model:** intent, risk and authorization stay with people, while the agent does drafting, checking and execution.
**How to apply it:** before automating a step, name the human who owns the decision it feeds.

## 🔑 Key takeaways
- Code generation is no longer the bottleneck; planning, review, deployment and approvals are, and they still run at human pace.
- Each SDLC phase hands off by committing a file to git (intent.md, spec.md, plan.md, code, incident record), and humans and agents read the same file.
- Policy and compliance rules should be enforced when the spec is written, using skills, with conflicts routed to the policy owner, not caught in a review meeting weeks later.
- Tests written before a fix, locked by a hook, are the proof that a bug is gone; the AI should not be able to edit them.
- Production sign-off stays with a named human. Autonomy is tiered by environment, and rollback should be the best-rehearsed path.
- Monitoring is deterministic. The AI is woken only past a statistical threshold, with authority that grows with the deviation, and incidents feed the next intent.

## 🏛️ For Kenji
Your work on architecture governance is where this applies most directly: the playbook's claim is that governance has to move from review gates to the moment artifacts are written, and that the handoff files become the architecture's audit trail. This is your interpretation, not the author's. The question it raises for your architecture practice is which of your current governance decisions could be encoded as skills or hooks, and which must stay with a named human because they are judgment calls about risk.

## 📚 Reading plan
### From the article
- No usable external links. The article's inline links point to bare filenames (intent.md, spec.md, plan.md, CLAUDE.md, prd-spec.md), not to resources, so there is nothing to follow.

### To go further
- Anthropic, The AI-Native SDLC playbook (dated 2026-08-21 in the article's references) — the primary source this article summarizes, and the source of the sixteen plays.
- Agile and Scrum history — the article uses agile's nearly ten-year path from manifesto to default as its adoption benchmark.
- DevOps, Kubernetes and SRE — the article's comparison point for how earlier paradigm shifts produced new tooling and roles.
- Spec-driven development — the practice behind the article's step two, where specs drive how agents build software.

## Concepts
- [sdlc](/concepts/software-engineering/sdlc.md)
- [devops](/concepts/software-engineering/devops.md)
- [agile-scrum](/concepts/software-engineering/agile-scrum.md)
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [agent-harness](/concepts/ai-ml/agent-harness.md)
- [context-engineering](/concepts/ai-ml/context-engineering.md)
- [software-requirements](/concepts/software-engineering/software-requirements.md)
- [software-testing](/concepts/software-engineering/software-testing.md)
- [version-control-code-review](/concepts/software-engineering/version-control-code-review.md)
- [ci-cd](/concepts/software-engineering/ci-cd.md)
- [observability](/concepts/cloud-infra/observability.md)
- [incident-response](/concepts/security/incident-response.md)
- [spec-driven-development](/concepts/software-engineering/spec-driven-development.md)
