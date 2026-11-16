---
type: Concept
title: Prefill-decode disaggregation
description: Serving LLM prefill and decode phases on separate GPU pools, moving the KV cache between them.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: draft
generated:
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
relations:
- type: uses
  target: /concepts/kv-cache.md
- type: part-of
  target: /concepts/inference-tradeoffs.md
---

# Prefill-decode disaggregation

Serving LLM prefill and decode phases on separate GPU pools, moving the KV cache between them.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Proposed by the daily run on 2026-11-16, waiting for Kenji to certify.

## Relations
- **uses** [KV cache](/concepts/kv-cache.md)
- **part-of** [Inference throughput/latency/cost](/concepts/inference-tradeoffs.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
