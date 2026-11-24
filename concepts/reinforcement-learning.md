---
type: Concept
title: Reinforcement learning
description: Machine learning in which an agent learns by trial and error which actions to take in an environment to maximise a cumulative reward, rather than from labelled examples.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: Sutton, R. S. and Barto, A. G. (2018). Reinforcement Learning - An Introduction, 2nd ed. MIT Press
  resource: 'http://incompleteideas.net/book/the-book-2nd.html'
  kind: book
- title: 'Ouyang, L. et al. (2022). Training language models to follow instructions with human feedback. NeurIPS 2022, arXiv:2203.02155'
  resource: 'https://arxiv.org/abs/2203.02155'
  kind: paper
- title: Silver, D. et al. (2016). Mastering the game of Go with deep neural networks and tree search. Nature 529
  resource: 'https://doi.org/10.1038/nature16961'
  kind: paper
foundation_candidate: true
---

# Reinforcement learning

**Definition.** In reinforcement learning (RL) an agent observes a state, takes an action, receives a reward and a new state, and learns a policy (a rule for choosing actions) that maximises long-term reward. The central difficulty is the trade-off between exploring new actions and exploiting what already works, and crediting rewards to actions taken long before.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

RL powered game-playing systems (AlphaGo), robotics and recommendation. For LLMs it matters through RLHF (reinforcement learning from human feedback) and newer RL on verifiable tasks, which turn pre-trained models into helpful assistants and stronger reasoners. RL is data-hungry and sensitive to badly designed rewards, which the model may "hack".

## Example

A model writes two answers, humans pick the better one, a reward model learns those preferences, and RL tunes the LLM to produce answers the reward model scores highly.

## Narrower concepts

- [AI agent](/concepts/ai-agent.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. Sutton, R. S. and Barto, A. G. (2018). *Reinforcement Learning - An Introduction*, 2nd ed. MIT Press. <http://incompleteideas.net/book/the-book-2nd.html> — Standard textbook definition - agent, environment, reward, policy, exploration.
2. Ouyang, L. et al. (2022). *Training language models to follow instructions with human feedback*. NeurIPS 2022, arXiv:2203.02155. <https://arxiv.org/abs/2203.02155> — RLHF applied to LLMs (InstructGPT).
3. Silver, D. et al. (2016). *Mastering the game of Go with deep neural networks and tree search*. Nature 529. <https://doi.org/10.1038/nature16961> — Landmark deep RL result (AlphaGo), independent of the LLM context.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
