---
type: Article Summary
title: How OpenAI’s Performance Factory keeps ChatGPT fast
description: OpenAI's Performance Factory uses Codex agents in task workflows to investigate, fix and verify performance regressions,
  with adversarial review and human sign-off.
resource: https://openai.com/index/how-openai-performance-factory-keeps-chatgpt-fast
tags:
- backend-systems
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-25T06:47:00-03:00'
sources:
- resource: https://openai.com/index/how-openai-performance-factory-keeps-chatgpt-fast
  title: How OpenAI’s Performance Factory keeps ChatGPT fast
  author: OpenAI
status: stable
source_url: https://openai.com/index/how-openai-performance-factory-keeps-chatgpt-fast
source: OpenAI News
source_type: blog
published: '2026-11-19'
domain: backend-systems
concepts:
- ai-agent
- agent-harness
- agent-memory
- llm-as-a-judge
- model-evaluation
- mlops-llmops
- observability
- least-privilege
- event-driven-architecture
- version-control-code-review
- caching
- latency-throughput
- silent-failure
- ci-cd
- performance-regression
- adversarial-review
selected_by: theme
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/423baf74172d811d82b4d44fdb14e40f
---

## 🎯 Core thesis
OpenAI's Performance Factory uses Codex agents inside tracked, end-to-end workflows to investigate, fix and verify performance regressions, with adversarial review and engineer sign-off at the points where judgment is needed.

## 🧭 Why the author wrote it
The article says the pace of agentic development has made traditional performance engineering unsustainable, because engineers investigating each slowdown by hand cannot keep up with the roadmap. It shares how the team's use of Codex evolved into the Performance Factory, walks through the first running workflow, and offers early lessons for other service teams adopting agentic workflows. If the thesis holds, most of the investigation and fixing moves to agents, and engineers spend their time on decisions.

## Introduction: the pace problem and the first workflow
The article opens by stating that the "staggering pace of agentic development" has made traditional approaches to performance engineering unsustainable. The authors built the Performance Factory, which they describe as an autonomous system that uses the full capabilities of Codex and their models to find and fix regressions at unprecedented speed. It runs in the cloud, automates both investigating and resolving performance problems, and lets them keep pace with the ambition of their roadmap.

### The Performance Factory
The Factory is the system that runs recurring jobs as agent workflows. Engineers define each workflow and write the instructions the Codex agents follow, and the Factory handles tracking the work, assigning agents to it and carrying work across steps. Its first workflow is the Regression Workflow, which the article covers in detail later. The article presents the whole system as a way to make agentic work reliable enough for production performance problems.

### Performance regression
> 🔎 Added context: A performance regression is a measurable slowdown relative to an earlier baseline, usually introduced by a code, configuration or traffic change. The article uses the term for things like a rise in ChatGPT first-token latency.

The article does not define the term, but its workflow depends on it: the work is to find when the slowdown started, which requests it affected, and what caused it, and then to confirm that performance returned to baseline after a fix. The scale is the headline figure. In just over three months, the Regression Workflow tracked more than 1,700 investigations and root caused over 600 cases, which the authors put at 17 automated investigations every day.

The article then previews three things: how their use of Codex evolved into the Factory, the first running workflow, and early lessons from running agent workflows in production.

## The progression: six stages from manual tools to the factory
The authors describe a progression in how their team worked with Codex, and they hope other service teams can learn from it as they adopt agentic workflows in other domains.

### 1. Pre-Codex: manual investigation
Before Codex, engineers manually examined metrics and logs, compared traces, debugged live machines and read code to investigate a slowdown. The work was time consuming, and knowing where to look often required deep knowledge of the service, so relevant signals were easy to miss. When the same kinds of problems recurred, such as regressions in ChatGPT's first-token latency, the team built advanced tools like CPU and allocation profilers to make those investigations easier. Over time they accumulated more than 30 performance tools. Each tool helped with one kind of investigation, but the engineer still had to know which tool to use and when.

### 2. Codex as a partner
Once Codex was a partner, an engineer could ask "Why did this get slower?" and work through the answer with it, even in a system they did not know well. Codex could search large codebases, find relevant metrics and traces, follow interactions across services and run appropriate profiling to piece together what had happened. Engineers still had to steer the process, deciding which performance tools to use and how to guide the investigation. The gain was that difficult investigations became debuggable for more people, with less expertise and time required.

### 3. Reusable skills: `perf-forensics`
To automate further, the performance team captured its investigation process in a shared skill called `perf-forensics`. The skill tells Codex which tools are available, when to use them, and how to work through a slowdown from several angles:
- Find when the slowdown began
- Identify the affected requests
- Investigate whether deployments or configuration changes could explain it

> 🔎 Added context: An eval is a repeatable test set with known expected outcomes, used to measure whether a change to a prompt, skill or model makes results better or worse.

The team tested and refined the skill with evals built from past regressions. For the types of regression they focused on, such as ChatGPT latency and availability drops, an engineer could now give Codex an alert and get back a diagnosis in a single run, without guiding it through each step.

### 4. The human proxy
As the team added and refined perf-related skills, they started to feel like a proxy between alerts and Codex. An alert would fire, an engineer would copy and paste it into Codex with the right skill, and then paste the findings back into Slack.

> ❌ **Anti-pattern — the human copy-paste proxy:** an engineer manually relays each alert to the agent and each finding back to the team. **Why people do it:** the article does not say; it is the natural first step when the agent works but nothing connects it to the alerts. **Consequence:** the number of alerts investigated is capped by one person's attention, which the article's move to automation implies was the constraint. **Fix:** start investigations automatically from the alert, as in the next stage.

### 5. Automated investigations
The team then moved investigations to the cloud so an alert could start a Codex session automatically. Codex investigated the slowdown and posted its findings in the Slack thread, but the automation ended with that reply. An engineer still had to pick up the diagnosis, develop a fix and check whether it worked after deployment. As alerts accumulated, even working out which regressions needed attention meant sifting through Slack threads.

> ❌ **Anti-pattern — automating only the diagnosis:** the agent's run ends with a message in a chat thread, and the fix and the post-deployment check stay manual. **Why people do it:** the article does not say; investigation is the easiest step to automate on its own. **Consequence:** the backlog of findings grows and prioritization falls back to people reading threads. **Fix:** keep the work as a persistent task that carries through the fix and verification, which is what the Factory does.

### 6. The factory
The team built the Performance Factory to run end-to-end agentic performance engineering workflows. It tracks what each active task needs next and assigns an agent when that step is ready. Each task keeps its state and findings, so work can pass between agents and engineers and resume after a code review or deployment. For a regression, the work starts with investigating an alert and ends with verifying recovery in production. Common optimizations can become workflows that run regularly across services, and a workflow that spots a potential optimization can open a task in another workflow to test and implement it.

## Anatomy of the Performance Factory
The article says the Factory has four components: a task tracker with configurable workflows, a dispatch loop that assigns ready work to agents, an API that runs Codex in the cloud, and subscriptions that wake agents when something changes, such as a GitHub review comment being posted or a deployment completing.

### Task tracker and workflow configuration
Engineers define a workflow by adding configuration files and prompts for the agents, written in Markdown. The configuration lists the states a task can move through, the transitions allowed between them, and which steps need an engineer. Separate prompts tell agents what to do at each state. Each piece of work becomes a task in a workflow, and all workflows are visible in a UI, where engineers can see what is open, follow progress and share a task with a teammate who may have more context.

### Dispatch loop
A dispatch loop runs periodically and checks which tasks are ready for their next step. For each ready task it assigns an agent by starting a Codex turn with the instructions for that task's current state. The agent records its progress on the task and moves the task to the next state if it completes the step.

### Cloud Codex API and service identity
Once assigned, the agent needs a place to work, with the same tools an engineer would use. The team built an internal API that invokes a Codex turn programmatically in the cloud. The turn runs in an environment inside their network, with access to code repositories and to observability tools such as metrics, logs, traces, CPU profiles and memory snapshots. The agent runs under a service identity, so its permissions can be scoped and its actions can be told apart from those of engineers. Running in the cloud lets work proceed in parallel without depending on engineers' laptops being open or on managing individual cloud development boxes.

> 🔎 Added context: A service identity is a non-human account with its own credentials and permissions, separate from any person's account.

### Subscriptions: waiting without running
Some work has to wait for a person or a deployment, and an agent should not keep running in the meantime. A task can subscribe to events, such as review comments or failed CI checks on a GitHub pull request, and be notified when it is time to resume. When an incoming event matches the subscription, the task becomes ready again and the dispatch loop can assign an agent to continue the work.

## The Regression Workflow in detail
Finding, root causing and solving regressions is the first workflow built inside the Factory. A regression moves through four stages: **needs investigation**, **root caused**, **fix proposed** and **resolved**. The workflow reflects those same stages and moves each task through them.

### Stages and task state
When one of the configured performance alerts fires, the Regression Workflow opens a task, and agents pick it up and move it across the stages. The task records its status and key findings outside any single agent's context, so other agents or engineers can continue the work. Engineers can follow progress in Slack or the Regression Workflow UI, add context and review proposed changes. After a fix is deployed, an agent checks whether performance has recovered before resolving the task.

### Finding the root cause
This is the first major step. The agent figures out which metrics got worse, when they started to change and which service requests were affected. An alert fires when a metric crosses a threshold, but the regression may have started hours earlier, so the agent looks back through the data for the first point where the metric got worse. It then compares groups of requests. Did similar requests actually get slower, or were more requests being served that were already slow? The article notes that these two findings point to different causes. Once it knows when and where to look, the agent examines deployments and feature flag changes, reads the relevant code, checks when a change actually reached the affected requests, and uses logs or traces to test whether the code could explain what happened. The `perf-forensics` skill gives Codex access to the tools performance engineers use and a playbook for these questions.

### Adversarial review of the diagnosis
Early on, agents sometimes stopped at the first plausible explanation they found, and some of those diagnoses were wrong. The fix was an adversarial review: a separate Codex session whose job is to disprove the proposed diagnosis. The reviewer rechecks the assumptions and tests counterfactuals. If a rollout caused the slowdown, did similar requests that were not exposed stay healthy? If the suspected change was rolled back, did the affected metric recover?

> ❌ **Anti-pattern — stopping at the first plausible explanation:** the agent accepts the first cause that fits and marks the investigation found. **Why people do it:** a plausible story feels finished, and the article reports that early agents did this. **Consequence:** wrong diagnoses move on to fixes. **Fix:** an independent reviewer that tries to disprove the diagnosis and runs counterfactual checks.

The article gives an example. An agent traced failing ChatGPT conversations to errors in image downloads and marked the cause as found. The reviewer noticed that the internal service responsible for locating files had already recovered while image requests were still failing. The diagnosis did not explain the continuing failures, so the reviewer reopened the investigation and recorded what still needed to be established.

Breaking larger jobs into focused Codex turns has also helped. In a manual review of 100 root cause suggestions, the team graded 70% as accurate, and 30% needed further refinement or did not fully establish causality. In the Regression Workflow, different turns investigate regressions, challenge diagnoses and propose fixes, which the authors describe as checks and balances for accuracy.

> 🔎 Added context: LLM-as-a-judge means using a model, guided by a rubric, to grade another model's output. The adversarial reviewer applies the same idea, but its brief is to find the flaw rather than to score.

### Proposing and reviewing the fix
Once the diagnosis passes adversarial review, the workflow asks Codex to implement a fix and open a pull request. An engineer reviews the PR and decides whether to merge it. The team put substantial work into making this handoff easy. Each task has a report with an automatically generated summary of the regression, its impact, the root cause and the proposed fix. The summaries are tuned to be easy to scan and include metrics time series, CPU flame graphs and traces, so engineers can check the evidence quickly. Engineers can edit the PR directly or use the diagnosis to write their own fix, and if they leave comments on the PR, the workflow wakes an agent to address them.

### Adversarial review of the fix
The article says getting fixes right has been harder than finding the cause. Agent-proposed changes have sometimes been more complicated than necessary or have introduced new issues. So each proposed fix gets an adversarial review that checks whether it addresses the root cause and whether it catches bugs or unintended consequences. Some fixes also need architectural decisions. Adding a cache, for example, might improve performance but introduce stale data, and such changes benefit from an engineer who knows the service and can weigh the tradeoffs.

> ❌ **Anti-pattern — unchecked or over-complicated fixes:** the agent proposes a change that is more elaborate than the problem needs, or one that introduces new bugs. **Why people do it:** the article does not say; a fix that looks complete is easy to accept. **Consequence:** new issues reach review, and the reviewer carries the extra cost. **Fix:** an adversarial check on each fix, plus engineer judgment on architectural tradeoffs.

### Verifying recovery after deployment
After an engineer deploys the fix, the workflow wakes an agent to check whether performance has returned to its earlier baseline. If it has, the agent marks the task resolved. If performance has only partly recovered, the agent resolves the original regression and opens a new task for the remaining slowdown, which may have a different cause. If there is no improvement, the agent records what happened and sends the regression back for investigation.

### The evaluation loop
The team is building an evaluation and improvement loop from these cases. By comparing the fixes agents propose with the changes engineers ultimately merge, they can see where proposals fall short, build evals and improve the workflow. They are also working on synthetic performance tests so that agents can measure a proposed fix and revise it before asking an engineer to review it. Finally, they are exploring how agents could use their other agentic review tools to identify low-risk changes and eventually ship those fixes without a human in the loop.

## Lessons for teams building agentic workflows
The authors close the technical part with lessons for teams building their own agentic workflows.

### Make agent output decision-ready
The raw output from a Codex investigation can be a wall of text that is hard to parse. Engineers must work out what the agent is claiming and find the graphs that show whether the data supports it. The goal is for engineers to spend their attention on decisions rather than on reconstructing the agent's work.

> ❌ **Anti-pattern — a UI full of unranked findings:** the first version gave engineers many findings, but someone still had to decide which ones mattered. **Why people do it:** the article does not say; producing more output is easier than deciding what matters. **Consequence:** the review burden moves to people. **Fix:** structured summaries with the evidence attached, as in the Regression Workflow reports.

### Group related incidents and rank by impact
Agents can investigate more regressions than a team has time to read about. One regression can also show up as several incidents across alerts, monitors and channels. The workflow now links related incidents, groups them by the metrics teams care about, and ranks their estimated impact, so engineers can focus on adding context, weighing tradeoffs and deciding what to ship.

> ❌ **Anti-pattern — treating each alert as a separate problem:** the same regression is investigated several times under different alerts. **Why people do it:** each alert arrives as its own item. **Consequence:** duplicated investigation and a longer queue. **Fix:** link and group related incidents before investigating.

### Test before changing a skill or model
Updating an investigation skill or upgrading the model can make the agent better at diagnosing some cases and worse at others, so the impact must be tested before the change ships. The authors test two things separately: how well the agent investigates, and how useful its summaries are to engineers.

> ❌ **Anti-pattern — shipping a skill or model update untested:** the new version is assumed to be better everywhere. **Why people do it:** the article does not say; a newer model feels like an upgrade. **Consequence:** some cases silently get worse. **Fix:** rerun the evaluation suite before the update and score investigation and summaries separately.

### Backtest investigations against past incidents
For investigations, the team backtests against past production incidents. They turn those incidents into a suite of test cases that can be rerun whenever the skill changes. A Codex agent attempts to investigate and resolve each incident, and its behavior is compared with the actual past resolution data. Each case starts with a fresh Codex investigator and the original signal, with any details that might give away the answer removed.

Historical tests need care. Metrics and traces expire, and evidence collected after recovery can reveal the cause. A test is useful only if you know what evidence the investigator could see. In their setup, the agent can inspect code and query production telemetry within set limits. It cannot use saved memories (notes from earlier investigations), read the incident writeup or see results from previous runs. The team also inspects each transcript afterward for information that might have spoiled the test.

> ❌ **Anti-pattern — a leaky backtest:** the test agent can see post-recovery evidence, the incident writeup or earlier results, so it can pass by reading the answer. **Why people do it:** the article does not say; the richest evidence is also the most tempting. **Consequence:** accuracy numbers look better than the agent really is. **Fix:** limit the visible evidence, start from a fresh investigator and audit the transcript.

### Score the dimensions separately
The team scores when the problem began, how large it was and which requests were affected. They also evaluate how the agent uses evidence to rank possible causes and how it explains what remains uncertain. The reason for separating these is that an agent can get the timing right and still blame the wrong deployment.

> ❌ **Anti-pattern — a single blended score:** timing, scope and cause are rolled into one number. **Why people do it:** one number is easy to report. **Consequence:** a correct timing can hide a wrong cause. **Fix:** score each dimension on its own.

### Evaluate summaries from saved snapshots
The team evaluates task summaries using snapshots of saved tasks. These make especially good backtests, because summarization does not depend on external metrics or traces that expire. A separate grader checks whether an engineer unfamiliar with the system could understand what happened and why it matters, whether useful charts were kept, and whether the proposed next step is supported by the evidence.

### Monitor the agents as well as the services
Agents need monitoring just as much as the services they investigate. A broken profiler or a missing permission can stop an agent from verifying what caused a slowdown, and such failures can be hard to spot in the final report. The team runs an agent every day to look through messages and logs from recent runs for tool failures and access errors.

> ❌ **Anti-pattern — assuming agents fail loudly:** an agent that cannot run its tools still produces a report that looks complete. **Why people do it:** the article does not say; the final output is the only thing most people read. **Consequence:** a cause stays unverified and the gap is hard to see. **Fix:** a scheduled check of run logs for tool and access errors.

### Let the factory repair itself
When the same tool error blocks several investigations, the agent creates a task to fix it. If the cause is clear, an agent proposes a fix, and an engineer reviews the change and decides whether to merge it. After merging, the team uses evals to check whether agents can now get past the step that previously blocked them.

## Where the factory goes next
The Regression Workflow is one instance of what the article calls the agent factory pattern, which moves tasks through explicit states. Agents advance a task when more work is ready, separate reviewers challenge the conclusions, and engineers step in when the next decision requires judgment. Other workflows can reuse the same machinery with different states, instructions and events.

### Proactive detection without an alert
Today the Regression Workflow starts when an alert fires. The team wants the Factory to find slowdowns and optimizations even when no alert fires. By comparing batches of CPU and memory profiles, heap dumps and request traces across releases, an agent might notice a small increase in CPU for a given request. It could then open a Regression Workflow task to trace the increase to its cause, implement a fix and open a PR for review.

### Encoding known signals
Where the team already knows what to look for, it is encoding those signals into more workflows. The article's example is a service rightsizing skill that helps agents spot autoscaling settings that keep more replicas running than a service needs. Once a pattern like that is recognized, the team wants to build and run a workflow for it across all services, so that it keeps optimizing in a fast-changing environment.

### Lessons that compound
What an engineer learns from one slowdown can shape how the next one is investigated and fixed, even months later in a different service. The article's closing point is that the bottleneck today may be waiting for an engineer to review and merge a fix, and the team is already exploring whether agents can do this themselves. Its final line: "Codex already helps us build ChatGPT faster. Now it's helping us keep ChatGPT fast."

## 🧠 Mental models
### The agent factory pattern
**The model:** A task moves through explicit states. Agents advance it when work is ready, separate reviewers challenge its conclusions, and engineers step in when a decision needs judgment.
**How to apply:** Before writing any agent prompt, define the states, the allowed transitions and which transitions require a human, then reuse the same machinery for each new workflow.

### Durable task state over agent context
**The model:** The task, not the agent, holds the status, findings and evidence, so any agent or engineer can resume the work later.
**How to apply:** Store findings, evidence and the next step on the work item, so a new agent starts from the task record rather than from a previous chat transcript.

### Counterfactual challenge
**The model:** A conclusion is credible only if it survives checks that would look different were it false.
**How to apply:** For each proposed cause, write down the observation that should differ if the cause is wrong, such as similar unexposed requests staying healthy or the metric recovering after a rollback, and check it.

### Score each dimension separately
**The model:** An answer can be right on one axis and wrong on another, and a blended score hides which.
**How to apply:** Score timing, magnitude, affected scope and cause ranking as separate metrics, and report uncertainty alongside them.

## 🔑 Key takeaways
- The Regression Workflow tracked more than 1,700 investigations in just over three months and root caused over 600, which the authors put at 17 automated investigations per day.
- Automating only the diagnosis moves the bottleneck to people; the gain came from persistent task state that carries work through the fix and the post-deployment check.
- Early agents stopped at the first plausible explanation, and an adversarial reviewer that tries to disprove the diagnosis and tests counterfactuals was added to catch this.
- In a manual review of 100 root cause suggestions, 70% were graded accurate and 30% needed refinement or did not fully establish causality.
- A backtest is only meaningful if you control what evidence the agent can see: a fresh investigator, a stripped signal, no saved memories or earlier results, and a transcript audit.
- Timing, magnitude, affected requests and cause ranking should be scored separately, because an agent can get the timing right and still blame the wrong deployment.
- Agents need their own monitoring, since a broken profiler or missing permission can silently stop an investigation from verifying its cause.

## 🏛️ For Kenji
This is my interpretation, not the author's. The Factory is a governed workflow: declared states, scoped service identities, human approval gates and an auditable record of what each agent did. That is structurally close to the AI governance controls you already work with, and it suggests an architecture-level question: which of your own decisions would need the same explicit states, approval gates and evaluation loop before an agent could act on them without a person in the loop?

## 📚 Reading plan
### From the article
- The article cites no external sources. Its only link is a "View all" link to the OpenAI news index under a "Keep reading" footer, which is site navigation, so it is not listed here.

### To go further
- Site Reliability Engineering (Google, 2016) — for the SLO, alerting and postmortem practices that frame the alert-to-fix loop the Factory automates.
- "Dapper, a Large-Scale Distributed Systems Tracing Infrastructure" (Google, 2010) — background on the distributed traces the agents read during investigations.
- Designing Data-Intensive Applications (Martin Kleppmann, 2017) — the replication and consistency tradeoffs behind the caching example and its stale-data risk.
- Backtesting LLM agents against historical incidents, and keeping evaluation data free of leakage — the practical method the article describes.
- Continuous profiling (CPU and heap) as a regression signal — the basis of the proactive detection the authors are building toward.

## Concepts
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [agent-harness](/concepts/ai-ml/agent-harness.md)
- [agent-memory](/concepts/ai-ml/agent-memory.md)
- [llm-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md)
- [model-evaluation](/concepts/ai-ml/model-evaluation.md)
- [mlops-llmops](/concepts/ai-ml/mlops-llmops.md)
- [observability](/concepts/cloud-infra/observability.md)
- [least-privilege](/concepts/security/least-privilege.md)
- [event-driven-architecture](/concepts/architecture/event-driven-architecture.md)
- [version-control-code-review](/concepts/software-engineering/version-control-code-review.md)
- [caching](/concepts/backend-systems/caching.md)
- [latency-throughput](/concepts/backend-systems/latency-throughput.md)
- [silent-failure](/concepts/backend-systems/silent-failure.md)
- [ci-cd](/concepts/software-engineering/ci-cd.md)
- [performance-regression](/concepts/software-engineering/performance-regression.md)
- [adversarial-review](/concepts/ai-ml/adversarial-review.md)
