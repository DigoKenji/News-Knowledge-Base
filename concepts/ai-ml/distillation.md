---
type: Concept
title: Distillation
description: Training a smaller or cheaper student model to reproduce the outputs of a larger teacher model, so the student keeps much of the teacher's ability at a fraction of the cost.
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
  at: '2026-11-18T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Hinton, G., Vinyals, O. and Dean, J. (2015). Distilling the Knowledge in a Neural Network. arXiv:1503.02531'
  resource: 'https://arxiv.org/abs/1503.02531'
  kind: paper
- title: Bucilă, C., Caruana, R. and Niculescu-Mizil, A. (2006). Model Compression. KDD 2006
  resource: 'https://dl.acm.org/doi/10.1145/1150402.1150464'
  kind: paper
- title: 'Sanh, V. et al. (2019). DistilBERT, a distilled version of BERT. arXiv:1910.01108'
  resource: 'https://arxiv.org/abs/1910.01108'
  kind: paper
- title: 'Xu, X. et al. (2024). A Survey on Knowledge Distillation of Large Language Models. arXiv:2402.13116'
  resource: 'https://arxiv.org/abs/2402.13116'
  kind: paper
---

# Distillation

**Definition.** Knowledge distillation transfers what a teacher model has learned into a student model by training the student on the teacher's outputs instead of (or in addition to) the original labels. Hinton et al. showed that the teacher's full probability distribution ("soft targets") carries more information than the single correct label, for example that a picture of a 2 looks a bit like a 3.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Classic distillation minimises the difference between student and teacher output distributions, softened with a temperature, plus the normal loss on true labels. For LLMs the common forms are logit distillation (match token probabilities, which needs access to the teacher's logits) and sequence-level or "black-box" distillation (fine-tune the student on text the teacher generated, which only needs API access). DistilBERT kept about 97% of BERT's language-understanding performance with 40% fewer parameters and 60% faster inference.

## Why it matters and trade-offs

Distillation is how vendors ship small, fast models (the "mini" and "flash" tiers) and how teams build cheap task-specific models. Students rarely match teachers on hard reasoning, inherit the teacher's errors and biases, and are capped by the quality of the generated data. Distilling a competitor's model through its API usually breaks its terms of service; providers call this adversarial distillation and try to detect it.

## Example

Generate 50,000 labelled support tickets with a frontier model, then fine-tune a 3-billion-parameter model on them to classify tickets in production for a fraction of the cost.

## Review notes for Kenji

The draft ("extract capabilities") described the adversarial case only; the definition is now the general technique, with the adversarial use as a caveat.

## Sources

1. Hinton, G., Vinyals, O. and Dean, J. (2015). *Distilling the Knowledge in a Neural Network*. arXiv:1503.02531. <https://arxiv.org/abs/1503.02531> — Soft targets, temperature, the teacher-student method.
2. Bucilă, C., Caruana, R. and Niculescu-Mizil, A. (2006). *Model Compression*. KDD 2006. <https://dl.acm.org/doi/10.1145/1150402.1150464> — The earlier idea of compressing an ensemble into one small model trained on its outputs.
3. Sanh, V. et al. (2019). *DistilBERT, a distilled version of BERT*. arXiv:1910.01108. <https://arxiv.org/abs/1910.01108> — Concrete size, speed and quality figures for a distilled language model.
4. Xu, X. et al. (2024). *A Survey on Knowledge Distillation of Large Language Models*. arXiv:2402.13116. <https://arxiv.org/abs/2402.13116> — White-box vs black-box distillation for LLMs.

## Relations
- (none yet)

## Notes
- [Introducing Claude Fable 5.2 and Claude Mythos 5.2](/notes/ai-ml/2026-11-18-introducing-claude-fable-5-2-and-claude-mythos-5-2.md)
