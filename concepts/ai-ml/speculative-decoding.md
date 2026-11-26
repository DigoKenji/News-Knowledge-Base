---
type: Concept
title: Speculative decoding
description: A lossless way to speed up LLM generation - a fast draft model guesses several next tokens and the large model verifies them all in one forward pass, keeping the ones it agrees with.
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
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Leviathan, Y., Kalman, M. and Matias, Y. (2023). Fast Inference from Transformers via Speculative Decoding. ICML 2023, arXiv:2211.17192'
  resource: 'https://arxiv.org/abs/2211.17192'
  kind: paper
- title: 'Chen, C. et al. (2023). Accelerating Large Language Model Decoding with Speculative Sampling. arXiv:2302.01318'
  resource: 'https://arxiv.org/abs/2302.01318'
  kind: paper
- title: 'Cai, T. et al. (2024). Medusa - Simple LLM Inference Acceleration Framework with Multiple Decoding Heads. ICML 2024, arXiv:2401.10774'
  resource: 'https://arxiv.org/abs/2401.10774'
  kind: paper
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 8 Speculative decoding, pp. 295-303'
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 9 Model Optimization (overcoming the autoregressive bottleneck), pp. 426-440'
relations:
- type: part-of
  target: /concepts/ai-ml/inference-tradeoffs.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Speculative decoding

**Definition.** Speculative decoding (also speculative sampling) uses a small draft model to propose k tokens, then runs the target model once over all k in parallel. Tokens are accepted up to the first disagreement, and a correction token is sampled there. A rejection-sampling rule guarantees the output has exactly the same distribution as the target model alone, so quality is unchanged.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Decoding is memory-bound: generating one token or checking five costs nearly the same time, because the bottleneck is reading the weights. If the draft is right often (high acceptance rate), several tokens come out per expensive step, typically a 2-3× latency gain. Drafts can be a separate small model, extra prediction heads on the target (Medusa), a feature-level draft (EAGLE), or n-gram lookup from the prompt. Leviathan et al. (Google) and Chen et al. (DeepMind) published the method independently in 2022-2023.

## Why it matters and trade-offs

Faster responses at the same quality, which is why most serving engines support it. Gains shrink with large batch sizes (the GPU is already busy), the draft costs memory and compute, and acceptance rates drop on creative or high-temperature output.

## Example

The draft proposes "the cat sat on the"; the target accepts "the cat sat", rejects "on" and samples "by" instead, so three tokens plus one correction come from a single target pass.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 8 Speculative decoding, pp. 295-303: Draft-and-verify decoding with code.
- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 9 Model Optimization (overcoming the autoregressive bottleneck), pp. 426-440: Speculative decoding alongside other decoding speed-ups.

## Sources

1. Leviathan, Y., Kalman, M. and Matias, Y. (2023). *Fast Inference from Transformers via Speculative Decoding*. ICML 2023, arXiv:2211.17192. <https://arxiv.org/abs/2211.17192> — Method and proof that outputs match the target distribution.
2. Chen, C. et al. (2023). *Accelerating Large Language Model Decoding with Speculative Sampling*. arXiv:2302.01318. <https://arxiv.org/abs/2302.01318> — Independent DeepMind derivation, 2-2.5× speedup on Chinchilla 70B.
3. Cai, T. et al. (2024). *Medusa - Simple LLM Inference Acceleration Framework with Multiple Decoding Heads*. ICML 2024, arXiv:2401.10774. <https://arxiv.org/abs/2401.10774> — Draft-free variant using extra heads on the target model.
4. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 8 Speculative decoding, pp. 295-303. Draft-and-verify decoding with code.
5. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 9 Model Optimization (overcoming the autoregressive bottleneck), pp. 426-440. Speculative decoding alongside other decoding speed-ups.

## Relations
- **part-of** [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
