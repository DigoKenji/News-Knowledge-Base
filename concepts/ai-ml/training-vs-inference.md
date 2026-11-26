---
type: Concept
title: Training vs inference
description: The two phases of a model's life - training, where its parameters are learned from data (slow, expensive, done once or occasionally), and inference, where the trained model is used to produce outputs for new inputs (done for every request).
tags:
- ai-ml
- ml-fundamentals
- core
domain: ai-ml
subdomain: ml-fundamentals
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: Goodfellow, I., Bengio, Y. and Courville, A. (2016). Deep Learning, ch. 6 and 8. MIT Press
  resource: 'https://www.deeplearningbook.org/'
  kind: book
- title: 'Pope, R. et al. (2023). Efficiently Scaling Transformer Inference. MLSys 2023, arXiv:2211.05102'
  resource: 'https://arxiv.org/abs/2211.05102'
  kind: paper
- title: Google. Machine Learning Glossary - training, inference
  resource: 'https://developers.google.com/machine-learning/glossary'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 9 Inference Optimization, pp. 405-447 (inference overview p. 406; performance metrics p. 412)'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 10 Inference Pipeline Deployment, pp. 355-400'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Training vs inference

**Definition.** Training runs data forward through the model, measures the error and updates the weights backwards (backpropagation), over many passes. Inference only runs the forward pass with fixed weights. For LLMs, training splits further into pre-training (self-supervised, on huge corpora) and post-training (fine-tuning, RLHF); inference splits into prefill (reading the prompt) and decode (generating tokens).

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ml-fundamentals`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

The two phases have different economics and engineering. Training is a big, batch, compute-bound job measured in GPU-weeks. Inference is a latency-sensitive service whose cost grows with usage and often exceeds training cost over a model's life. Many KB concepts are inference optimisations (KV cache, speculative decoding, disaggregation) or cheap training variants (LoRA, distillation).

## Example

A lab spends months and millions of dollars training a model once; every Telegram question Kenji sends to Hermes is a few seconds of inference.

## Narrower concepts

- [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md)
- [KV cache](/concepts/ai-ml/kv-cache.md)
- [Speculative decoding](/concepts/ai-ml/speculative-decoding.md)
- [Prefill-decode disaggregation](/concepts/ai-ml/prefill-decode-disaggregation.md)
- [Low-rank adaptation (LoRA)](/concepts/ai-ml/lora.md)
- [Distillation](/concepts/ai-ml/distillation.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 9 Inference Optimization, pp. 405-447 (inference overview p. 406; performance metrics p. 412): Inference as its own engineering problem: compute- vs memory-bound phases, TTFT/TPOT, accelerators.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 10 Inference Pipeline Deployment, pp. 355-400: Online, asynchronous and batch deployment types and their trade-offs.

## Sources

1. Goodfellow, I., Bengio, Y. and Courville, A. (2016). *Deep Learning*, ch. 6 and 8. MIT Press. <https://www.deeplearningbook.org/> — Forward pass, backpropagation and optimisation in training.
2. Pope, R. et al. (2023). *Efficiently Scaling Transformer Inference*. MLSys 2023, arXiv:2211.05102. <https://arxiv.org/abs/2211.05102> — Inference cost, latency and the prefill/decode distinction.
3. Google. *Machine Learning Glossary* - training, inference. <https://developers.google.com/machine-learning/glossary> — Practitioner definitions of both terms.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 9 Inference Optimization, pp. 405-447 (inference overview p. 406; performance metrics p. 412). Inference as its own engineering problem: compute- vs memory-bound phases, TTFT/TPOT, accelerators.
5. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 10 Inference Pipeline Deployment, pp. 355-400. Online, asynchronous and batch deployment types and their trade-offs.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
