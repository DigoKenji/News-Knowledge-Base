---
type: Concept
title: Transformer and attention
description: The neural network architecture behind modern LLMs, built on self-attention - a mechanism that lets every token weigh every other token in the input to decide what is relevant - processing sequences in parallel instead of word by word.
tags:
- ai-ml
- core
domain: ai-ml
subdomain: core
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
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
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 3 Transformer Anatomy, pp. 57-86 (self-attention p. 61; positional embeddings p. 73)'
- title: 'Raschka, S., Liu, Y. and Mirjalili, V. (2022). Machine Learning with PyTorch and Scikit-Learn. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781801819312'
  kind: book
  locator: 'ch. 16 Transformers, pp. 539-588 (self-attention p. 544; original architecture p. 552)'
- title: 'Foster, D. (2023). Generative Deep Learning, 2nd edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098134181'
  kind: book
  locator: 'ch. 9 Transformers, pp. 235-265'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Transformer and attention

**Definition.** Introduced in "Attention Is All You Need" (2017), the transformer stacks layers of multi-head self-attention and feed-forward networks. In attention, each token produces a query, a key and a value; the query is compared with all keys to get weights, and the output is the weighted mix of values. Positional information is added separately (for example with RoPE).

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Attention existed before (Bahdanau et al. used it with recurrent networks for translation); the transformer removed recurrence entirely, which made training highly parallel on GPUs and enabled scaling to billions of parameters. Variants: encoder-only (BERT, used for embeddings), decoder-only (GPT, Claude, Llama, used for generation) and encoder-decoder (T5). Attention cost grows with the square of sequence length, which drives work on long context, KV cache size and efficient attention.

## Example

In "the animal didn't cross the street because it was tired", attention lets the token "it" put most of its weight on "animal" rather than "street".

## Narrower concepts

- [Rotary position embedding (RoPE)](/concepts/ai-ml/rope.md)
- [Multi-head latent attention](/concepts/ai-ml/multi-head-latent-attention.md)
- [KV cache](/concepts/ai-ml/kv-cache.md)
- [Mixture of experts](/concepts/ai-ml/mixture-of-experts.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Tunstall, L., von Werra, L. and Wolf, T. (2022)**, *Natural Language Processing with Transformers, revised edition*, ch. 3 Transformer Anatomy, pp. 57-86 (self-attention p. 61; positional embeddings p. 73): Encoder and decoder internals explained by Hugging Face authors.
- **Raschka, S., Liu, Y. and Mirjalili, V. (2022)**, *Machine Learning with PyTorch and Scikit-Learn*, ch. 16 Transformers, pp. 539-588 (self-attention p. 544; original architecture p. 552): From attention in RNNs to scaled dot-product and multi-head attention.
- **Foster, D. (2023)**, *Generative Deep Learning, 2nd edition*, ch. 9 Transformers, pp. 235-265: The transformer as a generative (autoregressive) model.

## Sources

1. Vaswani, A. et al. (2017). *Attention Is All You Need*. NeurIPS 2017, arXiv:1706.03762. <https://arxiv.org/abs/1706.03762> — Original transformer architecture and scaled dot-product attention.
2. Bahdanau, D., Cho, K. and Bengio, Y. (2015). *Neural Machine Translation by Jointly Learning to Align and Translate*. ICLR 2015, arXiv:1409.0473. <https://arxiv.org/abs/1409.0473> — The attention mechanism before transformers.
3. Alammar, J. (2018). *The Illustrated Transformer*. <https://jalammar.github.io/illustrated-transformer/> — Widely used visual explanation of queries, keys, values and multi-head attention.
4. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 3 Transformer Anatomy, pp. 57-86 (self-attention p. 61; positional embeddings p. 73). Encoder and decoder internals explained by Hugging Face authors.
5. Raschka, S., Liu, Y. and Mirjalili, V. (2022). *Machine Learning with PyTorch and Scikit-Learn*. Packt. <https://books.google.com/books?vid=ISBN9781801819312> — ch. 16 Transformers, pp. 539-588 (self-attention p. 544; original architecture p. 552). From attention in RNNs to scaled dot-product and multi-head attention.
6. Foster, D. (2023). *Generative Deep Learning, 2nd edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098134181> — ch. 9 Transformers, pp. 235-265. The transformer as a generative (autoregressive) model.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
