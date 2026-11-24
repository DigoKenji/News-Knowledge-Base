---
type: Concept
title: Agent harness
description: The software around a language model that turns it into an agent - it builds the context, exposes tools, runs the loop, enforces rules and keeps state between steps.
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
  at: '2026-11-17T21:40:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Anthropic (2024). Building effective agents
  resource: 'https://www.anthropic.com/research/building-effective-agents'
  kind: practitioner
- title: Anthropic (2025). Effective harnesses for long-running agents
  resource: 'https://www.anthropic.com/research/effective-harnesses-for-long-running-agents'
  kind: practitioner
- title: 'Yang, J. et al. (2024). SWE-agent - Agent-Computer Interfaces Enable Automated Software Engineering. NeurIPS 2024, arXiv:2405.15793'
  resource: 'https://arxiv.org/abs/2405.15793'
  kind: paper
---

# Agent harness

**Definition.** An agent harness is everything in an AI agent that is not the model weights: the code that assembles the prompt and context for each step, offers tools and executes the calls the model asks for, runs the plan-act-observe loop, decides when to stop, and stores memory, logs and checkpoints. The same model can behave very differently inside different harnesses.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ai-agents`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

A typical harness loop: (1) build the context window from the system prompt, task, relevant files, memory and the results of earlier tool calls; (2) call the model; (3) parse the reply, and if it requests a tool, validate the arguments, check permissions and run it (shell, browser, API, search); (4) append the result and repeat until the model signals completion or a budget, step limit or guardrail stops it. Around the loop the harness adds sandboxing, retries, context compaction when the window fills up, and checkpoints so long tasks can resume. SWE-agent called the tool layer an "agent-computer interface" and showed that designing it well raised task success with the same model.

## Why it matters and trade-offs

Rules that live only in the prompt are suggestions; rules enforced in harness code (allowed tools, spend limits, approval steps, output validation) are guarantees. The harness also drives cost, because it decides how much context is resent on every step. The trade-off is complexity: Anthropic's guidance is to start with the simplest loop and add structure (workflows, sub-agents, memory) only when a measured failure needs it.

## Example

Claude Code, Codex CLI and SWE-agent run models through harnesses that read and edit files, run tests in a sandbox and ask the user before risky commands. The Project_News daily run is itself a small harness: a scheduled task with fixed prompts, a source list and a validator.

## Not to be confused with

An agent framework (LangGraph, CrewAI) is a library for building harnesses; the harness is the running system. An evaluation harness (for example lm-evaluation-harness) is test tooling, a different use of the word.

## Sources

1. Anthropic (2024). *Building effective agents*. <https://www.anthropic.com/research/building-effective-agents> — Agent = model using tools in a loop; start simple, add workflow structure only when needed.
2. Anthropic (2025). *Effective harnesses for long-running agents*. <https://www.anthropic.com/research/effective-harnesses-for-long-running-agents> — The term "harness" and what it manages across long tasks (progress files, checkpoints, resuming).
3. Yang, J. et al. (2024). *SWE-agent - Agent-Computer Interfaces Enable Automated Software Engineering*. NeurIPS 2024, arXiv:2405.15793. <https://arxiv.org/abs/2405.15793> — Peer-reviewed evidence that the interface around a fixed model changes task success.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
