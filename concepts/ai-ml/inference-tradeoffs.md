---
type: Concept
title: Inference throughput/latency/cost
description: Inference throughput/latency/cost (llm-engineering).
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 9 Inference Performance Metrics p. 412; AI Accelerators p. 419; Inference Service Optimization (batching, parallelism) p. 440'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; short definition added'
---

# Inference throughput/latency/cost

**Definition.** Serving a model trades latency (time to first token, time per output token), throughput (tokens or requests per second) and cost. Batching raises throughput and lowers cost per token but can add latency; smaller or quantized models are cheaper and faster but may lose quality. Huyen explains that prefill is compute-bound and decoding is memory-bandwidth-bound, which is why they are optimised differently (ch. 9).

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 9 Inference Performance Metrics p. 412; AI Accelerators p. 419; Inference Service Optimization (batching, parallelism) p. 440: Latency, throughput, utilisation and cost metrics and the service-level levers.

## Sources

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 9 Inference Performance Metrics p. 412; AI Accelerators p. 419; Inference Service Optimization (batching, parallelism) p. 440. Latency, throughput, utilisation and cost metrics and the service-level levers.

## Relations
- [KV cache](/concepts/ai-ml/kv-cache.md) **part-of** this

## Notes
Notes tagged with this concept are listed in the concept index after each run.
