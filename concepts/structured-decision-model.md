---
type: Concept
title: Structured decision model
description: A model used to make a decision from a fixed set of typed outputs - a label, a score, a yes/no, a probability - rather than to write free text; in practice a classifier or a constrained-output LLM call.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Willard, B. T. and Louf, R. (2023). Efficient Guided Generation for Large Language Models. arXiv:2307.09702'
  resource: 'https://arxiv.org/abs/2307.09702'
  kind: paper
- title: OpenAI. Structured model outputs (API guide)
  resource: 'https://developers.openai.com/api/docs/guides/structured-outputs'
  kind: docs
- title: Anthropic. Structured outputs (Claude API docs)
  resource: 'https://platform.claude.com/docs/en/build-with-claude/structured-outputs'
  kind: docs
---

# Structured decision model

**Definition.** A structured decision model returns a machine-readable answer that conforms to a schema (for example {"route": "billing", "confidence": 0.92}) so code can act on it directly. It can be a small classifier trained for the task, or a general LLM forced into the schema with structured outputs or constrained decoding.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Two routes. (1) Constrain a generative model: the provider (or a library such as Outlines) masks tokens during decoding so the output always matches a JSON schema or grammar, which guarantees parseable output. (2) Use a discriminative model: train or distil a small classifier that outputs class probabilities directly, which is much cheaper and gives calibrated scores if trained well. The 2026-11-17 notes describe vendors selling (2) as a cheap replacement for LLM calls in routing, moderation and grading.

## Why it matters and trade-offs

For high-volume decisions, a typed answer is easier to validate, log, threshold and test than prose. A small classifier is cheap but needs labelled data and does not handle new categories; a constrained LLM is flexible but costly, and a schema guarantees format, not correctness.

## Example

Instead of asking an LLM "should this article go into the digest? explain", call a model that returns {"select": true, "domain": "security", "score": 0.81}.

## Review notes for Kenji

"Structured decision model" is not an established term; it comes from vendor articles (TypeSafe Jev). Consider rejecting it, or renaming to "structured outputs" or "classifier instead of LLM call". The definition above describes the real underlying idea.

## Sources

1. Willard, B. T. and Louf, R. (2023). *Efficient Guided Generation for Large Language Models*. arXiv:2307.09702. <https://arxiv.org/abs/2307.09702> — Constrained decoding that guarantees output matching a regex or JSON schema.
2. OpenAI. *Structured model outputs* (API guide). <https://developers.openai.com/api/docs/guides/structured-outputs> — Schema-constrained outputs offered by a major provider.
3. Anthropic. *Structured outputs* (Claude API docs). <https://platform.claude.com/docs/en/build-with-claude/structured-outputs> — The same capability from an independent provider.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
