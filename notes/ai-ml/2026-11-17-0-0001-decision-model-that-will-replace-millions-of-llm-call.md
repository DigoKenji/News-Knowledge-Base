---
type: Article Summary
title: $0.0001 Decision Model That Will Replace Millions of LLM Calls
description: Small typed decision models with calibrated confidence should front agent workflows, escalating only uncertain cases to frontier LLMs.
resource: https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab
tags:
- ai-ml
- llm-engineering
- inference-tradeoffs
- llm-as-a-judge
- multi-agent-orchestration
- rag
- calibration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab
  title: $0.0001 Decision Model That Will Replace Millions of LLM Calls
  author: Agent Native
status: stable
source_url: https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab
source: Medium (agentnativedev)
source_type: medium
published: '2026-09-18'
domain: ai-ml
subdomain: llm-engineering
concepts:
- inference-tradeoffs
- llm-as-a-judge
- multi-agent-orchestration
- rag
- calibration
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81d0b5f9edc20efc83c4
---

# $0.0001 Decision Model That Will Replace Millions of LLM Calls

**Source:** https://medium.com/@agentnativedev/0-0001-decision-model-that-will-replace-millions-of-llm-calls-4633222ff7ab
**Author:** Agent Native
**Date:** 2026-09-18
**Concepts:** [Inference throughput/latency/cost](/concepts/inference-tradeoffs.md) · [LLM-as-a-judge](/concepts/llm-as-a-judge.md) · [Multi-agent orchestration](/concepts/multi-agent-orchestration.md) · [Retrieval-augmented generation](/concepts/rag.md) · [Calibration](/concepts/calibration.md)

---

## 🎯 Core thesis
Many agent decisions are frequent, constrained and already labeled, so a small model trained on those outcomes should answer them directly with a calibrated confidence, and the frontier LLM should handle only the cases the small model is unsure about.

## 🧭 Why the author wrote it
Agent systems send every small decision (routing, tool choice, filtering, guardrails, extraction) through a frontier LLM because it is already there and it works, but that is slow and expensive. If the thesis holds, the frontier model stops being the default branch of the workflow and becomes the escalation path, and the real design choice becomes the confidence threshold at which a cheap model is allowed to act.

## The hidden cost of tiny decisions
Agent workflows contain many small decisions: which queue a ticket enters, whether an invoice is obviously safe, which tool should load, which retrieved chunks should survive, whether a prompt is suspicious, and whether the agent should continue, retry, ask a human or stop. Teams route all of them through an LLM. The author's point is that this is convenient but slow and expensive, and that most of these decisions are repeated and constrained.

The author argues that a large amount of value will come from putting small, decision-specialized models in front of agentic workflows and letting the frontier model see only the uncertain cases. The article uses TypeSafe AI's newly announced Jev as the example of this direction. Jev gives up free-form string generation and returns typed decisions with probabilities, which TypeSafe calls a "System One Model": unstructured state in, structured probabilistic decisions out.

The article cites two examples, both from embedded posts. Jev can find flights using a browser in about 7 seconds at a fraction of today's cost. It can also score thousands of personalized outreach messages in 40 seconds for under $0.1, to see which ones will perform.

The broader idea is not tied to one vendor. If you have historical outcomes for a repeated decision, you can train a much cheaper model to answer it, attach a confidence estimate, and abstain to the frontier model when it is unsure. That changes the architecture of the agent.

> 🔎 Added context: Selective prediction means a classifier may decline to answer. Abstention trades coverage (the share of inputs it answers) against the error rate on the answered inputs.

## The Frontier Model Becomes the Escalation Path
Take a support workflow that processes 10 million tickets per month, where every ticket reaches a frontier model for triage. The model chooses among a few actions: route to billing, route to technical support, route to sales, or escalate to a human. Most tickets are not ambiguous, and the company probably has years of tickets with the final routing decision attached.

The fix is to insert a decision model before the LLM. If the cheaper model is confident on 60% of traffic while meeting the required error rate on that covered set, the frontier model receives only 40% of the original calls. The article is careful to say that 60% is an illustration, not a universal figure, and it will vary by workflow. The important point is that coverage becomes a tunable systems parameter. You are no longer choosing between "cheap model" and "smart model." You are choosing the confidence threshold at which the cheap model may act.

This is the classic selective-prediction problem: predict when you know, abstain when you do not. Research on selective classification has treated it as a risk-versus-coverage trade-off for years. The more traffic you handle automatically, the more error you absorb. Agent infrastructure gives this old idea a new and valuable place to apply.

## Why Jev-Style Models Fit Agent Workflows
Traditional LLMs are optimized to generate sequences of tokens. That is the right tool when the output is code, prose, analysis or a plan. Many decisions inside an agent are not sequence-generation problems. If the valid actions are:

```javascript
["approve", "hold", "reject"]
```

then generating a JSON object token by token is an expensive way to select one element from a set the application already knows.

> ❌ **Anti-pattern — generating a choice token by token:** asking the LLM to write `{"decision": "approve"}` for a fixed set of options. **Why people do it:** the LLM is already in the stack and the output is easy to parse. **Consequence:** each decision pays for autoregressive decoding and a full frontier call to select one item from a known set. **Fix:** score the known options in one pass and return a probability for each.

Jev evaluates structured questions in parallel and returns probabilities directly. The article reports large latency and cost improvements on its own workflow evaluations, but these numbers are vendor-reported and should be validated on your own workload. The architecture is interesting even if you do not accept the benchmarks. A decision model can use a different interface:

```javascript
context + available actions -> probability distribution
```

That interface maps cleanly onto agent software. The application already knows the allowed actions. The model does not need to invent a string; it only needs to score the actions. The article's simplest starting point, if you have API access, is to install the TypeSafe skill with `npx skills add typesafe-ai/skills --skill typesafe-ai` and then ask your coding assistant to use `/typesafe-ai`. You can also read the repository to understand its internals.

```javascript
npx skills add typesafe-ai/skills --skill typesafe-ai
```

## A Small Open-Source Experiment: jevlike
Vinny LaRouge's jevlike repository makes the idea concrete. The author is precise that jevlike is not an implementation of Jev, because TypeSafe has not published Jev's architecture or training method. jevlike is an independent starter model with a similar input/output shape: text context plus a variable list of text options in, and one probability per option out.

Internally, each option is turned into a query vector. Each option attends over the context, producing an option-specific context representation. A shared scoring head emits one logit per option, followed by a softmax across the available choices. So instead of autoregressively writing:

```javascript
{"route": "billing"}
```

it computes a distribution over the options in one scoring pass:

```javascript
[
  {"option": "billing",   "probability": 0.94},
  {"option": "technical", "probability": 0.04},
  {"option": "sales",     "probability": 0.02}
]
```

The repository supports two encoder modes: a tiny byte-level encoder trained from scratch, or a frozen Hugging Face encoder with a small trainable attention head. The second mode suits teams that have domain data but limited training infrastructure.

### Quick Start
The article gives the setup steps. Clone the repository, create a virtual environment, and install it in editable mode:

```javascript
git clone https://github.com/vinnylarouge/jevlike.git
cd jevlike

uv venv
source .venv/bin/activate
uv pip install -e '.[dev]'
```

The repository includes a synthetic-data generator, a training CLI, an evaluator and a predictor:

```javascript
jevlike-data synthetic --output data/synthetic

jevlike-train data/synthetic/train.jsonl \
  --validation data/synthetic/validation.jsonl \
  --output runs/synthetic.pt

jevlike-eval runs/synthetic.pt data/synthetic/test.jsonl

jevlike-predict runs/synthetic.pt \
  --context "Choose the exact badge amber badger. Badge: amber badger." \
  --option "azure crane" \
  --option "amber badger" \
  --option "gold heron"
```

Your own training data is simple JSONL, one row per decision:

```javascript
{"context":"The customer needs a refund.","options":["refund","sales","technical support"],"label":0}
```

Each row may contain a different number of options. This matters for agents, because tool catalogs, subagents, routes and workflow actions change over time.

For stronger semantic representations, the repository can freeze a pretrained encoder and train only the scoring head:

```javascript
uv pip install -e '.[transformers]'

jevlike-train data/synthetic/train.jsonl \
  --validation data/synthetic/validation.jsonl \
  --output runs/qwen-head.pt \
  --encoder hf \
  --hf-model Qwen/Qwen2.5-0.5B \
  --rank 256 \
  --batch-size 8
```

The checkpoint stores the trained scorer head plus the encoder name. The large pretrained encoder stays frozen.

## Turning the Predictor Into an Agent Gate
The repository's CLI loads a checkpoint, collates one context with its options, runs the model and applies softmax to the logits. The same pieces can become a small routing function inside an agent service. The function below acts only when the top probability clears a threshold of 0.90, and otherwise returns `escalate`:

```py
import torch

from jevlike.data import ChoiceExample
from jevlike.model import load_checkpoint, select_device
from jevlike.train import move

DEVICE = select_device("auto")
MODEL, COLLATOR, _ = load_checkpoint("runs/router.pt", DEVICE)
MODEL.eval()


def decide(context: str, options: list[str], threshold: float = 0.90):
    batch = COLLATOR([ChoiceExample(context, tuple(options), 0)])
    batch = move(batch, DEVICE)

    with torch.no_grad():
        logits = MODEL(batch)[0, : len(options)]
        probs = logits.softmax(-1).cpu()

    idx = int(probs.argmax())
    confidence = float(probs[idx])

    if confidence < threshold:
        return {"action": "escalate", "confidence": confidence}

    return {
        "action": options[idx],
        "confidence": confidence,
        "probabilities": dict(zip(options, probs.tolist())),
    }
```

The expensive model then becomes the fallback:

```py
result = decide(ticket_text, ["billing", "technical", "sales", "human"])

if result["action"] == "escalate":
    result = frontier_llm_triage(ticket_text)

return result
```

The core pattern is to protect the LLM behind a confidence gate.

## Where I Would Deploy This First
The obvious targets are decisions that are frequent, constrained and already logged.

### 1. Model and request routing
Predict whether a request needs a cheap model, a frontier model or a human. The article notes that this is already an active research area, and that RouteLLM showed learned routers can cut model cost substantially while keeping quality on its evaluated settings. A Jev-style scorer pushes this pattern deeper into the stack.

### 2. Tool or subagent selection
Many agent frameworks put every tool description into the prompt. That grows the context and makes tool selection harder. Instead, score the available tools first, load only the top candidates, and let the frontier model reason over a smaller action space.

> ❌ **Anti-pattern — stuffing every tool into the prompt:** including the full description of every available tool on every call. **Why people do it:** it is the default in many frameworks and needs no extra component. **Consequence:** the context grows and tool selection becomes harder for the model. **Fix:** score the tools first and load only the top candidates.

### 3. Retrieval reranking
Retrieval often sends too much context downstream. A cheap scorer can rank candidate chunks against the current state and drop obvious misses before they reach the expensive context window.

### 4. Guardrails on every turn
Prompt injection, policy violations, contradiction checks, unsafe tool calls and "does this output satisfy the workflow invariant?" are all repeated decisions. Running a frontier judge after every agent turn can cost more than the agent itself. A specialized decision model can act as the first-line verifier and escalate only ambiguous cases.

> ❌ **Anti-pattern — a frontier judge on every turn:** calling the most capable model to check each agent step. **Why people do it:** it is the most reliable checker available. **Consequence:** verification costs can exceed the cost of the work being verified. **Fix:** put a specialized scorer first and escalate only the ambiguous cases.

### 5. Typed extraction before reasoning
Emails, PDFs, transcripts, claims, invoices and tickets often contain a small set of fields that downstream logic needs. Extract or classify the predictable pieces first, and reserve generative reasoning for cases that actually require it.

## Confidence Is the Product
The difficult part is not top-1 accuracy. It is calibration. If a model says 0.95, that number has to mean something operationally useful: across many predictions scored near 95%, roughly 95% should be correct. The article notes that neural networks are often miscalibrated.

> 🔎 Added context: Expected calibration error (ECE) measures the gap between stated confidence and observed accuracy, averaged over confidence bins. A low ECE means a threshold like 0.90 corresponds to roughly 90% correctness.

For an agent gate, the article says to evaluate at least six things:

> 📋 **Checklist — evaluation for an agent gate**
> - [ ] Top-1 accuracy: basic decision quality.
> - [ ] Expected calibration error: whether confidence matches observed correctness.
> - [ ] Coverage: fraction of traffic handled without escalation.
> - [ ] Selective risk: error rate on the traffic you auto-handle.
> - [ ] Escalation rate: how much frontier-model traffic remains.
> - [ ] Cost per completed workflow: the metric finance eventually cares about.

Do not tune the threshold on the test set. Choose it on validation data, freeze it, and measure the final system on a held-out split that reflects production entities and time periods. Group related records into the same split to avoid leakage. This matters in business workflows, where the same customer, merchant, document template or target page can appear many times.

> ❌ **Anti-pattern — tuning on the test set:** picking the threshold by looking at test results. **Why people do it:** it is the quickest way to hit a target number. **Consequence:** the reported performance is optimistic and will not hold in production. **Fix:** choose the threshold on validation data and freeze it before the final test.

> ❌ **Anti-pattern — random splits across related records:** splitting rows at random when many rows share a customer, merchant or template. **Why people do it:** random splitting is the default. **Consequence:** the model sees near-duplicates in test, so accuracy and calibration look better than they are. **Fix:** group related records into the same split.

## The Economic Model Is Simple
If the decision model handles a fraction c of requests, the new cost is roughly the decision model's cost on all requests, plus the frontier model's cost on the remaining (1 - c) of requests, plus escalation and retry overhead:

```javascript
new_cost
  = decision_model_cost_on_all_requests
  + frontier_model_cost_on_(1 - c)_requests
  + escalation/retry overhead
```

The architecture wins when the first term is much smaller than the frontier call it avoids, and the quality loss on covered traffic stays inside your tolerance. This is why tiny pre-processors are more interesting than "use a smaller chat model everywhere." They need to solve one repeated decision boundary very cheaply.

> ❌ **Anti-pattern — a smaller chat model everywhere:** replacing the frontier model with a smaller general model on every call. **Why people do it:** it is a simple swap. **Consequence:** quality drops across all traffic, with no measured boundary and no way to pick a threshold. **Fix:** solve one repeated decision boundary with a targeted scorer and keep the frontier model for escalations.

## What I Would Build in Production
The author would start with one high-volume decision point that already has clean labels. For example, an invoice arrives, deterministic fields are extracted, and a custom scorer decides: approve at 0.98 goes to the normal workflow, hold at 0.94 goes to the review queue, and anything uncertain goes to the frontier LLM:

```javascript
invoice arrives
   |
   v
extract deterministic fields
   |
   v
custom decision scorer
   |
   +-- approve 0.98 ---> normal workflow
   |
   +-- hold 0.94 ------> review queue
   |
   +-- uncertain ------> frontier LLM
```

The author recommends running it in shadow mode first:

> 📋 **Checklist — production rollout**
> - [ ] Log the scorer's probabilities next to the existing LLM decision.
> - [ ] Build calibration plots.
> - [ ] Measure risk versus coverage.
> - [ ] Pick the threshold from actual business tolerance.
> - [ ] Then turn on automation for the highest-confidence slice.

Once that works, repeat the pattern at the next decision point. The article's long-term picture is a distributed software system: deterministic code where possible, tiny learned decision functions where useful, retrieval and rules for state, and frontier reasoning only where uncertainty or novelty justifies the cost. The frontier model remains the most capable component, but it stops being the default branch.

## 🔑 Key takeaways
- Repeated, constrained agent decisions with clean historical labels are the first target for a small scorer.
- A scorer that returns a probability per known option avoids generating a choice token by token.
- The confidence threshold, not the model choice, sets how much traffic the cheap model may handle.
- Calibration matters more than top-1 accuracy, because the threshold only has meaning if confidence matches correctness.
- Thresholds must be chosen on validation data, and splits must group related records to avoid leakage.
- The economics work only if the decision model's cost on all requests is far below the frontier calls it avoids.
- Vendor benchmarks (Jev's latency and cost claims) should be validated on your own workload before you rely on them.

## 🏛️ For Kenji
Many of the decisions in your own AI governance and agent designs (routing, guardrails, tool selection) are repeated and logged, which fits the gate pattern well. This is my interpretation, not the author's: the question it raises for an architect is which decision boundary in your system has enough clean historical outcomes to calibrate, and who owns the threshold once it starts trading error for cost.

## 📚 Reading plan
### From the article
- [embedded post on flight search](https://x.com/gregpr07/status/2100411066966749359) — the vendor example of Jev finding flights in about 7 seconds.
- [embedded post on outreach scoring](https://x.com/romanbuildsaas/status/2100891604735099103) — the example of scoring thousands of messages for under $0.1.
- [jevlike repository](https://github.com/vinnylarouge/jevlike.git) — the open-source starter model and its training and predict CLI.
### To go further
- RouteLLM (learned LLM routing) — the article's reference for cost reduction from learned routers.
- Selective classification (risk-versus-coverage trade-off) — the research background for abstaining when unsure.
- Calibration and expected calibration error — how to check whether confidence scores mean what they say.
