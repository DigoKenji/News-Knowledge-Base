---
type: Article Summary
title: 'QCon San Francisco 2026: Automating Spark Upgrades with Codex Agents at OpenAI'
description: Agents create the most value in open-ended platform operations when deterministic systems provide the guarantees
  and guardrails.
resource: https://www.infoq.com/news/2026/11/openai-spark-fleet
tags:
- data
- ai-agent
- shadow-testing
- authentication-authorization
- observability
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-26T06:47:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/openai-spark-fleet
  title: 'QCon San Francisco 2026: Automating Spark Upgrades with Codex Agents at OpenAI'
  author: Robert Krzaczyński
status: stable
source_url: https://www.infoq.com/news/2026/11/openai-spark-fleet
source: InfoQ
source_type: blog
published: '2026-11-25'
domain: data
concepts:
- ai-agent
- shadow-testing
- authentication-authorization
- observability
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/424baf74172d812f95adf975b1319a75
---

## 🎯 Core thesis
AI agents are most valuable for open-ended operational work that cannot be reduced to fixed playbooks, but only when deterministic systems supply the guarantees and safety mechanisms underneath them.

## 🧭 Why the author wrote it
The article starts from OpenAI's data platform, which runs at exabyte scale with hundreds of thousands of datasets and a Spark fleet on millions of cores. Mouli Mukherjee's QCon San Francisco 2026 talk argues against using agents to rebuild that platform. If the thesis is right, teams build hard guarantees as ordinary systems and let agents use them as tools for the judgment-heavy work around them.

## The platform and the claim
At QCon San Francisco 2026, Mouli Mukherjee, Engineering Manager for Data Infrastructure at OpenAI, argued that agents are most useful not for rebuilding a data platform but for open-ended operational work that is hard to turn into fixed playbooks. She showed how Codex-driven agents can take on that work when the right systems sit underneath them. The scale she describes is exabyte scale, hundreds of thousands of datasets, and a Spark fleet on millions of cores.

### AI agent
An agent, in this article's sense, is a system in which a language model decides, step by step, what to do next. It chooses an action, uses a tool, reads the result and then decides what to try next. The article's point is that this loop fits tasks where the next step depends on what the previous step revealed. It does not fit tasks that must return the same output for the same input every time.

### Apache Spark
> 🔎 Added context: Spark is a distributed engine that runs data-processing jobs across many machines at once. The article treats the Spark fleet as the thing being upgraded and tuned, and does not discuss Spark's internals.

## Deterministic systems and agents
Mukherjee separated two kinds of components. Anything that needs consistent outputs for the same inputs, such as access control, configuration resolution and data correctness guarantees, belongs in proper systems. Agents use those systems as tools and do not reimplement them. She put the principle this way:

> We should definitely build the systems where we want guarantees and guardrails.

Agents earn their place where failure modes and remediations cannot all be listed in advance. Debugging is the example: the agent follows the evidence, decides what to try next, and stays inside the guarantees of its tools.

### Deterministic systems
A deterministic system returns the same output for the same input, so its behaviour can be predicted, tested and audited. The article places access control, configuration resolution and data correctness here because a wrong answer in any of them is costly and hard to notice. Keeping these in ordinary code means the guarantees are written once and checked the same way every time. Agents then call them rather than trying to reason their way to an equivalent answer.

> ❌ **Anti-pattern — Agent reimplements a deterministic system:** the agent works out access decisions, config values or correctness checks itself instead of calling the system that owns them. **Why people do it:** the article does not discuss the motive; it is a tempting shortcut when a model can already produce plausible logic. **Consequence:** the article's position is that the answer may not be consistent for the same inputs, so guarantees are lost. **Fix:** keep the guarantee in a system and expose it to the agent as a tool.

## Making the Spark upgrade safe
The Spark upgrade is the worked example. Before any agent acted, the team built three safeguards. Each one was in place before the next step depended on it, and the sequence is the core of the example.

### Shadow run
A shadow run is a copy of a job that runs on a new system while the production system still controls the real outcome. The team's shadow-run service reused production inputs, so the test saw the same data production saw. It ran on a low-priority queue so that test work did not affect live operations.

> 🔎 Added context: a shadow run tests the new version against real traffic without letting its results reach users. In this article, the "traffic" is production job inputs.

### Configuration override service
The team built a configuration override service so that experiments could not change global settings. Without it, an experiment on one job could alter behaviour for everything else. The service keeps each trial local to the job it tests.

### Validation skill
The team also built a validation skill that compares the production run with the shadow run. It is the check that decides whether the new Spark version behaves acceptably. The article does not describe the comparison criteria.

### Agent-driven migration sequence
Only after those safeguards were in place did agents begin the work. They claimed jobs, launched shadow runs, reviewed validation reports, classified errors and transitioned jobs to the new Spark version. The agents' role was the repetitive coordination and triage, and the systems defined what counted as safe.

> ❌ **Anti-pattern — Letting agents act before the safety mechanisms exist:** agents launch and promote upgrades while there is no shadow test, no isolation of experiments and no comparison. **Why people do it:** the article does not discuss the motive; agents make fast progress on visible work. **Consequence:** the article's concern is that experiments become unrepeatable, unobservable and hard to reverse. **Fix:** build shadow runs, isolation and validation first, then let the agent operate inside them.

## Routine platform operations
Mukherjee reported that the same primitives now cover more of the routine work. The team uses them for tuning, releases, failures and support.

### Continuous job tuning
Agents tune job efficiency by inspecting query plans, trying a configuration change, comparing the runs and iterating. The loop is the same as in debugging: observe, change one thing, measure, repeat. The article presents this as continuous work rather than a one-off project.

### Direct promotion for low-risk releases
For lower-risk incremental library releases, the team sometimes skips the shadow step and promotes directly. This trades capacity, meaning the spare resources a shadow run would consume, for higher rollback risk. It is a deliberate choice made per release type, not a default.

### Auto-rollback
A job that passes shadow validation but later fails in production can roll back automatically to the previous Spark version. Pipelines keep running without manual intervention. The mechanism is what makes the direct-promotion trade acceptable: a bad release has a defined way back.

### Observability for support
Agents search past incident reports, suggest runbook updates and trace why a table is stale through orchestrator and scheduler logs. Those logs are the evidence the agent follows. The article presents this as a way to answer "why is this stale" without manual log digging.

### Access control and user credentials
Agents help users understand access-control denials. Permissions still enforce the user's own credentials, so the agent explains a decision the system made and does not grant access itself.

## Recommendation: build the bounds first
Mukherjee recommended starting with repetitive, open-ended tasks where judgment matters and failure modes are too many to hard-code. Build the safety mechanisms first, then let agents operate inside those bounds.

### Repeatable, observable and reversible experimentation
The article's stated goal is that experimentation stays repeatable, observable and reversible. Shadow runs make it repeatable, logs and reports make it observable, and rollback makes it reversible. Each safeguard in the article maps to one of those three properties.

## 🧠 Mental models
### Guarantees in systems, judgment in agents
**The model:** Anything that must give the same answer for the same input belongs in a deterministic system. Agents handle the open-ended judgment around those systems.
**How to apply it:** For each step in a workflow, ask whether the answer must be consistent. If it must, make it a tool the agent calls rather than logic the agent improvises.

### Guardrails before autonomy
**The model:** Agents should not be given autonomy until the safety mechanisms exist. The article built shadow runs, isolation and validation first.
**How to apply it:** Before enabling an agent on a live system, list the mechanisms that make a bad action visible and reversible. If any are missing, build them first.

### Trade capacity for rollback risk
**The model:** Skipping a validation step saves capacity but raises the cost of a failure. The article makes this trade per release type, accepting it for lower-risk releases only.
**How to apply it:** Classify changes by risk. Keep the full validation path for high-risk changes, and use the cheaper path only where an automatic rollback exists.

## 🔑 Key takeaways
- Mukherjee's split is explicit: consistency-critical functions such as access control, configuration resolution and data correctness stay in deterministic systems, and agents use them as tools.
- Agents earn their place where failure modes and remediations cannot be fully listed in advance, such as debugging.
- OpenAI built a shadow-run service on a low-priority queue, a configuration override service and a validation skill before agents operated the Spark upgrade.
- Shadow runs reuse production inputs and keep test work off live operations, so the upgrade is tested without affecting production.
- For lower-risk incremental library releases, the team sometimes promotes directly without a shadow step, trading capacity for higher rollback risk.
- Jobs that pass shadow validation but later fail in production can auto-roll back to the previous Spark version without manual intervention.
- Mukherjee's recommendation is to start with repetitive, open-ended tasks, build the safety mechanisms first, and then let agents operate inside those bounds.

## 🏛️ For Kenji
> 🔎 This is your interpretation, not the author's. In an architect's work, the useful lesson is where to draw the line between a system that owns a guarantee and an agent that operates on top of it, such as an upgrade pipeline or a configuration review. The question it raises: for a workflow you own, which of its steps must return the same answer every time, and is that guarantee currently in a system, or implied by an agent's behaviour?

## 📚 Reading plan
### From the article
- [QCon San Francisco 2026](https://qconsf.com/presentation/nov2026/building-data-platform-ai) — the session page for Mukherjee's talk, the source of the claims summarised here.

### To go further
- Shadow testing and running a new system against production traffic — the mechanism behind the Spark upgrade's safety check.
- Deterministic versus agentic components in production systems — how to decide which parts must be guaranteed and which can be left to judgment.
- Rollback and progressive delivery — the release patterns behind auto-rollback and direct promotion.

## Concepts
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [shadow-testing](/concepts/backend-systems/shadow-testing.md)
- [authentication-authorization](/concepts/security/authentication-authorization.md)
- [observability](/concepts/cloud-infra/observability.md)
