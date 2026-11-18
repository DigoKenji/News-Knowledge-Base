---
type: Paper Summary
title: Self-Improvement Memory Enables Continual Recursive Self-Improvement
description: Recording why each harness edit was made, and what it predicted and achieved, lets continued optimization beat harness-only transfer.
resource: https://arxiv.org/abs/2611.18856
tags:
- ai-ml
- mlops
- llm-as-a-judge
- model-context-protocol
- agent-harness
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.18856
  title: Self-Improvement Memory Enables Continual Recursive Self-Improvement
  author: (see paper)
status: stable
source_url: https://arxiv.org/abs/2611.18856
source: arXiv (forwarded via LinkedIn)
source_type: arxiv
published: 2026-11
domain: ai-ml
subdomain: mlops
concepts:
- llm-as-a-judge
- model-context-protocol
- agent-harness
selected_by: forwarded
content: handoff
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d816e920dc492fef2d65e
---

## 🎯 In one sentence
Keeping a structured record of why each harness edit was made, what it predicted and what it achieved lets a continued harness optimizer beat harness-only continuation in six cross-dataset and four Opus-to-Gemini settings, and it adds 2.36 points on average across ten coding runs (paired p = 0.003).

## ❓ The problem
An LLM agent depends on two things: the policy model, which is the LLM that does the tasks, and the harness, which is the code around it that manages context, tools and execution flow. Changing the harness can raise scores without touching the model's weights. Recent work automates harness improvement with recursive self-improvement (RSI), a loop that proposes harness edits, runs them on a benchmark (a fixed set of scored tasks) and keeps the ones that score well. Such loops usually run for one fixed model and one benchmark. When either changes, an evolved harness can stop helping: an Opus-evolved harness scores 68.41 on Gemini's workspace tasks, below the 73.89 of the seed harness (the unevolved starting point). The optimizer then has to decide which inherited edits still apply, and the harness alone does not record why each edit was made or how it performed.

## 💡 The idea
Store the optimizer's experience next to the harness, not inside the task-solving model. The authors call this Self-Improvement Memory (SIM). SIM groups past edits by the harness problem they address. For each edit it keeps the rationale, the outcomes predicted before testing, the outcomes observed afterward, and the evaluation context (domain, benchmark, model and round). When optimization continues on a new benchmark or model, the proposer and the selector read this history before they propose or accept an edit. They can then keep an inherited fix whose reason still holds, revise it, replace it or remove it.

## ⚙️ How it works
### Component 1: the SIM record
Each recurring harness problem c_i, such as premature completion, is an entry m_i that holds its attempts:

```text
m_i = (c_i, [(a_ij, o_ij, p_ij)]_j)
```

Here j indexes the attempts in order. a_ij is the edit and its rationale, o_ij is the predicted and observed task outcomes, and p_ij is the provenance (domain, benchmark, policy and round). The predicted outcome, meaning which tasks the edit is expected to fix, is written before evaluation and checked afterward. The paper's run files store each entry as JSON with fields such as predicted, fixed, missed, passed and total.

### Component 2: the proposer and selector read the whole record
Each round, the proposer P receives the complete SIM in its system prompt. The selector S reads the same record when it screens and chooses among candidates. Both read every entry, with no retrieval step and no truncation. The update loop becomes:

```text
C_t = P(H_t, F_t, D_t; M_t)
H_{t+1} = S(H_t, F_t, C_t, F_t^C; M_t)
```

H_t is the harness at round t. F_t is the feedback from the evolve set (the tasks the optimizer may see). D_t is the run's history of earlier feedback and code diffs. C_t is the set of k = 5 candidate harnesses proposed per round. F_t^C is the feedback from the candidates that passed screening and were run. M_t is the memory. Held-out tasks never feed any of these steps.

### Component 3: continual transfer and the memory update
Source optimization starts from an empty memory and produces a harness H_S and a memory M_S. Target optimization starts from H_S and M_S, either on a new benchmark with the policy fixed (cross-dataset) or with a new policy on the same benchmark (cross-model). After each round, only the chosen candidate e_t is recorded, including its measured outcome even when the edit regresses. The update M_{t+1} = U(M_t, e_t) appends the record to the entry it cites or creates a new entry. If nothing is accepted, the memory is unchanged. Entries with identical root-cause text are merged without losing their histories. The paper compares four conditions from the same starting point: Cold Start (seed harness, no memory), Harness Transfer (H_S, no memory), Problem-Only Memory (H_S with root causes only) and Full SIM (H_S with the complete record).

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Coding, AHE seed, TB2.1 to TB4 (cross-dataset) | Harness Transfer 20.61 | Full SIM 24.85 | pass@1 after target RSI |
| Coding, Terminus-2 seed, TB2.1 to TB4 (cross-dataset) | Harness Transfer 19.39 | Full SIM 21.82 | pass@1 after target RSI |
| Text classification, FewShotAll, source suite to 8 target datasets | Harness Transfer 74.12 | Full SIM 77.92 | accuracy after target RSI |
| Workspace, ReAct, Harvey LAB to merged target (cross-dataset) | Harness Transfer 85.07 | Full SIM 87.89 | mean of three workspace benchmarks |
| Engineering, ReAct, EngDesign to Frontier-Eng (cross-dataset) | Harness Transfer 42.61 | Full SIM 46.96 | medal score |
| Workspace, Opus 5.5 to Gemini 3.8 Flash (cross-model) | Harness Transfer 73.42 | Full SIM 84.75 | mean of three workspace benchmarks |
| Engineering, Opus 5.5 to Gemini 3.8 Flash (cross-model) | Harness Transfer 42.61 | Full SIM 50.43 | medal score |
| Paired coding, ten independent Terminus-2 source runs, TB4 target | No SIM 19.33 | SIM 21.70 | pass@1, mean final score |

Full SIM finished highest in all six cross-dataset settings and in all four cross-model domains. Its margin over Harness Transfer was 2.43 to 4.35 points in the cross-dataset set and 3.60 to 11.33 points in the cross-model set. Problem-Only Memory had mixed effects, from 1.82 points below Harness Transfer to 2.40 above it. In the paired coding study, SIM finished higher in nine of ten pairs, and the one losing pair favored no SIM by 0.61 points. The evidence does not cover every domain with repeated runs: the paired study is the only one with ten independent source runs, the other conditions are single runs, and the held-out sets range from 23 to 500 items. Swapping the optimizer model to Claude Opus 5.5 changed held-out source scores by only 1.06 points (AHE seed) and 0.05 points (Terminus-2 seed).

## ⚠️ Limitations
- The authors state that independent RSI repetitions focus on coding; the other domains are evaluated with one run per condition.
- The authors state that they study settings with compatible harness interfaces and keep the optimizer's planning and search procedures fixed.
- The authors name more repeated runs, longer transition sequences, and adaptation of planning, token allocation and tool interfaces as future work.
- The authors' ethics statement warns that a self-modifying harness could satisfy the evaluator without solving the intended task, and that workspace scores may reflect biases of the LLM judge.
- 🔎 Added context: the paired-study margins are coarse. The TB4 held-out set has 165 rollouts, so one rollout is about 0.61 points (100/165). The one losing pair (-0.61) is roughly a single rollout.
- 🔎 Added context: SIM is passed in full with no retrieval, so the prompt grows with every attempt. The paper gives no token or cost comparison between conditions; it says only that token use and runtime can differ.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. If you run automated optimization of prompts, tools or agent workflows, the SIM idea maps to a decision log keyed by problem, written by the optimizer and read at every iteration, with predicted outcomes recorded before each test run. It would live beside the evaluation harness and the code it edits, and it grows with every accepted change, so its storage is cheap but its prompt cost is not. To trust it, you would need held-out evaluation that the optimizer never sees, verifiers that cannot be gamed, and a check that predictions are actually being recorded and resolved, including the ones that miss.

## 🔑 Key takeaways
- Recording why each edit was made, and what it predicted and achieved, gives an optimizer grounds to keep, revise or remove inherited changes.
- Keeping only the root-cause diagnosis is not enough: Problem-Only Memory had mixed effects, from 1.82 points below Harness Transfer to 2.40 above it.
- A harness evolved for one model or benchmark can start below the seed: 68.41 against 73.89 on Gemini workspace tasks, and 72.92 against 75.72 on text classification.
- In the paired coding study, SIM won nine of ten pairs, with a mean advantage of 2.36 points (95% CI [1.05, 3.68]; paired p = 0.003).
- Against fresh RSI from the seed, continuation was mixed: it improved on Gemini 3.5 Flash by 3.5 points and on Gemini 3.8 Flash by 1.6 points, but finished 0.8 points lower on Gemini 4 Argon.
- The strongest evidence is for coding. The other domains rest on single runs per condition, so their margins are descriptive.

## 📚 Reading plan
### From the paper
- [Meta-Harness](https://openreview.net/forum?id=tmbOUyFx3R) — the executable-harness optimizer this work builds on, and the source of the text-classification environment.
- [Agentic Harness Engineering (AHE)](https://arxiv.org/abs/2604.25850) — makes harness edits observable and checks edit-level predictions against outcomes; the closest relative to SIM's predicted-versus-observed records.
- [RRSI](https://arxiv.org/abs/2609.24972) — source of the workspace and engineering environments and scoring interfaces used here.
- [Terminal-Bench](https://openreview.net/forum?id=a7Qa4CcHak) — the coding benchmark family (TB2.1 and TB4) behind the paired study.
### To go further
- "From Harness Optimization to Recursive Self-Improvement: A Survey of Language Model Agents" (Yu et al., 2026) — a map of the harness optimization workflows the paper builds on.
- "Causal Improvement Graph for Agentic Harness Optimization" (Zhang et al., 2026) — another way to store evidence, hypotheses, interventions and outcomes; compare its record with SIM's.
- "Agentic Context Engineering" (Zhang et al., 2026) — playbook memory for task-solving agents, the contrast case the paper discusses in Appendix A.3.
