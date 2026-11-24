---
type: Concept
title: LLM-as-a-judge
description: Using a strong language model, with a rubric, to score or compare the outputs of another model - a scalable stand-in for human evaluation that must itself be validated against human judgments.
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
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Zheng, L. et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. NeurIPS 2023, arXiv:2306.05685'
  resource: 'https://arxiv.org/abs/2306.05685'
  kind: paper
- title: 'Liu, Y. et al. (2023). G-Eval - NLG Evaluation using GPT-4 with Better Human Alignment. EMNLP 2023, arXiv:2303.16634'
  resource: 'https://arxiv.org/abs/2303.16634'
  kind: paper
- title: 'Gu, J. et al. (2024). A Survey on LLM-as-a-Judge. arXiv:2411.15594'
  resource: 'https://arxiv.org/abs/2411.15594'
  kind: paper
---

# LLM-as-a-judge

**Definition.** LLM-as-a-judge is an evaluation method in which a model receives a task, one or more candidate answers and grading criteria, and returns a score, a ranking or a preference. It is used where exact-match metrics fail: open-ended answers, summaries, helpfulness, tone.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `mlops`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Three setups: single-answer grading (score 1-10 against a rubric), pairwise comparison (which of A or B is better) and reference-guided grading (compare with a gold answer). Zheng et al. found that GPT-4 as a judge agreed with human experts about as often as humans agreed with each other (over 80%) on MT-Bench. Known biases: position bias (favours the first or second answer), verbosity bias (favours longer answers), self-preference (favours its own style) and limited ability on maths or code it cannot verify. Mitigations: swap positions and average, use chain-of-thought rubrics (G-Eval), several judges, and calibrate against a human-labelled sample.

## Why it matters and trade-offs

It makes evaluation of LLM apps cheap and continuous, enabling regression tests on prompts and models. But a judge is a model with its own errors; treat its scores as a measurement that needs validation, not as ground truth, and never let the same model be judge and contestant unchecked.

## Example

Before changing the summarizer prompt, run 50 articles through old and new prompts and ask a judge model which summary follows the deep-dive rubric better, with positions swapped.

## Sources

1. Zheng, L. et al. (2023). *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena*. NeurIPS 2023, arXiv:2306.05685. <https://arxiv.org/abs/2306.05685> — The term, agreement with humans, and position, verbosity and self-enhancement biases.
2. Liu, Y. et al. (2023). *G-Eval - NLG Evaluation using GPT-4 with Better Human Alignment*. EMNLP 2023, arXiv:2303.16634. <https://arxiv.org/abs/2303.16634> — Rubric plus chain-of-thought judging and its correlation with humans.
3. Gu, J. et al. (2024). *A Survey on LLM-as-a-Judge*. arXiv:2411.15594. <https://arxiv.org/abs/2411.15594> — Taxonomy of setups, biases and reliability practices.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
