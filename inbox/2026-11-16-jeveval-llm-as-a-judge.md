---
type: Handoff
url: https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU
title: LLM-as-a-Judge has a scoring problem.
source_type: linkedin
origin: forwarded
author: Sarthak Rastogi
published: 2026-11-09
---
LLM-as-a-Judge has a scoring problem.

Most custom LLM evaluation metrics ask a model to interpret vague criteria, make a judgment, generate a score, and explain its reasoning in one step.

That makes it difficult to understand exactly why a response scored 0.71 instead of 0.66 on another run.

JevEval takes a different approach: Jev-as-a-Judge.

Instead of asking an LLM to generate a verdict, you define explicit questions that Jev answers using calibrated probability distributions. DeepEval then converts those probabilities into scores using fixed, inspectable math.

There are three types of questions:

- Noul: Is a proposition true?
- Score: Where does the response fall on an ordered scale?
- Choice: Which behavior occurred?

You can combine them into custom metrics. For example, a tool-faithfulness metric can check whether an agent's claims came from tool outputs, whether it reported returned values accurately, and whether it presented unsupported information as fact.

Each question has its own weight, and you can inspect the probability distributions and score breakdown behind the final result. Strict mode also lets you turn evaluation criteria into hard acceptance tests.

JevEval supports multi-turn conversational evaluations too, making it useful for testing whether agents remember constraints, verify identity, follow workflows, and stay within their assigned roles.

The interesting idea here is that your evals become explicit specifications rather than prompts asking another LLM for an opinion.

Jev makes the decisions, and deterministic code calculates the score. No generated verdicts or explanations that can drift from the underlying result.

♻️ Share it with anyone building reliable AI agents :)

I share tutorials on how to build + improve AI apps and agents, on my newsletter AI Agent Engineering: https://lnkd.in/ehxEYRJY

#AI #LLMs #AIAgents
