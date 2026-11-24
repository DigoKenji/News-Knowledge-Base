---
type: Post Summary
title: LLM-as-a-Judge has a scoring problem (JevEval)
description: JevEval swaps one-shot LLM judge scores for explicit questions answered as calibrated probabilities, with fixed code computing an inspectable score.
resource: https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU
tags:
- ai-ml
- mlops
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU
  title: LLM-as-a-Judge has a scoring problem (JevEval)
  author: Sarthak Rastogi
status: stable
source_url: https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU
source: LinkedIn (forwarded)
source_type: linkedin
published: '2026-11-09'
domain: ai-ml
subdomain: mlops
concepts:
- llm-as-a-judge
selected_by: forwarded
content: handoff
notion_url: https://app.notion.com/p/41abaf74172d8100b77ec67af665737b
---

# LLM-as-a-Judge Has a Scoring Problem (JevEval)

**Source:** https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU (LinkedIn post)
**Author:** Sarthak Rastogi
**Date:** 2026-11-09
**Tags:** #llm-evaluation #agents #deepeval #format-linkedin-post

---

## What This Is About

The post argues that most custom [LLM-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md) metrics ask one model call to do everything at once: interpret vague criteria, make a judgment, generate a score, and explain the reasoning. That is why the same response can score 0.71 on one run and 0.66 on another, with no clear reason. JevEval (the post calls the approach "Jev-as-a-Judge") is the proposed fix. You write the evaluation as explicit questions, the judge model answers them with calibrated probabilities, and fixed code turns those probabilities into a score.

## How It Works

The post describes three question types:

- **Noul:** is a proposition true? (spelled "Noul" in the post)
- **Score:** where does the response fall on an ordered scale?
- **Choice:** which behavior occurred?

Questions are combined into custom metrics, and each question has its own weight. The example is a tool-faithfulness metric that checks whether an agent's claims came from tool outputs, whether returned values were reported accurately, and whether unsupported information was presented as fact. You can inspect the probability distributions and the score breakdown behind each result. Strict mode turns criteria into hard acceptance tests. Multi-turn conversational evals are supported too, for checking that agents keep constraints, verify identity, follow workflows, and stay in their assigned roles.

> The key idea: the evals become explicit specifications instead of prompts asking another LLM for an opinion. The judge makes the decisions, deterministic code computes the score, and there are no generated verdicts or explanations that can drift away from the result.

## Patterns & Techniques Spotted

- **Decomposed rubric:** a vague criterion is split into small, separately answerable questions, each with its own weight.
- **Probabilities in, fixed math out:** the model supplies calibrated probabilities and the scoring is inspectable arithmetic, so judgment and computation are separated.
- **Hard gates:** strict mode treats a criterion as a pass/fail acceptance test rather than one soft contribution to a score.

## Key Takeaways

- A score from a single free-text judge call is hard to debug. Breaking the criteria into explicit questions makes the failure visible.
- Keeping judgment (the model) separate from score calculation (code) means the explanation cannot drift away from the number.
- Treat evals as specifications. A metric you can read question by question is easier to trust and change than a judging prompt.

## Follow-up Reading

- 🔗 Sarthak Rastogi, *AI Agent Engineering* newsletter — https://lnkd.in/ehxEYRJY → The author's tutorials on building and improving AI apps and agents, linked from the post.
- 📚 Zheng et al., 2023, *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena* (NeurIPS 2023) → The standard study of LLM judges and their biases, useful background for the reliability problem this post targets.

---
*Summary generated: 2026-11-16*
