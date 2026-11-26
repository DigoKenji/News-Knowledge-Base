---
type: Concept
title: Mixture of experts
description: A neural-network layer made of many expert sub-networks plus a router that activates only a few experts per token, so total parameters grow without a matching growth in compute per token.
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
- title: Jacobs, R. A. et al. (1991). Adaptive Mixtures of Local Experts. Neural Computation 3(1)
  resource: 'https://doi.org/10.1162/neco.1991.3.1.79'
  kind: paper
- title: 'Shazeer, N. et al. (2017). Outrageously Large Neural Networks - The Sparsely-Gated Mixture-of-Experts Layer. ICLR 2017, arXiv:1701.06538'
  resource: 'https://arxiv.org/abs/1701.06538'
  kind: paper
- title: 'Fedus, W., Zoph, B. and Shazeer, N. (2022). Switch Transformers. JMLR, arXiv:2101.03961'
  resource: 'https://arxiv.org/abs/2101.03961'
  kind: paper
- title: 'Jiang, A. Q. et al. (2024). Mixtral of Experts. arXiv:2401.04088'
  resource: 'https://arxiv.org/abs/2401.04088'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Mixture of experts

**Definition.** A mixture-of-experts (MoE) layer replaces one large feed-forward block with N smaller "experts" and a learned gating network. For each token the router picks the top-k experts (often 1 or 2 of 8 to 256) and combines their outputs. The model has many parameters in total but uses only a fraction of them for any given token, which is called sparse activation.

Domain: [ai-ml](/domains/ai-ml.md). Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

The idea dates from Jacobs et al. (1991). Shazeer et al. (2017) made it work at scale in deep learning with a sparsely-gated layer and an auxiliary loss that balances load across experts, so the router does not send everything to a few favourites. Switch Transformer simplified routing to top-1. Mixtral 8x7B has about 47B parameters but uses about 13B per token. Experts do not specialise in human topics like "law" or "maths"; they tend to specialise in token-level patterns. Experts are usually spread across GPUs (expert parallelism), which adds all-to-all communication.

## Why it matters and trade-offs

MoE gives more model capacity for the same training and inference compute, which is why many frontier and open models (Mixtral, DeepSeek-V3, Qwen MoE) use it. The cost is memory: all experts must be loaded even if few are used, plus training instability, load-balancing work and harder serving.

## Example

DeepSeek-V3 has 671B parameters but activates about 37B per token, so it runs at roughly the per-token cost of a 37B dense model.

## Sources

1. Jacobs, R. A. et al. (1991). *Adaptive Mixtures of Local Experts*. Neural Computation 3(1). <https://doi.org/10.1162/neco.1991.3.1.79> — Original mixture-of-experts idea with a gating network.
2. Shazeer, N. et al. (2017). *Outrageously Large Neural Networks - The Sparsely-Gated Mixture-of-Experts Layer*. ICLR 2017, arXiv:1701.06538. <https://arxiv.org/abs/1701.06538> — Sparse top-k gating and load balancing at scale.
3. Fedus, W., Zoph, B. and Shazeer, N. (2022). *Switch Transformers*. JMLR, arXiv:2101.03961. <https://arxiv.org/abs/2101.03961> — Top-1 routing and training stability.
4. Jiang, A. Q. et al. (2024). *Mixtral of Experts*. arXiv:2401.04088. <https://arxiv.org/abs/2401.04088> — Total vs active parameters in a production open model; expert specialisation analysis.

## Relations
- (none yet)

## Notes
- [The US-China Open-Weight Feedback Loop](/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md)
