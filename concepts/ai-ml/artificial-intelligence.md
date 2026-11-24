---
type: Concept
title: Artificial intelligence
description: The field and the systems that perform tasks normally associated with human intelligence - perceiving, reasoning, learning, deciding, generating content - by producing outputs such as predictions, recommendations or decisions from the input they receive.
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
- title: OECD (2024). Explanatory memorandum on the updated OECD definition of an AI system
  resource: 'https://oecd.ai/en/wonk/ai-system-definition-update'
  kind: standard
- title: ISO/IEC (2022). ISO/IEC 22989 - Artificial intelligence concepts and terminology
  resource: 'https://www.iso.org/standard/74296.html'
  kind: standard
- title: Russell, S. and Norvig, P. (2021). Artificial Intelligence - A Modern Approach, 4th ed., ch. 1. Pearson
  resource: 'https://aima.cs.berkeley.edu/'
  kind: book
foundation_candidate: true
---

# Artificial intelligence

**Definition.** AI is both a research field and a kind of system. The OECD (definition updated in 2023, also used by the EU AI Act) describes an AI system as a machine-based system that, for explicit or implicit objectives, infers from the input it receives how to generate outputs such as predictions, content, recommendations or decisions that can influence physical or virtual environments. ISO/IEC 22989 is the international standard vocabulary for the field.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

AI is the umbrella. Older approaches encode knowledge by hand (rules, search, logic, expert systems); modern AI is dominated by machine learning, where behaviour is learned from data. Inside ML, deep learning and then large language models drive most of what the KB covers today. "Narrow" AI solves specific tasks; "general" AI (AGI) would match humans across tasks and does not exist yet.

## Example

A spam filter, a route planner, a chess engine and a chatbot are all AI systems; only some of them use machine learning.

## Narrower concepts

- [Machine learning](/concepts/ai-ml/machine-learning.md)
- [AI agent](/concepts/ai-ml/ai-agent.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## Sources

1. OECD (2024). *Explanatory memorandum on the updated OECD definition of an AI system*. <https://oecd.ai/en/wonk/ai-system-definition-update> — Intergovernmental definition of an AI system, adopted by the EU AI Act.
2. ISO/IEC (2022). *ISO/IEC 22989 - Artificial intelligence concepts and terminology*. <https://www.iso.org/standard/74296.html> — International standard vocabulary for AI (paywalled; cited, not quoted).
3. Russell, S. and Norvig, P. (2021). *Artificial Intelligence - A Modern Approach*, 4th ed., ch. 1. Pearson. <https://aima.cs.berkeley.edu/> — Standard textbook framing of AI as building rational agents; history of approaches.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
