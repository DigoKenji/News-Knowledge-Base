---
type: Article Summary
title: 'Jev vs LLMs: Do AI Agents Really Need a Language Model for Every Decision?'
description: Agents should route routine judgments to typed decision models and reserve LLMs for tasks that truly need generated language.
resource: https://medium.com/@ravjot03/jev-vs-llms-do-ai-agents-really-need-a-language-model-for-every-decision-fbf9240a28e3
tags:
- ai-ml
- llm-engineering
- multi-agent-orchestration
- inference-tradeoffs
- llm-as-a-judge
- structured-decision-model
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@ravjot03/jev-vs-llms-do-ai-agents-really-need-a-language-model-for-every-decision-fbf9240a28e3
  title: 'Jev vs LLMs: Do AI Agents Really Need a Language Model for Every Decision?'
  author: Ravjot Singh
status: stable
source_url: https://medium.com/@ravjot03/jev-vs-llms-do-ai-agents-really-need-a-language-model-for-every-decision-fbf9240a28e3
source: Medium
source_type: medium
published: '2026-09-22'
domain: ai-ml
subdomain: llm-engineering
concepts:
- multi-agent-orchestration
- inference-tradeoffs
- llm-as-a-judge
- structured-decision-model
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d811ca5cbf40ffc57c3cd
---

# Jev vs LLMs: Do AI Agents Really Need a Language Model for Every Decision?

**Source:** https://medium.com/@ravjot03/jev-vs-llms-do-ai-agents-really-need-a-language-model-for-every-decision-fbf9240a28e3
**Author:** Ravjot Singh
**Date:** 2026-09-22
**Concepts:** [Multi-agent orchestration](/concepts/ai-ml/multi-agent-orchestration.md) · [Inference throughput/latency/cost](/concepts/ai-ml/inference-tradeoffs.md) · [LLM-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md) · [Structured decision model](/concepts/ai-ml/structured-decision-model.md)

---

## 🎯 Core thesis
Most AI agent decisions (which tool to call, whether a result is relevant, whether to retry, where to route) are judgments that software needs as typed answers, so they should go to a specialized decision model and not to a generative LLM, which should be kept for the work that actually requires generated language.

## 🧭 Why the author wrote it
The author starts from a habit that has become default: every routing, classification and evaluation step is sent to an LLM that writes text, which is then parsed back into a decision. The article asks what changes if the most useful unit is a decision rather than a string, and argues that the architecture of agents would become more like ordinary software with a fuzzy decision layer.

## Opening: the habit of calling an LLM for every choice
The opening lists a run of ordinary questions (answer a customer, classify a ticket, pick a tool, check relevance, decide on a retry, evaluate the previous LLM) and notes that each one is answered by calling a language model. The author admits this works but asks why a system whose main strength is generating language should be used to answer questions with a small set of possible answers. The point of the opening is the mismatch between the job and the tool: "Should this ticket go to Billing, Technical Support, or Fraud?" needs one of three labels, not an essay.

The author also frames the rest of the article around a claim about the future: systems may use different kinds of intelligence for different jobs, so the question is not whether Jev itself matters but whether the architectural idea behind it does.

## First: What Exactly Is Jev?
Jev was introduced by TypeSafe AI on 2026-09-15. TypeSafe describes it as the first "System One Model", a model built to make fast, structured decisions that software can consume directly. The author contrasts the usual shape with the shape Jev is designed around, and quotes TypeSafe's summary: "Decisions, not strings."

```javascript
input → generated text
```

```javascript
state + question → decision + probability
```

**What this does:**
- The first line is the usual flow: an input goes in and free text comes out, so the software must interpret that text.
- The second line is Jev's flow: the input is state plus a typed question, and the output is a decision with a probability attached.
- The difference is in the output type, not in the input, which is why the author calls it architectural rather than cosmetic.

The author stresses that this is an architectural difference, not just a formatting one. According to TypeSafe's documentation, Jev takes some state (a customer message, application state, an agent trajectory, a retrieved document or structured data) and evaluates typed questions against it.

### Choice, Score and Noul primitives
Jev exposes three primitives. **Choice** selects one of a set of predefined alternatives. **Score** rates something against an ordered rubric. **Noul** returns the probability that a statement is true. Several questions can be evaluated against the same state in parallel.

> 🔎 Added context: "Noul" is the article's name for this primitive, as written. The article does not explain the term further, so treat it as the article's label for a probability-of-truth question.

### Parallel typed questions on one state
The ticket example shows how this looks in use. The customer writes "My account was charged twice this morning. I need this fixed immediately." Instead of asking an LLM for a JSON object with department, priority, churn risk and escalation, the software poses four typed questions about the same message: a department Choice, an urgency Score from 1 to 5, a human_escalation Noul, and a churn_risk Score from 1 to 5. The software then receives decisions it can use directly, with no customer-facing paragraph, no reasoning essay, no JSON prompt engineering and no output parser.

```javascript
State:
"My account was charged twice this morning..."
Questions:
department:
Choice [billing, technical, account, fraud]
urgency:
Score [1...5]
human_escalation:
Noul
churn_risk:
Score [1...5]
```

**What this does:**
- `State:` gives the input once, so every question is answered against the same context.
- `department` uses `Choice` with a closed list, which removes the possibility of an invented department name.
- `urgency` and `churn_risk` use `Score` on a bounded scale, so the answer is a rank rather than free text.
- `human_escalation` uses `Noul`, a yes/no judgment expressed as a probability, which the software can threshold.
- The model's job is "simply to judge", in the author's phrase. The software keeps the control flow.

## So Is Jev an LLM?
The author says not in the usual sense. TypeSafe positions Jev as a different model class rather than a smaller chat LLM. Traditional LLMs are optimized for language generation: given a prefix, they predict the next token, one after another. That is what makes them flexible enough to write Python, summarize a contract, generate SQL or compose an email.

The author names the cost of that flexibility. When software needs the following, an extremely flexible text generator is used to produce a very constrained answer.

```javascript
{
  "priority": "high"
}
```

**What this does:**
- The object has one field with two allowed values in practice, `priority` and a label such as `"high"`.
- The software needs only that label, so the surrounding generation is overhead the author is pointing at.
- The example is the constrained answer the author contrasts with open-ended text generation. Jev makes the opposite tradeoff: it gives up free-form generation and specializes in bounded decisions.

### RLCD calibrated training
TypeSafe says it built a separate model architecture, a parallel sampling approach and a training method it calls Reinforcement Learning for Calibrated Decisions (RLCD). The author notes that TypeSafe has not publicly disclosed everything about the internals, so claims about how Jev works should be treated accordingly. The interface, the author argues, shows the philosophy: an LLM tries to determine what to say, while Jev tries to determine what software should believe about a question.

## The Real Difference: Generation vs Judgment
The author rejects the simple framing "LLM = text and Jev = JSON". Modern LLMs already support structured outputs from OpenAI, Anthropic, Google and others, and tool calling has made integration with software much more reliable. The author's point is that structured output changes the format of an LLM's answer but does not necessarily change the job the model was optimized for.

The agent questions listed (which tool to call, whether it succeeded, whether a document is relevant, whether to retry, whether the request is satisfied, whether another agent should handle it, whether a result is safe, whether a more capable model is needed) are decisions, not writing tasks.

### Generation pipeline versus decision pipeline
The author shows the usual implementation and the proposed alternative as two pipelines.

```javascript
Agent state
     ↓
Prompt LLM
     ↓
Generate tokens
     ↓
Generate structured response
     ↓
Parse response
     ↓
Validate schema
     ↓
Execute decision
```

```javascript
Agent state
     ↓
Decision model
     ↓
Probability / choice / score
     ↓
Execute decision
```

**What this does:**
- The first pipeline has four steps between the state and the action (generate, structure, parse, validate) that exist only because the model produces text.
- The second pipeline removes those steps: the model's output is already a choice, score or probability.
- Each removed step is a place where the system can fail: malformed text, a schema the model ignored, or a parser that disagrees with the model.
- The author's point is that the decision model does not make the agent smarter by itself; it removes a translation layer that the agent does not need.

> ❌ **Anti-pattern — generating tokens to make a decision:** Prompting an LLM to write a structured response, then parsing and validating it, for a question with a small set of answers. **Why people do it:** the LLM is already in the stack and handles ambiguity well, so it becomes the default for every step. **Consequence:** each decision pays for full generation, parsing and validation, and failures appear at the parsing boundary. **Fix:** route closed-set judgments to a decision model or a classifier, and keep generation for text the user reads.

## Why This Matters More for Agents Than Chatbots
For a chatbot, a two-second model call may be acceptable because a person is reading the answer anyway. For an autonomous agent the cost is different. The author gives an example of a task requiring 40 steps, during which the agent might make 12 tool-selection decisions, 8 relevance checks, 6 retry decisions, 5 safety checks, 5 completion checks and 4 routing decisions. That is dozens of model calls for one task, and across thousands or millions of agents the bottleneck becomes the many small decisions between actions rather than the final answer.

The author cites LangChain, which made a similar observation after Jev's release: agent loops repeatedly use models to decide what to do, evaluate results and decide what happens next, and even with tool calling and structured outputs those extra calls add latency and cost.

### Today's Agent Architecture
In the common design, one LLM decides what to do, a tool runs, another LLM call evaluates the result, and then another action follows. The author notes that the LLM is being used as planner, router, classifier, evaluator, verifier, writer, tool selector and conversation engine at the same time. The author calls this convenient but doubts it is the architecture to use at scale.

```javascript
             ┌──────────────┐
             │     LLM      │
             └──────┬───────┘
                    │
             decide what to do
                    │
                    ▼
                 Tool
                    │
                    ▼
             ┌──────────────┐
             │     LLM      │
             └──────┬───────┘
                    │
             evaluate result
                    │
                    ▼
             another action
```

**What this does:**
- The same LLM box appears twice, once to choose the action and once to judge the result, which is the source of the extra per-step calls.
- The arrows show that each judgment sits on the critical path between two tool actions, so its latency is paid every step.
- The diagram has no separate decision role; the LLM plays the decision role inside each generation.
- It is the baseline the author's next diagram changes.

## A Different Architecture: Jev + LLM
The author proposes splitting responsibilities. A decision layer (Jev) sits between agent state and the actions: it picks a tool, routes to escalation, or checks whether the task is complete. Only when generation is needed does a generative LLM produce the final response. Code handles deterministic business rules, and tools perform actions.

```javascript
                      USER
                        │
                        ▼
                   Agent State
                        │
                        ▼
                ┌─────────────┐
                │     Jev     │
                │  Decision   │
                │    Layer    │
                └──────┬──────┘
                       │
        ┌──────────────┼──────────────┐
        │              │              │
        ▼              ▼              ▼
      Tool A          Tool B        Escalate
        │              │
        └───────┬──────┘
                ▼
           New State
                │
                ▼
              Jev
                │
         Is task complete?
           /          \
         No            Yes
         │              │
     Continue           ▼
                  Generative LLM
                        │
                        ▼
                  Final Response
```

**What this does:**
- The first Jev box is the routing decision after the user request, choosing among tools or escalation.
- The second Jev box is a completion check on the new state, which decides whether to continue or to generate a final answer.
- The Generative LLM appears only once, at the end, where text is actually needed.
- The author's summary is that this looks more like traditional software architecture than a large prompt wrapped around an LLM.

## Think of Jev as a "Fuzzy If Statement"
The article's most useful mental model is a semantic if statement. Ordinary code handles deterministic rules well:

```py
if transaction_amount > 10000:
    request_review()
```

Many real decisions have no clean rule:

```py
if customer_message_indicates_fraud:
    send_to_fraud_team()
```

**What this does:**
- The first block is a deterministic rule: a numeric threshold on a field that code can read directly.
- The second block is the hard case: `customer_message_indicates_fraud` is a semantic judgment, with no obvious ordinary-code implementation.
- The author notes that hundreds of keyword rules would still be brittle, because language is messy.

LLMs handle that ambiguity well, which is why companies started using them as classifiers. Jev's proposition is to keep the fuzzy semantic judgment but expose it through an interface that behaves like software.

```py
if fraud_probability > 0.90:
    route_to_fraud()

elif fraud_probability > 0.65:
    request_human_review()
else:
    continue_normal_flow()
```

**What this does:**
- The condition is a probability, so the fuzzy judgment becomes a value that ordinary code can compare.
- The 0.90 threshold routes to the fraud path, which is the high-confidence action.
- The 0.65 threshold sends the middle band to human review rather than to an automatic action.
- The `else` branch continues the normal flow, so the software keeps control of what happens for each band.

The author's summary: AI handles ambiguity, and software retains control.

> 🔎 Added context: the thresholds 0.90 and 0.65 are illustrative values in the article's example, not values TypeSafe recommends.

## The Confidence Layer Might Be More Important Than the Decision
Jev does not only return an answer; it also exposes probabilities and confidence for its decision types. That enables architectures built around uncertainty. The author shows a clear case, where billing is 0.97, technical is 0.02 and fraud is 0.01, and a less clear one, where billing is 0.42, technical is 0.36 and fraud is 0.22. The first can be routed safely. The second should behave differently, perhaps with a stronger model, a clarifying question to the user, or a human.

```javascript
billing issue: 0.97
technical issue: 0.02
fraud: 0.01
```

```javascript
billing issue: 0.42
technical issue: 0.36
fraud: 0.22
```

**What this does:**
- The first block is a confident decision: one label holds almost all the probability, so routing is safe.
- The second block is a near tie between billing and technical, with fraud not far behind.
- The same classifier output gives two different actions depending on the margin, which is the point of the example.
- The author's rule: autonomy should depend on uncertainty, risk and reversibility, not on "the LLM said something".

```javascript
High confidence
      ↓
Autonomous action
      ↓
Medium confidence
      ↓
Stronger model / more context
Low confidence
      ↓
Human review
```

**What this does:**
- The flow maps confidence bands to actions, from autonomous to stronger model to human.
- Each band has a different cost of error, so each has a different action.
- It makes the uncertainty explicit, which a plain text answer does not.
- The author calls this "much closer to how production AI systems need to operate".

> ❌ **Anti-pattern — executing whatever the LLM said:** Treating a model's output as authority and executing it, with no regard for confidence. **Why people do it:** a single answer is simpler to code and looks decisive. **Consequence:** low-confidence and high-risk decisions run with the same autonomy as easy ones. **Fix:** gate actions on probability, risk and reversibility, and send uncertain cases to a stronger model or a person.

## But Doesn't Structured Output Already Solve This?
The author expects this objection and treats it as reasonable. A Pydantic-style class with a `Literal` route, an integer priority and a boolean escalate flag gives reliable structured output from an LLM. The author's reply is that "structured output" hides several different problems, and Jev addresses four of them.

```py
class Decision(BaseModel):
    route: Literal["billing", "technical", "fraud"]
    priority: int
    escalate: bool
```

**What this does:**
- `route` is a `Literal`, so the schema restricts the answer to three labels.
- `priority` is an `int`, which fixes the type but not the scale or the meaning of the number.
- `escalate` is a `bool`, which hides how certain the model was when it chose true or false.
- The author's argument is that a schema fixes the shape of the answer but not its speed, cost or calibration.

### 1. Schema reliability
Structured output helps ensure `route = billing` rather than a sentence such as "I believe this request should probably be sent to the billing team". Jev addresses this natively by restricting the output space to the declared choices.

### 2. Latency
Generative models produce sequences, one token after another, so the time grows with the length of the answer. Jev evaluates multiple questions in parallel. TypeSafe currently reports end-to-end latency of roughly 70 to 500 milliseconds for Jev.

### 3. Cost
TypeSafe currently lists Jev at $0.042 per million input tokens, with no separately billed generated output. For occasional decisions this difference may be irrelevant; for millions or billions of agent decisions, the author says, it becomes architectural.

> 🔎 Added context: "per million input tokens" is a unit of pricing, so the cost of a decision scales with how much state is sent to the model, not with how much text it writes.

### 4. Calibration
A structured LLM response might say `{"fraud": true}`. The harder question is how certain the system should be about that answer. The author says Jev is explicitly trained around calibrated decisions rather than eloquent generation, according to TypeSafe. That does not make every decision correct, but it changes what the model is optimized to provide.

> 🔎 Added context: a calibrated model is one whose stated probabilities match how often those outcomes actually occur. The article does not give calibration measurements for Jev.

## Wait — "Zero Hallucinations"?
TypeSafe's marketing claims zero hallucinations, and the author separates two meanings. Because Jev's outputs are predefined, it cannot invent a new output type: if the choices are billing, technical and fraud, it cannot return `super-premium-customer-success-department`. In that sense, schema or type hallucination disappears.

It can still be wrong. It can confidently classify a fraud request as billing. TypeSafe itself separates its type-safety guarantee from broader correctness and acknowledges uncertainty in some of its evaluations. The author's wording is that Jev is "incapable of producing an invalid output type, rather than incapable of making an incorrect decision", and the second claim would be much stronger and much harder to prove.

> ❌ **Anti-pattern — equating valid type with correct decision:** Reading "the output always has a valid type" as "the output is right". **Why people do it:** a marketing claim about hallucinations is easy to read as a claim about accuracy. **Consequence:** systems skip accuracy evaluation and trust confident wrong labels. **Fix:** measure decision accuracy on your own data, separately from schema validity.

## The Performance Claims Are Impressive — But Very New
TypeSafe reports large gains on its System One workflows, with Jev about 193.6 times faster and 444.6 times cheaper in the comparison workflows. The author says the numbers should be read carefully. TypeSafe itself says the results are likely at the higher end of real-world gains, and the workflows were created by members of its own model capabilities team, so evaluation bias cannot be fully excluded. Jev was also released only days before the article.

The author does not conclude that LLMs are "400 times too expensive". The more durable point is that specializing a model for the actual computational task may produce large efficiency gains. The author says that principle is larger than Jev itself.

> 🔎 Added context: the article's "400×" appears in the author's own rhetorical line and is not one of TypeSafe's reported figures; the reported figures are 193.6× and 444.6×.

## This Could Change Multi-Agent Systems
The author finds the concept most interesting in multi-agent systems. Take a system with a Customer, Product, Order, Refund, Fraud, Knowledge and Human Escalation agent. Something must decide which agent handles a request. Today that is often another LLM call, then another to pick a tool, another to evaluate the answer and another to check completion. The author's line: we have built an organization where every employee is a highly paid essay writer, including the receptionist who only needed to point someone toward the right room.

A decision model could act as the routing fabric between agents, sitting in front of the agents and again after each one to ask whether the task is complete.

```javascript
                Incoming Request
                        │
                        ▼
                 Decision Layer
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   Order Agent      Product Agent    Refund Agent
        │               │               │
        └───────────────┼───────────────┘
                        ▼
                 Decision Layer
                        │
                 Is task complete?
```

**What this does:**
- The first Decision Layer routes the incoming request to one of several specialist agents.
- The three agents shown are examples from the list; the same pattern applies to the other agents the author names.
- The second Decision Layer converges the outputs and asks whether the task is complete.
- The author's point: different models perform different types of cognition, which is the layered intelligence he expects agent engineering to move toward.

## Where Jev Could Actually Be Useful
The author argues the obvious use cases are not glamorous, and that is why they matter. The article lists ten: agent routing, tool selection, retrieval filtering (is a document relevant enough to include in context), guardrails (does an output violate a policy), escalation (should a person take over), model routing (does a request need an expensive reasoning model or a cheaper one), quality evaluation (does a generated response meet the request), retry decisions (did a tool call fail in a recoverable way), customer service routing (queue, priority, sentiment or workflow) and fraud and risk checks (does the state justify more verification). The author's point is that these are the invisible decisions repeated hundreds of times inside production software.

## Where You Probably Shouldn't Use Jev
The author is clear about what Jev does not replace. Writing an email, explaining why a SQL query fails, summarizing ten documents, generating code and open-ended reasoning over an ambiguous problem all still belong to an LLM or a capable reasoning model. Jev is for questions where the answer is A or B, how much, yes or no, which route, and how confident we are. The author concludes that "Jev vs LLM" is the wrong framing, and that the better architecture is Jev plus an LLM plus deterministic code plus tools.

## Maybe the Future Isn't One Foundation Model
The author observes that the industry has often aimed for an increasingly powerful foundation model that does everything, and that this has produced remarkable systems. Production engineering, however, tends to specialize. Databases did not eliminate caches, cloud did not eliminate CDNs, CPUs did not eliminate GPUs and microservices did not make every service identical. AI may follow the same path.

The author lists the possible specialized parts: generative models for language and content, reasoning models for hard multi-step problems, decision models for routing, scoring and classification, embedding models for semantic representation, vision models for perception, code for deterministic logic and tools for actions. An intelligent application is then an orchestration layer across these capabilities, not "everything → giant LLM".

## Why the Name "Jev"?
TypeSafe says the name honours economist William Stanley Jevons and the Jevons paradox. The simplified idea is that making a resource more efficient to use can increase total consumption of it rather than reduce it. The author applies this to intelligence: an intelligent decision that costs seconds and meaningful money is used occasionally, while one that costs milliseconds and almost nothing might be used once per user request, once per agent step, several times per step, or continuously throughout an application.

> 🔎 Added context: the Jevons paradox was first described for coal use in 19th-century England, where cheaper and more efficient engines led to more total coal consumption. The article itself gives only the simplified statement above.

The author describes this as the bigger bet: not merely making existing AI cheaper, but making intelligence cheap enough that software uses it where AI previously made no sense.

## My Take: The Important Idea Isn't Jev. It's the Separation of Intelligence.
The author says Jev is extremely new, its strongest benchmark claims need broader independent validation, and "System One Model" is TypeSafe's terminology rather than an established industry standard. Jev might become an infrastructure primitive, LLM providers might build similar capabilities into their models, open-source alternatives might appear, or another architecture might win.

The author's real question is why one model should perform every cognitive operation in an AI system. Writing and deciding differ, as do reasoning and routing, and generating an answer and verifying it. The article ends with a future stack in which user or event flows into a decision layer, which dispatches to code, tools and agents; agents may use a reasoning LLM, a decision or verify step and a generative LLM before output. The author's closing claim is that LLMs are not disappearing but may stop being asked to do every job, and that the most useful contribution of Jev may be prompting AI engineers to ask: does this step actually need a language model, or does my software simply need a decision?

```javascript
                ┌──────────────────┐
                │   User / Event   │
                └────────┬─────────┘
                         ▼
                ┌──────────────────┐
                │  Decision Layer  │
                │      Jev         │
                └────────┬─────────┘
                         ▼
            ┌────────────┼────────────┐
            ▼            ▼            ▼
          Code         Tools        Agents
                                       │
                                       ▼
                              ┌────────────────┐
                              │ Reasoning LLM  │
                              └───────┬────────┘
                                      ▼
                              Decision / Verify
                                      │
                                      ▼
                              Generative LLM
                                      │
                                      ▼
                                   Output
```

**What this does:**
- The top of the diagram is the trigger, which can be a user message or a system event.
- The Decision Layer sits first, deciding whether the event goes to code, to tools or to agents.
- Agents may call a reasoning LLM, then a decision or verification step, then a generative LLM to produce output.
- The author's point is that the generative LLM is the last step, not the first, so most of the stack is ordinary decision and tool logic.

## 🧠 Mental models
### Fuzzy if statement
**The model:** Jev acts as an `if` statement whose condition is a semantic judgment, returning a probability that ordinary code can compare against thresholds.
**How to apply it:** Replace a keyword-rule or prompt-and-parse step with a typed question, then set thresholds for each action band, with a human path for the middle band.

### Generation vs judgment
**The model:** An LLM is optimized to produce text, while a decision model is optimized to produce a bounded answer, so the two are different jobs even when both return structured data.
**How to apply it:** For each step in an agent, ask whether the output is text the user reads or a choice the software acts on, and route accordingly.

### Confidence-gated autonomy
**The model:** The right action depends on the model's confidence, the risk of a wrong action and whether the action can be reversed.
**How to apply it:** Map probability bands to actions, for example autonomous action above a high threshold, a stronger model in the middle and human review below a low one.

### Specialization
**The model:** Infrastructure tends to specialize over time, with each workload finding an architecture built for it.
**How to apply it:** Treat the foundation model as one component among several, and assign each cognitive operation to the cheapest component that can do it reliably.

## 🔑 Key takeaways
- Most agent steps, such as tool selection, relevance checks, retries and routing, are closed-set judgments that do not need generated text.
- Jev's interface returns Choice, Score and Noul answers over shared state, evaluated in parallel, so software receives decisions rather than strings to parse.
- The article's example reports latency of about 70 to 500 milliseconds and a price of $0.042 per million input tokens, both from TypeSafe.
- Probabilities allow different actions by confidence, so a 0.97 routing decision and a 0.42 near-tie should not trigger the same behavior.
- A type-valid output rules out invented labels but does not rule out wrong decisions, so accuracy has to be measured separately.
- TypeSafe's 193.6× speed and 444.6× cost figures come from workflows created by its own team and were released only days before the article, so they need independent validation.
- The article's practical question for any agent design is whether a step needs a language model or only a decision.

## 🏛️ For Kenji
Your staff-level work on agent systems and AI governance is where this applies most directly: a decision layer with calibrated probabilities is a natural place to attach risk thresholds, escalation rules and audit records. This is your interpretation, not the author's. The question it raises for you: which of the decisions in your current agent designs would you be comfortable gating on a probability, and how would you measure whether that probability is calibrated on your own data?

## 📚 Reading plan
### From the article
- The article cites no links. Its sources are named only in text: TypeSafe AI (the Jev announcement and documentation), LangChain (a post on agent loops and model calls after Jev's release), and William Stanley Jevons (the Jevons paradox).
### To go further
- Structured outputs and tool calling in LLM APIs — to compare the schema-constrained approach the author treats as the baseline.
- Calibration of probabilistic classifiers, including reliability diagrams — to test whether a Noul-style probability means what it says.
- LLM-as-a-judge evaluation — for the quality and verification steps the author says are often done by another LLM call.
- Multi-agent orchestration patterns — for routing and completion checks between specialist agents.
- Jevons paradox — for the economic argument about cheaper intelligence increasing its use.
