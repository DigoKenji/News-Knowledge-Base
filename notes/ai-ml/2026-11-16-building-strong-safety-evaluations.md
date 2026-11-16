---
type: Article Summary
title: Building strong safety evaluations
description: 'Anthropic''s guide: a safety claim is only as good as its eval, so measure the right construct, validate LLM graders against humans and report uncertainty.'
resource: https://www.anthropic.com/research/building-strong-safety-evaluations
tags:
- ai-ml
- ml-governance
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://www.anthropic.com/research/building-strong-safety-evaluations
  title: Building strong safety evaluations
  author: Anthropic
status: stable
source_url: https://www.anthropic.com/research/building-strong-safety-evaluations
source: Anthropic News
source_type: blog
published: '2026-11-13'
domain: ai-ml
subdomain: ml-governance
concepts:
- llm-as-a-judge
selected_by: discovery
content: open
notion_url: https://app.notion.com/p/41abaf74172d810a8885c55d50d4cd8a
---

# Building Strong Safety Evaluations

**Source:** https://www.anthropic.com/research/building-strong-safety-evaluations
**Author:** Ruth Appel and colleagues (Anthropic)
**Date:** 2026-11-13
**Tags:** #ai-safety #evals #methodology #anthropic

---

## 💬 What This Is About
Anthropic's practical guide to building safety evals, the tests behind claims like "this model is safer than its predecessor." The authors argue that such a claim is only as sound as its eval, and they borrow reforms from psychology's replication crisis (preregistration, better measurement, stronger statistics) to share lessons from building evals at Anthropic.

## Safety first
A safety eval measures a model's propensity to cause or contribute to harm, not a capability. The core question is "Given a specific input, did the model do the right thing?" Evals are easy to game and often too poorly documented to reproduce.

## Build a team with the right skills
Methods experts decide how to sample, grade and analyze. Domain experts (psychologists, biologists, political scientists) decide what counts as harm and what a good response looks like. Missing either group yields evals that are rigorous but narrow, or deep but poorly measured.

## Run evals safely
Run safety evals in a sandbox with no internet access, with real-time monitoring so a run can be stopped. After its own incidents, Anthropic hardened its environments and paused many evals that give models internet access.

## Design

### Match the design to the question
Most evals are observational: they show that one model scored differently, not why. Causal claims need a design that varies one thing at a time, such as the same model with and without a system prompt change.

### Measure what you mean to measure
This is construct validity. Refusal is the obvious construct, but for suicide and self-harm it is often the wrong one: a model that refuses everything would ace a refusal-rate eval and still not be safer. Anthropic reports an appropriate response rate, judged against expert rubrics. In dual-use fields like biology, the right answer engages with the legitimate request and withholds operational specifics.

> 💡 Refusal is often the wrong construct. Measure whether the response was appropriate, not whether it declined.

## Make sure the results generalize

### Capture nuance via multi-turn simulations
Single-turn prompts saturate and miss gradual escalation. Distress accumulates, so an early disclosure changes how a later, ordinary-looking question should be handled.

### Design scenarios that model a range of conditions
Experts write a specification (goals, tactics, persona, tone). The first user turn is fixed, a separate Claude model plays later user turns, the tested model answers, and each scenario runs several times with different trait combinations. A grader scores each transcript. Vary dimensions independently so their effects can be separated.

### Take real-world interaction patterns into account
Build scenarios from observed usage, red-team transcripts and incident reports, and keep revising them. Cover languages, product surfaces and the harness a model runs in, since tools and system prompts change behavior.

## Run

### Keep measures internally valid
Avoid contamination (use data newer than the training cutoff), close shortcuts such as reward hacking, and randomize details like answer order. Single-turn suites sit at 94% to 98% harmless response rates, so make saturated evals harder while preserving the construct, not with trick questions.

### Carefully calibrate your grader
Most modern evals use another model as an [LLM-as-a-judge](/concepts/llm-as-a-judge.md), since no human team can read every transcript. Graders can fail silently, so the post recommends:

- Use atomic yes/no criteria rather than one holistic score, and state what does not meet each one.
- Keep the grader blind to design metadata, prefer a different model than the one tested, and check for length and position bias.
- Fail loudly: flag and exclude items the grader breaks on.
- Validate against human labels with chance-corrected agreement.

### Preregister
Write down metrics, comparisons and the threshold for an improvement before seeing results. Use 100 items as a floor for complex multi-turn evals and 1,000 for simple single-turn ones.

## Report

### Report statistics fully
Break results out by category and always report uncertainty. Multi-turn intervals can exceed ±10 points. In one romance-scam eval, Claude Opus 5 and Opus 5.5 scored 98% and 94%, but the gap was not statistically significant, so no winner could be declared.

### Document everything
Report the full configuration. For Claude Fable 5.1, the appropriate response rate differed by up to 34 points between the raw API and claude.ai, where the system prompt applies. Record versions, settings, prompts and grader version, and never compare scores from different graders.

## Disclose flaws responsibly
Reviewing self-harm transcripts, Anthropic saw Claude sometimes implicitly validate self-harm as a coping strategy. A system prompt instruction against it raised the cited appropriate response rate on claude.ai to 94%, from 60% on the API. The lesson is to run evals continuously during fine-tuning, and to tell developers about severe flaws and allow time for a fix before publishing.

## Patterns & Techniques Spotted
- **Fixed-opener multi-turn simulation:** a shared first turn and a separate model playing the user, so scenarios diverge realistically yet stay comparable.
- **Paired harmful/benign prompts:** each topic gets both kinds, so over-refusal is measured alongside compliance.
- **Atomic binary rubrics:** many yes/no criteria instead of one score, which makes grader behavior easier to inspect.

## Key Takeaways
- Write down the construct before collecting data; most eval failures start with measuring the wrong thing.
- Refusal rate is a poor safety proxy in sensitive areas, where a flat refusal can itself be a failure.
- Trust a grader only once it agrees with experts about as often as experts agree with each other.
- Report confidence intervals; a difference inside them is noise, not a win.

## Follow-up Reading
- 📄 Designing evals for AI agents (Anthropic), no URL in source text → the author's earlier post on capability evals, which contrasts with safety evals.
- 🔗 Inspect framework (UK AI Security Institute), no URL in source text → open-source tooling for running, logging and grading evals.
- 🔗 Petri toolkit (Anthropic), no URL in source text → built on Inspect, it automates the multi-turn testing described here.

---
*Summary generated: 2026-11-16*
