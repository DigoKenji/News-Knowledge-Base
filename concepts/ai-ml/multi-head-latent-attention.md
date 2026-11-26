---
type: Concept
title: Multi-head latent attention
description: An attention variant introduced in DeepSeek-V2 that compresses each token's keys and values into one small shared latent vector, shrinking the KV cache several-fold while keeping quality close to standard multi-head attention.
tags:
- ai-ml
- deep-learning
domain: ai-ml
subdomain: deep-learning
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'DeepSeek-AI (2024). DeepSeek-V2 - A Strong, Economical, and Efficient Mixture-of-Experts Language Model. arXiv:2405.04434'
  resource: 'https://arxiv.org/abs/2405.04434'
  kind: paper
- title: 'Shazeer, N. (2019). Fast Transformer Decoding - One Write-Head is All You Need. arXiv:1911.02150'
  resource: 'https://arxiv.org/abs/1911.02150'
  kind: paper
- title: 'Ainslie, J. et al. (2023). GQA - Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints. EMNLP 2023, arXiv:2305.13245'
  resource: 'https://arxiv.org/abs/2305.13245'
  kind: paper
- title: 'Meng, F. et al. (2025). TransMLA - Multi-Head Latent Attention Is All You Need. arXiv:2502.07864'
  resource: 'https://arxiv.org/abs/2502.07864'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Multi-head latent attention

**Definition.** Multi-head latent attention (MLA) stores, for each past token, a single low-dimensional latent vector instead of separate key and value vectors for every attention head. At attention time the per-head keys and values are reconstructed (or folded into the query and output projections) from that latent.

Domain: [ai-ml](/domains/ai-ml.md). Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

During generation, every token's keys and values are cached so they are not recomputed; for long contexts this KV cache, not the weights, limits batch size and speed. Earlier fixes shared keys and values across heads: multi-query attention (MQA, one shared K/V) and grouped-query attention (GQA, a few groups), trading some quality for memory. MLA instead applies a learned low-rank down-projection to a latent of a few hundred dimensions and up-projects per head when needed. Because RoPE position rotation does not commute with that compression, DeepSeek keeps a small decoupled key part that carries position. DeepSeek-V2 reported a 93% smaller KV cache than its dense predecessor and higher quality than GQA.

## Why it matters and trade-offs

Smaller KV cache means longer contexts, bigger batches and cheaper serving. The cost is extra projection math, a more complex implementation (kernels must support the latent layout), and the fact that it must be designed in at pre-training; converting existing GQA models is an active research area.

## Example

In DeepSeek-V3, each token caches a 512-dimensional latent plus a 64-dimensional position key, instead of 128 heads × 128 dimensions for both keys and values.

## Sources

1. DeepSeek-AI (2024). *DeepSeek-V2 - A Strong, Economical, and Efficient Mixture-of-Experts Language Model*. arXiv:2405.04434. <https://arxiv.org/abs/2405.04434> — Introduction of MLA, decoupled RoPE, KV-cache reduction figures.
2. Shazeer, N. (2019). *Fast Transformer Decoding - One Write-Head is All You Need*. arXiv:1911.02150. <https://arxiv.org/abs/1911.02150> — Multi-query attention, the earlier way to shrink the KV cache.
3. Ainslie, J. et al. (2023). *GQA - Training Generalized Multi-Query Transformer Models from Multi-Head Checkpoints*. EMNLP 2023, arXiv:2305.13245. <https://arxiv.org/abs/2305.13245> — Grouped-query attention, the baseline MLA is compared with.
4. Meng, F. et al. (2025). *TransMLA - Multi-Head Latent Attention Is All You Need*. arXiv:2502.07864. <https://arxiv.org/abs/2502.07864> — Independent analysis showing MLA is more expressive than GQA at equal cache size.

## Relations
- (none yet)

## Notes
- [The US-China Open-Weight Feedback Loop](/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md)
