---
type: Article Summary
title: Jev Clearly Explained
description: 'Bounded decisions should be scored rather than generated: one forward pass yields calibrated probabilities code
  can threshold, though correctness still needs independent evaluation.'
resource: https://medium.com/gitconnected/jev-clearly-explained-3203d6d48de3
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-25T06:47:00-03:00'
sources:
- resource: https://medium.com/gitconnected/jev-clearly-explained-3203d6d48de3
  title: Jev Clearly Explained
  author: Level Up Coding (Gitconnected)
status: stable
source_url: https://medium.com/gitconnected/jev-clearly-explained-3203d6d48de3
source: Level Up Coding (Gitconnected)
source_type: medium
published: '2026-09-21'
domain: ai-ml
concepts:
- structured-decision-model
- calibration
- tokenization
- large-language-model
- ai-agent
- llm-as-a-judge
- inference-tradeoffs
- model-evaluation
- logit
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/423baf74172d81139ef4f54799137a98
---

## 🎯 Core thesis
When code already knows the possible answers to a question, a model should score those answers in one forward pass instead of writing them out, and the resulting probabilities should drive the branching logic.

## 🧭 Why the author wrote it
Fareed Khan starts from a common pattern: an application asks a full generative model for a one-word judgment, then parses the prose back into a label, paying for token-by-token generation on every call. He presents Jev, a closed decision model from TypeSafe AI that returns typed answers with probabilities and never writes text. If the thesis holds, agents and pipelines can make many small decisions faster and more cheaply, with the decision thresholds kept in ordinary code.

## Opening: Scoring a Decision Instead of Writing It
The article opens with a fraud check on an invoice. A generative model would answer in a sentence such as "this invoice appears to be legitimate", which the application must then interpret and hope the wording never breaks its parser. A scoring model instead returns three numbers for three declared outcomes, fraud 0.07, clean 0.88 and review 0.05, in one pass with no sentence to parse. The author uses this single example to show the whole idea, from the first state to the line of code that branches.

### Typed decision
A typed decision is an answer restricted to a list the developer declared in advance, such as fraud, clean or review. Because the list is fixed, the output can be a probability per option rather than free text.

> 🔎 Added context: "Closed weights" means the model's parameters are not published, so outside readers cannot inspect them directly.

## The Problem We Keep Paying Full Price For
Most of what software needs from a language model is a judgment, not a paragraph: whether a ticket is urgent, which model should handle a request, whether a shell command is dangerous, whether a passage answers a question. Each such judgment has one of a handful of known answers. Teams still send every question to a full generative model and parse the result back into the label they wanted. Inside an agent loop the cost compounds, because the agent makes these small calls constantly.

```javascript
while not task_done:
    action = llm(context)          # full generation, just to pick a tool
    result = run_tool(action)
    context += result
    task_done = llm(context)       # full generation again, just to check "done or not"
```

**What this does:**
- Runs a loop in which every iteration calls the model twice for decisions that need only a few bits.
- Calls `llm` once to choose the next tool and again to test whether the task is done.
- Shows that the cost of each decision is a full generation, token by token, even though the answer is one of a few labels.

The gap comes down to how many times the model must run. Writing T tokens costs T sequential passes through the model, because each pass depends on the token the previous pass wrote. A bounded answer needs only one pass to read the scores for the declared options.

> ❌ **Anti-pattern — generate-then-parse:** Asking a generative model for a sentence or JSON object just to recover a label that the code could have scored. **Why people do it:** It is the default interface, and it works for open-ended tasks. **Consequence:** Repeated sequential generation in agent loops, plus a parser that can fail when the wording changes. **Fix:** When the answer set is known, ask for a score over those options.

Jev's bet is that generation is the wrong interface once the outcomes can be listed ahead of time. For that case the author argues the model should score, not write.

### Sequential versus single-pass cost
Generation is sequential because each token depends on the one before it. Scoring has no such dependency, so the model's work for a bounded decision happens in a single pass whose output is read directly.

## What Jev Is
The article describes Jev as a semantic decision engine. The caller sends state, meaning the current situation, plus one or more questions about it. Jev returns a typed answer with a probability and never a sentence. The picture at the top of the post shows this flow: state and questions go in, every question is evaluated in the same pass, a confidence threshold decides the branch, and ordinary code runs it.

Every question declares its answer shape up front. Jev supports three primitives:
- **Choice** picks one option from a list the developer defines and returns a probability for every option, not only the winner.
- **Score** places the input on an ordered scale the developer defines, such as a missed-to-resolved scale, and returns a value such as 1.74.
- **Noul** answers a yes-or-no question with the probability that it is true. The article notes this is TypeSafe's own name for the boolean-flavored primitive.

Every answer is a number between 0 and 1 that code can act on directly.

### Score as a probability-weighted average
Score is not a rounding artifact. It is the probability-weighted average across every rung of the scale, not just the top pick. Missed, partial and resolved each carry a probability, and their weighted sum is 1.74. A certain "resolved" would land near 2.0, so 1.74 shows a strong lean toward resolved with real weight on partial, information a single label discards.

Here is a request for the support ticket the author follows through the post.

```json
{
  "model": "jev-latest",
  "state": "I was charged twice for the same subscription.",
  "questions": {
    "urgent": {
      "type": "noul",
      "instructions": "Does this need attention right now?"
    },
    "team": {
      "type": "choice",
      "instructions": "Which team should own this ticket?",
      "criteria": {
        "billing": "Charges, invoices, refunds",
        "technical": "Product bugs and errors",
        "account": "Login and access problems"
      }
    }
  }
}
```

**What this does:**
- Sends one state string and two questions in a single request.
- Declares `urgent` as a yes-or-no question and `team` as a choice among three named criteria.
- Shows that the answer shape is fixed before the model runs.

The response contains no prose, only labels and numbers.

```json
{
  "urgent": {"probability": 0.61},
  "team": {
    "choice": "billing",
    "probabilities": {"billing": 0.91, "technical": 0.06, "account": 0.03}
  }
}
```

**What this does:**
- Returns a probability of 0.61 that the ticket is urgent.
- Returns billing as the choice, with probabilities 0.91, 0.06 and 0.03 across the three teams.
- Shows that `team` can only ever be one of the three declared names, so the model cannot invent a fourth team.

This differs from structured output. A structured-output call still decodes one token at a time, and the schema only constrains the shape of those tokens. Jev's scoring path never enters the decode loop and reads the answer from a single forward pass. The article lines the two approaches up across output, sampling, best fit, control and failure mode, and they differ in every row. Structured output stays autoregressive under its schema, while fixed-answer scoring reads its answer before decoding begins.

## Where That Probability Comes From
A tokenizer turns the prompt into token IDs. The model then produces one vector for the next position, holding a logit, a raw score, for each token in the vocabulary, which means tens of thousands of numbers. Ordinary generation turns those logits into probabilities with softmax over the whole vocabulary, after dividing by a temperature that sharpens or flattens the distribution. Generation repeats this cycle, appending one token at a time, until a stop token or a length limit ends it.

> 🔎 Added context: Softmax converts a list of raw scores into positive numbers that sum to 1. A temperature below 1 sharpens the distribution, and a temperature above 1 flattens it.

Restricted scoring needs only the first vector, and only three of its entries. The author labels the answers A, B and C in the prompt and stops the prompt where the model would begin writing the label. The model still produces its usual vector, but the code keeps only the logits for A, B and C and applies softmax to those three. The denominator shrinks from the whole vocabulary to the three declared labels.

### Restricted softmax
Restricted softmax is the same softmax formula with the denominator limited to the declared options. It answers how the model splits its preference among exactly the choices the code allowed, not what the probability is across the entire vocabulary. With logits A = 6.8, B = 4.3 and C = 3.1, the restricted softmax gives billing 0.90, technical 0.07 and account 0.02. The article describes this as swapping a sum over roughly 150,000 terms for a sum over 3.

### Single-letter labels
Labels are single letters because a word may span several tokens. The word "billing" might be one token for one tokenizer and several for another, and "technical support" will usually span more than one position. A single letter always occupies exactly one output position, and its meaning lives in the prompt text beside it.

> ❌ **Anti-pattern — multi-token labels:** Spelling out full option names as the scored answer. **Why people do it:** The words read naturally. **Consequence:** The token count changes with the tokenizer, so the scored position is not fixed. **Fix:** Use one-token labels such as single letters, and describe each one in the prompt.

### Leading-space trap
Tokenizers often fold a leading space into a token, so `A` and ` A` can map to two different token IDs. A chat template can insert that whitespace without the developer noticing. The fix is to render the full prompt through the model's own chat template, check the exact continuation it expects, and confirm each label is a single token before trusting a scoring call.

> ❌ **Anti-pattern — unverified label tokens:** Assuming the label-to-token mapping without checking it against the actual chat template. **Why people do it:** The mapping looks obvious. **Consequence:** Scores are read from the wrong positions and look plausible anyway. **Fix:** Tokenize each label through the target model and fail if any label is not one token.

### Exhaustive choices
If the declared choices are not exhaustive, restricted softmax still pushes all the probability mass onto the nearest declared answer. A security incident sent to a router that knows only billing, technical and account is filed confidently under one of those three. Adding an OTHER or ESCALATE option is cheap insurance.

> ❌ **Anti-pattern — closed choice list with no escape:** Offering only the answers you expect. **Why people do it:** Adding an escape option feels like extra work. **Consequence:** Out-of-scope inputs are assigned confidently to the wrong category. **Fix:** Always include an OTHER or ESCALATE option.

## Let's Build the Same Trick Ourselves
Jev is closed, but the same scoring trick already runs in open serving stacks. The author reproduces it with SGLang as the server and Qwen2.5-0.5B-Instruct as the model, building a client named `decide.py` one block at a time. The model server does all the GPU work, and the client talks to it over HTTP.

### Starting the Model Server
Start a clean environment with two packages, then launch the model server. The first launch downloads the model from Hugging Face and keeps it in memory on port 30000 for later requests.

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install "sglang[all]==0.5.10.post1" "requests==2.34.2"
```

**What this does:**
- Creates and activates an isolated Python environment.
- Pins SGLang to 0.5.10.post1 and requests to 2.34.2, so the run is reproducible.

```bash
python -m sglang.launch_server \
    --model-path Qwen/Qwen2.5-0.5B-Instruct \
    --host 127.0.0.1 \
    --port 30000
```

**What this does:**
- Starts the SGLang server, the only process that does GPU work.
- Serves the Qwen2.5-0.5B-Instruct model on 127.0.0.1 port 30000.

### Defining the Choices and Building the Prompt
The first client block defines the labels and the ticket. The dictionary order must survive tokenizing, scoring and mapping back, so the prompt is built from the dictionary rather than retyping the labels.

```py
# decide.py
import json
import requests

BASE_URL = "http://127.0.0.1:30000"
MODEL = "Qwen/Qwen2.5-0.5B-Instruct"

# A is the token I actually score, "billing and payments" is what my
# application does with the answer once it comes back
choices = {
    "A": "billing and payments",
    "B": "technical support",
    "C": "account access",
}
ticket = "I was charged twice for the same subscription."
```

**What this does:**
- Keeps the label-to-meaning mapping in one dictionary that all later steps read.
- Separates the scored label (`A`) from the application meaning it maps to.

```py
# decide.py (continued)
choice_lines = "\n".join(
    f"{label} = {meaning}" for label, meaning in choices.items()
)
prompt = f"""Ticket:
{ticket}
Question:
Which category matches the ticket?
Allowed labels:
{choice_lines}
Return only the label.
Label:
"""
print(prompt)
```

**What this does:**
- Generates the allowed-label lines from the dictionary, so labels are never typed twice.
- Ends the prompt at `Label:`, the position whose vocabulary vector gets read. The model is never asked to generate the token that follows.

```javascript
#### OUTPUT ####
Ticket:
I was charged twice for the same subscription.
Question:
Which category matches the ticket?
Allowed labels:
A = billing and payments
B = technical support
C = account access
Return only the label.
Label:
```

**What this does:**
- Shows the exact prompt the model receives.
- Confirms the prompt stops at `Label:`.

### Resolving the Label Token IDs
Before scoring, the client looks up the real token ID for each label under this tokenizer, rather than assuming one from another model.

```py
# decide.py (continued)
label_token_ids = []

for label in choices:
    response = requests.post(
        f"{BASE_URL}/tokenize",
        json={
            "model": MODEL,
            "prompt": label,
            "add_special_tokens": False,
        },
        timeout=30,
    )
    response.raise_for_status()
    token_ids = response.json()["tokens"]
    if len(token_ids) != 1:
        raise ValueError(f"{label!r} is not a single token: {token_ids}")
    print(f"{label!r} -> {token_ids}")
    label_token_ids.append(token_ids[0])
```

**What this does:**
- Tokenizes each label through the server and keeps the resulting IDs in the same order.
- Raises an error if any label is more than one token, so a bad label stops the run before scoring.

```javascript
#### OUTPUT ####
'A' -> [32]
'B' -> [33]
'C' -> [34]
```

**What this does:**
- Shows that each label maps to exactly one token ID: 32, 33 and 34.
- Confirms that each option occupies a single vocabulary position.

### Scoring the Three Positions
With the IDs confirmed, the decision is one HTTP call.

```py
# decide.py (continued)
response = requests.post(
    f"{BASE_URL}/v1/score",
    json={
        "model": MODEL,
        "query": prompt,
        "items": [""],
        "label_token_ids": label_token_ids,
        "apply_softmax": True,
    },
    timeout=120,
)

response.raise_for_status()
score_response = response.json()

print(json.dumps(score_response, indent=2))

scores = score_response["scores"][0]
```

**What this does:**
- Sends the full prompt as `query`, with an empty string in `items` so the position right after the prompt is scored.
- Names the three label positions to read and sets `apply_softmax` so the readings become probabilities.

```javascript
#### OUTPUT ####
{
  "scores": [
    [
      0.67776233,
      0.310878605,
      0.011359035
    ]
  ]
}
```

**What this does:**
- Returns three probabilities in the same order as the token IDs were passed.
- Shows that no token was generated to produce them.

The raw logits before softmax were 25.2776, 24.4982 and 21.1888 for A, B and C. Applying the restricted softmax by hand gives the same 0.678 that SGLang printed. The computation is the formula from the mechanism section, run once on three numbers.

### Mapping the Scores Back to a Decision
The last step is plain Python that turns three floats back into the labels the application uses.

```py
# decide.py (continued)
probabilities = {
    choices[label]: float(score)
    for label, score in zip(choices, scores, strict=True)
}

decision = max(probabilities, key=probabilities.get)

print(json.dumps({"decision": decision, "probabilities": probabilities}, indent=2))
```

**What this does:**
- Pairs each score with its application meaning, using `strict=True` so a length mismatch raises an error.
- Picks the highest-probability option as the decision and prints both the decision and the full distribution.

```javascript
#### OUTPUT ####
{
  "decision": "billing and payments",
  "probabilities": {
    "billing and payments": 0.6777623295783997,
    "technical support": 0.31087860465049744,
    "account access": 0.011359035037457943
  }
}
```

**What this does:**
- Shows billing and payments as the decision, at 0.678.
- Shows technical support at 0.311 and account access at 0.011, which together make up the rest of the distribution.

Billing wins, but at 0.678 rather than the 0.99 or higher the author might have expected. That is the kind of case a threshold of 0.70 should send for a second look instead of automating it.

### Measured speed
The author ran the scoring path and a standard chat-completion path on the same SGLang server, with the same model and the same 100 support-ticket cases, started at the same moment. When scoring finished all 100 cases, standard generation, which decodes up to 32 tokens per answer, had completed 54. Scoring averaged 463 milliseconds per case against 848 milliseconds for generation, a measured 1.83x gap.

## Why the Number Next to the Answer Matters More Than the Answer
A typed answer tells you what won, and the probability tells you how close the race was. The second number is what code should branch on.

```json
{
  "choice": "billing",
  "probabilities": {"billing": 0.52, "technical": 0.46, "account": 0.02},
  "confidence": 0.06
}
```

**What this does:**
- Reports billing as the choice, with its probability of 0.52.
- Adds a confidence of 0.06, which is the gap between billing and technical.

The article defines confidence as the margin between the top answer and its closest rival, not the winning probability alone. Billing at 0.52 against technical at 0.46 is practically a coin flip, so this ticket should not take the same branch as a 0.91 winner.

```py
if probabilities["billing"] > 0.9:
    route_automatically(ticket, team="billing")
elif confidence < 0.3:
    send_to_human_review(ticket)
else:
    escalate_to_larger_model(ticket)
```

**What this does:**
- Automates only when the billing probability is above 0.9.
- Sends low-confidence cases to a human, and everything in between to a larger model.

The routing rule has two thresholds, and they live in code where they can be reviewed, tested and changed, not inside model weights. A dashboard label can tolerate a shaky prediction, but a command that deletes customer data needs a much higher bar.

> ❌ **Anti-pattern — automating on the winning label alone:** Routing a ticket because its label won, whatever the margin. **Why people do it:** The label is the only output most code reads. **Consequence:** A 0.52 win is automated the same way as a 0.91 win. **Fix:** Branch on probability and margin together.

### Calibration
TypeSafe trains Jev with what it calls Reinforcement Learning for Calibrated Decisions, RLCD. The goal is calibration rather than accuracy alone: predictions made at 90 percent confidence should be right roughly 90 percent of the time. To check this, bin every prediction by its stated confidence and compare each bin's average confidence with how often that bin was right. A model can be accurate on average and still be confidently wrong in the band you would trust most. Checking calibration requires a labeled evaluation set of your own.

## The Hallucination Claim, Said More Carefully
TypeSafe says Jev cannot hallucinate. That holds under a narrow definition. If the declared answers are billing, technical and account, a legal response cannot appear, because that option does not exist in the schema. Malformed prose is also ruled out. The schema says nothing, however, about whether a valid answer is correct. A login bug scored as billing is schema-valid and still wrong, and a refund may be issued over a problem that was never about a charge.

> ❌ **Anti-pattern — equating schema validity with correctness:** Reading "cannot hallucinate" as "always right". **Why people do it:** The guarantee sounds absolute. **Consequence:** Wrong but well-formed decisions flow straight into actions. **Fix:** Measure correctness on a labeled set, and gate high-impact actions on confidence.

A more careful statement is that Jev cannot break its declared output schema, but it can still be wrong. Type safety and correct judgment are separate guarantees, and only the first comes for free. The article suggests placing Jev checkpoints beside an ordinary agent loop at three points: choosing the model, approving a tool, and verifying the result.

## Where Jev Earns Its Place Inside an Agent
Jev works alongside a generative model, not instead of one. The generative model plans, writes, explains and calls tools. Jev handles the frequent small decisions around that work.

### Model routing
A simple lookup does not need the model used for a hard architecture review. Jev scores the incoming request and picks the least expensive model likely to finish it. The router decides which model answers, not what the answer says.

### Tool risk gating
Before an agent runs a shell command, Jev can classify it as read-only, reversible or destructive, and also say whether it touches production or leaves the sandbox. A high-confidence read-only command keeps running, while a destructive or uncertain one pauses for a human. One shell command produces one Choice call, and the three risk classes lead to three different consequences.

### Verification
An agent can claim a task is finished while its tests still fail. Jev can answer bounded questions about the state, such as whether the tests passed, whether the agent is repeating a failed action, or whether the output follows policy. Hard tests still catch what they are built to catch, and Jev adds a semantic check for what a hard rule cannot express.

## Problems Jev Can Solve Today
The article says workloads that suit Jev share three properties: every possible answer can be named ahead of time, a careful human could judge the input quickly, and the decision happens often enough that latency or cost matter.

### Support and operations
Classify intent, urgency, department, spam and frustration in one pass. Route refunds through several small checks instead of one large prompt. Rank incidents by severity before a human reads them.

### Search and retrieval
Rerank passages by whether they answer the query, not just by semantic closeness. Check whether a citation supports a claim. Filter irrelevant chunks before they reach an expensive model.

### Quality and safety
Screen prompts for jailbreak attempts or injection, check generated content against a written policy, and flag a risky code change or tool call before it runs. These checks sit beside deterministic checks and do not replace them.

### High volume classification
Label documents or customer messages at a scale that was previously unaffordable. Turn free text into features for a traditional ML model. Score every row of a corpus against the same rubric.

### Real time interfaces
Choose the next browser action from a known set of page elements, or score tone while someone is still typing. Jev is text-only today, so the environment must first be turned into text.

## Does Any of This Hold Up
The article treats TypeSafe's own numbers with skepticism. TypeSafe reports 70 to 500 milliseconds of latency, $0.042 per million input tokens with output free, and claims of roughly 200 times faster and 400 times cheaper than comparable workflows. The author treats these as a ceiling, not a promise. The mechanism is real, as the reproduction above shows, but marketing multiples sit at the favorable end.

The author prefers an independent comparison. LangChain tested Jev against three LLM judges, GPT-5.6 Luna, GPT-5.6 Terra and Claude Sonnet 4.6, using its own Deep Agents harness and LangSmith for the dataset.

One example shows the mechanism difference. Asked the same refund-policy question, an LLM judge reasons through a paragraph before producing a score of 0.67. The judge noted that the answer gives a 30-day window, which matches policy, but leaves out the exception for opened items, so it is accurate but not complete. A Jev-style judge skips the paragraph and returns three calibrated probabilities directly: correct 0.98, grounded 0.92 and complete 0.11. Those combine to the same 0.67.

### Accuracy against a human oracle
Across 500 repeated pass/fail judgments against a human oracle, Jev matched the human on 100.0 percent, GPT-5.6 Terra on 99.8 percent, GPT-5.6 Luna on 96.4 percent and Claude Sonnet 4.6 on 80.0 percent.

### Variance
Accuracy alone is half the story, since a judge can be right on average and still swing run to run. Across 100 repetitions on one frozen case, Jev's mean variance was 0.0000149. Claude Sonnet 4.6 was 92 times higher, GPT-5.6 Luna 433 times higher and GPT-5.6 Terra 913 times higher. Low variance does not prove correctness, since a judge can be consistently wrong, but an accurate judge with low variance is something to build automation on.

### Cost and latency
Jev averaged $0.00035 and 0.44 seconds per call. GPT-5.6 Luna averaged $0.00039 and 2.50 seconds, GPT-5.6 Terra $0.00289 and 2.83 seconds, and Claude Sonnet 4.6 $0.02811 and 2.16 seconds. Projected to 10,000 traces a day for 30 days, the totals are $103.59 for Jev, $117.24 for GPT-5.6 Luna, $866.61 for GPT-5.6 Terra and $8,434.08 for Claude Sonnet 4.6.

### Signal value
Signal value is oracle agreement multiplied by repeatability, meaning the chance that two independent calls on the same frozen trace reach the same verdict. A judge earns credit only if it is both correct and repeatable. Jev scored 100.0 percent, GPT-5.6 Terra 99.4 percent, GPT-5.6 Luna 90.1 percent and Claude Sonnet 4.6 80.0 percent.

The experiment used one agent, five test cases and a narrow evaluation slice, so the author does not treat it as the final word. It is an independent measurement, though, and it points in the direction the mechanism predicts.

## Where Jev Falls Apart
Jev is less useful once the answer space cannot be known ahead of time.
- It cannot write a response, summarize a document, generate code or explain its reasoning.
- It is unreliable at arithmetic, counting, date comparison or exact string manipulation, so keep those in ordinary code.
- It struggles when a decision needs several hidden reasoning steps. Split the judgment into smaller questions, or use a reasoning model.
- It cannot extract an unknown value from text. Find candidates with another method first, then let Jev choose among them.
- Irrelevant context can lower its accuracy, so send only the state a decision needs.
- Closed weights, an early-access window, text-only input and thin independent calibration data mean it is too early to trust blindly on decisions with real consequences.

The article's summary: bounded, high-volume answers are Jev's sweet spot, open-ended work still belongs to an LLM, and rare bounded decisions belong to a plain if statement. If deterministic code already solves the problem correctly, keep the code. A plain if statement is faster, cheaper and easier to test than any model.

## A Sane Way to Roll This Out
A cheap model can still become expensive if its mistakes create retries, manual review or an incident, so the rollout must measure the whole workflow, not only the token price.

> 📋 **Checklist — Rolling out a scored decision**
> - [ ] Choose one bounded, low-risk decision with a small, clearly named set of possible answers, not the riskiest step in the pipeline.
> - [ ] Write the rubric before calling the model. Decide in words what belongs in every option. The rubric is part of the system.
> - [ ] Collect representative examples first, including deliberately ambiguous and adversarial cases.
> - [ ] Run in shadow mode. Let the model answer every real case without changing anything, and log its probabilities beside the current outcome.
> - [ ] Plot accuracy against confidence and set thresholds from that data. The cut-off should come from the shadow-mode numbers, not a guess.
> - [ ] Automate the safest branch first. Keep a human, or a stronger model, as the fallback for anything the thresholds flag as uncertain.
> - [ ] Version everything. Pin the model version, questions, criteria and thresholds together, so a change can be replayed against the same evaluation set.

The article says step five does real work. Aim for the highest threshold that still automates enough volume to be worth building. A threshold set too high means the system never decides anything, which the author calls expensive logging rather than automation. The coverage constraint keeps the threshold honest.

## What's Proven Here
The article considers the mechanism proven, since the author built it and ran it, with real token IDs and a real restricted softmax producing 0.678. The independent evaluation numbers are also real, published by a party with nothing to gain from making Jev look better. The following remain unproven:
- Jev's weights are closed, so the verified mechanism is a faithful reconstruction on an open model, not an inspection of Jev.
- TypeSafe's latency and cost claims sit at the favorable edge of their own comparisons.
- Independent calibration data beyond the one cited evaluation is still thin.
- Access is early and the interface is text-only, so images, UIs and raw sensor data need a separate step to become text.

None of this makes the idea wrong. It makes the claims around it precise rather than settled.

## Where This Leaves Us
The article argues that Jev is interesting not because it writes better than a language model, but because it does not write at all. Its answer types are fixed, uncertainty is explicit on every answer, every question is evaluated in parallel, and the branching logic stays in code. The generative model still produces plans, explanations and code. Jev routes requests, gates risky actions, checks results and signals when uncertainty is high enough that a human should look.

The author's advice: do not start by rebuilding an agent around a typed decision model. Find one place where code pays for a full generative call just to get a one-word answer, give it the minimum state it needs, and let it earn one branch before anything more is handed to it.

## 🧠 Mental models
### Restricted softmax
**The model:** Read the model's preference only among the options your code declared, by applying softmax to the logits of those options and ignoring the rest of the vocabulary.
**How to apply it:** Label each option with a one-token label, verify the token IDs, and always include an OTHER option so that the probability mass has somewhere to go.

### Margin as confidence
**The model:** Confidence is the gap between the winning option and its closest rival, not the winning probability alone.
**How to apply it:** Branch on both the probability and the margin. A 0.52 win over 0.46 should escalate, not automate.

### Schema guarantees membership, not correctness
**The model:** A typed output can only contain declared values, but any declared value may still be the wrong one.
**How to apply it:** Measure accuracy on a labeled evaluation set before automating, and keep high-impact actions behind a human or stronger check.

### Score as a weighted average
**The model:** A score on an ordered scale is the probability-weighted average of every rung, so 1.74 on a missed-to-resolved scale carries information that a single label drops.
**How to apply it:** Keep the full distribution, not only the top label, when a downstream rule needs to know how close the call was.

### If statement first
**The model:** When deterministic code already solves a bounded problem correctly, that code is cheaper, faster and easier to test than any model.
**How to apply it:** Before adding a model call, check whether an ordinary rule or comparison gives the same answer.

## 🔑 Key takeaways
- Scoring a known set of options in one forward pass avoids the token-by-token generation that a one-word judgment does not need. The author measured 463 ms per case for scoring against 848 ms for generation, a 1.83x gap, on the same hardware.
- The probability reported by restricted softmax is the model's split among declared labels only, so every label must be a verified single token and the choices should include an escape option.
- Thresholds belong in code. The routing rule uses a 0.9 automation threshold and a 0.3 confidence threshold, and both can be reviewed and tested.
- Jev's schema guarantees that its answers are members of the declared set, not that they are correct, so accuracy must be measured separately.
- In LangChain's independent test on a narrow slice of one agent and five test cases, Jev matched the human oracle on 100.0 percent of 500 judgments, against 80.0 percent for Claude Sonnet 4.6, at $0.00035 per call against $0.02811.
- Before automating, set thresholds from shadow-mode data, keep humans or a stronger model for uncertain cases, and pin model, questions, criteria and thresholds together.

## 🏛️ For Kenji
> Your interpretation, not the author's: In an architect's work, this pattern fits any place where a control or routing decision draws on a language model for a bounded label, such as AI governance checks, request routing in an AI gateway, or the tool-risk gating of agent platforms. The question it raises is how you would govern the thresholds and the labeled evaluation set that justify automation, and who owns that evidence once the model is updated.

## 📚 Reading plan
### From the article
- [decide.py](http://decide.py) — The author's client script, used for the reproduction. The link points to a file name, not an external reference.
### To go further
- LangChain's evaluation of Jev against LLM judges — the independent comparison that the article uses for its accuracy, variance and cost figures.
- SGLang and its `/v1/score` endpoint — the open serving stack the author used to reproduce restricted scoring.
- Calibration and expected calibration error — the method for binning predictions by confidence and comparing each bin with its accuracy.
- Structured output versus fixed-answer scoring — the contrast between schema-constrained decoding and reading a single forward pass.

## Concepts
- [structured-decision-model](/concepts/ai-ml/structured-decision-model.md)
- [calibration](/concepts/ai-ml/calibration.md)
- [tokenization](/concepts/ai-ml/tokenization.md)
- [large-language-model](/concepts/ai-ml/large-language-model.md)
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [llm-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md)
- [inference-tradeoffs](/concepts/ai-ml/inference-tradeoffs.md)
- [model-evaluation](/concepts/ai-ml/model-evaluation.md)
- [logit](/concepts/ai-ml/logit.md)
