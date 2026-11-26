---
type: Concept
title: Multi-agent orchestration
description: 'Coordinating several AI agents with different roles or tools - who acts when, how they hand over work and share state - to complete a task none would do as well alone.'
tags:
- ai-ml
- ai-agents
domain: ai-ml
subdomain: ai-agents
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 6 RAG and Agents: Agents (planning, multi-agent systems), p. 281'
- title: 'Wu, Q. et al. (2023). AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation. arXiv:2308.08155'
  resource: 'https://arxiv.org/abs/2308.08155'
  kind: paper
- title: 'Anthropic (2025). How we built our multi-agent research system'
  resource: 'https://www.anthropic.com/engineering/multi-agent-research-system'
  kind: practitioner
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# Multi-agent orchestration

**Definition.** Multi-agent orchestration splits a task across several LLM agents - for example a planner, workers with different tools, and a critic - and defines how they are coordinated: a central orchestrator that delegates and merges results, a fixed pipeline, or agents that converse until done. Huyen notes that even intent routing or reflection can be framed as a multi-agent system.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ai-agents`. Expanded on 2026-11-26 with independent sources.

## How it fits

It buys parallelism, separation of context and specialisation at the cost of more tokens, harder debugging and new failure modes (agents talking past each other, duplicated work). Anthropic reports large gains for broad research tasks with an orchestrator-worker design, and also much higher token use.

## Example

A research assistant's lead agent splits 'compare three vector databases' into three sub-questions, runs a search agent for each in parallel, and writes the comparison from their findings.

## Sources

1. Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 6 RAG and Agents: Agents (planning, multi-agent systems), p. 281. Multi-agent framing of planning, routing and reflection.
2. Wu, Q. et al. (2023). AutoGen: Enabling Next-Gen LLM Applications via Multi-Agent Conversation. arXiv:2308.08155. <https://arxiv.org/abs/2308.08155> — Framework for agents that coordinate by conversation.
3. Anthropic (2025). How we built our multi-agent research system. <https://www.anthropic.com/engineering/multi-agent-research-system> — Orchestrator-worker design, gains and token cost in production.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
