---
type: Article Summary
title: The Complete Guide to Agent Harnesses (With Code)
description: Agent harness code, not the model, governs agent cost and rule enforcement, so enforce rules in code.
resource: https://medium.com/data-science-collective/the-complete-guide-to-agent-harnesses-with-code-6fa11cecd004
tags:
- ai-ml
- agent-harness
- inference-tradeoffs
- multi-agent-orchestration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://medium.com/data-science-collective/the-complete-guide-to-agent-harnesses-with-code-6fa11cecd004
  title: The Complete Guide to Agent Harnesses (With Code)
  author: Data Science Collective
status: stable
source_url: https://medium.com/data-science-collective/the-complete-guide-to-agent-harnesses-with-code-6fa11cecd004
source: Medium (Data Science Collective)
source_type: medium
published: '2026-08-21'
domain: ai-ml
concepts:
- agent-harness
- inference-tradeoffs
- multi-agent-orchestration
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81b1866ecfc255dd007c
---

## 🎯 Core thesis
The code around a model, the harness, decides what an agent costs and what it is allowed to do, so a rule you need to hold should be enforced in code rather than stated in a prompt.

## 🧭 Why the author wrote it
The author put a rule ("never apply an edit the human has not approved") in a config file, and an agent ran through it eleven times in one afternoon, producing about seventy-six unapproved edits in a client deliverable. If the thesis is right, a rule that runs as a hook in code before each tool call stops the edit, while the same rule in the prompt does not, and the team gets something concrete to debug when the rule is wrong.

## What the Harness Actually Buys You
The core agent loop is six lines: ask the model for the next call, stop if there is none, run the call, append the result. Error handling and a retry bring it to about twenty lines. The author's point is that the interesting part is the `execute` function, where you decide what the model may do, what it can reach, what it remembers, who checks it and what it sees. Those decisions are the harness.

```py
while agent.turns < max_turns:
    call = agent.next_call(observations)
    if call is None:
        break
    result = execute(call)
    observations.append(result)
```

### Agent harness
**What it is:** the code around a fixed model: the tool loop, the checks before a tool runs, the sandbox, memory, verification and context assembly. The article uses "harness" in this sense throughout and admits the field uses the word more loosely.
**How it works:** the model only proposes a call. The harness decides whether that call executes, with what resources, and what result comes back into the context. Every decision that matters for behavior lives in that path.
**Why it matters:** a rule that exists only as model text depends on the model remembering it. A rule in the harness runs on every call, including calls where the model has forgotten it.
**When it applies:** any time an agent can change files, reach the network or spend tokens on your behalf.

The author's first claim is about cost, and it is backed by a June 2026 preprint. The study ran three harnesses (Goose, OpenCode, OpenHands-SDK) across two models on a stratified 50-task subset of Terminal-Bench Pro, a harder variant of a terminal-agent benchmark. It found up to a 40x difference in tokens per solved task between harnesses. Upgrading the model moved the same number by only 1.0 to 1.3x.

> 🔎 Added context: Tokens per solved task is the amount of model input and output consumed for each task the agent completed. A higher number means more money per success.

The second claim is about capability, and it is weaker. The same study found pass-rate differences between harnesses of 0 to 8 percentage points, and bootstrap confidence intervals crossed zero on all but the largest gap. The authors describe their accuracy results as descriptive, given n=50. The practical reading is that the same task, on the same model, at the same success rate, can cost forty times more on one harness than on another, while the harness does not make a weak model smarter.

## Move the Rule Into Code
A rule in a system prompt is a bet that the model remembers it, reads it as intended and follows it under context pressure. The model wins that bet most of the time, and the author argues that this reliability is the danger, because it is the reason people stop checking. A rule in code runs on every call.

The article names five places in the agent loop where code can enforce a rule. Each has a question, a guard and a failure case. The author's advice is to start with the layer whose failure you can picture happening to you this week, not to build all five on day one.

> ❌ **Anti-pattern — prompt-only rule:** the policy lives in a system prompt and nothing checks the call. **Why people do it:** it is the fastest way to state a rule and it works most of the time. **Consequence:** when the model drifts, the violation passes silently, with no error and no record. **Fix:** move the rule to a function that runs before the tool call.

## Layer 1: The Execution Boundary
**The question:** what is the agent allowed to do?

The boundary is a function that runs between the model's decision and the tool call. The article shows the failure first. The system prompt says never to delete a file without asking, and the agent has three tool calls queued. The naive `execute` deletes without checking.

```py
SYSTEM_PROMPT = "IMPORTANT: never delete a file without asking the user first."

def execute(call):
    if call.name == "delete_file":
        FILES.pop(call.args["path"], None)
        return f"deleted {call.args['path']}"
```

**What this does:**
- `SYSTEM_PROMPT` holds the rule as text, which the model may or may not follow.
- `execute` dispatches on the tool name and performs the deletion directly, with no check in between.
- The function is the only place the tool runs, so it is the natural place to enforce policy.
- Result in the article: the agent deletes `cache.tmp`, then `client-deliverable.md`, and never asks.

### Execution boundary
**The model:** a guard function that wraps `execute` and inspects each call before it runs. It returns a denial string when a rule matches, and otherwise passes the call through.
**How it works:** `boundary(rules)` returns a wrapper. Each rule is a name, a predicate over the call, and a reason. The first matching rule returns `DENIED by <name>: <reason>` and the tool is never called.

The fix is a wrapper that runs before the tool. The approval rule applies to deletions that lack an explicit human approval flag.

```py
def boundary(rules):
    def wrap(execute):
        def guarded(call):
            for name, deny_if, reason in rules:
                if deny_if(call):
                    return f"DENIED by {name}: {reason}"
            return execute(call)
        return guarded
    return wrap

NEEDS_APPROVAL = [(
    "delete-needs-approval",
    lambda c: c.name == "delete_file" and not c.args.get("approved_by_human"),
    "deletion requires an explicit human approval flag on the call",
)]
```

**What this does:**
- `boundary` is a decorator-style factory: it takes the rules and returns a function that wraps any `execute`.
- Each rule is a tuple of name, predicate and reason, so a denial explains which rule fired.
- The predicate `c.args.get("approved_by_human")` lets an approved deletion pass, so the rule blocks by default and allows on purpose.
- `NEEDS_APPROVAL` is data, so new rules are added to a list instead of edits inside `execute`.
- Same agent, same script, same prompt: nothing gets deleted.

> ❌ **Anti-pattern — rule in the prompt only:** the sentence "never delete without asking" sits in the system prompt with no code path that checks it. **Why people do it:** it costs nothing to write and it reads as a policy. **Consequence:** the deletion happens and nothing records that a rule was broken. **Fix:** add a boundary function that denies the call unless the approval flag is present.

> 🔎 Added context: In the article's first section the author counts 76 unapproved edits and 46 marked lines, while the closing section counts 15 violations, 11 of them in one afternoon. The note keeps each figure as the author wrote it, because the two sets measure different things.

## Layer 2: Sandboxing
**The question:** what can the agent reach?

A sandbox is the set of directories and hosts the agent's process can touch at all. You build it by starting from nothing and adding back what the job needs: an allowlist of paths, an allowlist of hosts, and a process that cannot see anything outside them. A deny list, the usual first version, names paths the agent must not touch.

```py
DENY = ["secrets/"]

def execute(call):
    path = call.args["path"]
    if any(path.startswith(d) for d in DENY):   # checks the spelling
        return "DENIED by deny-list"
    real = os.path.normpath(path)               # the ../ collapses HERE, after the check
    return DISK.get(real, "not found")
```

```javascript
read('secrets/api_key')          -> DENIED by deny-list
read('work/../secrets/api_key')  -> sk-live-DO-NOT-LEAK
```

**What this does:**
- `DENY` lists prefixes, and the check compares the raw string the agent typed.
- `os.path.normpath` runs after the check, so `..` segments collapse only at lookup time.
- The two example reads show the gap: the first is refused, the second reaches the same file.
- The article's point is the order of operations: the check and the lookup disagree on what the path is.

### Path resolution before comparison
**The model:** normalize the path first, then compare the normalized path against the allowed roots.
**How it works:** `resolve` collapses `..` and other spellings, then accepts the path only if it is equal to, or sits under, an allowed root. Anything else returns `None`.

The fix resolves before comparing:

```py
ALLOW_ROOTS = ["work"]

def resolve(path):
    real = os.path.normpath(path)               # resolve FIRST
    if not any(real == r or real.startswith(r + os.sep) for r in ALLOW_ROOTS):
        return None
    return real
```

**What this does:**
- `normpath` runs first, so every spelling of a path becomes one canonical string before any rule sees it.
- The check `real == r or real.startswith(r + os.sep)` matches the root exactly or as a directory prefix, so `workshop/` does not pass for `work/`.
- Both spellings from the example now become `secrets/api_key`, outside `work`, and both are refused.
- The allowlist replaces the deny list because a deny list only blocks the spellings its author thought of.

The article is clear that this layer is not a real security boundary. A check inside your own program sees only the calls it recognises. The same file can be reached through a shell command, a symlink or a `..` path that the check never inspects. In production, the author says, the same logic is enforced by the process itself, a user or container with no read permission outside `work/`.

> ❌ **Anti-pattern — deny list on the raw string:** the check compares the string the agent typed, and the lookup uses the resolved path. **Why people do it:** it is easy to write and it blocks the obvious path. **Consequence:** `work/../secrets/api_key` reads the secret while the rule reports it denied. **Fix:** resolve the path first and allow only paths under known roots, then enforce the same limit at the operating system level.

> 🔎 Added context: The inline links on file names such as sandbox.py and persistence.py point to .py domains. They are file names in the repo, not separate references, so the repo itself is the source to read.

## Layer 3: Memory Persistence
**The question:** what survives a context reset?

The author's most useful change was to restart conversations without hesitation. Context fills, quality drops and the agent starts contradicting itself, so instead of nursing a session, make sure the things that matter live somewhere a restart cannot touch: committed code, config, the harness itself and extracted notes.

Every piece of state the agent produces has to go to one of three places: the conversation, where it dies with the session; disk, where it survives and can be retrieved; or the harness config, where it shapes every session that follows. Most teams never choose explicitly, which is why their agents feel amnesiac in some ways and stubbornly wrong in others.

```py
CONVERSATION, DISK, HARNESS_CONFIG = [], {}, {}

def remember(kind, key, value):
    """'chat' dies with the session, 'disk' survives it,
    'config' shapes every session that follows."""
    {"chat": lambda: CONVERSATION.append(value),
     "disk": lambda: DISK.__setitem__(key, value),
     "config": lambda: HARNESS_CONFIG.__setitem__(key, value)}[kind]()
```

**What this does:**
- Three containers stand for the three lifetimes: conversation, disk and config.
- `remember` makes the destination a required argument, so no one can store a value without naming its lifetime.
- The dictionary dispatch uses lambdas so only the chosen branch runs.
- The article's demo (`layers/03_memory_persistence.py`) shows the conversation returning an empty list while the decision and the rule remain on disk and in config.

### Three memory destinations
**The model:** every piece of agent state has a lifetime, and the harness should name it.
**How it works:** `chat` values are cleared with the session, `disk` values survive restarts and can be retrieved, and `config` values change how every later session behaves.
**When it applies:** deciding where a decision, a lesson from a failure or an extracted note should live before the session that produces it ends.

> ❌ **Anti-pattern — everything in the conversation:** decisions and rules live only in the chat history. **Why people do it:** it is the default, and nothing needs to be written down. **Consequence:** a reset loses the decision, and the next session repeats the mistake. **Fix:** write decisions to disk and rules to the harness config, and keep the chat for working state only.

## Layer 4: Verification Loops
**The question:** who checks the work, and what stops them fixing it themselves?

A verification loop is a second pass over the agent's output by something that can report a problem but cannot fix it. The author's warning is that a reviewer with write access will quietly repair what it finds. That looks efficient but removes the signal, because you can no longer tell which work was right and which was wrong and patched.

The second pattern is the dry run. For anything irreversible, the tool takes a flag that defaults to describing the action, and it acts only when the flag is explicitly turned off. A forgotten flag is then harmless.

```py
def execute(call):
    if call.name == "review":
        snapshot = dict(CODE)               # a copy, so the reviewer cannot write
        src = snapshot[call.args["path"]]
        return f"VERDICT: {'off-by-one' if '+ 1' in src else 'looks good'}"
    if call.name == "apply_fix":
        if call.args.get("dry_run", True):  # on by default, turned off on purpose
            return f"DRY RUN: would rewrite {call.args['path']}, nothing written"
        CODE[call.args["path"]] = call.args["new"]
        return f"wrote {call.args['path']}"
```

**What this does:**
- The `review` branch works on `snapshot`, a copy, so it can read the code but cannot change it.
- It returns a verdict string, which is a critique with no side effects.
- `apply_fix` defaults `dry_run` to `True`, so the write happens only when the caller turns the flag off on purpose.
- The dry-run message says what would happen and confirms nothing was written; in the demo the off-by-one bug is still in the file at the end.

### Read-only reviewer
**The model:** a reviewer produces a critique, and a separate step with its own record acts on it.
**How it works:** the reviewer receives a copy of the state and returns a verdict. Applying the verdict is a different call, with its own flag and log.
**When it applies:** any agent that checks its own or another agent's output.

### Dry-run default
**The model:** irreversible tools describe their effect unless the caller explicitly says to act.
**How it works:** the flag `dry_run` defaults to on, so omitting it is safe.
**When it applies:** deletions, writes to shared documents, deployments and anything that cannot be undone.

> ❌ **Anti-pattern — reviewer with write access:** the reviewing subagent can edit the file it reviews. **Why people do it:** it saves a step and the fix looks right. **Consequence:** the review no longer shows what was wrong, and the team cannot audit the change. **Fix:** make the reviewer read-only and apply fixes in a separate step.

## Layer 5: Context Pipelines
**The question:** what does the model actually see, and what does that cost?

A context pipeline is the route information takes into the model's window and the choice of how much arrives. The cost is the central issue: every token in the window is paid on that turn and on every turn after it. This is why the forty-fold gap in the first section shows up here rather than in the execution boundary.

The naive harness loads everything. It sends the whole file when the agent asked about one function, the whole search output when one line was needed, and every earlier turn. Turn twenty therefore pays again for everything turns one to nineteen read.

The fix is delegation with distillation. A subagent gets its own context window, explores tens of thousands of tokens and returns one to two thousand. The main thread sees only the answer.

```py
def subagent_search(query):
    """Its own window. The main thread never pays for this reading."""
    global subagent_tokens
    subagent_tokens += sum(len(v.split()) for v in CORPUS.values())
    return next(f"{n}: {b.split('ANSWER:')[1].strip()}"
                for n, b in CORPUS.items() if "ANSWER:" in b)
def execute(call):
    global main_tokens
    distilled = subagent_search(call.args["q"])
    main_tokens += len(distilled.split())    # the only line that bills you
    return distilled
```

**What this does:**
- `subagent_search` charges its reading to `subagent_tokens`, a separate counter, so the cost of exploration is tracked apart from the main thread.
- The function returns only the text after `ANSWER:` for the first matching document, which is the distilled result.
- `execute` charges the main thread only for the distilled string, the one line marked as the billing point.
- The split between the two counters is the whole idea: which counter gets incremented determines what the bill shows.

The repo's demo is the smallest honest version. The question and answer are identical in both cases, while the main thread reads 9,602 tokens in one version and 2 in the other.

### Context isolation through a subagent
**The model:** a subagent does exploration in its own window and returns a short answer, so the main context pays only for the answer.
**How it works:** the subagent's reading is charged to its own counter. The main thread receives a distilled string, usually one to two thousand tokens, instead of tens of thousands.
**When it applies:** searches, reads of large files and any exploration whose output is much bigger than its answer.

### Sequential subagents
**The model:** a chain of subagents, each waiting for the last, buys isolation and no parallelism.
**How it works:** each step runs after the previous one returns, so the total time is the sum of the steps.
**When it applies:** the author says to run steps in parallel when they are independent, and to be honest about isolation cost when they are not.

> ❌ **Anti-pattern — sequential chain described as parallel:** steps that wait on each other are presented as a speed or throughput gain. **Why people do it:** multi-agent diagrams look like parallel work. **Consequence:** the expected speedup does not appear, and the extra model calls add cost. **Fix:** run only independent steps in parallel, and count the chained cost as the price of isolation.

> ❌ **Anti-pattern — loading everything each turn:** the whole file, the whole search output and all earlier turns go into every call. **Why people do it:** it is the simplest harness and it works. **Consequence:** the cost rises with every turn, because each one pays for all the earlier reads. **Fix:** delegate exploration to a subagent that returns a distilled answer.

> 📋 **Checklist — the five layer questions (derived from the article's five places and their questions)**
> - [ ] Execution boundary: what is the agent allowed to do, and is that enforced in code?
> - [ ] Sandboxing: what can the agent reach, and is access checked after the path is resolved?
> - [ ] Memory persistence: for each piece of state, is it in the conversation, on disk or in the harness config?
> - [ ] Verification loops: who checks the work, and can the checker change it?
> - [ ] Context pipelines: what enters the window on each turn, and what does it cost?

> 🔎 Added context: The article's rule of thumb is that any box you cannot tick with a file path is a rule you are hoping for.

## What Nobody Can Tell You Yet
This section lists the limits of the evidence, and it is the part where the author is most careful.

- **Not one controlled study here is peer reviewed.** Every harness-isolation result as of August 2026 is an un-refereed arXiv preprint. The most careful experiment describes itself as preliminary work under review at a workshop, and the most quoted framing paper is a self-labelled position paper with no experiment. The author's conclusion is to re-run comparisons on your own tasks before budgeting against any of them.
- **Harness gains often do not survive new tasks.** A July 2026 paper found that automatic harness evolution "does not consistently outperform simple test-time scaling methods and exhibits limited generalization," because the search and the final evaluation share a benchmark. On a continual-learning evaluation on Terminal-Bench 2.0, GEPA rose to 70.8% on its tuning tasks and then fell to 54.5% across the wider set, below the 56.8% it started from. The lesson is to measure a harness on tasks you did not use to build it.
- **A fine-tuning result can be scored on its own reward signal.** Baseten fine-tuned Gemma 3 27B for clinical note generation, moving it from 35% worse than Claude Sonnet 4 to 60% better. Their write-up says "the eval harness itself is used as the reward signal during training." The author calls this legitimate, and says that when a vendor reports a win, you should ask what the eval was during training.
- **A harness is not portable across model changes.** A June 2026 study found that applying a harness only after training recovers little of the benefit of training with it in place. Models post-trained on a minimal harness returned "Invalid tool format" in 75.1% of attempts once the tool definitions changed shape, and one Qwen2.5–7B variant scored 10.8 points below the base model it started from. The lesson is to choose the harness before post-training, and to expect a mid-project swap to cost more than it looks.
- **The field has not agreed what "harness" means.** A May 2026 Hugging Face glossary separates harness from scaffold, and the article notes that Claude Code, Codex and others call the whole thing a harness anyway. The five-layer split is one decomposition, and under a narrower definition much of the evidence measures something else. The lesson is to check which layers each study includes before comparing results.
- **Harness effects may shrink as models improve.** Harness-Bench reports that stronger model backends show "higher mean scores while exhibiting lower cross-harness variance." The author's advice is to weight harness work toward cost control, which persists, over accuracy, which a new model may absorb.

> ❌ **Anti-pattern — tuning and testing on the same tasks:** a harness or fine-tune is scored on the set it was built or trained against. **Why people do it:** it is the set they have, and gains show up quickly there. **Consequence:** the gain disappears on new tasks, as the GEPA result shows. **Fix:** hold out tasks the harness never saw, and report results on them.

> ❌ **Anti-pattern — swapping the harness after post-training:** the model is trained on one harness and then placed in another. **Why people do it:** the harness looks like a replaceable component. **Consequence:** tool-format errors rise and much of the benefit of training is lost. **Fix:** choose the harness before post-training and keep it stable.

## The Repo
The author provides ten scripts at github.com/paoloap-py/agent-harness-guide. Each layer appears with its guard and again without it, so you can compare the two runs. The model is replaced by a scripted stand-in that emits a fixed sequence of tool calls, so no API key is needed.

```javascript
git clone https://github.com/paoloap-py/agent-harness-guide
cd agent-harness-guide

python3 run_all.py        # all five layers, guard off then on, side by side
python3 test_harness.py   # asserts every difference above, 10 checks
```

**What this does:**
- `git clone` fetches the repo, which contains the layer scripts and the failure versions.
- `run_all.py` runs all five layers twice, with the guard off and then on, so differences are visible in one output.
- `test_harness.py` asserts the differences the article describes, with 10 checks.
- The scripts are scripted stand-ins for the model, so the results are repeatable on any machine.

## Where This Leaves You
The author's rule was sound, but it sat in the wrong place for two weeks. That cost fifteen violations, eleven of them in one afternoon. Moving the rule into a hook is what enforced it, and the wording did not change. The model is one purchase. The harness around it decides what the agent costs and how often it acts without sanction, and both are engineering work.

## FAQ
**What is an agent harness?** The code around the model: the tool loop, the checks before a tool runs, the sandbox, what persists across sessions, what reviews the output and what context each turn receives. The loop is six lines, or about twenty with error handling.

**Does the harness really matter more than the model?** On cost, yes, by up to 40x in tokens per solved task. On whether a task is solved, the measured difference between harnesses is 0 to 8 percentage points with confidence intervals crossing zero. The Terminal-Bench authors conclude that model selection usually matters more for performance.

**Where does harness choice cost the most?** In context. Every token sent on every turn is paid for on every turn, so the context layer is where the money goes, and delegating a search to a subagent changes the bill without changing the answer.

**Are permission rules a security boundary?** No. The harness check runs on the call the agent made, and the same file can be reached through a shell command, a symlink or a `..` path the check never sees. The limit that holds is the one the operating system places on the process: start it with access to nothing and add back what the job needs.

**Should I build my own harness or adopt one?** Adopt one and commit to it. Switching resets the institutional knowledge in your config files, and every rule written after a real failure has to be earned again.

## 🧠 Mental models
### Rules in code, not in prompts
**The model:** a rule is real only when code enforces it on every call. A sentence in a prompt is a request the model may forget.
**How to apply it:** for each rule you care about, name the function that checks it. If you cannot name one, the rule is a hope.

### Resolve before you compare
**The model:** a check and a lookup must see the same canonical value, or the check protects the wrong thing.
**How to apply it:** normalize paths, identifiers and inputs first, then apply every rule to that normalized value.

### Reviewers report, they do not repair
**The model:** a checker that can change the work destroys the record of what was wrong.
**How to apply it:** make reviewers read-only and put every change in a separate, logged step.

### Pay for what enters the window
**The model:** context is a recurring cost, paid again on every turn it stays in the window.
**How to apply it:** let subagents explore in their own windows and return the answer, and count the cost of each one separately.

## 🔑 Key takeaways
- A harness rule enforced in code blocked the unapproved edit that a prompt rule had failed to stop, and the author found the wrong line by reading the file.
- The same agent on the same model at the same success rate cost up to 40x more tokens on one harness than another, while the model upgrade moved the number by 1.0 to 1.3x.
- Checking a path before resolving it lets `work/../secrets/api_key` through, so resolve first and allow only known roots.
- Reviewers should be read-only, and irreversible tools should default to a dry run.
- Subagents with their own windows cut the main thread's context from 9,602 tokens to 2 in the article's demo, but sequential chains buy isolation, not speed.
- Harness gains often vanish on new tasks: GEPA fell from 70.8% on its tuning set to 54.5% across the wider set, below its 56.8% start.

## 🏛️ For Kenji
Your architecture work faces the same gap between stated and enforced rules whenever you put an AI agent on a delivery path. The layer questions in the checklist give you a way to review an agent setup the way you would review any control, by asking where each rule is enforced and what it costs. This is your interpretation, not the author's. The question it raises for you: which of your current agent rules would fail an audit that asks for a file path rather than a sentence?

## 📚 Reading plan
### From the article
- [github.com/paoloap-py/agent-harness-guide](http://github.com/paoloap-py/agent-harness-guide) — the ten scripts with each layer's guard and the same agent without it; run them to see the difference.
### To go further
- Terminal-Bench and Terminal-Bench Pro — the agent benchmark behind the 40x cost comparison; read it to judge how the tasks were chosen.
- Hugging Face harness-versus-scaffold glossary (May 2026) — the vocabulary source the article cites for defining a harness.
- Harness-Bench — the report on lower cross-harness variance in stronger model backends.
- Baseten's Gemma 3 27B clinical-note fine-tuning write-up — the source of the eval-as-reward-signal caveat.
- GEPA continual-learning evaluation on Terminal-Bench 2.0 — the source of the 70.8% to 54.5% drop on new tasks.
