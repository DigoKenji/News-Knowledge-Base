---
type: Article Summary
title: I Tested TypeSafe's Jev, a 478× Cheaper Decision Model, Against Claude, GPT-6, Kimi, MiniMax and DeepSeek
description: A specialist decision model matches frontier LLMs on most typed decisions at a fraction of the cost, but misjudges ambiguity.
resource: https://medium.com/data-science-collective/i-tested-typesafes-jev-a-470-cheaper-decision-model-against-claude-gpt-6-kimi-minimax-and-d36ed152e861
tags:
- ai-ml
- llm-engineering
- inference-tradeoffs
- calibration
- structured-decision-model
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/data-science-collective/i-tested-typesafes-jev-a-470-cheaper-decision-model-against-claude-gpt-6-kimi-minimax-and-d36ed152e861
  title: I Tested TypeSafe's Jev, a 478× Cheaper Decision Model, Against Claude, GPT-6, Kimi, MiniMax and DeepSeek
  author: Manjunath Janardhan
status: stable
source_url: https://medium.com/data-science-collective/i-tested-typesafes-jev-a-470-cheaper-decision-model-against-claude-gpt-6-kimi-minimax-and-d36ed152e861
source: Data Science Collective
source_type: medium
published: '2026-09-19'
domain: ai-ml
subdomain: llm-engineering
concepts:
- inference-tradeoffs
- calibration
- structured-decision-model
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81cc90d8e9c155504851
---

# I Tested TypeSafe's Jev, a 478× Cheaper Decision Model, Against Claude, GPT-6, Kimi, MiniMax and DeepSeek

**Source:** https://medium.com/data-science-collective/i-tested-typesafes-jev-a-470-cheaper-decision-model-against-claude-gpt-6-kimi-minimax-and-d36ed152e861
**Author:** Manjunath Janardhan
**Date:** 2026-09-19
**Concepts:** [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md) · [Calibration](/concepts/ai-ml/calibration.md) · [Structured decision model](/concepts/ai-ml/structured-decision-model.md)

---

## 🎯 Core thesis
A small, single-purpose "System One" model can answer typed decision questions nearly as accurately as frontier LLMs at a fraction of the cost and latency, but only the most expensive model clearly beats it, and only that model's probabilities track genuine human disagreement.

## 🧭 Why the author wrote it
Much of what an AI application does is deciding (routing a ticket, flagging spam, grading an answer, judging tone), and each decision is a small multiple-choice question that teams usually send to a general chat LLM. The author tests whether a model built only for such questions is good enough, and whether its probabilities mean anything on items where people themselves disagree. If Jev holds up, the default choice for high-volume decisions changes from a flagship model to a fast specialist.

## What Jev is, in one call
Jev is not a chat model and does not generate text. You send some input (the "state") and one or more typed questions: a `choice` from a list, a `score` on an ordered scale, or a `noul` (the probability that something is true). It returns a probability for every option. TypeSafe released it on 15 September 2026 and calls it a "System One" model, after the fast, intuitive half of Kahneman's thinking model. On OpenRouter it costs $0.042 per million input tokens, and output is free. The author pays $0.025 per thousand decisions in the test, and the whole test cost $4.83.

```py
import json, os, urllib.request

body = {
    "model": "typesafe/jev-1.13",
    "state": {"message": "I was charged twice for the same coffee"},
    "questions": {
        "team": {
            "type": "choice",
            "instructions": "Which team should handle `message`?",
            "criteria": {"payments": "Charges, refunds, payouts",
                         "cards": "Card delivery, activation, PINs",
                         "other": None},
        }
    },
}
req = urllib.request.Request(
    "https://openrouter.ai/api/alpha/decisions",
    data=json.dumps(body).encode(),
    headers={"Authorization": f"Bearer {os.environ['OPENROUTER_API_KEY']}",
             "Content-Type": "application/json"},
)
print(json.load(urllib.request.urlopen(req))["answers"]["team"])
# {"type": "choice", "choice": "payments",
#  "probabilities": {"cards": 0, "payments": 1, "other": 0}, "confidence": 1}
```

**What this does:**
- The `state` holds the input to judge, and `questions` holds one or more typed questions about it, so the request is a decision rather than a prompt.
- The `choice` question lists its options as `criteria`, with `other` as the fallback; the model can only answer from this list.
- The call goes to OpenRouter's own decisions endpoint, not the chat completions endpoint, because Jev is not a chat model.
- The response returns `probabilities` for every option, so the caller can threshold or escalate on them; the example's probability of 1 for `payments` is the model's own confidence.
- The request uses only the standard library, which keeps the example free of a client dependency.

### Typed decision question
A typed question tells the model what kind of answer to return, so the output is a closed set rather than free text. The three types are `choice`, `score` and `noul`. This is what separates the model from a chat LLM: the answer space is fixed in advance, so the model can return a probability for each option instead of a sentence.

### Probability output
Each answer carries a `probabilities` map over the options, and the top value is the model's own confidence in its choice. The article is explicit that Jev's separate `confidence` field is a summary statistic and not a probability, so the author uses the top probability instead. Several questions about the same input can be asked in one call and are answered in parallel.

> ❌ **Anti-pattern — sending decisions to a chat LLM by default:** using a general model that is built to write essays for yes/no gates, routing and classification. **Why people do it:** it is the tool already at hand and it works, so nobody measures the alternative. **Consequence:** high per-call cost and multi-second waits on decisions that a specialist answers in under half a second. **Fix:** benchmark a System One model on your own decision set before defaulting to a flagship.

### Calibration
Calibration asks whether a model's stated probabilities match how often it is actually right. The author reports Jev's calibration error as 0.13 on the three standard tasks and 0.25 on the ambiguous task, against 0.07 on standard tasks in another independent benchmark. An out-of-distribution test also found Jev's choice answers overconfident. Treat calibration as something to measure on your own data, not something to assume.

## The test
Six models were compared: Jev, Claude Fable 5.1, GPT-6 Astra, Kimi K3, MiniMax M3 and DeepSeek V4.1 Flash. There were four tasks of 50 items each, drawn from public datasets with known answers: yes/no questions about a passage, routing across 77 banking intents plus "other" (78 options), star ratings, and the ChaosNLI ambiguity task. That makes 200 items in total.

The fairness rules matter. Every model saw the same input, question and options. The LLMs had to reply in strict JSON that allowed only the listed options and gave a probability for each. For the 78-option routing task the LLMs returned one answer and one confidence instead, because Anthropic's API refused the larger format and it would have cost six times as much. The LLMs ran at "medium" reasoning effort. Jev followed TypeSafe's own guidance (the full intent list, and questions that name the field they concern). Each open-weight model was pinned to one host so that speed did not depend on which server OpenRouter picked. Every call was logged, and all numbers come from those logs.

### Human-label ambiguity
Some items have no single right answer. ChaosNLI labels each sentence pair with 100 human judgments, and on many pairs those judgments split. A good decision system in that situation should express the split rather than pick one label with high confidence.

## Result 1: only the two most expensive models clearly beat it
Compared item by item, Claude Fable 5.1 beats Jev by 11.5 points and GPT-6 Astra by 6.5 points, and both gaps hold statistically. Kimi K3, MiniMax M3 and DeepSeek V4.1 Flash are 2 to 3.5 points ahead, which is within noise on 200 items. Those three cost 6 to 150 times as much as Jev and take 2 to 15 times as long. Claude Fable 5.1 buys 11.5 more points of accuracy for 478 times the price and about 10 times the wait. Whether that is worth it depends on the cost of a wrong answer.

### Item-level comparison
Comparing models item by item, on the same 200 items, is more sensitive than comparing overall accuracy, because it removes the difficulty of the items from the comparison. This is why the author can call the two large gaps statistically solid while the 2 to 3.5 point gaps are not. The 50-item-per-task design still limits per-task conclusions.

## Result 2: where it wins and where it doesn't
On yes/no questions about a passage, Jev matches every other model in the test. Routing across 77 similar-sounding banking intents is harder: Jev scores 76%, which is in line with published results for it on this dataset (78 to 83%, with wide error bars). Star ratings are its weakest task. It is usually within one star of the correct rating but gets the exact count right less often. The practical reading is that Jev is strongest where the options are clearly separated and weakest where they overlap.

## Result 3: it answers in under half a second
Jev's median latency was 0.43 seconds from India through OpenRouter, network included, and its slowest 5% still returned within 0.6 seconds. The fastest LLM, MiniMax M3, took about a second. The two flagships took 2.2 seconds (Claude Fable 5.1) and 4.3 seconds (GPT-6 Astra). DeepSeek's slowest 5% took more than 23 seconds. For a support widget or a form that routes itself as the user types, this gap matters more than a few points of accuracy.

### Latency percentiles
The author reports the median and the slowest 5% (the 95th percentile). Tail latency is what a waiting user experiences at the worst moments, so the slow end of the distribution often matters more than the average. A model with a fast median and a long tail, like DeepSeek here, can still feel unreliable in an interactive flow.

## Result 4: only one model knows when people disagree
This was the author's most surprising result, and they found no earlier measurement of it for Jev. In the ChaosNLI example, the 100 human labels for one sentence pair split across the options rather than agreeing. Claude Fable 5.1 hedged in roughly the same proportions as the people. Jev and GPT-6 Astra were nearly certain, and they chose the minority view.

### Distance from the human label distribution
To score this, the author measures how far each model's probability vector is from the distribution of human labels, across all 50 pairs. Claude Fable 5.1 is about 2.5 times closer to the human split than the next best model, MiniMax M3, and 3.5 times closer than Jev. The uncomfortable point is that a model answering one third for every option on every pair beats Jev, GPT-6 Astra and DeepSeek, and ties Kimi. Only Claude Fable 5.1 and MiniMax M3 do better than a blind guess.

> ❌ **Anti-pattern — a confident single label for a judgment call:** returning one label with high confidence when the underlying question is contested. **Why people do it:** a single answer is simpler to consume and to threshold. **Consequence:** the model is 96% sure of a coin flip and fails exactly where you would want it to ask for help. Jev chose "neither" on 29 of 50 pairs where the human majority chose it on 19, at an average confidence of 83%. **Fix:** keep the full probability distribution, and route high-disagreement items to review or to a larger model.

If your decisions include real judgment calls, such as moderation edge cases, "is this answer good enough?" or tone, this result matters more than headline accuracy.

## A surprise: the frontier models barely reasoned
The LLMs were asked to reason at medium effort, but on these short questions they mostly did not. Claude Fable 5.1 used about 2 reasoning tokens per question, GPT-6 Astra about 20, Kimi K3 about 89 and DeepSeek V4.1 Flash about 236. So a flagship model with reasoning enabled, on a routing question, is mostly paying for the base model rather than for extra thinking. For short decisions the "System Two" models behave like System One models, only slower and more expensive.

> ❌ **Anti-pattern — paying for reasoning on short decisions:** enabling a reasoning budget for a one-label routing question and assuming it adds quality. **Why people do it:** reasoning is the advertised feature of a frontier model. **Consequence:** cost and latency rise while the model spends almost no tokens thinking. **Fix:** measure reasoning token use on your own items, and enable reasoning only for decisions that need multi-step work.

## Using both: Jev first, then escalate
Jev's probabilities allow a cascade: accept Jev's answer when its top probability clears a threshold, and send the remainder to a larger model. At a 90% threshold the cascade gets within 1.5 points of Claude Fable 5.1 alone for 37% of its cost, and 62% of requests still get Jev's half-second answer. At a 70% threshold it beats Kimi K3 on accuracy for less than half of Kimi's price. The author warns that the thresholds were chosen after seeing the data, so on your own traffic you should set the threshold on one sample and verify it on another.

### Cascade routing
A cascade is a two-stage pipeline in which a cheap, fast model handles the easy cases and a stronger model handles the uncertain ones. Its quality depends on the confidence signal being useful for deciding what to escalate, even if the absolute values are overconfident. The author's point is that Jev's ranking of its own answers is informative even when its raw probabilities are too extreme.

> ❌ **Anti-pattern — tuning the threshold on the reported data:** choosing the cutoff that looks best on the same items you report results for. **Why people do it:** it produces a clean number to publish. **Consequence:** the reported saving will overstate what you get in production. **Fix:** fix the threshold on a held-out sample before measuring the saving.

## So which should you use?
- Yes/no gates and filters: Jev. It matched every frontier model in the test at a fraction of a cent per decision.
- Routing and classification at volume: Jev, with low-confidence items escalated to a larger model. Its confidence is still useful for choosing what to escalate.
- Judgment calls where "it depends" is the right answer: Claude Fable 5.1 was the only model whose probabilities tracked human disagreement.
- Anything a user waits for: Jev's 0.43 seconds is in a different class from 1 to 6 seconds.

## Caveats
The author flags several limits. There are 50 items per task, so per-task gaps under about 10 points are within noise, while the overall ranking is more solid. The whole run was from one location, India, through OpenRouter, so latency includes network time and would be lower nearer the servers for every model. Calibration is worse than other independent measurements found, and the author's items were harder and fewer, so their calibration number is one data point. Routing answers from the LLMs were compact (one answer and one confidence), not the full probability vector. Jev is early access (version 1.13, build dated 17 September 2026), and every model here is updated over time, so the numbers describe one week in September 2026.

## Update, 20 September 2026
The author later re-ran 20 items on both OpenRouter and TypeSafe's own API, interleaved. OpenRouter had a median of 384 ms and the direct API 874 ms, both from India. The two agreed on 19 of 20 decisions, and the one disagreement was a near-tie between two routing intents (0.25 and 0.24). TypeSafe reports 70 to 500 ms from the US West Coast, so the gap appears to be distance from their servers rather than the API route. US readers should expect the direct route to be faster than the author measured.

The test harness, all 1,200 raw responses, the item manifest and the chart code are published at [manjunathshiva/jev-frontier-bench](https://github.com/manjunathshiva/jev-frontier-bench). The analysis re-runs from the published logs without an API key. The author paid for every call, and no company sponsored or reviewed the test.

## 🔑 Key takeaways
- Jev matched every frontier model on yes/no questions, and trailed all five LLMs on overall accuracy by 2 to 11.5 points, at roughly 1/478th of the price of the most accurate model tested.
- Only Claude Fable 5.1 (11.5 points ahead of Jev) and GPT-6 Astra (6.5 points ahead) clearly beat it; the other three LLMs were within noise.
- Jev's median latency was 0.43 seconds, and its slowest 5% stayed under 0.6 seconds, against 1 to 6 seconds for the LLMs and a 23-second tail for DeepSeek.
- Jev is weakest on routing with overlapping intents (76%) and on exact star ratings, where it is usually within one star.
- On ambiguous items, Jev and GPT-6 Astra gave near-certain answers that chose the minority human view, and Jev chose "neither" on 29 of 50 pairs where the human majority chose it on 19.
- Claude Fable 5.1 used about 2 reasoning tokens per question, so flagship reasoning adds little on short decisions while adding cost.
- A cascade that sends Jev's low-confidence answers to a larger model keeps most of the speed and most of the accuracy at a fraction of the cost, provided the threshold is validated on held-out data.

## 🏛️ For Kenji
This is your interpretation, not the author's. In a Staff-level architecture role, the useful move is to classify the decision workload in your AI systems into gates, high-volume routing, and genuine judgment calls, then match each to a model tier. The open question it raises for AI governance work is how you would evidence calibration and disagreement-awareness to a reviewer, given that the article shows a model can be fast, cheap and confidently wrong on contested items.

## 📚 Reading plan
### From the article
- [Introducing System One Models & Jev (TypeSafe AI Blog)](http://typesafe.ai) — the vendor's own description of the model and its design goal.
- [jev-decision-bench](https://github.com/OmarMujahid/jev-decision-bench) — the most thorough independent benchmark, 49 tasks against GPT-5.6 Luna.
- [jev-ood-calibration](https://github.com/scienthoon/jev-ood-calibration) — the out-of-distribution calibration test that found Jev's choice answers overconfident.
- [jev-frontier-bench](https://github.com/manjunathshiva/jev-frontier-bench) — the full harness, raw responses and item manifest for this comparison.
### To go further
- ChaosNLI (human label distributions for sentence-pair inference) — the source of the disagreement measurement and the best template for testing your own ambiguous decisions.
- Model calibration and proper scoring rules — needed to judge whether a model's probabilities can be trusted before you build a cascade on them.
- Cascade and model-routing patterns for LLM cost control — the general pattern behind the "Jev first, then escalate" result.
