---
type: Concept
title: Position bias
description: A systematic tendency of a model, especially when acting as a judge or ranker, to favour an option because of where it appears (first, last, or in a given slot) rather than because of its content.
tags:
- ai-ml
- mlops
domain: ai-ml
subdomain: mlops
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Wang, P. et al. (2024). Large Language Models are not Fair Evaluators. ACL 2024, arXiv:2305.17926'
  resource: 'https://arxiv.org/abs/2305.17926'
  kind: paper
- title: 'Zheng, L. et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. NeurIPS 2023, arXiv:2306.05685'
  resource: 'https://arxiv.org/abs/2306.05685'
  kind: paper
- title: 'Shi, L. et al. (2024). Judging the Judges - A Systematic Study of Position Bias in LLM-as-a-Judge. arXiv:2406.07791'
  resource: 'https://arxiv.org/abs/2406.07791'
  kind: paper
---

# Position bias

**Definition.** Position bias is present when swapping the order of the candidates changes the verdict. In LLM-as-a-judge pairwise comparisons it shows up as a preference for answer A or B regardless of quality; in multiple choice, as a preference for certain letters; in long contexts, as better use of information at the start and end than in the middle.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `mlops`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Measure it by running each comparison twice with positions swapped: consistency is the share of pairs with the same verdict both ways. Wang et al. showed that simply swapping the order let a weaker model "beat" ChatGPT on most of a benchmark when judged by GPT-4. Bias is strongest when the two answers are close in quality. Mitigations: evaluate both orders and count a win only if consistent (otherwise a tie), randomise order across a dataset, ask the judge to reason before deciding, and use several judges.

## Why it matters and trade-offs

Unchecked position bias makes automated evaluations and rankings wrong in a way that looks precise. Swapping doubles evaluation cost, and order is only one of several judge biases (verbosity, self-preference).

## Example

Ask a judge "is summary A or B better?" and it says A; swap them and it still says the first one. The verdict tells you about the slot, not the summaries.

## Sources

1. Wang, P. et al. (2024). *Large Language Models are not Fair Evaluators*. ACL 2024, arXiv:2305.17926. <https://arxiv.org/abs/2305.17926> — Order swapping flips verdicts; balanced position calibration.
2. Zheng, L. et al. (2023). *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena*. NeurIPS 2023, arXiv:2306.05685. <https://arxiv.org/abs/2306.05685> — Position bias measured with consistency rates across judges.
3. Shi, L. et al. (2024). *Judging the Judges - A Systematic Study of Position Bias in LLM-as-a-Judge*. arXiv:2406.07791. <https://arxiv.org/abs/2406.07791> — Systematic metrics (repetition stability, position consistency, preference fairness) across many judges.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
