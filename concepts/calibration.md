---
type: Concept
title: Calibration
description: How well a model's stated confidence matches its actual accuracy - of all answers given with 80% confidence, a calibrated model is right about 80% of the time.
tags:
- ai-ml
- mlops
domain: ai-ml
subdomain: mlops
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Guo, C. et al. (2017). On Calibration of Modern Neural Networks. ICML 2017, arXiv:1706.04599'
  resource: 'https://arxiv.org/abs/1706.04599'
  kind: paper
- title: 'Kadavath, S. et al. (2022). Language Models (Mostly) Know What They Know. arXiv:2207.05221'
  resource: 'https://arxiv.org/abs/2207.05221'
  kind: paper
- title: scikit-learn. Probability calibration (user guide 1.16)
  resource: 'https://scikit-learn.org/stable/modules/calibration.html'
  kind: docs
---

# Calibration

**Definition.** A model is calibrated when its predicted probabilities can be read as real frequencies. Calibration is separate from accuracy: a model can be highly accurate yet overconfident, or weak but honest about its uncertainty.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `mlops`. Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

Measure it by grouping predictions into confidence bins and comparing average confidence with observed accuracy in each bin, shown as a reliability diagram; Expected Calibration Error (ECE) is the weighted average gap. Guo et al. (2017) found that modern deep networks are systematically overconfident and that temperature scaling, one parameter fitted on held-out data, fixes much of it. Other methods are Platt scaling and isotonic regression. For LLMs, calibration can be measured on token probabilities or on verbalised confidence ("I am 70% sure"); Kadavath et al. found large models fairly calibrated on multiple-choice questions, while RLHF tuning tends to worsen it.

## Why it matters and trade-offs

Any decision that uses a score as a threshold (auto-approve above 0.9, escalate to a human below) needs calibrated scores, or the threshold means nothing. Calibration is measured on a distribution, so it can break under data drift, and a calibrated model can still be useless if it is uncertain about everything.

## Example

A weather forecaster who says "70% chance of rain" on 100 days is calibrated if it rains on about 70 of them.

## Sources

1. Guo, C. et al. (2017). *On Calibration of Modern Neural Networks*. ICML 2017, arXiv:1706.04599. <https://arxiv.org/abs/1706.04599> — Reliability diagrams, ECE, overconfidence of deep networks, temperature scaling.
2. Kadavath, S. et al. (2022). *Language Models (Mostly) Know What They Know*. arXiv:2207.05221. <https://arxiv.org/abs/2207.05221> — Calibration of large language models and self-evaluation of answers.
3. scikit-learn. *Probability calibration* (user guide 1.16). <https://scikit-learn.org/stable/modules/calibration.html> — Calibration curves, Platt (sigmoid) and isotonic calibration in practice.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
