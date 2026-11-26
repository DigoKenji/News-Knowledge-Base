---
type: Concept
title: AI agent
description: A system in which a language model decides, step by step, which actions to take with tools to reach a goal, observing results and adapting, with limited human supervision.
tags:
- ai-ml
- ai-agents
domain: ai-ml
subdomain: ai-agents
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Russell, S. and Norvig, P. (2021). Artificial Intelligence - A Modern Approach, 4th ed., ch. 2 Intelligent Agents. Pearson
  resource: 'https://aima.cs.berkeley.edu/'
  kind: book
- title: 'Yao, S. et al. (2023). ReAct - Synergizing Reasoning and Acting in Language Models. ICLR 2023, arXiv:2210.03629'
  resource: 'https://arxiv.org/abs/2210.03629'
  kind: paper
- title: 'Wang, L. et al. (2024). A Survey on Large Language Model based Autonomous Agents. Frontiers of Computer Science, arXiv:2308.11432'
  resource: 'https://arxiv.org/abs/2308.11432'
  kind: paper
- title: Anthropic (2024). Building effective agents
  resource: 'https://www.anthropic.com/research/building-effective-agents'
  kind: practitioner
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 6 Agents, pp. 275-300 (tools p. 278; planning p. 281; failure modes and evaluation p. 298)'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# AI agent

**Definition.** In classic AI, an agent is anything that perceives its environment and acts on it to achieve goals. An LLM-based AI agent is a system where the model itself directs the process: it plans, chooses tools, reads the results and decides what to do next until the goal is reached or it hands back to a human. Anthropic draws the line between workflows, where code fixes the sequence of LLM calls, and agents, where the model controls the sequence.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ai-agents`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

The core pattern is ReAct: the model alternates reasoning ("I need the failing test's log") with actions (call a tool) and observations (the tool's output). Around it sits a harness that supplies tools, memory and guardrails. Agents can be single or multi-agent (an orchestrator delegating to specialised sub-agents). Autonomy is a spectrum: from a chatbot that can call one API, to coding agents that run for hours, to systems that act without any human checkpoint.

## Why it matters and trade-offs

Agents handle open-ended tasks whose steps cannot be scripted in advance. In exchange they cost more (many model calls), are less predictable, compound errors over long chains, and widen the security surface because they act on untrusted input with real permissions. Use a fixed workflow when the steps are known; use an agent when they are not.

## Example

A coding agent given "fix the failing CI job" reads the log, opens the file, edits it, reruns the test and opens a pull request, choosing each step itself.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 6 Agents, pp. 275-300 (tools p. 278; planning p. 281; failure modes and evaluation p. 298): Agent anatomy, planning approaches and how agents fail.

## Sources

1. Russell, S. and Norvig, P. (2021). *Artificial Intelligence - A Modern Approach*, 4th ed., ch. 2 Intelligent Agents. Pearson. <https://aima.cs.berkeley.edu/> — The classic definition of an agent as perceiving and acting in an environment.
2. Yao, S. et al. (2023). *ReAct - Synergizing Reasoning and Acting in Language Models*. ICLR 2023, arXiv:2210.03629. <https://arxiv.org/abs/2210.03629> — The reason-act-observe loop behind most LLM agents.
3. Wang, L. et al. (2024). *A Survey on Large Language Model based Autonomous Agents*. Frontiers of Computer Science, arXiv:2308.11432. <https://arxiv.org/abs/2308.11432> — Survey of agent components (profile, memory, planning, action).
4. Anthropic (2024). *Building effective agents*. <https://www.anthropic.com/research/building-effective-agents> — Workflows vs agents, and when each is appropriate.
5. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 Agents, pp. 275-300 (tools p. 278; planning p. 281; failure modes and evaluation p. 298). Agent anatomy, planning approaches and how agents fail.

## Relations
- (none yet)

## Notes
- [You Think AI Is Overhyped? Just Wait Until You See What Comes Next.](/notes/ai-ml/2026-11-20-you-think-ai-is-overhyped-just-wait-until-you-see-what-comes.md)
