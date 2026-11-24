---
type: Concept
title: Joint-embedding predictive architecture (JEPA)
description: Joint-Embedding Predictive Architecture - a self-supervised design that learns by predicting the abstract representation of a hidden part of the input from a visible part, instead of reconstructing raw pixels or tokens.
tags:
- ai-ml
- deep-learning
domain: ai-ml
subdomain: deep-learning
status: draft
generated:
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: LeCun, Y. (2022). A Path Towards Autonomous Machine Intelligence. OpenReview
  resource: 'https://openreview.net/forum?id=BZ5a1r-kVsf'
  kind: paper
- title: 'Assran, M. et al. (2023). Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture (I-JEPA). CVPR 2023, arXiv:2301.08243'
  resource: 'https://arxiv.org/abs/2301.08243'
  kind: paper
- title: 'Bardes, A. et al. (2024). Revisiting Feature Prediction for Learning Visual Representations from Video (V-JEPA). arXiv:2404.08471'
  resource: 'https://arxiv.org/abs/2404.08471'
  kind: paper
---

# Joint-embedding predictive architecture (JEPA)

**Definition.** A JEPA has two encoders and a predictor. One encoder embeds a visible "context" (part of an image or video); the other embeds a hidden "target"; the predictor learns to predict the target's embedding from the context's. Because the loss is computed in embedding space, the model can ignore unpredictable detail (exact texture, noise) and focus on structure and semantics. Yann LeCun proposed it in 2022 as a building block for world models.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `deep-learning`. Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

I-JEPA (images) masks large blocks of an image and predicts their representations; V-JEPA does the same across space and time in video. The target encoder is an exponential moving average of the context encoder, a trick that, with the predictor, prevents "collapse" (all inputs mapped to the same vector) without needing negative examples as in contrastive learning. After pre-training, the encoder is reused for downstream tasks, often with a light probe on top.

## Why it matters and trade-offs

JEPAs learn semantic features more efficiently than pixel-reconstruction methods such as masked autoencoders, and they are Meta's bet for models that understand the physical world. They are not generative: you cannot sample an image or text from them directly, and they are still mainly a research direction, not a production standard.

## Example

Show V-JEPA a video with a region masked out over several frames; it predicts an embedding meaning "a hand reaching for the cup", not the exact pixels.

## Review notes for Kenji

All three sources come from Meta researchers; independent replication exists but is less prominent. Good enough for a concept, not for a foundation.

## Sources

1. LeCun, Y. (2022). *A Path Towards Autonomous Machine Intelligence*. OpenReview. <https://openreview.net/forum?id=BZ5a1r-kVsf> — Original proposal of JEPA and why prediction in representation space matters.
2. Assran, M. et al. (2023). *Self-Supervised Learning from Images with a Joint-Embedding Predictive Architecture (I-JEPA)*. CVPR 2023, arXiv:2301.08243. <https://arxiv.org/abs/2301.08243> — Image JEPA, masking strategy, comparison with pixel reconstruction.
3. Bardes, A. et al. (2024). *Revisiting Feature Prediction for Learning Visual Representations from Video (V-JEPA)*. arXiv:2404.08471. <https://arxiv.org/abs/2404.08471> — Video JEPA trained purely on feature prediction.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
