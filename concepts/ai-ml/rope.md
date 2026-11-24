---
type: Concept
title: Rotary position embedding (RoPE)
description: Rotary Position Embedding - a way to encode token positions by rotating query and key vectors by position-dependent angles, so attention scores depend on the relative distance between tokens.
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
- title: 'Su, J. et al. (2021). RoFormer - Enhanced Transformer with Rotary Position Embedding. arXiv:2104.09864 (Neurocomputing 2024)'
  resource: 'https://arxiv.org/abs/2104.09864'
  kind: paper
- title: Biderman, S. et al. (2021). Rotary Embeddings - A Relative Revolution. EleutherAI blog
  resource: 'https://blog.eleuther.ai/rotary-embeddings/'
  kind: practitioner
- title: 'Peng, B. et al. (2024). YaRN - Efficient Context Window Extension of Large Language Models. ICLR 2024, arXiv:2309.00071'
  resource: 'https://arxiv.org/abs/2309.00071'
  kind: paper
- title: 'Vaswani, A. et al. (2017). Attention Is All You Need. NeurIPS 2017, arXiv:1706.03762'
  resource: 'https://arxiv.org/abs/1706.03762'
  kind: paper
---

# Rotary position embedding (RoPE)

**Definition.** Transformers have no built-in sense of order, so position must be injected. RoPE does this by splitting each query and key vector into pairs of dimensions and rotating each pair by an angle proportional to the token's position, with a different frequency per pair. The dot product of a rotated query and key then depends only on their positions' difference.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

The original Transformer added fixed sinusoidal vectors to the embeddings (absolute position); later models learned position vectors. RoPE (Su et al., 2021) applies the rotation inside attention, which gives relative-position behaviour with no extra parameters and works with efficient attention kernels. Low-frequency pairs track long-range position, high-frequency pairs short-range. It became the default in LLaMA, Mistral, Qwen, DeepSeek and most open LLMs. Extending context beyond the training length is done by rescaling the frequencies: position interpolation, NTK-aware scaling and YaRN.

## Why it matters and trade-offs

RoPE is simple, parameter-free and good at relative position, which is why it won. Models still degrade beyond their trained length without rescaling, and RoPE complicates KV-cache compression (see multi-head latent attention, which needs a decoupled RoPE part).

## Example

Words 5 and 8 in a sentence get the same attention relationship as words 105 and 108, because only the gap of 3 positions enters the score.

## Sources

1. Su, J. et al. (2021). *RoFormer - Enhanced Transformer with Rotary Position Embedding*. arXiv:2104.09864 (Neurocomputing 2024). <https://arxiv.org/abs/2104.09864> — Original method and its relative-position property.
2. Biderman, S. et al. (2021). *Rotary Embeddings - A Relative Revolution*. EleutherAI blog. <https://blog.eleuther.ai/rotary-embeddings/> — Independent explanation and experiments comparing RoPE with other position methods.
3. Peng, B. et al. (2024). *YaRN - Efficient Context Window Extension of Large Language Models*. ICLR 2024, arXiv:2309.00071. <https://arxiv.org/abs/2309.00071> — Context extension by rescaling RoPE frequencies.
4. Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS 2017, arXiv:1706.03762. <https://arxiv.org/abs/1706.03762> — The sinusoidal absolute-position baseline RoPE replaced.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
