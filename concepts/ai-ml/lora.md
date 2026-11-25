---
type: Concept
title: Low-rank adaptation (LoRA)
description: Low-Rank Adaptation - a parameter-efficient fine-tuning method that freezes a model's weights and trains small low-rank matrices added to some layers, cutting trainable parameters by orders of magnitude with little quality loss.
tags:
- ai-ml
domain: ai-ml
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Hu, E. J. et al. (2022). LoRA - Low-Rank Adaptation of Large Language Models. ICLR 2022, arXiv:2106.09685'
  resource: 'https://arxiv.org/abs/2106.09685'
  kind: paper
- title: 'Aghajanyan, A. et al. (2021). Intrinsic Dimensionality Explains the Effectiveness of Language Model Fine-Tuning. ACL 2021, arXiv:2012.13255'
  resource: 'https://arxiv.org/abs/2012.13255'
  kind: paper
- title: 'Dettmers, T. et al. (2023). QLoRA - Efficient Finetuning of Quantized LLMs. NeurIPS 2023, arXiv:2305.14314'
  resource: 'https://arxiv.org/abs/2305.14314'
  kind: paper
- title: Hugging Face. PEFT - LoRA conceptual guide
  resource: 'https://huggingface.co/docs/peft/main/en/conceptual_guides/lora'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 7 Parameter-Efficient Finetuning, pp. 332-347'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 5 LoRA p. 213 and QLoRA p. 215'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Low-rank adaptation (LoRA)

**Definition.** LoRA adapts a large pre-trained model to a new task by learning only a low-rank update to selected weight matrices. Instead of changing a d×d weight matrix W, it learns two thin matrices B (d×r) and A (r×d) with a small rank r (for example 8), and uses W + BA at inference. The original weights stay frozen and shared.

Domain: [ai-ml](/domains/ai-ml.md). Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

The idea rests on the finding that the changes needed to adapt a model have low "intrinsic dimension". Hu et al. applied LoRA to the attention projections of GPT-3 175B and reduced trainable parameters about 10,000 times and GPU memory about 3 times, matching full fine-tuning quality. The adapter can be merged into W after training, so there is no extra inference latency, or kept separate so one base model serves many swappable adapters. QLoRA adds 4-bit quantisation of the frozen base, which made fine-tuning a 65B model possible on one 48 GB GPU.

## Why it matters and trade-offs

LoRA makes fine-tuning affordable and adapters tiny (megabytes), so it is the default way to customise open models. It can underperform full fine-tuning when the new task is far from pre-training (for example a new language or heavy continued pre-training), and rank and target layers are hyperparameters to tune.

## Example

Train a 20 MB LoRA adapter that teaches an open 8B model Kenji's deep-dive note format, and load it on top of the shared base only when summarising.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 7 Parameter-Efficient Finetuning, pp. 332-347: LoRA among PEFT methods, with memory maths.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 5 LoRA p. 213 and QLoRA p. 215: Practical LoRA/QLoRA settings for fine-tuning an 8B model.

## Sources

1. Hu, E. J. et al. (2022). *LoRA - Low-Rank Adaptation of Large Language Models*. ICLR 2022, arXiv:2106.09685. <https://arxiv.org/abs/2106.09685> — Method, parameter and memory savings, no added inference latency.
2. Aghajanyan, A. et al. (2021). *Intrinsic Dimensionality Explains the Effectiveness of Language Model Fine-Tuning*. ACL 2021, arXiv:2012.13255. <https://arxiv.org/abs/2012.13255> — Why low-rank updates are enough.
3. Dettmers, T. et al. (2023). *QLoRA - Efficient Finetuning of Quantized LLMs*. NeurIPS 2023, arXiv:2305.14314. <https://arxiv.org/abs/2305.14314> — LoRA on a 4-bit quantized base model.
4. Hugging Face. *PEFT - LoRA conceptual guide*. <https://huggingface.co/docs/peft/main/en/conceptual_guides/lora> — Practical parameters (rank, alpha, target modules) and merging.
5. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 7 Parameter-Efficient Finetuning, pp. 332-347. LoRA among PEFT methods, with memory maths.
6. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 5 LoRA p. 213 and QLoRA p. 215. Practical LoRA/QLoRA settings for fine-tuning an 8B model.

## Relations
- (none yet)

## Notes
- [LiveLoRA: Exact KV Cache Migration Across LoRA Updates for LLM Agents](/notes/ai-ml/2026-11-24-livelora-exact-kv-cache-migration-across-lora-updates-for-ll.md)
