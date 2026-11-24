---
type: Concept
title: Transformer and attention
description: The neural network architecture behind modern LLMs, built on self-attention - a mechanism that lets every token weigh every other token in the input to decide what is relevant - processing sequences in parallel instead of word by word.
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
- title: 'Vaswani, A. et al. (2017). Attention Is All You Need. NeurIPS 2017, arXiv:1706.03762'
  resource: 'https://arxiv.org/abs/1706.03762'
  kind: paper
- title: 'Bahdanau, D., Cho, K. and Bengio, Y. (2015). Neural Machine Translation by Jointly Learning to Align and Translate. ICLR 2015, arXiv:1409.0473'
  resource: 'https://arxiv.org/abs/1409.0473'
  kind: paper
- title: Alammar, J. (2018). The Illustrated Transformer
  resource: 'https://jalammar.github.io/illustrated-transformer/'
  kind: practitioner
---

# Transformer and attention

**Definition.** Introduced in "Attention Is All You Need" (2017), the transformer stacks layers of multi-head self-attention and feed-forward networks. In attention, each token produces a query, a key and a value; the query is compared with all keys to get weights, and the output is the weighted mix of values. Positional information is added separately (for example with RoPE).

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Attention existed before (Bahdanau et al. used it with recurrent networks for translation); the transformer removed recurrence entirely, which made training highly parallel on GPUs and enabled scaling to billions of parameters. Variants: encoder-only (BERT, used for embeddings), decoder-only (GPT, Claude, Llama, used for generation) and encoder-decoder (T5). Attention cost grows with the square of sequence length, which drives work on long context, KV cache size and efficient attention.

## Example

In "the animal didn't cross the street because it was tired", attention lets the token "it" put most of its weight on "animal" rather than "street".

## Narrower concepts

- [Rotary position embedding (RoPE)](/concepts/rope.md)
- [Multi-head latent attention](/concepts/multi-head-latent-attention.md)
- [KV cache](/concepts/kv-cache.md)
- [Mixture of experts](/concepts/mixture-of-experts.md)

## Sources

1. Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS 2017, arXiv:1706.03762. <https://arxiv.org/abs/1706.03762> — Original transformer architecture and scaled dot-product attention.
2. Bahdanau, D., Cho, K. and Bengio, Y. (2015). *Neural Machine Translation by Jointly Learning to Align and Translate*. ICLR 2015, arXiv:1409.0473. <https://arxiv.org/abs/1409.0473> — The attention mechanism before transformers.
3. Alammar, J. (2018). *The Illustrated Transformer*. <https://jalammar.github.io/illustrated-transformer/> — Widely used visual explanation of queries, keys, values and multi-head attention.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
