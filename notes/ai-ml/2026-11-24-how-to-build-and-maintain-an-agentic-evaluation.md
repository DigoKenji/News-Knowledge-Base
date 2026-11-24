---
type: Article Summary
title: How to build and maintain an agentic evaluation
description: Agentic evaluations are trustworthy only when every component is built, validated and shared openly, since each
  one moves the score.
resource: https://huggingface.co/blog/agentic-evals
tags:
- ai-ml
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://huggingface.co/blog/agentic-evals
  title: How to build and maintain an agentic evaluation
  author: Avijit Ghosh, Ben Burtenshaw, Irene Solaiman
status: stable
source_url: https://huggingface.co/blog/agentic-evals
source: Hugging Face
source_type: blog
published: '2026-11-23'
domain: ai-ml
concepts:
- agent-harness
- ai-agent
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d81dd9b05c464970d8244
---

## 🎯 Core thesis
An agentic evaluation is only trustworthy when its task, environment, harness, verifier and traces are each built, checked and shared openly, because every part of the surrounding software can change the score.

## 🧭 Why the author wrote it
98.2% of model-benchmark scores are reported by only one party, and held-back test sets make results harder to check. The authors argue that sharing infrastructure, grading methods and evidence, with controlled review where public release is unsafe, makes scores checkable and reusable.

## Overview
The guide covers the three eras of evaluation (section 1), an eight-step walkthrough (section 2 and Appendix A), how benchmark errors are corrected (section 3), and what to open strategically (section 4).

### Agentic evaluation
An evaluation gives a system tasks, checks its answers or actions, and produces a score. An agentic evaluation scores what an agent changed in its environment, not only the text it returned. The Hugging Face hub hosts about 3,500 eval leaderboards and about 17,000 eval datasets, a roughly 16X growth in the past year.

## 1. Introduction: how evaluation design has changed
Evaluation design has moved through three forms as models became more capable: question-answer, agentic, and teams or swarms (Figure 1). The article focuses on agentic evaluations because they dominate model release announcements and show clearly how design choices affect scores. Team and swarm evaluations are out of scope.

### Question-answer benchmark
MMLU (2020) and GPQA (2023) check a response against a key, measuring knowledge and reasoning through the answers a model produces. Scoring needs no account of the environment the system ran in.

### Agentic benchmark
SWE-bench and WebArena ask a system to resolve software issues or complete website tasks, so the setup must account for the environment and for the changes the system makes. In an audit of Terminal-Bench, researchers kept the model fixed and changed only the agent harness; accuracy varied by up to 29.9 percentage points.

## The anatomy of an agentic evaluation
Figure 2 shows the parts. Each can be published, inspected and reused separately.

### Task
The task gives the agent its instruction. Its units, parameter definitions and output format must be explicit, or the score partly measures guesswork.

### Agent harness
The agent harness wraps the model and its tools, letting it work inside a pinned environment and turning model responses into actions such as shell commands.

### Rollout
A rollout is one attempt at a task from start to finish. The agent may run several commands, inspect their output and retry before it stops.

### Evaluation harness
The evaluation harness runs several rollouts per task, collects traces and aggregates scores. 
### Verifier
When a rollout ends, the verifier grades the environment's final state. A sound verifier grades the saved output, not the agent's claim to have finished.

### Trace
A trace is the full record of a rollout: messages, tool calls and what each command returned. Traces and scores together form the published evidence.

## Why benchmark release decisions matter
Artificial Analysis says 60% of its Intelligence Index v5, announced in October 2026, rests on private test sets or held-out answers. Google DeepMind has piloted double-blind evaluations.

### Held-out test set
Test items kept private so models cannot be trained or tuned on them. Privacy reduces contamination but does not guarantee quality, and withholding supporting material makes expert review harder.

### Double-blind evaluation
A secure enclave hides an outside evaluator's test prompts from the model provider and hides the model's weights from the evaluator. This reduces contamination while keeping outside evaluation possible.

## 2. Building an eval: what to reuse, adapt, and validate
The authors build one agentic evaluation for a physics lab, adapting an open task so most effort goes into domain expertise and validation. The eight steps are: define the claim, find a task, run it unchanged, adapt it, check the evaluation, read the traces, report the score, and compare models.

## Method
### Claim
A claim is a plain statement of what the agent can do that the evaluation supports. This one is deliberately narrow: fit the specified Raman peaks, calculate the requested D-to-G ratio (also called the defect ratio, the intensity of one peak relative to another, which can indicate defects), and save accurate results consistently. Reviewing a whole lab report would need more skills and evidence.

**Starting task and baseline.** The authors reused Harbor's task format and Terminal-Bench's raman-fitting task. Harbor is an open-source evaluation harness; Terminal-Bench is an open benchmark of tasks completed in a terminal. Only the agent harness needed no new work.

**Adaptation.** raman-lab has eight synthetic spectra with known parameters and varied noise and defect levels. It fits three peaks instead of two and reports a defect ratio. An agent helped generate the spectra; the authors reviewed them by hand and wrote new instructions, reference fits and output checks.

### Development set
Tasks used while building and debugging an evaluation, as opposed to a held-back test set for final scores. Eight tasks are too few to generalize; broader coverage should follow the variation in real measurements.

**Checks.** Before running more models, the authors tested the evaluation itself with three controls: a reference solution, a do-nothing agent and a shortcut.

### Oracle
The oracle runs the known correct solution and should pass every task. If it fails, the fault lies in the task, environment or verifier, and model failures cannot be read until it is fixed.

### No-op agent
An agent that does nothing and should fail every task. A blank exam paper should not pass.

### Shortcut
The shortcut skipped fitting: it read each peak's tallest point from the data, guessed widths and divided D height by G height. It returned the expected defect ratio on all eight tasks, so a ratio-only grader would have given full marks. The authors added checks on each fitted peak, and the final verifier rejects it.

> ❌ **Anti-pattern — ratio-only grading:** the verifier checks only the final number. **Why people do it:** the expected answer is easy to state and compare. **Consequence:** a method that skips the required skill scores full marks (here 8 of 8 tasks). **Fix:** check what the output demonstrates, such as each fitted peak.

## Results
**Baseline.** On the original task, gpt-oss-120b passed 0 of 3 rollouts. Two declared completion before observing relevant output; the third could not read the data file, which uses decimal commas that the reference solution converts before loading. The score alone could not tell these from an incorrect fit. The traces could.

### Early completion
The agent declares the task finished before observing any command output, here by calling `mark_task_complete` in its first response. The article uses it as a diagnostic: most failures in this run came from stopping early.

**Traces and score.** On the eight adapted tasks, with three rollouts each, the setup passed 11 of 24. In 19 rollouts the model completed early; 11 of those produced no results file, often because a script failed on missing NumPy or SciPy. Of the 13 failures, only two failed the fit check (Figure 7). Pass@3 was 7/8 and pass^3 was 1/8. The mean pass rate was 0.46, with a clustered standard error of about 0.11. A repeat run passed 10 of 24.

> ❌ **Anti-pattern — reading the score alone:** treating a low pass count as a fitting defect. **Why people do it:** the score is the only summary most harnesses show. **Consequence:** most failures here were early stops and missing packages, not wrong fits. **Fix:** group failed rollouts by cause from the traces: parsing, tool execution, stopping early, or grading.

## Model comparison
The authors compared six models on the same eight tasks. Several providers serve one open-weight model with different quantization and context limits, so they pinned one provider per model. With DeepInfra pinned, gpt-oss-120b passed 13 of 24 rollouts, against 11 of 24 on the default provider. Only gpt-oss-120b declared completion in its first response, in 22 of 24 rollouts. Qwen3.8-27B, DeepSeek-V4.1-Flash and MiniMax-M3.1-Flash passed all 24 rollouts, so these tasks cannot rank them.

### Quantization
Storing model weights at lower numerical precision to run faster and cheaper, which can change outputs slightly. Two providers serving the same model can therefore score differently.

### Pinning
Fixing exact versions of software, images or model providers so a run can be repeated. The pinned provider should be recorded beside each score.

> ❌ **Anti-pattern — unpinned model provider:** letting the router choose the provider. **Why people do it:** it is the default and needs no thought. **Consequence:** the same model scored 11 of 24 on the default provider and 13 of 24 on DeepInfra. **Fix:** pin the provider (for example `:deepinfra`) and record it with the score.

## 3. Bitter lessons: when a benchmark is wrong
The shortcut returned the right answer without fitting a spectrum. The authors found it by trying to break the grader with a solution they knew was incomplete. Like a student who copies the final number without the working, it was right for the wrong reason. The lesson: a faster valid method should pass, and a method that skips a required skill should not. A review of agentic benchmarks found that flaws in task setup or reward design can shift measured performance by up to 100% in relative terms.

### Tolerance
The margin a verifier allows when comparing a result with a reference. Set too tight, it rejects valid alternative fits; set too loose, it accepts work that misses the task's point. Domain experts help set it.

## Public review is essential for correcting benchmarks
Public review exposes errors that builders missed. Terminal-Bench 2.1 fixed 28 of its 89 tasks after user reports, including one whose tests expected Spark SQL while its instructions asked for PostgreSQL; agent rankings stayed stable. In July 2026 OpenAI estimated that about 30% of SWE-Bench Pro tasks were broken and withdrew its recommendation to adopt it.

### Outside audit
An independent check of a benchmark's tasks and tests by reviewers outside its builders. Its cost depends on what reviewers can read. CritPt keeps reference solutions private, so its physicist auditors had to solve the problems and found errors in 21 of the 56 challenges they reviewed. OpenAI's SWE-bench Verified audit, working from readable tasks and tests, found material issues in 59.4% of the 138 problems it examined.

## Check the claim as well as the code
A consistent score can still measure the wrong thing. A review of 445 LLM benchmarks by 29 expert reviewers (Measuring what Matters) found recurring patterns that undermine construct validity. Fitting synthetic spectra does not show that an agent can review lab reports; that may also require recognizing a poor measurement or asking for help. Harder Than the Task extends this to organizational work, where fewer than one quarter of prominent benchmarks claim to cover authority or dependencies. If those factors matter to the claim, they belong in the evaluation; if not, say they remain untested.

### Construct validity
Whether a benchmark's tasks and metrics support the claims made about the capability it is meant to measure. A high, consistent score does not establish it.

Each error deserves a short record: the failed assumption, how it was found, what changed, the affected versions, and whether earlier scores remain comparable.

## Contamination vs saturation
The SWE-bench Verified audit also found contamination: every frontier model tested could reproduce the reference patch or verbatim problem details for some tasks, because public repositories and wide use exposed tasks to training.

### Contamination
Test questions or solutions appearing in training data, so a score may reflect memory rather than skill.

### Saturation
The point where most models reach top scores on a benchmark, so it no longer separates them. A study of 60 language model benchmarks found similar saturation in public and private ones; what correlated with lower saturation was expert curation and a larger test set. Hiding data therefore does not by itself prevent saturation.

> ❌ **Anti-pattern — hiding the test set as the fix:** keeping items private and stopping there. **Why people do it:** contamination is visible and hiding is easy. **Consequence:** hidden benchmarks saturated much like public ones. **Fix:** pair protection with expert curation, a larger test set and a refresh plan.

## 4. Open sourcing an evaluation strategically
Creators can release part by part: protect active test tasks while sharing infrastructure, grading methods and reference examples. ARC-AGI-2 publishes a training set and a 120-task evaluation set, and holds back semi-private and private sets of 120 tasks each. AILuminate keeps official prompts confidential but publishes its methods and taxonomy. No public reviewer can check a protected item, so whatever stays closed needs its own review route.

### Canary string
A unique marker in benchmark files that lets model trainers find and exclude them from training data. Terminal-Bench marks its task files this way.

### Gated dataset
A Hugging Face dataset that can be downloaded only after the owner approves an access request. The article suggests one for active test items, with manual approval of each request.

Table 1 sets out, by component, what to open, where it lives on the Hub, and what can stay protected. The condensed version:

| Part | Can be open | Can stay protected |
|---|---|---|
| Task | Specs, categories, construction methods, public dev split | Active test items and answers (gated); retired items where permissible |
| Environment | Container definition, build files, pinned dependencies, synthetic fixtures | Private data, credentials, licensed assets, dangerous details |
| Agent harness and tools | Loop code, tool interfaces, adapters, permissions, budgets | Secrets, private services, sensitive tool code; give safe substitutes |
| Rollout and trace | Public-task traces, reviewed excerpts, analysis code | Traces exposing tasks, private data or restricted behavior |
| Reference solution | Solution method and code for public examples | Solutions revealing protected items; authorized reviewers check them |
| Evaluation harness | Orchestration, model config, logging, aggregation, version records | Credentials and item-identifying config; a reviewable record of the restricted run |
| Verifier | Rubrics, grader code, tolerances, worked examples | Reference answers and logic revealing protected items |
| Claims and task design | Intended use, selection rationale, assumptions, validation evidence | Sensitive source material (may need redaction) |
| Results | Scores, uncertainty, costs, exclusions, limitations, setup | Task-revealing detail; publish safe summaries |
| Corrections | Correction history naming affected versions | Protected example behind a correction |

On the Hub, task and environment go in a dataset repository with a dataset card; results are registered as a Hub Benchmark, with scores in each model repo's .eval_results/ folder linked to the trace dataset. The authors' raman-lab release follows this pattern. A clinical evaluation could likewise open its harness and grading framework while keeping patient records under controlled access. Each restriction should come with a condition for revisiting it.

## Strategic open sourcing checklist
Before releasing or retiring an evaluation, check that another builder can:

> 📋 **Checklist — Strategic open sourcing**
> - [ ] Identify what it measures, its intended use, and the conditions it leaves out.
> - [ ] Find the reusable artifacts, their licenses and versions, and a way to report problems.
> - [ ] See how a do-nothing agent scores and, where possible, how a human expert does.
> - [ ] Read each score with its uncertainty, and with guidance on what known flaws mean for it.
> - [ ] Learn which flaws were found, how they were handled, and how far they could move the results.
> - [ ] Know how and when tasks will be refreshed to limit contamination and overfitting.

## 5. Conclusion: giving the evaluation a useful afterlife
Benchmarks are built, adopted, optimized against, and eventually stop separating the systems they were made to compare. The saturation study rated 29 of its 60 benchmarks as highly saturated, and saturation is not a failure in itself. When tasks, environments, verifiers and traces stay open, each keeps working after the leaderboard stops: a solved task becomes a regression check, and its verifier can seed a harder successor. MMLU's questions became MMLU-Redux, and Terminal-Bench 2.0 became 2.1.

### Benchmark lifecycle
The stages a benchmark passes through, from construction to saturation. Open releases stay useful at every stage, while a published score alone supports little beyond reading the leaderboard.

> ❌ **Anti-pattern — publishing only the score:** releasing the leaderboard number without its tasks, environment or traces. **Why people do it:** the score is the headline, and packaging the rest takes effort. **Consequence:** future builders cannot rerun, adapt or check it. **Fix:** publish tasks, environments, verifiers and traces alongside the score.

## Appendix A. Practical example: agentic deployment in a physics lab
The lab wants an agent to fit peaks in a Raman spectrum. Terminal-Bench 2 already includes raman-fitting, which fits a Lorentzian curve to two peaks and saves eight parameters to /app/results.json. That is a runnable starting point for a narrower lab evaluation. Table 2 lists what each part provides and what to check before reuse.

| Part | In raman-fitting | Check before reuse |
|---|---|---|
| Task | Two peak fits saved to /app/results.json | Are units, parameter definitions and output format explicit? |
| Environment | Pinned container image with the spectrum; resource limits in task.toml | Which packages must the agent install? Are resources and permissions suitable? |
| Agent harness and tools | Terminus-2 connects the model to shell commands | Can the agent inspect what its commands produced? |
| Rollout and trace | One attempt, in agent/trajectory.json | Does the record show what happened before the agent stopped? |
| Reference solution | solution/solve.sh runs the known fit | Does it pass in the supplied environment? |
| Evaluation harness | Harbor runs rollouts and collects scores and traces | Are versions and execution settings recorded? |
| Verifier | tests/test.sh checks eight fitted values against tolerances | Does it grade the saved file and accept valid alternative fits? |

### Raman spectroscopy
Raman spectroscopy shines a laser on a sample and measures shifts in the frequency of the scattered light. The resulting peaks reveal properties of the material.

### Graphene D, G and 2D peaks
For graphene, an atom-thick sheet of carbon, the G and 2D peaks near 1580 and 2700 cm⁻¹ identify the number of layers. The D peak appears when the lattice is disordered, and its intensity relative to G can indicate defects.

### Lorentzian
A mathematical curve commonly used to model the shape of a spectral peak.
## Step 1. Define the claim
A claim is a plain statement of what the agent can do. Break it into checkable skills, and ask domain experts to choose realistic noise levels, tolerances and software.

## Step 2. Find a starting task
Look under the Benchmark and Environment dataset types on the Hugging Face Hub, and inspect each candidate's instructions, environment, reference solution and tests. Even a task that models already solve can provide useful infrastructure.

## Step 3. Run the task unchanged
Run the reference solution before any model; if it fails, fix the task, environment or verifier first. The authors ran Harbor inside an HF Job, each rollout in its own HF Sandbox, with Inference Providers supplying model responses.

### HF Sandbox
An isolated Hugging Face environment where a single rollout runs its commands and stores its files.

### Inference Providers
A Hugging Face service that routes model requests to third-party providers through one API, called the router.

```bash
harbor run -t terminal-bench/raman-fitting \
  -a oracle -e hf-sandbox
OPENAI_API_KEY="$HF_TOKEN" harbor run \
  -t terminal-bench/raman-fitting -k 3 -e hf-sandbox \
  -a terminus-2 -m openai/openai/gpt-oss-120b \
  --ak api_base=https://router.huggingface.co/v1
```

**What this does:**
- The first command runs the oracle, the known solution, to confirm the task is solvable in the sandbox.
- The second runs three rollouts (`-k 3`) of the Terminus-2 terminal agent with gpt-oss-120b.
- `-t` names the task, `-a` the agent, `-e` the environment, `-m` the model and `-k` the rollouts per task.
- Setting `OPENAI_API_KEY` to the Hugging Face token authenticates the calls to the router.

## Step 4. Adapt the task
The original instruction.md asks for x0, gamma, amplitude and offset without defining them or their units. The reference solution treats them as peak center, half-width at half-maximum, height above background and background level, and converts the x-axis from nanometers to Raman shift. The adapted instructions state these choices, and the defect ratio must say whether intensity means height or integrated area.

> 🔎 Added context: The half-width at half-maximum is the distance from a peak's center to the point where its height falls to half the maximum. Raman shift is the frequency difference between the laser light and the scattered light, measured in cm⁻¹.

### Synthetic data
Data generated from known parameters, so the correct answer is known in advance. The authors generated raman-lab's spectra this way and then reviewed whether the examples represented the intended problem.

The shared format can stay as it is while the data and checks change. This command creates a task scaffold:

```bash
harbor task init my-org/task-01 -p datasets/my-evals
```

**What this does:**
- Creates a new task scaffold under `datasets/my-evals`.
- Keeps the shared task format fixed, so only the data and checks change between tasks.

> 📋 **Checklist — Verifier design rules**
> - [ ] Grade what the agent produced: inspect the saved output, not its claim to have finished. τ-bench does this by comparing the final database state with an annotated goal state.
> - [ ] State every checked fact in the instruction, including units, parameter conventions, tolerances, and required fields.
> - [ ] Generate synthetic data from known parameters so the reference answer is available, then review whether the examples represent the intended problem.

## Step 5. Check the evaluation
Before any model run, test the adapted tasks in `datasets/my-evals`:

```bash
harbor run -p datasets/my-evals -a oracle -e hf-sandbox
harbor run -p datasets/my-evals -a nop -e hf-sandbox
```

**What this does:** the first command runs the oracle, which should pass every task; the second runs the no-op agent (`nop`), which should fail every task.

> 📋 **Checklist — Validate the evaluation before model runs**
> - [ ] The oracle should pass every task, i.e., the known solution must work in the supplied environment.
> - [ ] The no-op agent does nothing, so it should fail every task. A blank exam paper should not pass. A review of agentic benchmarks found one that counted empty responses as successes.
> - [ ] A shortcut that skips required work should fail. One way to test this is with a deliberately lazy solution, such as one that outputs a plausible answer without doing the work, to see whether the verifier accepts it. Also try plausible wrong answers and alternative correct methods, to catch both false passes and false failures.

## Step 6. Run it and read the traces
Harbor records messages, tool calls and observations for each rollout in agent/trajectory.json. Group failures by cause (parsing, tool execution, stopping early, or grading). Missing NumPy or SciPy was a common early failure; preinstalling them would change the condition, so keep the model, harness, tasks and rollout count fixed when comparing. Terminal-Bench 2.1 repaired nine tasks whose external dependencies had drifted.

### ATIF
The Agent Trajectory Interchange Format, a shared format for an agent's messages, tool calls and observations. Harbor writes it for each rollout.

## Step 7. Report the score
### pass@k
The share of tasks where at least one of k rollouts succeeds. Pass@3 was 7/8: seven tasks succeeded at least once. This suits work where a person reviews the output and retries are acceptable.

### pass^k
The share of tasks where all k rollouts succeed. Pass^3 was 1/8. Unattended processing needs consistency, so this measure matters more.

### Clustered standard error
Rollouts of the same spectrum share its difficulty, so they are not independent. Clustering by task gives the honest margin: a mean of 0.46 with a standard error of about 0.11.

> ❌ **Anti-pattern — reporting pass@k for unattended work:** headlining the at-least-once number. **Why people do it:** it is the higher, more flattering figure. **Consequence:** a task that succeeds once in three counts as solved. **Fix:** report pass^k, or the mean with its clustered standard error, when no person reviews the output.

Report tokens, elapsed time and tool calls beside the score, so readers can compare setups that reach similar results at different costs.

## Step 8. Compare models
A single failing run cannot tell whether the model, the harness or the evaluation is at fault. When several models fail the same task, inspect the instructions, environment and verifier for a shared cause. When one model fails in a distinctive way, compare its traces with successful attempts. Each new model is one more value for `-m`, and a suffix such as `:deepinfra` pins the provider.

```bash
OPENAI_API_KEY="$HF_TOKEN" harbor run -p datasets/my-evals -k 3 \
  -e hf-sandbox -a terminus-2 -m openai/Qwen/Qwen3.8-27B:deepinfra \
  --ak api_base=https://router.huggingface.co/v1
```

**What this does:** runs three rollouts per task (`-k 3`) of Terminus-2 over `datasets/my-evals`, with Qwen3.8-27B pinned to the DeepInfra provider by the `:deepinfra` suffix.

Inference for all six models cost under $5 at list prices. The median raman-lab rollout used about 6,000 tokens for gpt-oss-120b and about 90,000 for MiniMax-M3.1-Flash.

## How we reused existing open components
Reuse covered the task format, Harbor's runner, Terminus-2 and the `solution/solve.sh` and `tests/test.sh` conventions. The new work was the narrower claim, the spectra generator, the reference fits, and the fit and shortcut checks.

## References
The article's roughly 30 references are not reproduced; the reading plan keeps those most tied to the argument.

## Glossary
Terms not defined above: **context limit** (the most text, in tokens, a model reads in one request); **container image** (a fixed software package, so every rollout starts from the same environment); **run record** (the exact task, environment, model configuration and code versions behind a score).

## 🏛️ For Kenji
This is your interpretation, not the author's: for architecture work, an evaluation is a system with its own dependencies, versions and correction history, so it needs the same governance as the agents it judges. The run record, pinned harness and corrections log are artifacts that an architecture review or AI governance process could require. Question: which of our agent acceptance tests would a no-op or shortcut pass?

## 📚 Reading plan
### From the article
- [Harbor documentation](https://docs.harborframework.com/) — the evaluation harness used throughout, including its trace exporter.
- [Terminal-Bench 2.1 release notes](https://www.tbench.ai/news/terminal-bench-2-1) — how user reports fixed 28 tasks and a Spark SQL versus PostgreSQL mismatch.
- [Why we no longer evaluate SWE-bench Verified](https://openai.com/index/why-we-no-longer-evaluate-swe-bench-verified/) — the audit behind the 59.4% figure and the contamination findings.
- [Measuring what Matters](https://huggingface.co/papers/2511.04703) — the 445-benchmark review behind the construct-validity argument.
- [When AI Benchmarks Plateau](https://huggingface.co/papers/2602.16763) — the 60-benchmark saturation study.
- [Harder Than the Task](https://arxiv.org/abs/2611.03494) — why organizational difficulty is missing from most benchmarks.
- [Piloting the world's first double-blind AI evaluations](https://deepmind.google/blog/piloting-the-worlds-first-double-blind-ai-evaluations/) — the secure-enclave pilot.
- [τ-bench](https://huggingface.co/papers/2406.12045) — the goal-state comparison that the article uses as a model for grading outputs.
### To go further
- Clustered standard errors for rollout-based scores — how to avoid overstating confidence when tasks are few.

## 🔗 Concepts
- [agent-harness](/concepts/ai-ml/agent-harness.md)
- [ai-agent](/concepts/ai-ml/ai-agent.md)
