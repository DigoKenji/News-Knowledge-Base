---
type: Concept
title: In-context learning
description: A language model's ability to pick up a task from instructions or examples placed in its prompt and perform it on new inputs, without any change to its weights.
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
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Brown, T. et al. (2020). Language Models are Few-Shot Learners. NeurIPS 2020, arXiv:2005.14165'
  resource: 'https://arxiv.org/abs/2005.14165'
  kind: paper
- title: 'Dong, Q. et al. (2024). A Survey on In-context Learning. EMNLP 2024, arXiv:2301.00234'
  resource: 'https://arxiv.org/abs/2301.00234'
  kind: paper
- title: 'Min, S. et al. (2022). Rethinking the Role of Demonstrations - What Makes In-Context Learning Work? EMNLP 2022, arXiv:2202.12837'
  resource: 'https://arxiv.org/abs/2202.12837'
  kind: paper
- title: 'Olsson, C. et al. (2022). In-context Learning and Induction Heads. Transformer Circuits Thread, arXiv:2209.11895'
  resource: 'https://arxiv.org/abs/2209.11895'
  kind: paper
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 5 In-Context Learning: Zero-Shot and Few-Shot, pp. 213-215'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 9 In-Context and Few-Shot Learning with Prompts, p. 288'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# In-context learning

**Definition.** In-context learning (ICL) is learning at inference time from the context alone. Given a few input-output demonstrations (few-shot) or just a description (zero-shot), the model infers the pattern and applies it to the next input. Nothing is trained; remove the examples and the skill is gone.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

GPT-3 (Brown et al., 2020) made ICL famous by showing that few-shot performance grows with model scale. Why it works is still studied: Olsson et al. linked it to "induction heads", attention circuits that copy patterns seen earlier in the context; other work frames it as implicit Bayesian inference or as the model running a learning algorithm in its forward pass. Min et al. found a surprising result: the labels in demonstrations matter less than expected, while the format, the input distribution and the label space matter a lot.

## Why it matters and trade-offs

ICL lets you adapt a model in minutes with no training pipeline, which is why prompting with examples is the first tool to try. It is sensitive to example choice and order, uses context tokens on every call (cost and latency), and for large, stable tasks fine-tuning or LoRA usually beats it.

## Example

Prompt "great movie → positive; boring plot → negative; loved the cast →" and the model answers "positive".

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 5 In-Context Learning: Zero-Shot and Few-Shot, pp. 213-215: How shots in the prompt substitute for training examples.
- **Tunstall, L., von Werra, L. and Wolf, T. (2022)**, *Natural Language Processing with Transformers, revised edition*, ch. 9 In-Context and Few-Shot Learning with Prompts, p. 288: Few-shot prompting compared with fine-tuning when labels are scarce.

## Sources

1. Brown, T. et al. (2020). *Language Models are Few-Shot Learners*. NeurIPS 2020, arXiv:2005.14165. <https://arxiv.org/abs/2005.14165> — Zero-, one- and few-shot in-context learning and its growth with scale.
2. Dong, Q. et al. (2024). *A Survey on In-context Learning*. EMNLP 2024, arXiv:2301.00234. <https://arxiv.org/abs/2301.00234> — Definition, demonstration design and explanations of ICL.
3. Min, S. et al. (2022). *Rethinking the Role of Demonstrations - What Makes In-Context Learning Work?* EMNLP 2022, arXiv:2202.12837. <https://arxiv.org/abs/2202.12837> — Format and input distribution matter more than correct labels.
4. Olsson, C. et al. (2022). *In-context Learning and Induction Heads*. Transformer Circuits Thread, arXiv:2209.11895. <https://arxiv.org/abs/2209.11895> — A mechanistic explanation of ICL.
5. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 5 In-Context Learning: Zero-Shot and Few-Shot, pp. 213-215. How shots in the prompt substitute for training examples.
6. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 9 In-Context and Few-Shot Learning with Prompts, p. 288. Few-shot prompting compared with fine-tuning when labels are scarce.

## Relations
- (none yet)

## Notes
- [AI on Excel Sheet: NVIDIA's Kumo Runs 23x Faster Than Google's TabFM](/notes/data/2026-11-20-ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-googles.md)
