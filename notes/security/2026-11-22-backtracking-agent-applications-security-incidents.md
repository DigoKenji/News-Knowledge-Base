---
type: Paper Summary
title: Backtracking Agent Applications Security Incidents
description: Origin, a cross-layer graph tool, correctly identifies the compromise entry point in all eight agent-app attack
  incidents tested.
resource: https://arxiv.org/abs/2611.26525
tags:
- security
- ai-agent
- agent-memory
- indirect-prompt-injection
- rag
- multi-agent-orchestration
- memory-poisoning
- information-flow-control
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.26525
  title: Backtracking Agent Applications Security Incidents
  author: Yicheng Cai, Taegyu Kim, Peng Liu
status: stable
source_url: https://arxiv.org/abs/2611.26525
source: arXiv
source_type: arxiv
published: '2026-11-19'
domain: security
concepts:
- ai-agent
- agent-memory
- indirect-prompt-injection
- rag
- multi-agent-orchestration
- memory-poisoning
- information-flow-control
selected_by: discovery
content: open
complete: 'no'
notion_url: https://app.notion.com/p/420baf74172d8139a64adaef06d9f6e4
---

## 🎯 In one sentence
Origin, a backtracking tool built on a cross-layer graph, identifies the correct compromise entry point (CEP) in all eight attack incidents tested, across five open-source agent applications, with a single candidate per incident.

## ❓ The problem
When an LLM agent application is attacked, responders must find the compromise entry point (CEP): the function call that first let malicious content into the app. The two dominant attacks are indirect prompt injection (IPI), where instructions hidden in retrieved content such as web pages hijack the agent, and memory and context poisoning (MC-Poison), where malicious data is written into a store the agent later reads. The usable evidence is a trajectory log, which records agent functionality such as planning or recalling memory, and the source code; but the log rarely names the responsible function call, so the analyst must bridge the two layers by hand. In the paper's GPT-Researcher example, static taint analysis (tracing data from a source call through program variables to a sink) flagged 1,895 API calls as candidates, and log-guided pruning left 1,518 that still needed manual inspection, which the authors say would take hours. Existing static analyzers reason only about program variables and have no notion of which agent functionality a variable serves.

## 💡 The idea
Instead of matching code to agent functions one by one, the authors group agent functionalities into a few regions according to the kind of information source they touch. An external source is anything outside the app, such as user input, a web page, an email or LLM output; an internal source is something inside the app, such as a knowledge base or memory. Each region is a set of program variables that implement one group of functionality, so an analyst can ask a region-level question such as `CHECK PA DEPENDS_ON RR` and get back a small graph. The hard step is assigning variables to regions, because a name like `loader` or `ret_data` says nothing about its role. The authors handle that by encoding the steps an analyst takes during code inspection as a few rules, which need only a small set of developer-provided labels.

## ⚙️ How it works

### Cross-Layer Dependency Graph (CLDG)
The CLDG is a directed graph with two planes. The variable plane comes from forward taint propagation from the return value of every API call in the app, context-, flow- and field-sensitive, so any CEP matching the definition lies on a path in it. The function plane holds every function reachable from those API calls, and each function node receives a region label. Cross-plane edges connect each function to every program variable it defines or uses, which lets a region-level answer be traced back to concrete variables.

```text
G = (R, V_fn, λ, E_cross, V_var, E_var)
```

R is the set of regions (the paper's five representative regions are Per, Perceiving; PA, Planning and Acting; RR, Recalling and Reflecting; PK, Prompt Kitchen; and UNK, Unknown). V_fn is the set of function nodes reachable from API calls. λ maps each function node to a region or UNK. V_var is the set of program variables reachable by taint, and E_var is the set of taint edges between them. E_cross connects each function to the variables it defines or uses.

### Developer labels and region inference
Before inference, the developer labels each function that touches an information source as EXT (external) or INT (internal); the labels come from a function-to-source table collected once per application. The authors use an LLM to draft this labeling as a structured report, and a human validates it, which cuts the labeling work to reviewing a report. Region inference then applies three label-propagation rules and nine condition-action rules (Table 1). Propagation carries a label forward from a read of a source and backward from a write, and tags prompt-assembly calls such as `format` or names containing "prompt" within a single function. The condition-action rules combine these labels into a region: a function that receives a user request gets an external label and no prompt-assembly label, so it is classed as Per, which only passively reads external information.

### Query language and evaluation
An analyst submits `CHECK` queries whose atoms are `DEPENDS_ON` assertions between regions or variables, combined with AND and OR, and chained with an arrow so each step narrows the previous result. A region-level query returns the function nodes on taint paths between the queried regions; the analyst then picks source and sink variables for a variable-level query. Variable-level queries use the call graph and control-flow graphs to keep only control-flow-feasible paths, and the output is a set of function-level paths that shows the CEP. The prototype is about 16 KLoC of Python built on Joern (v4.0.360), with Pyrefly (v0.45.0) for type inference.

```text
CHECK (PK DEPENDS_ON PA) AND (PA DEPENDS_ON PK)
```

This is the query the paper uses for IPI backtracking. The first atom asks whether data from the PA region, such as a crawled web page, can flow into the PK region, where a prompt is assembled. The second asks whether the assembled prompt can flow back into PA, for example when the LLM's output is written to a document.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| 8 incidents, 5 applications: correct CEP | No numeric baseline reported | 8 of 8 correct, 1 candidate each | CEP and path identified (Table 3) |
| Verifying the CEP path, 8 incidents | Joern 0 of 8, CodeQL 0 of 8, Pysa 1 of 8 (MetaGPT IPI) | 8 of 8 (Origin) | Correct taint path found (Table 4) |
| Inspection scope | Full variable-level taint graph (100%) | Variable plane reduced to 6.49%–35.76% of original | Share of variable-plane nodes returned (introduction states 64–94% reduction) |
| Region inference accuracy | Not reported | Precision and recall 0.70 to 0.88 across five apps; aider lowest | Precision and recall |
| Developer labeling effort | Not reported | 2.4%–8.9% of functions receive labels | Share of functions with EXT or INT labels |
| Knowledge collection with LLM report | Manual labeling | Speed +15.4–75.0%, accuracy +8.3–50.4% | Speed and accuracy change (Section 10.8) |
| Quivr MC-Poison (M), analyst effort | Inspecting 2,615 LoC of Quivr source | 8 function names and 2 taint subgraphs | Code artifacts inspected |
| Runtime and memory | Not reported | CLDG construction over 95% of total time; queries under 90 ms on average; peak memory under 7 GB | Time and peak memory |

The headline result is eight of eight CEPs identified with one candidate each, but the extract does not say how many analysts or repeated runs were used per incident. The static-tool comparison tests verification rather than discovery: Joern, CodeQL and Pysa were given the ground-truth CEP and sink, and still rarely reported the correct path. All numbers come from the authors' own evaluation on five applications, and region accuracy is given as a range rather than per application. This extract ends mid-sentence in Section 10.5, so Tables 5 to 8 and the appendices were not available to check.

## ⚠️ Limitations
- The authors scope the work to predefined-workflow agents (C1), where code rather than the LLM decides control flow. OpenClaw-like agents (C2), where the LLM picks the next functionality at runtime, and agent library frameworks are out of scope and left to future research.
- Region inference needs developer-provided labels, and the LLM-drafted labels still need human validation.
- The authors call their condition-action rules "practical rather than exhaustive." aider had the lowest accuracy because its interactive commands are not chained automatically, which yields short, disjoint taint paths.
- Joern had three limitations that caused false negatives; the authors patched them and added type-informed call resolution through Pyrefly.
- The authors say the Origin source code will be released on GitHub; the extract contains no link.
- 🔎 Added context: The eight incidents are attacks the authors built using methods from prior work on five open-source applications; nothing here shows results on production incidents or closed-source systems.
- 🔎 Added context: The human study had four participants, and the extract does not say how many analysts ran the eight backtracking cases, so the 8-of-8 result rests on a thinly described analyst sample.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. Origin would sit in an agent platform's incident-response path: trajectory and application logs point to the suspect functionality, and a CLDG built offline from the repository gives responders a region-level query interface. The cost is mostly offline, since building the graph dominates runtime and peak memory stays under 7 GB, but someone must maintain the EXT/INT labels as the code changes. To trust it in production you would need the labels reviewed on your own codebase, the false-negative rate measured on a larger incident set, and evidence it works on code the authors did not test.

## 🔑 Key takeaways
- Trajectory logs describe agent functionality but seldom name the function call that caused a compromise, so backtracking needs a bridge between the two layers.
- The paper's bridge is a cross-layer dependency graph that groups variables into regions by the kind of information source they touch.
- Origin identified the correct CEP, with one candidate, in all eight incidents across five open-source agent applications.
- Joern and CodeQL verified none of the eight correct taint paths in Table 4, and Pysa verified one, so generic taint tools are not enough on their own.
- Region-level queries shrink the variable plane to 6.49%–35.76% of its original size, but the approach depends on developers labeling 2.4%–8.9% of functions and on human review of LLM drafts.
- When a call is not logged, the authors recommend placing log statements around the CEP call so the application becomes diagnosable for future incidents.

## 📚 Reading plan
### From the paper
- [arXiv:2611.26525](https://arxiv.org/abs/2611.26525) — the full paper; this extract stops in Section 10.5, so read Tables 5 to 8 and the appendices there.
### To go further
- BackTracker (cited as [45]) — the backtracking-by-dependence idea the paper builds on.
- Joern, CodeQL and Pysa — the static taint analyzers the paper benchmarks against, and where its false negatives came from.
- OWASP Top 10 threats for agent applications (cited as [54]) — the threat list that places IPI and MC-Poison as the two most prevalent.

## Concepts
- [AI agent](/concepts/ai-ml/ai-agent.md)
- [Agent memory](/concepts/ai-ml/agent-memory.md)
- [Indirect prompt injection](/concepts/security/indirect-prompt-injection.md)
- [Retrieval-augmented generation](/concepts/ai-ml/rag.md)
- [Multi-agent orchestration](/concepts/ai-ml/multi-agent-orchestration.md)
- [Memory poisoning](/concepts/security/memory-poisoning.md)
- [Information flow control](/concepts/security/information-flow-control.md)
