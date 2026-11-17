---
type: Article Summary
title: 'Jev + Claude Code: The Cheap Judge That Can Still Cost You'
description: Cheap judges belong where their errors are measured and recoverable, so shadow-run them first and gate only after end-to-end data proves it.
resource: https://medium.com/@UdaykiranEstari/jev-claude-code-the-cheap-judge-that-can-still-cost-you-275cc54b7ab2
tags:
- ai-ml
- llm-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@UdaykiranEstari/jev-claude-code-the-cheap-judge-that-can-still-cost-you-275cc54b7ab2
  title: 'Jev + Claude Code: The Cheap Judge That Can Still Cost You'
  author: Udaykiran Estari
status: stable
source_url: https://medium.com/@UdaykiranEstari/jev-claude-code-the-cheap-judge-that-can-still-cost-you-275cc54b7ab2
source: Medium
source_type: medium
published: '2026-09-21'
domain: ai-ml
subdomain: llm-engineering
concepts: []
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81749f60eba4c395e111
---

# Jev + Claude Code: The Cheap Judge That Can Still Cost You

**Source:** https://medium.com/@UdaykiranEstari/jev-claude-code-the-cheap-judge-that-can-still-cost-you-275cc54b7ab2
**Author:** Udaykiran Estari
**Date:** 2026-09-21

---

## 🎯 Core thesis
A cheap, narrow judge such as Jev is worth putting inside a coding-agent loop only where its position matches its error rate: as a hard gate its misses become invisible, so measure the complete pipeline and begin in shadow mode.

## 🧭 Why the author wrote it
The tempting Claude Code design sends every failure signal through a cheap judge first, so that the expensive coding model only investigates what the judge flags. The author argues that this can be an excellent architecture and also quietly hide the defect the coding agent needed to see. If the thesis is right, the right unit of evaluation is the whole pipeline, and the judge should earn authority step by step.

## Series context and the question this article asks
This is Part 3 of the author's Jev series. Part 1 argued that Jev's real advantage is typed, software-consumable decisions rather than price; Part 2 asked whether an application should trust fast, cheap decisions. This article does not repeat that foundation. It asks where a narrow judge should sit inside a Claude Code workflow, and when adding one makes the complete system worse. The central sentence of the opening is that a cheap decision is not automatically a cheap system: the cost of a judge includes the work it prevents, the work it creates, and the errors it hides.

## Start with the loop, not the launch story
### Jev as a typed decision model
Jev is a probabilistic decision model from TypeSafe AI. It accepts state, evaluates bounded questions, and returns typed Boolean, choice, or score answers. TypeSafe lists its price at $0.042 per million input tokens, with no output-token charge, and reports response times of 70 to 500 ms. These properties make it attractive for repeated routing and evaluation work.

Jev does not write code, diagnose a repository, fix a regression, or prove that a change is safe. Its useful role is to turn messy evidence into a small number of typed signals that ordinary policy code can act on.

### The build-verify-fix loop
The article's sensible loop runs deterministic checks first. If they pass, the workflow finishes. If they fail, the evidence (logs, diff, test history) goes to Jev, which returns bounded typed signals. Policy code then decides whether to rerun, investigate, or request review. Claude investigates the selected evidence, a fix is applied, and the change is re-verified.

```javascript
// Flow style view for the changes

build or edit
     │
     ▼
deterministic checks ── pass ───────────────► finish
     │ fail
     ▼
evidence: logs + diff + test history
     │
     ▼
Jev: bounded typed signals
     │
     ▼
policy code: rerun / investigate / review
     │
     ▼
Claude investigates the selected evidence
     │
     ▼
fix ────────────────────────────────────────► re-verify
```

> 🔎 Added context: "Deterministic checks" here means compilers, linters, type checkers, test runners, security scanners and invariant checks.

**What this does:**
- Places deterministic tools first; they stay authoritative wherever they can make a deterministic judgment.
- Shows Jev only after evidence has been produced, never as a replacement for those tools.
- Makes the routing step explicit as "policy code", separating the judge's signal from the decision.
- Ends in a re-verify loop, so every fix is checked by the same authoritative tools again.
- Keeps Claude on the investigation path, reserved for items the policy selects.

> 📋 **Checklist — loop ordering**
> - [ ] Run compilers, linters, type checkers, test runners and security scanners before any judge.
> - [ ] Feed the judge evidence the tools already produced, not a substitute for it.
> - [ ] Keep routing decisions in explicit policy code.

## "Typed" prevents malformed answers, not mistaken decisions
Constrained output solves a real problem. If the question allows only regression, flaky, environment and unknown, the model cannot invent a fifth route or wrap its answer in unusable prose. It can still select the wrong valid route. Jev can prevent free-form fabrication and schema-breaking output, but it cannot guarantee that its judgment about a test failure, security finding or diff is correct. The article cites Vercel for the point that a schema constrains the answer without guaranteeing correctness, and Arize's independent analysis for the same distinction: a schema-correct classification can still be wrong.

> ❌ **Anti-pattern — reading "typed" as "correct":** Treating a schema-valid answer as a verified decision, often summarized as "it can't hallucinate". **Why people do it:** The output looks clean and structured, so its content feels trustworthy. **Consequence:** Wrong but well-formed routes pass silently through the pipeline. **Fix:** Measure decision correctness against labeled outcomes, separately from format validity.

### The operational consequence
If Jev is advisory, a wrong answer is just another signal. If Jev is a hard gate, a wrong negative may prevent the stronger coding model from ever seeing the evidence. The same error has very different costs depending on where the judge sits.

## The architecture changes what an error costs
"Put Jev in front of Claude" describes several different systems. The article distinguishes four roles, summarized in this table:

| Pattern | What Jev controls | Main benefit | Main risk |
| --- | --- | --- | --- |
| Hard gate | Whether Claude sees an item at all | Maximum call reduction | False negatives become invisible |
| Soft prioritizer | Order and urgency | Faster attention without discarding evidence | Low-ranked items may still be neglected |
| Independent signal | One input to deterministic policy | Combines semantic judgment with explicit rules | More policy and monitoring work |
| Uncertainty router | Which ambiguous items escalate | Automates clear cases and preserves a fallback | Thresholds require calibration on your data |

The hard gate is the most seductive because its savings are easiest to show, and it is the design in which Jev's recall becomes a ceiling for the rest of the pipeline. The article's illustration: a gate passes 90% of genuine regressions, and the downstream coding agent correctly identifies 97% of the regressions it receives. Ignoring correlated errors, end-to-end recall is about 0.90 × 0.97 = 0.873, or 87.3%. The illustration is explicitly not a reported Jev benchmark.

> 🔎 Added context: Recall is the share of real positives a stage keeps; when stages run in sequence, a missed item at any stage is lost to every later stage.

```javascript
end-to-end recall = gate recall × downstream recall
                  = 0.90 × 0.97
                  = 0.873, or 87.3%
```

**What this does:**
- Defines end-to-end recall as the product of the stage recalls, which is what a sequential pipeline actually delivers.
- Uses 0.90 for the gate, the share of real regressions that survive the pre-filter.
- Uses 0.97 for the downstream agent, its recall on the items it does receive.
- Produces 0.873, showing that the stronger model's 97% cannot recover the 10% lost upstream.
- Illustrates the article's main point: every pre-filter must be evaluated as part of the complete pipeline.

> ❌ **Anti-pattern — hard gate as an invisible filter:** Letting a judge decide which items the coding model may see, with no record of what it suppressed. **Why people do it:** The call savings are easy to show and the misses are hard to see. **Consequence:** Downstream recall is capped by the gate, and the loss never appears in the coding model's metrics. **Fix:** Use the judge as a soft prioritizer or uncertainty router first, and keep suppressed items auditable.

A soft prioritizer saves time without permanently removing evidence. An uncertainty router is safer still: clear low-risk cases take an automated path, while ambiguous or high-impact cases escalate.

## What the published numbers actually show
The strongest independent comparison the article links from its draft is Ben Greenberg's DEV Community experiment. It used 102 archived submissions and three passes, producing 306 decisions on a narrow binary judgment task. The composite Jev setup reported 100.0% accuracy against 99.0% for Claude Sonnet 5 with high reasoning. Median latency was 378 ms for Jev and 3,554 ms for Sonnet. The article estimated $2.27 versus $129.74 for 10,000 equivalent evaluations, about a 57x difference.

**One task, not a universal ranking.** The result is impressive evidence that Jev can compete with a much larger model on a bounded, well-specified classification problem. It is not evidence that adding Jev in front of an existing coding agent improves the full workflow, because the experiment compares judges and does not measure a Jev-gated Claude Code loop. TypeSafe's own launch material also presents Jev as competitive on decision workloads rather than universally superior, and Arize describes the evidence as promising but early and task-dependent. The article's conclusion is that Jev's price and latency are broadly attractive, but its accuracy advantage must be established separately for each question, dataset, threshold and workflow position.

> ❌ **Anti-pattern — judge benchmark as system benchmark:** Citing a judge-versus-judge win as proof that the coding loop got better. **Why people do it:** The headline numbers are clean and the comparison is intuitive. **Consequence:** The decision to adopt rests on a measurement that never touched the pipeline. **Fix:** Measure the complete variant on your own historical cases.

## A runnable screening layer using the current Vercel AI SDK
The example uses the interface Vercel documents: `experimental_evaluate`, the `typesafe-ai/jev` model ID, structured state, and Boolean, choice and score questions. Jev produces the signals; ordinary TypeScript owns the routing decision.

```ts
import { experimental_evaluate as evaluate } from 'ai';

type FailedTest = {
  testName: string;
  stdout: string;
  stderr: string;
  diffHunk: string;
  recentRuns: Array<{ passed: boolean; commit: string }>;
};

export async function screenFailure(failure: FailedTest) {
  const result = await evaluate({
    model: 'typesafe-ai/jev',
    state: failure,
    questions: {
      isRegression: {
        type: 'boolean',
        instructions:
          'Did the current code change probably cause this test failure?',
        criteria: {
          true: 'The diff plausibly causes the observed failure.',
          false: 'The failure is probably flaky, environmental, or unrelated.'
        }
      },
      failureType: {
        type: 'choice',
        instructions: 'What best explains this failure?',
        criteria: {
          regression: 'The current diff introduced a product defect.',
          flaky: 'The test is nondeterministic or timing-sensitive.',
          environment: 'Infrastructure or configuration caused the failure.',
          unknown: 'The evidence is insufficient to choose another category.'
        }
      },
      severity: {
        type: 'score',
        instructions: 'How strongly should this failure block the change?',
        criteria: [
          'Low: non-blocking or clearly unrelated',
          'Medium: plausible issue requiring more evidence',
          'High: likely release-blocking regression'
        ]
      }
    }
  });
  const { isRegression, failureType, severity } = result.answers;
  const confidence = result.providerMetadata?.typesafe?.confidence as
    | Record<string, number>
    | undefined;
  const typeConfidence = confidence?.failureType ?? 0;
  const selectedTypeProbability =
    failureType.probabilities?.[failureType.choice] ?? 0;
  // Policy belongs in code, not in the model prompt.
  if (
    isRegression.probability >= 0.75 &&
    typeConfidence >= 0.60 &&
    selectedTypeProbability >= 0.70
  ) {
    return {
      action: 'investigate-with-claude' as const,
      severity: severity.score,
      evidence: failure
    };
  }
  if (
    isRegression.probability <= 0.20 &&
    (failureType.choice === 'flaky' || failureType.choice === 'environment') &&
    selectedTypeProbability >= 0.75
  ) {
    return { action: 'rerun-and-collect-evidence' as const };
  }
  return { action: 'review-ambiguous-case' as const };
}
```

**What this does:**
- Imports the evaluate function from the `ai` package and passes the typed `FailedTest` record as structured state.
- Asks three questions: a Boolean for regression, a choice among four failure types, and a severity score.
- Reads the answer probabilities and the provider's confidence metadata, which the policy uses instead of trusting the top label.
- Keeps routing thresholds (0.75, 0.60, 0.70, 0.20, 0.75) in ordinary code, so they can be tuned and tested outside the prompt.
- Returns one of three actions: investigate with Claude, rerun and collect evidence, or review an ambiguous case, so no path silently discards the item.

The article notes that a low Jev probability is never treated as proof that the software is correct. Likely flaky or environmental failures are rerun, ambiguous cases are preserved, and likely regressions go to the stronger investigator. The thresholds are examples rather than production defaults: choose them from labeled failures that resemble your own, because a security gate, a flaky-test router and a documentation-quality scorer have different error costs.

> ❌ **Anti-pattern — policy hidden in the prompt:** Encoding routing rules as instructions to the model. **Why people do it:** It looks simpler than writing code around the judge. **Consequence:** Thresholds cannot be tested, versioned or calibrated, and the behavior drifts with model changes. **Fix:** Keep the signal in the model and the routing rule in code, as the example does.

## Measure the system you are actually deploying
Accuracy of the judge alone is not enough. Before changing the loop, the article recommends a frozen set of historical cases with known outcomes and a comparison of complete variants:

- Claude-only baseline: every eligible item goes directly to the current coding model.
- Jev-only judgment: measure whether the bounded question suits Jev at all.
- Jev hard gate to Claude: only Jev-positive items reach the coding model.
- Jev soft priority to Claude: everything stays visible, but Jev controls ordering.
- Jev uncertainty router: clear cases follow policy, ambiguous cases escalate.
- Cheaper general-model baseline: compare against a smaller model tier before adding a component.

For each variant, capture end-to-end recall and precision, false negatives by severity, the share of items escalated to Claude, total input and output tokens across the whole path, median and tail (p95) latency, retries and timeouts, human-review volume, and the estimated cost of a missed defect rather than only the API bill.

The article's cost argument: if a workflow processes 10,000 items and a false negative can ship a serious regression, saving several model calls is not automatically an improvement. Expected total cost is model calls, plus retries and review, plus integration operations, plus false-positive work, plus the expected cost of false negatives.

```javascript
expected total cost = model calls
                    + retries and review
                    + integration operations
                    + false-positive work
                    + expected cost of false negatives
```

**What this does:**
- Sums every cost component of the pipeline, not only the judge's per-token price.
- Puts the expected cost of false negatives in the total, which is where a gate's hidden misses are priced.
- Includes retries and review, so the cheaper path cannot hide operational load.
- Includes false-positive work, so over-escalation is counted as a real cost.
- Treats the formula as a comparison tool between variants rather than an absolute estimate.

> ❌ **Anti-pattern — counting only model calls:** Judging a change by how many calls it saves. **Why people do it:** Call counts are the easiest number to see. **Consequence:** The savings ignore review load, retries and missed defects. **Fix:** Use the expected-total-cost formula across all variants.

## When Jev is a strong fit
Jev is most compelling when the decision is narrow, repeated, measurable and reversible. The article's examples are classifying a failure as regression, flaky, environmental or unknown; prioritizing a large queue of test failures or review observations; routing clear cases while escalating uncertainty; scoring evidence against an explicit rubric; and generating typed signals for deterministic policy. It is a weaker fit for repository-wide causal reasoning, arithmetic, multi-step investigation, synthesis of contradictory evidence, or generating a fix. Those remain jobs for deterministic tools, a capable coding model or a human reviewer.

## The rollout I would trust
The article is explicit: do not begin with a hard gate; begin with shadow mode.

1. Run Jev beside the existing workflow without changing any route.
2. Log its probabilities, selected categories, confidence metadata, latency and cost.
3. Compare its decisions with final outcomes.
4. Investigate disagreement cases, especially false negatives.
5. Introduce soft prioritization before suppression.
6. Automate only the regions where error rates and consequences are acceptable.
7. Keep a fallback for low confidence, timeouts, schema changes and model unavailability.

> 📋 **Checklist — shadow-mode rollout**
> - [ ] Run Jev beside the live workflow without changing any route.
> - [ ] Log probabilities, categories, confidence metadata, latency and cost.
> - [ ] Compare decisions with final outcomes; review every false negative.
> - [ ] Add soft prioritization before any suppression.
> - [ ] Automate only the regions with acceptable error rates and consequences.
> - [ ] Keep a tested fallback for low confidence, timeouts, schema changes and outages.

The article's point is that this is slower than dropping a cheap judge into every step, but it reveals whether the component saves money in your system rather than merely having a low list price.

## Conclusion: use Jev to expose decisions, not hide evidence
Jev can turn unstructured coding-agent evidence into typed probabilities and categories at a price and latency that make repeated evaluation practical, and early testing shows strong performance on a narrow classification task. The architecture decides whether that helps. A hard pre-filter multiplies its misses into the rest of the pipeline, while a soft signal or uncertainty router keeps the speed and structure without treating the judge as infallible. The three questions the author recommends answering with measured data are: which bounded decision Jev should own, what happens when it is uncertain, and how the team will detect the cases it gets wrong. If it cannot answer them, the low token price is not a reason to place Jev between the tests and the agent that fixes them. The author would begin with shadow mode.

## 🧠 Mental models
### Four roles for a judge
**The model:** A judge can rank, inform policy, route uncertainty, or gate. Each role turns its mistakes into different consequences.
**How to apply it:** Before deploying, name the role explicitly and state what a wrong answer costs in that role.

### Recall multiplies across sequential stages
**The model:** In a sequential pipeline, end-to-end recall is the product of stage recalls, so a pre-filter caps everything downstream.
**How to apply it:** Multiply the recalls of every stage to see the ceiling before adding a filter.

### Shape is not correctness
**The model:** Typed output guarantees the form of an answer; only evaluation against labeled outcomes establishes whether it is right.
**How to apply it:** Test decisions against ground truth, separately from schema validation.

### Judge the system, not the judge
**The model:** A component's value is its effect on the complete pipeline's recall, precision and total cost.
**How to apply it:** Compare complete variants on frozen historical cases, using the expected-total-cost formula.

### Authority is earned in stages
**The model:** A new judge should first observe, then prioritize, then route, and only then gate, each step justified by measured disagreement data.
**How to apply it:** Gate a route only after shadow-mode data shows its error rate and consequences are acceptable.

> 🔎 Added context: "Shadow mode" means running a new component on live inputs and logging its decisions without letting them change routing.

## 🔑 Key takeaways
- Jev's typed output prevents malformed answers but not wrong decisions, so judge correctness on labeled outcomes.
- A 90%-recall hard gate in front of a 97%-recall coding agent gives about 87.3% end-to-end recall in the article's illustration.
- The published comparison (100.0% vs 99.0% accuracy, 378 ms vs 3,554 ms median) covers one bounded binary task on 306 decisions, not a Jev-gated coding loop.
- Estimated cost for 10,000 equivalent evaluations was $2.27 for Jev versus $129.74 for Sonnet 5 in the cited experiment, about 57x.
- Compare complete variants (Claude-only, hard gate, soft priority, uncertainty router) on end-to-end recall, false negatives by severity, tokens, p95 latency and the cost of missed defects.
- Start with shadow mode, add soft prioritization before suppression, and keep a fallback for low confidence, timeouts and outages.

## 🏛️ For Kenji
In an architect's role, this is a pattern for any cheap classifier placed in front of an expensive agent, such as test triage, security-finding routing or review queues, where a gate's misses silently cap the whole system. A useful question to carry into design reviews: which stage in our pipeline has the authority to suppress evidence, and where do we measure what it suppressed? *(This is your interpretation, not the author's.)*

## 📚 Reading plan
### From the article
- TypeSafe AI: Introducing System One Models and Jev — the launch material; no URL is present in the extracted text, so find it from the original Medium page. Read it for the pricing and the decision-workload positioning.
- Vercel: How to Classify, Route, and Score with Jev and AI SDK — the source of the `experimental_evaluate` interface and the schema-versus-correctness point. No URL in the extracted text.
- Arize AI: TypeSafe Jev LLM Judge Analysis — the independent review calling the evidence promising but early and task-dependent. No URL in the extracted text.
- Ben Greenberg: Jev vs. Claude — Who Wins 4M Tokens? — the DEV Community experiment behind the accuracy, latency and cost numbers. No URL in the extracted text.
- Cloudflare Workers AI: Jev Model Documentation — the model's documentation. No URL in the extracted text.

### To go further
- Evaluating LLM-as-a-judge pipelines — how to measure judge accuracy against labeled outcomes.
- Shadow-mode deployment of ML and AI components — how to log and compare decisions without changing routes.
- Calibrating confidence thresholds on labeled data — how to set routing cutoffs for your own error costs.
