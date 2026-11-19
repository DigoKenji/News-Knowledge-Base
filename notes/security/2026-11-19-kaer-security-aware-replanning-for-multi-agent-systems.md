---
type: Paper Summary
title: 'Kaer: Security-Aware Replanning for Multi-Agent Systems'
description: Kaer restricts agent feedback to fixed failure templates and deterministic repairs, resisting replanning attacks with higher security than baselines.
resource: https://arxiv.org/abs/2611.24839
tags:
- security
- indirect-prompt-injection
- multi-agent-orchestration
- model-context-protocol
- llm-as-a-judge
- threat-modeling
- information-flow-control
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.24839
  title: 'Kaer: Security-Aware Replanning for Multi-Agent Systems'
  author: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
status: stable
source_url: https://arxiv.org/abs/2611.24839
source: arXiv cs.AI/cs.LG/cs.CL (top 3 by relevance)
source_type: arxiv
published: '2026-11-18'
domain: security
concepts:
- indirect-prompt-injection
- multi-agent-orchestration
- model-context-protocol
- llm-as-a-judge
- threat-modeling
- information-flow-control
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d8180aac8ee7f22afe44f
---

## 🎯 In one sentence
Kaer shows that replanning in a multi-agent system can resist malicious agents if the planner sees only fixed failure templates and repairs are deterministic: it reaches a 92.3% security score on gpt-5.6-sol, against 69.8% for ControlValve and 35.0% for Magentic-One.

## ❓ The problem
An orchestrator is a central component that writes a plan, hands each step to a specialist agent (an LLM with tools), and revises the plan when a step fails. That revision step, called replanning, reads the agent's failure report, and the report is text the agent wrote. A malicious agent can hide instructions in that text (indirect prompt injection) to make the planner add steps, swap agents or redirect the work. This is control hijacking: the attacker never breaks the orchestrator, it steers the plan. Existing defenses check each tool call or fix the whole plan before execution, so the replanning step itself stays unprotected. Fixing the plan in advance is hard too, because many real tasks reveal what they need only while they run.

## 💡 The idea
Kaer does not let agents explain failures in free text. An agent reports one condition from a fixed list, and the planner receives only that condition as a template with suggested repair operators, never the agent's output or notes. The authors compare this to type narrowing: passing only booleans, numbers or enums across a trust boundary limits what an attacker can say to the number of possible values. Repairs must also be built from a small set of deterministic operators, and a revised plan is accepted only if it passes every task and agent policy. Together, the attacker can only pick among pre-defined failure labels, and each label can trigger only policy-compliant changes.

## ⚙️ How it works
### Plan structure and the validity rule
A subtask is defined as a tuple of an assigned agent, a set of granted capabilities and an input, and a plan is valid only if each subtask meets the policy conditions.

```text
S_j = (A_j, {C_i}_j, (P_j, O_{j-1}))                    (2)
{C_i}_j ⊆ CT ∪ X_j                                      (3)
```

Symbols: S_j is subtask j. A_j is the agent assigned to it. {C_i}_j is the set of capabilities that agent may use for this subtask. P_j is the natural-language directive. O_{j-1} is the stored outputs of earlier subtasks this one depends on. CT is the set of capabilities the user's task policy allows. X_j is the set of extra capabilities outside CT that the user has explicitly authorized for this subtask and agent, empty until a grant. Condition (3) says a subtask may use only capabilities that the task policy allows or the user has granted.

### Trusted gateway for least privilege
All tool and LLM calls pass through a trusted gateway that the orchestrator configures from the accepted plan. The gateway forwards a call only when the caller is the active agent and the target belongs to one of that subtask's capabilities. Calls made after the agent has finished are rejected. An agent therefore cannot escalate privileges or act for another subtask. The gateway does not inspect the arguments or return values of permitted calls, but every call stays observable in the trace. This component is what enforces granted capabilities at run time; the rest of Kaer covers replanning.

### Failure templates as the only feedback channel
When a subtask fails, the agent returns a close-out that names one or more conditions from a fixed set of 14. Each condition becomes a template that names the issue and suggests repair operators, so the planner gets a finite vocabulary rather than free text. The gateway adds conditions too, for example when it rejects a call or a tool fails. Successful subtasks are not assessed: the orchestrator passes their outputs downstream without checking them. Agent progress reports travel back only to the same agent, never to the planner.

### Deterministic repair operators and acceptance
A repair is a sequence of one or more of seven operators, applied in order to a copy of the plan: RE-EXECUTE, REVISE-DIRECTIVE, RESCOPE, REASSIGN, INSERT, PRUNE and REWIRE. Only uncompleted subtasks can change, so completed outputs stay fixed. The orchestrator rejects a repair that targets an earlier position, cannot be applied, or leaves an invalid plan. Operators that grant a capability outside CT, such as RESCOPE, REASSIGN or INSERT, take effect only after the user authorizes that capability. A repair that gets rejected goes back to the planner with the reasons, and the task ends after a fixed number of consecutive rejections.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| gpt-5.6-sol, attack trials | ControlValve 69.8%; Magentic-One 35.0% | Kaer 92.3% | Security score (fraction of blocked attack trials) |
| gpt-5.6-terra, attack trials | ControlValve 80.4%; Magentic-One 47.8% | Kaer 88.0% | Security score |
| deepseek-v4-pro, attack trials | ControlValve 78.7%; Magentic-One 50.1% | Kaer 79.9% | Security score |
| gpt-5.6-sol, Kaer vs. free-text ablation | Kaer w/ free text 75.7% | Kaer 92.3% | Security score |
| gpt-5.6-sol, benign trials | Magentic-One 91.4%; ControlValve 54.3% | Kaer 64.3% | Benign task completion |
| gpt-5.6-terra, benign trials | Magentic-One 78.6%; ControlValve 34.3% | Kaer 65.7% | Benign task completion |
| deepseek-v4-pro, benign trials | Magentic-One 84.3%; ControlValve 54.3% | Kaer 41.4% | Benign task completion |
| deepseek-v4-pro, with output check | Kaer 29 of 70 | Kaer w/ output validation 38 of 70 | Benign trials succeeded |
| gpt-5.6-sol and gpt-5.6-terra, with output check | Kaer 86.6% and 87.0% | 96.8% on both | Defense rate |

The results support the core claim: on all three models Kaer beats both baselines on security, and on the two GPT models it also completes more benign tasks than ControlValve. The main caveats are in the table itself. Magentic-One has higher benign completion on the GPT models, and Kaer has the lowest benign completion of the five systems on deepseek-v4-pro. Cintra is simulated and built by the same authors, with 14 tasks and five runs per task, so the percentages rest on modest trial counts. The attacks that still succeed against Kaer mostly run through success reports, which the failure templates do not inspect.

## ⚠️ Limitations
- Out of scope, as the authors state: misuse inside an agent's own granted capabilities (malformed arguments, locally falsified tool outputs) and compromise of trusted components such as the orchestrator, planner, registry, MCP servers and LLM servers.
- Kaer does not check whether a success output is correct, because verifying correctness of open-ended tasks is hard. The authors add an LLM check as an extension, which uses the same LLM as the planner.
- Failure templates omit runtime context on purpose, so tasks that need to discover data-source locations stay largely unsolved.
- The gateway does not inspect the arguments or return values of calls it permits.
- 🔎 Added context: every attack trial places the malicious agent in one role (filesurfer), so attacks by several colluding agents are not measured here.
- 🔎 Added context: the paper reports no confidence intervals, and each configuration has 5 runs per task or pair, so a few points of difference may rest on very few trials.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. In a real architecture, the pattern fits on the agent-to-orchestrator contract: failure reports become a typed enum, the orchestrator keeps a per-subtask allow list at the gateway, and a plan validator sits between the planner and execution. The costs are a failure taxonomy you must maintain, lower completion on tasks that need runtime discovery, and an extra LLM call per success if you add an output check. To trust it, you would need to test on your own task mix and tool set, because the benchmark here is simulated and written by the paper's team.

## 🔑 Key takeaways
- Free-text failure reports are the attack channel: with them, the planner can be steered into running a planted script, and Kaer's security score falls from 92.3% to 75.7% on gpt-5.6-sol when the free-text ablation is used.
- Kaer's planner sees only one of 14 fixed failure conditions, and each condition comes with suggested operators that the planner may ignore.
- Repairs are limited to seven deterministic operators, completed subtasks cannot change, and every revised plan must pass the task and agent policies.
- Kaer is not a utility win everywhere: on deepseek-v4-pro its benign completion is 41.4%, the lowest of the five systems, against 84.3% for Magentic-One.
- Most successful attacks against Kaer use agents that report success with wrong output, and the authors' LLM output check raised its defense rate to 96.8% on both GPT models.
- Least privilege is enforced by the gateway at run time, not by prompt wording: each subtask reaches only the endpoints of its granted capabilities.

## 📚 Reading plan
### From the paper
- [ControlValve (Jha et al.)](https://openreview.net/forum?id=PNU9Rj5RDQ) — the state-of-the-art defense Kaer is compared with, and the control-flow policy approach to read first.
- [Magentic-One](https://arxiv.org/abs/2411.04468) — the undefended orchestrator baseline whose replanning Kaer modifies.
- [Jacob et al., type-directed privilege separation](https://arxiv.org/abs/2509.25926) — the source of the "booleans, numbers or enums" idea behind the failure templates.
- [Progent](https://arxiv.org/abs/2504.11703) — privilege control for agents, a runtime-defense alternative to compare against.
- [Architecting secure AI agents](https://arxiv.org/abs/2603.30016) — a survey of system-level defenses against indirect prompt injection, for the wider design context.
- [Agent-Sentry](https://arxiv.org/abs/2603.22868) — bounding LLM agents via execution provenance, a recent runtime-enforcement approach.
### To go further
- Failure taxonomy of multi-agent LLM systems (Cemri et al., "Why do multi-agent LLM systems fail?") — the taxonomy Kaer's failure templates are derived from.
- Information-flow control for AI agents (Costa et al., "Securing AI agents with information-flow control") — the endorsement and downgrading ideas Kaer builds on.
- Control-flow hijacking in multi-agent systems — the attack class the paper names and the reason plan integrity matters.
- Planning-time attacks on workflows (FlowSteer, Li et al.) — why a plan that the planner can rewrite is itself an attack surface.

## 🔗 Concepts
- [indirect-prompt-injection](/concepts/indirect-prompt-injection.md)
- [multi-agent-orchestration](/concepts/multi-agent-orchestration.md)
- [model-context-protocol](/concepts/model-context-protocol.md)
- [llm-as-a-judge](/concepts/llm-as-a-judge.md)
- [threat-modeling](/concepts/threat-modeling.md)
- [information-flow-control](/concepts/information-flow-control.md)
