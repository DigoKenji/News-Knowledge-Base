---
type: Concept
title: Large language model and foundation model
description: A transformer model with billions of parameters, pre-trained on vast text to predict the next token and then tuned to follow instructions; a foundation model is the broader category of large pre-trained models adapted to many downstream tasks.
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
- title: 'Bommasani, R. et al. (2021). On the Opportunities and Risks of Foundation Models. Stanford CRFM, arXiv:2108.07258'
  resource: 'https://arxiv.org/abs/2108.07258'
  kind: paper
- title: 'Zhao, W. X. et al. (2023). A Survey of Large Language Models. arXiv:2303.18223'
  resource: 'https://arxiv.org/abs/2303.18223'
  kind: paper
- title: NIST (2024). AI 600-1 - Artificial Intelligence Risk Management Framework - Generative Artificial Intelligence Profile
  resource: 'https://doi.org/10.6028/NIST.AI.600-1'
  kind: standard
---

# Large language model and foundation model

**Definition.** An LLM learns language, facts and reasoning patterns as a by-product of predicting the next token over trillions of tokens (self-supervised pre-training). Post-training (supervised fine-tuning on instructions, RLHF and similar) turns it into an assistant. Stanford's CRFM coined "foundation model" (2021) for models trained on broad data at scale that can be adapted to a wide range of tasks; LLMs are the main example, and many are now multimodal.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

LLMs are adapted without retraining through prompting and in-context learning, grounded with retrieval (RAG), customised cheaply with LoRA or distillation, and given tools to act as agents. Their known weaknesses are hallucination (fluent but false output), sensitivity to prompts, knowledge cut-offs, cost, and security issues such as prompt injection; NIST's generative AI profile lists these risks.

## Example

Claude, GPT, Gemini and Llama are LLMs; the same base model can write a summary, translate to Portuguese or classify an article depending only on the prompt.

## Narrower concepts

- [In-context learning](/concepts/in-context-learning.md)
- [Low-rank adaptation (LoRA)](/concepts/lora.md)
- [Distillation](/concepts/distillation.md)
- [Retrieval-augmented generation](/concepts/rag.md)
- [AI agent](/concepts/ai-agent.md)
- [LLM-as-a-judge](/concepts/llm-as-a-judge.md)

## Sources

1. Bommasani, R. et al. (2021). *On the Opportunities and Risks of Foundation Models*. Stanford CRFM, arXiv:2108.07258. <https://arxiv.org/abs/2108.07258> — Definition of foundation models, adaptation, risks.
2. Zhao, W. X. et al. (2023). *A Survey of Large Language Models*. arXiv:2303.18223. <https://arxiv.org/abs/2303.18223> — Pre-training, post-training, capabilities and evaluation of LLMs.
3. NIST (2024). *AI 600-1 - Artificial Intelligence Risk Management Framework - Generative Artificial Intelligence Profile*. <https://doi.org/10.6028/NIST.AI.600-1> — Government-standard description of generative AI and its risks.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
