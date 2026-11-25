---
type: Concept
title: KV cache
description: KV cache (llm-engineering).
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
relations:
- type: part-of
  target: /concepts/ai-ml/inference-tradeoffs.md
sources:
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 8 KV cache, pp. 291-295'
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 9 Model Optimization (attention and KV cache), pp. 426-440'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; short definition added'
---

# KV cache

**Definition.** During autoregressive decoding every new token attends to all previous tokens. The KV cache stores the key and value vectors already computed for earlier tokens so they are not recomputed at each step. The price is memory that grows with sequence length, batch size, number of layers and heads, which often limits how many requests one GPU can serve (Iusztin and Labonne, ch. 8; Huyen, ch. 9).

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 8 KV cache, pp. 291-295: Why the cache speeds up decoding and how much memory it costs.
- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 9 Model Optimization (attention and KV cache), pp. 426-440: KV cache size as a main inference bottleneck.

## Sources

1. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 8 KV cache, pp. 291-295. Why the cache speeds up decoding and how much memory it costs.
2. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 9 Model Optimization (attention and KV cache), pp. 426-440. KV cache size as a main inference bottleneck.

## Relations
- **part-of** [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
