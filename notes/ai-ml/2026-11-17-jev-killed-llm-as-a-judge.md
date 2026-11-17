---
type: Article Summary
title: Jev killed LLM-as-a-Judge?
description: LLM judges carry biases and uncalibrated scores, so binary eval lanes should move to calibrated decision models after validation.
resource: https://medium.com/womenintechnology/jev-killed-llm-as-a-judge-0695a87ce149
tags:
- ai-ml
- llm-engineering
- llm-as-a-judge
- rag
- calibration
- position-bias
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/womenintechnology/jev-killed-llm-as-a-judge-0695a87ce149
  title: Jev killed LLM-as-a-Judge?
  author: Sakshi Jaiswal
status: stable
source_url: https://medium.com/womenintechnology/jev-killed-llm-as-a-judge-0695a87ce149
source: Women in Technology
source_type: medium
published: '2026-09-23'
domain: ai-ml
subdomain: llm-engineering
concepts:
- llm-as-a-judge
- rag
- calibration
- position-bias
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d8125a083ddaf118c8887
---

# Jev killed LLM-as-a-Judge?

**Source:** https://medium.com/womenintechnology/jev-killed-llm-as-a-judge-0695a87ce149
**Author:** Sakshi Jaiswal
**Date:** 2026-09-23
**Concepts:** [LLM-as-a-judge](/concepts/llm-as-a-judge.md) · [Retrieval-augmented generation](/concepts/rag.md) · [Calibration](/concepts/calibration.md) · [Position bias](/concepts/position-bias.md)

---

## 🎯 Core thesis
Binary evaluation lanes should move from LLM judges to calibrated decision models such as Jev, once validated on your own data, because judge verdicts drift and judge scores are not probabilities.

## 🧭 Why the author wrote it
The author ran the same 500-question RAG benchmark twice, 24 hours apart, with the same model and prompt, and 43 verdicts flipped because the judge changed its mind, not because the answers did. The article treats this as a structural problem rather than a testing problem, and says TypeSafe AI's Jev, launched in September 2026, changes the options. If that thesis holds, teams should keep LLM judges for open-ended, rationale-heavy evaluation and move binary checks such as faithfulness, safety, routing and guardrails to a model that returns calibrated probabilities.

## What LLM-as-a-Judge Actually Is (and Why Teams Adopted It So Fast)
The pattern is simple. Instead of paying human annotators to grade model outputs, you send those outputs to a capable LLM with a rubric and ask it to score or compare them. The technique caught on for operational reasons, and the appeal is partly architectural, because a judge can assess qualities that older metrics miss.

### LLM-as-a-Judge
The original MT-Bench paper by Zheng et al. (2023) tested this idea with real numbers. It graded 80 multi-turn questions across writing, reasoning, math and code, used GPT-4 as the judge, and measured agreement against pairs of human experts. Agreement between GPT-4 and humans landed in the same range as agreement between two humans. The article's practitioner comparison makes the economics concrete: three raters, 200 examples and a multi-criteria rubric is a week of work, while an LLM judge runs the same eval in 12 minutes for $4. For teams iterating quickly on prompts or fine-tunes, that ratio is hard to resist.

### Pairwise evaluation and pointwise grading
Judges usually work in one of two modes. In pairwise evaluation, the judge compares two candidate responses and selects the preferred one. In pointwise grading, it assigns a numerical score to a single output. The article says both modes are now ubiquitous in production AI systems.

### BLEU and ROUGE
These are the metrics judges are meant to replace. The article says they fail to capture coherence, factual accuracy and relevance. A language model judge can read a response the way a user would, notice that an answer is technically correct but unhelpful, and flag it. The article calls this a genuine capability and says the problem is what comes with it.

> 🔎 Added context: BLEU and ROUGE score n-gram overlap between generated text and reference text. They reward matching words, not meaning, which is why they miss the qualities the article lists.

### Faithfulness check as a pointwise judge
The article gives a minimal pointwise judge that checks whether a RAG answer is faithful to its retrieved context. It asks the model to list each claim, find the supporting sentence, and mark the response unfaithful if any claim lacks support. The verdict is parsed into a typed Pydantic model, and the call pins a model snapshot with temperature 0.0.

```py
# Minimal pointwise judge — faithfulness check for a RAG response
from openai import OpenAI
from pydantic import BaseModel

client = OpenAI()

JUDGE_PROMPT = '''
You are evaluating whether an assistant response is faithful to a retrieved context.
Faithful means every factual claim is explicitly supported by the context.
Steps:
1. List every factual claim in the response.
2. Locate the exact context sentence supporting each claim.
3. If any claim has no support, respond unfaithful.

Context: {context}
Response: {response}
'''

class Verdict(BaseModel):
    faithful: bool
    reason: str

def judge_faithfulness(context: str, response: str) -> Verdict:
    result = client.beta.chat.completions.parse(
        model='gpt-4o-2024-11-20',
        messages=[{'role': 'user', 'content': JUDGE_PROMPT.format(
            context=context, response=response)}],
        response_format=Verdict,
        temperature=0.0,
    )
    return result.choices[0].message.parsed
```

**What this does:**
- `JUDGE_PROMPT` makes the judge list claims and locate support before it gives a verdict, so the verdict rests on explicit checking steps.
- `Verdict` gives the output a typed `faithful: bool` plus a `reason: str`, so downstream code can branch on a boolean and still read a rationale.
- `beta.chat.completions.parse` with `response_format=Verdict` returns a parsed object, not free text that has to be scraped.
- `model='gpt-4o-2024-11-20'` pins a dated snapshot, so the judge does not change silently between runs. That matters for the 43-flip problem in the opening story.
- `temperature=0.0` removes sampling randomness from the judge call as far as the API allows.

## The Bias Catalog: What Research Actually Shows
The MT-Bench agreement result is real, but it depends on conditions that most production pipelines do not fully keep. Research over 2024 and 2025 has produced a taxonomy of failures. The three most consistently documented are position, verbosity and self-preference bias, and they interact in ways that make pipelines quietly unreliable.

### Position bias
Position bias means the judge's verdict changes when you swap which answer appears first. A study across 15 judges and approximately 150,000 evaluation instances found that when positions are swapped, judges either flip their verdict (repetition instability) or hold it (repetition stability). The practical consequence is that in a pairwise eval, running only order A-B means you are measuring judge preference as much as response quality.

> ❌ **Anti-pattern — single-order pairwise evaluation:** Each pair is judged once, as A then B, and the winner is recorded. **Why people do it:** running one order halves the judge calls, which makes it the cheap default. **Consequence:** the result mixes response quality with the judge's position preference, and the error is invisible in the output. **Fix:** run both orderings, count how often the verdicts flip, and track that flip rate over time.

The article's second code block implements that fix.

```py
# Cheap position-bias check: run both orderings, compare verdicts
def check_position_bias(judge_fn, prompt, response_a, response_b):
    verdict_ab = judge_fn(prompt, response_a, response_b)  # A first
    verdict_ba = judge_fn(prompt, response_b, response_a)  # B first
    flipped = verdict_ab['winner'] != swap_winner(verdict_ba['winner'])
    return {'flipped': flipped, 'ab': verdict_ab, 'ba': verdict_ba}

def swap_winner(winner):
    return 'B' if winner == 'A' else 'A'

# Run across a sample and report the flip rate
flips = [check_position_bias(my_judge, p, a, b) for p, a, b in eval_set]
flip_rate = sum(f['flipped'] for f in flips) / len(flips)
print(f'Position bias flip rate: {flip_rate:.1%}')
```

**What this does:**
- Calls the judge twice, once with A first and once with B first, so every pair is judged in both orders.
- `swap_winner` maps the second run's winner back to the original labelling before the two verdicts are compared.
- `flipped` is True when the two orders disagree. That disagreement is the position-bias signal.
- `flip_rate` aggregates over `eval_set`, and the printed percentage is the number to track over time.

### Verbosity bias
Verbosity bias is the tendency to favor longer responses whether or not the extra length adds value. The article reports that most models show a positive verbosity bias. In one systematic study, Llama 3.3–70B and Gemini 2.5 Pro scored strongly positive, while Claude Sonnet 4 alone showed a mild preference for conciseness.

### Self-preference bias
Self-preference bias means a model tends to rate its own outputs more favorably. The article calls it the most insidious bias in agentic systems. In an automated training loop, it is not just noise, because the judge's favoritism becomes the feedback signal that shapes the next model.

> ❌ **Anti-pattern — same-family judge in an automated loop:** The judge comes from the same model family as the generator it grades. **Why people do it:** one provider's API is already in the stack, so it is the easiest judge to wire in. **Consequence:** self-preference bias feeds back into training or selection, and the signal drifts toward the generator's own style. **Fix:** use a judge from a different model family, and treat it as one control among several.

### Authority bias, fallacy oversight and leniency
Researchers have also catalogued three further failures. Authority bias favors responses that cite sources, even fabricated ones. Fallacy oversight means logical errors in reasoning chains go unnoticed. Leniency means judges pass too much, which hides regressions.

> ❌ **Anti-pattern — trusting a lenient judge's green dashboard:** The judge passes most outputs, and the dashboard looks healthy. **Why people do it:** a green dashboard is the signal teams are watching for. **Consequence:** the regression the eval was built to catch goes unseen. **Fix:** check the judge's pass rate against a labelled sample, including known-bad outputs, before trusting the trend line.

### Measured failure rates and mitigations
IBM Research's CALM framework identified 12 distinct bias types across popular models. JudgeBiasBench found frontier models exceeding 50% error rates on advanced bias tests. The article says that even with all mitigations applied (randomizing order, using a judge from a different model family, and explicit rubric constraints), the biases cannot be eliminated entirely.

## The Calibration Gap: Why a Score Is Not a Probability
The article says the number a judge returns is not a probability. When a judge returns "4 out of 5" or "PASS", the verdict carries no calibrated confidence signal. You cannot threshold on it, route on it, or decide when to escalate to a human reviewer. The article's phrase is that it is "a label dressed as a number."

### Calibration
> 🔎 Added context: A model is well calibrated when its stated confidence matches how often it is right. A well-calibrated 0.8 is correct about 80% of the time.

Thresholds, escalation paths and drift monitors all need a probability score. An eval dashboard can look green while hiding a real failure rate, because the judge has no way to say "I am not sure about this one."

### Miscalibration in JudgeBench
A 2025 study of 14 models on JudgeBench found judges clustering their predictions at 90% to 100% confidence while their accuracy landed well below that. High stated confidence paired with lower actual accuracy is what miscalibration means. The article argues this matters most in agentic systems, where evaluation decides whether to retry, escalate or pass a result on. There, a miscalibrated judge is unpredictable in the wrong direction, and the systems that most need reliable signals are the ones where miscalibration costs the most.

> ❌ **Anti-pattern — thresholding a raw judge score:** The pipeline routes or escalates when the judge's score crosses a line, such as "4 or above passes". **Why people do it:** a score looks like a measurement, and it is easy to read. **Consequence:** the cutoff has no stated confidence behind it, so routing errors happen silently at the boundary. **Fix:** derive a probability, or measure agreement, before any threshold is set.

### Partial mitigations and their cost
The article lists two partial fixes. One samples multiple judge responses and computes empirical agreement. The other uses a panel of diverse judges and treats disagreement as a low-confidence signal. The article's point is that both multiply inference costs substantially.

## What Jev Is and What It Is Not
TypeSafe AI launched Jev on September 18, 2026. The article stresses that Jev is a decision model, not a chat model and not a smaller LLM. A generative LLM predicts tokens and can produce an open-ended string. Structured-output modes constrain that output to a schema, but the model still runs through a language-model interface. Jev instead starts from a closed answer space. You define the options, and it returns the selected value plus the full probability distribution over the permitted outcomes. It does not write an explanation.

### Decision model and closed answer space
In Jev, you define the options up front, and the model chooses among them. The output is a choice and a distribution, not prose. This is a different shape from a judge that writes a verdict and a justification.

### Choice, Score and Noul
Jev exposes three question primitives. Choice selects among named options that you supply. Score returns a value on a developer-defined rubric of 2 to 10 rungs. Noul is a probabilistic yes-or-no. Several questions over the same input state are evaluated in parallel in a single API call.

### RLCD
TypeSafe calls its training objective Reinforcement Learning for Calibrated Decisions (RLCD). It targets decision quality and useful probability estimates, not preferred conversational responses. The article says this is a genuinely different objective from standard RLHF.

### OpenRouter access and cost
As of launch, Jev is available on OpenRouter as `typesafe/jev-latest`, through OpenRouter's `/api/alpha/decisions` endpoint. It costs $0.042 per million input tokens and $0.00 per million output tokens, because it produces no output tokens. TypeSafe claims up to 200x faster and 400x cheaper than frontier LLMs on comparable classification tasks. An independent test found that a general-purpose model spent about 910 output tokens reasoning to each yes-or-no answer, while Jev spent 85 and does not bill for them.

> ❌ **Anti-pattern — reasoning-heavy generative model for yes/no checks:** A general-purpose model writes out a chain of reasoning to answer a binary question such as faithfulness. **Why people do it:** it is the tool already in the stack, and a single model handles every check. **Consequence:** the article's test shows about 910 output tokens per answer, billed on every call, to produce a binary result. **Fix:** route binary lanes to a closed-answer model where that trade-off fits, and keep generative judges for work that needs rationales.

### Calling Jev through AI SDK experimental_evaluate
The article's third code block shows the call. The Python SDK was not available at launch, so this TypeScript form is the only route the article gives.

```ts
// Jev call via AI SDK 7+ (experimental_evaluate)
// Note: Python SDK not yet available at launch — verify current docs
import { experimental_evaluate } from 'ai';

const result = await experimental_evaluate({
  model: 'typesafe/jev-latest',
  state: `User asked: ${userQuery}\nAssistant replied: ${assistantResponse}`,
  questions: [
    {
      type: 'noul',  // probabilistic yes/no
      question: 'Is every factual claim in the assistant reply directly supported by the retrieved context?',
    },
    {
      type: 'score',
      question: 'How helpful is this response?',
      scale: { min: 1, max: 5, labels: { 1: 'Not helpful', 5: 'Highly helpful' } },
    },
  ],
});

// result.answers[0].value -> probability (0.0 to 1.0)
// result.answers[1].value -> score (1 to 5)
// result.answers[1].distribution -> full probability over {1,2,3,4,5}
console.log('Faithful?', result.answers[0].value > 0.85);
console.log('Helpfulness score:', result.answers[1].value);
```

**What this does:**
- Sends one `state` string and two questions together, so both answers come back from a single call.
- The `noul` question returns a probability in `answers[0].value`. The code compares it to 0.85, which is the author's illustrative cutoff and not a validated threshold.
- The `score` question returns a value on a 1–5 scale plus a `distribution` over the rungs. That distribution is the calibration information a bare score lacks.
- The comment says the Python SDK was not yet available at launch, so the example is TypeScript only.

## Where Jev Falls Short: The Honest Accounting
The article is direct about the limits. It treats them as design constraints with real consequences, not as minor gaps.

### No rationale
Jev cannot write a sentence. If an evaluation needs to explain why a response failed, so an engineer can fix the prompt or model, Jev will not supply that. You receive the verdict and the probability distribution, and the diagnostic signal a thoughtful LLM judge gives, even with its biases, is absent. Teams that use judge rationales to label training data, or to build rubric-grounded debugging workflows, face a real gap.

### Context window
The context window is 32K to 64K tokens depending on the access route. The article says that is enough for most single-turn evaluation tasks, but it will constrain long agentic sessions and document-level evaluation.

### Early access and the alpha endpoint
Access was still early as of September 20, 2026. The API surface at launch is JavaScript-only through AI SDK 7+ `experimental_evaluate`, with no stable HTTP route for Python teams. The article notes that the `/api/alpha/decisions` endpoint is labelled alpha for a reason.

### Unreplicated claims and narrow calibration evidence
TypeSafe's 200x speed and 400x cost figures come from its own benchmarks. The article says independent replication at scale, on real task distributions, had not been published at the time of writing. The published calibration results (emails scored below 0.1 by Jev were 0.1% spam, and those above 0.9 were 99.9% spam) come from a narrow spam classification task, not from open-ended response quality evaluation.

### Capability boundary and closed rubrics
The article's sharper concern is that the tasks where LLM judges are most powerful are the ones where criteria are hard to specify in advance, such as nuanced helpfulness, tone appropriateness and creative quality. Jev's closed answer space means you must specify everything up front.

> ❌ **Anti-pattern — trusting a cheap score against an unchecked rubric:** The rubric is written once and Jev's scores are used as truth. **Why people do it:** the cost and speed make high-volume scoring feel safe to automate. **Consequence:** if the rubric is wrong, Jev scores confidently and cheaply against the wrong standard. The article says a miscalibrated LLM judge fails in visible ways, while a miscalibrated Jev "might fail silently and at scale". **Fix:** validate the option set and rubric against a labelled sample of your real task distribution before scoring at volume.

## A Practical Migration Path: Running Both in Shadow Mode
The article recommends a cautious route. Keep your existing judge, run Jev beside it, log both verdicts per evaluation, never act on Jev's output, and measure agreement.

### Shadow mode
> 🔎 Added context: Shadow mode runs a candidate system alongside the live one without letting its outputs affect anything. Only the live system's results are acted on, and the candidate's results are logged for comparison.

After a few hundred evaluations, the article says, you will have a real signal. Ask where the two judges agree, where they diverge, and whether the divergences are random noise or systematic. For example, does Jev consistently fail on a particular response length or query type? The article argues that this comparison, run on your own task distribution, is worth more than any published benchmark.

> ❌ **Anti-pattern — acting on Jev before the calibration check:** Jev's verdicts go straight into the pipeline because the vendor's numbers look good. **Why people do it:** the cost and speed make the switch attractive. **Consequence:** the article's point is that an 82% confident Jev verdict that turns out to be wrong is still a wrong verdict, and it now happens faster and more cheaply. **Fix:** run shadow mode first, measure agreement, and only then act on the output.

### Confidence-threshold gate
Once agreement is measured, the next step is a probability threshold. Above it, Jev's verdict goes straight into the pipeline. Below it, the case falls back to the LLM judge or to human review. The article's spam test shows the shape. Of inputs scored between 0.5 and 0.6 by Jev, only 38% were actually positive, so that mid-band is exactly where human review is needed. Routing only those uncertain cases to a human left the rest of the eval set at 99.5% accuracy. The article says the threshold and fallback will differ by task, but the pattern applies.

The article adds one more caution. Where the agent and the judge come from the same provider, self-preference bias is a risk, and a lenient judge hides the regressions it was built to catch. Even a partial migration, limited to binary classification lanes, is worth testing, because the cost saving funds more coverage.

### Shadow-mode harness
The article's fourth code block is a harness for this pattern.

```py
# Shadow-mode harness: run both judges, log agreement, never act on Jev
import json
from dataclasses import dataclass, asdict

@dataclass
class ShadowResult:
    input_id: str
    llm_judge_verdict: bool
    jev_probability: float   # 0.0 to 1.0 from Jev noul answer
    jev_verdict: bool        # probability > 0.5
    agree: bool

def shadow_eval(input_id, context, response, jev_client, llm_judge_fn):
    # LLM judge (acts on pipeline)
    llm_verdict = llm_judge_fn(context, response)

    # Jev (shadow — logged only, never acted on)
    jev_result = jev_client.evaluate(
        state=f'Context: {context}\nResponse: {response}',
        questions=[{'type': 'noul',
                    'question': 'Is this response faithful to the context?'}]
    )
    jev_prob = jev_result['answers'][0]['value']
    jev_verdict = jev_prob > 0.5

    return ShadowResult(
        input_id=input_id,
        llm_judge_verdict=llm_verdict,
        jev_probability=jev_prob,
        jev_verdict=jev_verdict,
        agree=(llm_verdict == jev_verdict),
    )

# After N runs: compute agreement rate and inspect disagreements
results = [shadow_eval(i, c, r, jev, my_judge) for i, c, r in eval_set]
agreement = sum(r.agree for r in results) / len(results)
print(f'Agreement: {agreement:.1%}')
# Inspect disagreements manually — they tell you where your rubric is ambiguous
disagreements = [r for r in results if not r.agree]
with open('disagreements.jsonl', 'w') as f:
    for d in disagreements:
        f.write(json.dumps(asdict(d)) + '\n')
```

**What this does:**
- The LLM judge still drives the pipeline. Jev's output is only recorded in `ShadowResult`, so the shadow run cannot change any decision.
- `jev_verdict = jev_prob > 0.5` turns Jev's probability into a yes/no only for comparison. The threshold is a convenience for the agreement count, not a calibrated cutoff.
- `agree` records per-item agreement, and the overall agreement rate is the first number to read.
- Disagreements are written to JSONL for manual review. The article says they show where the rubric is ambiguous, which is the most useful output of the run.

## Wrapping up
The article's closing argument is that Jev does not end LLM-as-a-Judge. It ends the idea that the LLM judge is the only tool for structured evaluation. Nuanced rubric interpretation, rationale generation and pairwise preference judgment on open-ended outputs remain judge work. Typed decisions, calibrated probabilities, no hallucinated prose, no output tokens and a fraction of the cost suit the binary lanes: faithfulness, safety, routing and guardrails. The article's line is that Jev "killed the part that should have been a classifier all along." It also says that using a frontier model to generate 910 tokens for a yes-or-no faithfulness answer was always a workaround, and Jev makes that cost visible.

## 🧠 Mental models
### A label dressed as a number
**The model:** A judge's "4 out of 5" or "PASS" looks like a measurement but carries no calibrated confidence. It cannot drive a threshold, a route or an escalation.
**How to apply it:** Do not threshold, route or escalate on a raw judge score. Derive a probability or measure agreement first.

### Measure the judge, not only the response
**The model:** Verdicts move with candidate order, response length and the judge's own family, so a pairwise result can reflect judge preference as much as quality.
**How to apply it:** Run both orderings in pairwise evals, and track the flip rate and the judge-to-generator family relationship alongside the scores.

### Route by confidence, escalate the middle band
**The model:** Confident verdicts at the extremes can be automated. The uncertain middle needs a human, and that band is where errors cluster.
**How to apply it:** Set a probability threshold, measure accuracy inside the mid-band, and send that band to review.

### A closed answer space means you specify everything upfront
**The model:** A decision model can only score against the options and rubric you define. A wrong rubric produces confident, cheap, wrong verdicts.
**How to apply it:** Validate the option set and rubric against labelled examples from your real task distribution before scoring at volume.

### Classifiers for binary checks, judges for open-ended quality
**The model:** Binary checks such as faithfulness, safety and routing are classifier-shaped. Nuanced quality judgments are judge-shaped.
**How to apply it:** Sort your eval lanes by shape, and move the classifier-shaped ones to a calibrated decision model after shadow testing.

## 🔑 Key takeaways
- In a 500-question RAG benchmark run twice, 24 hours apart with the same model and prompt, 43 verdicts flipped because the judge changed, which the author calls a structural problem.
- An eval that runs pairwise comparisons in only one order measures judge position preference as much as response quality, and the flip rate across both orders is the number to track.
- A 2025 study of 14 models on JudgeBench found judges clustering at 90% to 100% stated confidence while accuracy landed well below that.
- Jev costs $0.042 per million input tokens and $0.00 per million output tokens; TypeSafe claims 200x faster and 400x cheaper, and the article notes these claims lack independent replication.
- An independent test found a general-purpose model spent about 910 output tokens per yes-or-no answer, where Jev spent 85 and bills none of them.
- Jev's published calibration evidence comes from a narrow spam task, so it does not yet show calibration on open-ended response quality.
- In the spam test, Jev scores between 0.5 and 0.6 were only 38% positive, so routing that mid-band to humans kept the rest of the set at 99.5% accuracy.

## 🏛️ For Kenji
This is your interpretation, not the author's. Eval verdicts that cannot express uncertainty are a governance problem as well as an engineering one, because a dashboard can look green while hiding a failure rate. The same logic applies to guardrail checks in AI systems you review, where a probability with a validated calibration curve is far easier to defend than a score. A question to take into your next review: which of your current eval lanes are binary checks that could move to a calibrated model, and what evidence would convince an auditor that its probabilities mean what they say?

## 📚 Reading plan
### From the article
The article's text cites no URLs. MT-Bench (Zheng et al., 2023) is named without a link, so there is nothing to add here without inventing one.

### To go further
- MT-Bench paper by Zheng et al. (2023) — the original LLM-as-a-Judge evidence on GPT-4 agreement with human experts.
- JudgeBench (2025 study of 14 models) — the calibration result showing high stated confidence with lower accuracy.
- JudgeBiasBench — the benchmark behind the claim that frontier models exceed 50% error rates on advanced bias tests.
- IBM Research CALM framework — the catalogue of 12 bias types across popular models.
- Position-bias study across 15 judges and approximately 150,000 instances — the evidence for repetition instability versus stability.
- TypeSafe AI's Jev launch material and its OpenRouter listing — check the vendor's current docs and independent replications before relying on the claims.
