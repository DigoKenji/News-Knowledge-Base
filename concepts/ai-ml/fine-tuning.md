---
type: Concept
title: 'Fine-tuning'
description: 'Further training a pre-trained model on a smaller, task-specific dataset - supervised fine-tuning on examples, then optional preference alignment (RLHF, DPO) - to change its behaviour or format rather than to add fresh facts.'
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 2 Post-Training (SFT p. 80, preference finetuning p. 83); ch. 7 Finetuning, pp. 307-361 (Finetuning and RAG p. 316; PEFT p. 332); ch. 8 Dataset Engineering, p. 363'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 5 Supervised Fine-Tuning, pp. 177-228 (full fine-tuning p. 211, LoRA p. 213, QLoRA p. 215); ch. 6 Preference Alignment and DPO, pp. 229-260'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 2 Fine-Tuning Transformers, p. 45; ch. 9 Fine-Tuning a Language Model / a Classifier, pp. 289-297'
- title: 'Ouyang, L. et al. (2022). Training language models to follow instructions with human feedback. NeurIPS 2022'
  resource: 'https://arxiv.org/abs/2203.02155'
  kind: paper
- title: 'Rafailov, R. et al. (2023). Direct Preference Optimization: Your Language Model is Secretly a Reward Model. NeurIPS 2023'
  resource: 'https://arxiv.org/abs/2305.18290'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Fine-tuning

**Definition.** Fine-tuning is transfer learning: start from a model that already learned general patterns and adjust its weights for a narrower goal. For LLMs, Huyen separates post-training into supervised fine-tuning (SFT, training on prompt-response pairs so the model follows instructions) and preference fine-tuning (teaching it which of two answers people prefer, via RLHF or Direct Preference Optimization). Full fine-tuning updates every weight and needs a lot of memory; parameter-efficient fine-tuning (PEFT) such as LoRA or QLoRA trains a small number of extra parameters and gets close results far more cheaply. Her rule of thumb: "finetuning is for form, and RAG is for facts" (Huyen, p. 317).

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Both books treat fine-tuning as a later step, not a first one: try prompting, then RAG, and fine-tune when you need a consistent style or output format, a smaller and cheaper model that matches a bigger one on one task, or domain behaviour that prompts cannot reach (Huyen ch. 7 "When to Finetune"; Iusztin and Labonne ch. 5). The real cost is the dataset: quality and coverage of examples matter more than quantity (Huyen ch. 8). Fine-tuned models also need re-evaluation for regressions and safety.

## Example

Project_News could fine-tune a small open model on a few hundred approved deep-dive notes so it writes in the house format, while current article facts still arrive through retrieval.

## Broader concepts

- [Large language model and foundation model](/concepts/ai-ml/large-language-model.md)

## Narrower concepts

- [Low-rank adaptation (LoRA)](/concepts/ai-ml/lora.md)
- [Distillation](/concepts/ai-ml/distillation.md)

## Related concepts

- [Retrieval-augmented generation](/concepts/ai-ml/rag.md)
- [Quantization](/concepts/ai-ml/quantization.md)

## Sources

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 2 Post-Training (SFT p. 80, preference finetuning p. 83); ch. 7 Finetuning, pp. 307-361 (Finetuning and RAG p. 316; PEFT p. 332); ch. 8 Dataset Engineering, p. 363. When and when not to fine-tune, memory maths, PEFT, model merging, and data curation.
2. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 5 Supervised Fine-Tuning, pp. 177-228 (full fine-tuning p. 211, LoRA p. 213, QLoRA p. 215); ch. 6 Preference Alignment and DPO, pp. 229-260. End-to-end SFT and DPO on an 8B model, including instruction and preference dataset creation.
3. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 2 Fine-Tuning Transformers, p. 45; ch. 9 Fine-Tuning a Language Model / a Classifier, pp. 289-297. Classic encoder fine-tuning for classification and with few labels.
4. Ouyang, L. et al. (2022). *Training language models to follow instructions with human feedback*. NeurIPS 2022. <https://arxiv.org/abs/2203.02155> — SFT + RLHF recipe behind instruction-following models.
5. Rafailov, R. et al. (2023). *Direct Preference Optimization: Your Language Model is Secretly a Reward Model*. NeurIPS 2023. <https://arxiv.org/abs/2305.18290> — Direct Preference Optimization.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
