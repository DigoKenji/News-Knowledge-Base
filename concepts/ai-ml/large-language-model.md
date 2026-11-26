---
type: Concept
title: Large language model and foundation model
description: A transformer model with billions of parameters, pre-trained on vast text to predict the next token and then tuned to follow instructions; a foundation model is the broader category of large pre-trained models adapted to many downstream tasks.
tags:
- ai-ml
- llm-engineering
- core
domain: ai-ml
subdomain: llm-engineering
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: 'Bommasani, R. et al. (2021). On the Opportunities and Risks of Foundation Models. Stanford CRFM, arXiv:2108.07258'
  resource: 'https://arxiv.org/abs/2108.07258'
  kind: paper
- title: 'Zhao, W. X. et al. (2023). A Survey of Large Language Models. arXiv:2303.18223'
  resource: 'https://arxiv.org/abs/2303.18223'
  kind: paper
- title: NIST (2024). AI 600-1 - Artificial Intelligence Risk Management Framework - Generative Artificial Intelligence Profile
  resource: 'https://doi.org/10.6028/NIST.AI.600-1'
  kind: standard
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 1 From Language Models to Foundation Models, pp. 2-16; ch. 2 Understanding Foundation Models, pp. 49-111'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 1 The LLM Twin Concept and Architecture, pp. 1-24'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Large language model and foundation model

**Definition.** An LLM learns language, facts and reasoning patterns as a by-product of predicting the next token over trillions of tokens (self-supervised pre-training). Post-training (supervised fine-tuning on instructions, RLHF and similar) turns it into an assistant. Stanford's CRFM coined "foundation model" (2021) for models trained on broad data at scale that can be adapted to a wide range of tasks; LLMs are the main example, and many are now multimodal.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

LLMs are adapted without retraining through prompting and in-context learning, grounded with retrieval (RAG), customised cheaply with LoRA or distillation, and given tools to act as agents. Their known weaknesses are hallucination (fluent but false output), sensitivity to prompts, knowledge cut-offs, cost, and security issues such as prompt injection; NIST's generative AI profile lists these risks.

## Example

Claude, GPT, Gemini and Llama are LLMs; the same base model can write a summary, translate to Portuguese or classify an article depending only on the prompt.

## Narrower concepts

- [In-context learning](/concepts/ai-ml/in-context-learning.md)
- [Low-rank adaptation (LoRA)](/concepts/ai-ml/lora.md)
- [Distillation](/concepts/ai-ml/distillation.md)
- [Retrieval-augmented generation](/concepts/ai-ml/rag.md)
- [AI agent](/concepts/ai-ml/ai-agent.md)
- [LLM-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 1 From Language Models to Foundation Models, pp. 2-16; ch. 2 Understanding Foundation Models, pp. 49-111: Training data, architecture and size, post-training and sampling - why outputs are probabilistic.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 1 The LLM Twin Concept and Architecture, pp. 1-24: An LLM product designed end to end.

## Sources

1. Bommasani, R. et al. (2021). *On the Opportunities and Risks of Foundation Models*. Stanford CRFM, arXiv:2108.07258. <https://arxiv.org/abs/2108.07258> — Definition of foundation models, adaptation, risks.
2. Zhao, W. X. et al. (2023). *A Survey of Large Language Models*. arXiv:2303.18223. <https://arxiv.org/abs/2303.18223> — Pre-training, post-training, capabilities and evaluation of LLMs.
3. NIST (2024). *AI 600-1 - Artificial Intelligence Risk Management Framework - Generative Artificial Intelligence Profile*. <https://doi.org/10.6028/NIST.AI.600-1> — Government-standard description of generative AI and its risks.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 1 From Language Models to Foundation Models, pp. 2-16; ch. 2 Understanding Foundation Models, pp. 49-111. Training data, architecture and size, post-training and sampling - why outputs are probabilistic.
5. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 1 The LLM Twin Concept and Architecture, pp. 1-24. An LLM product designed end to end.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
