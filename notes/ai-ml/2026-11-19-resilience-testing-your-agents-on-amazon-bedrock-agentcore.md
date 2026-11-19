---
type: Article Summary
title: Resilience testing your agents on Amazon Bedrock AgentCore
description: Injecting tool faults into a live agent shows whether it fails safe or fabricates answers, and observability must catch the silent cases.
resource: https://aws.amazon.com/blogs/architecture/resilience-testing-your-agents-on-amazon-bedrock-agentcore
tags:
- ai-ml
- well-architected
- llm-as-a-judge
- silent-failure
- chaos-engineering
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/resilience-testing-your-agents-on-amazon-bedrock-agentcore
  title: Resilience testing your agents on Amazon Bedrock AgentCore
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/resilience-testing-your-agents-on-amazon-bedrock-agentcore
source: AWS Architecture
source_type: blog
published: '2026-11-18'
domain: ai-ml
concepts:
- well-architected
- llm-as-a-judge
- silent-failure
- chaos-engineering
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d81c2a6abe0f5f75377a5
---

## 🎯 Core thesis
Injecting faults into the tools of a running agent shows whether it fails safe or answers confidently from bad data, and because graceful failures are invisible to infrastructure monitoring, the agent's output itself has to be validated.

## 🧭 Why the author wrote it
Teams test whether services survive infrastructure failures, but they rarely test what an AI agent does when its tools time out or return wrong data. The article starts from the fact that tool calls, networks and upstream services fail in production, and that an untested agent runs on assumptions. If the thesis holds, a healthy-looking availability dashboard can hide an agent that presents corrupted answers as facts.

## Introduction (untitled in the source)
The article opens with a question: when an agent's tools start failing, does it retry, fall back, or admit it cannot finish? It warns that the worse outcomes are a confident answer built on corrupted data, a raw error shown to the user, or a silent failure with no alert to operations. The experiment uses AWS Fault Injection Service (AWS FIS) to test how a Strands agent handles tool-level faults before a real impairment does. The goal is not to check that the tools work. It is to learn what the agent does when a tool fails or returns incorrect data, and whether you can detect it when it happens. The article sets three learning goals:
- Structure a measurable resilience hypothesis for an AI agent.
- Inject tool-level faults (canceled or corrupted tool results) into a running Strands agent on Amazon Bedrock AgentCore, using AWS FIS and AWS Systems Manager (SSM).
- Build the observability to tell a safe failure from a silent one.

### Tool-level faults
A tool-level fault affects one tool call inside the agent, not the network or the infrastructure underneath it. The article distinguishes two kinds: a canceled call that returns an error, and a call that succeeds but carries corrupted data. Testing at this level tells you how the agent's reasoning reacts when one of its inputs is unreliable. This is different from testing whether the tools themselves work.

### Silent failure
A silent failure is one where the system keeps returning normal responses while the answer is wrong or incomplete. The article's main worry is this class of failure, because nothing in the request path looks broken. It treats the difference between a safe failure and a silent one as the thing to measure.

## Solution overview
In this experiment, AWS FIS runs a single SSM Automation action that turns fault injection on, holds it for a set duration, and turns it off. The faults are injected inside the agent, right before or right after a tool call, so the agent's own behavior is tested. The agent keeps serving traffic throughout, and nothing is redeployed to start or stop a run.

Figure 1: AWS FIS runs a single SSM Automation action, which writes /chaos/{runtime_id}/ in Parameter Store. The agent's controller reads the SSM parameters on each invocation and injects the configured faults.

The example agent is based on the public pokemon-team-advisor sample from strands-agents/samples. It has two tools, get_pokemon and get_move, which return structured JSON.

### Coarse and fine control
The article uses two controls to separate deploying the fault code from activating it. The coarse control answers "is chaos loaded?" and is set by one line in the agent constructor, plugins=chaos_plugins(). The fine control answers "is chaos active now?" and is read from values under /chaos/{runtime_id}/ in SSM Parameter Store. Those values are read on every invocation but cached for 10 seconds using a time to live (TTL). Each invocation skips fault injection until SSM returns active=true.

> 🔎 Added context: AWS Systems Manager Parameter Store is an AWS key-value store for configuration values. Here it is the switch that AWS FIS flips on and off during a run.

### Strands Evals ChaosPlugin
The Strands Evals SDK already provides a ChaosPlugin for offline chaos testing, documented in the Strands chaos-testing guide. The article's addition is the ability to run the same faults against a deployed AgentCore runtime serving live traffic, with AWS FIS as the controller. The experiment artifacts are in the FIS template library.

## Two families of fault
The faults fall into two families, and the article treats the second as the more dangerous one.

### Pre-hook faults
Pre-hook faults cancel the tool call before it executes and return an error to the agent. The effects are timeout, network_error, execution_error and validation_error. They test whether the agent detects a failed call and then retries, falls back, or acknowledges the failure.

### Post-hook faults
Post-hook faults let the tool run and then corrupt its response. The effects are truncate_fields, remove_fields and corrupt_values. The article's point is that the agent can receive a successful tool call carrying bad data and answer the user confidently without ever saying the data was degraded.

## Define your hypothesis
Before running the experiment, write down what you expect the agent to do when its tools fail, and name the signals you will judge it by. The article gives this template:

> When tool calls for [agent] are [canceled | corrupted], we expect it to [observable behavior] and to fail safe rather than fabricate. After {x} failed tool calls in {y} minutes | {z} failed tool calls in succession an alarm should be raised and the DevOps team notified within {a} minutes. On recovery, it returns to normal within [time], as measured by [metrics/signals].

After the experiment, compare actual behavior against this hypothesis. The article says the gaps are where the agent needs iteration and refinement.

### Resilience hypothesis
The hypothesis has three parts: the expected behavior under a named fault, the alarm and notification thresholds, and the recovery time with the signals used to measure it. Writing it first turns the experiment into a test with a pass condition rather than an open exploration. The template's placeholders are what you fill in for your own agent.

## Prerequisites
The walkthrough uses the agentcore-strands-agent-faults experiment from the FIS template library on GitHub. That repo provides the chaos controller module, the SSM Automation document, the FIS experiment template and least-privilege IAM policies for each role. You need the following:
> 📋 **Checklist — Prerequisites**
> - [ ] A Strands agent integrated with chaos_plugins() (vendors strands_agentcore_chaos.py and its dependencies: strands, strands-agents-evals, pydantic, and boto3).
> - [ ] The runtime ID of the target AgentCore runtime (resolved automatically from AGENTCORE_RUNTIME_URL).
> - [ ] The SSM Automation document deployed to your account and named ChaosExperiment.
> - [ ] An IAM role for AWS FIS (trusted by fis.amazonaws.com) with the least-privilege policy from the repo.
> - [ ] An IAM role for SSM Automation (trusted by ssm.amazonaws.com) with ssm:PutParameter and ssm:DeleteParameters on /chaos/*.
> - [ ] A chaos-build runtime execution role with ssm:GetParametersByPath on /chaos/*.
> - [ ] The FIS experiment template deployed to your account.

### Least-privilege roles
The setup uses three IAM roles, one each for AWS FIS, SSM Automation and the runtime. Each has only the permissions its step needs. The runtime role can read chaos parameters but cannot write them, and the SSM role is the only one that writes them.

## Integrating chaos into your agent
The chaos plugins attach to the agent constructor with plugins=chaos_plugins(). The article gives a complete deployable file:

```python
import logging
from strands import Agent
from strands_agentcore_chaos import chaos_plugins
from bedrock_agentcore.runtime import BedrockAgentCoreApp
from my_tools import get_pokemon, get_move

logging.getLogger("strands_agentcore_chaos").setLevel(logging.INFO)

agent = Agent(
    model="us.anthropic.claude-sonnet-4-5-20250929-v1:0",
    system_prompt=(
        "You are a Pokémon team-building advisor. Use the get_pokemon and get_move tools. "
        "If a tool call fails, immediately stop, notify the user which data you could not "
        "retrieve, and do not answer from guesses."
    ),
    tools=[get_pokemon, get_move],
    plugins=chaos_plugins(),
)

app = BedrockAgentCoreApp()

@app.entrypoint
def invoke(payload):
    return {"result": str(agent(payload.get("prompt", "")))}

if __name__ == "__main__":
    app.run()
```

**What this does:**
- `logging.getLogger("strands_agentcore_chaos").setLevel(logging.INFO)` sets the log level of the chaos module to INFO.
- `plugins=chaos_plugins()` is the coarse control. It is the one line that makes the runtime capable of injecting faults at all.
- The failure-handling sentence in `system_prompt` is what the article's runs showed producing safe behavior under a fault.
- The names in `tools=[get_pokemon, get_move]` are the names the `fault_injections` keys must match.
- `@app.entrypoint` exposes `invoke` as the runtime's entry point. It passes the prompt to the agent and returns the reply as a string.

### Failure-handling system prompt
The instruction "If a tool call fails, immediately stop, notify the user which data you could not retrieve, and do not answer from guesses" is what separated the good runs from the bad ones. The article is clear that this is a probabilistic mitigation, not a switch. That is why it recommends verifying the prompt with chaos experiments rather than trusting it.

> ❌ **Anti-pattern — Trusting the prompt alone:** Assuming a line in the system prompt guarantees fail-safe behavior. **Why people do it:** not covered in the article. **Consequence:** the behavior changes with nondeterministic runs and the gap is found only in production. **Fix:** verify the instruction with repeated chaos runs, as the article does.

## Configuring the experiment
Update the default FIS experiment template's documentParameters to match your requirements. Two of them matter most. FaultRate decides whether an individual invocation is faulted. FaultInjections defines which faults apply to which tools:

```json
{
    "get_move": [{"effect_type": "timeout"}],
    "get_pokemon": [{"effect_type": "truncate_fields", "max_length": 200}]
}
```

**What this does:**
- Each key is a tool name, and the keys must match the agent's registered tool names exactly.
- `get_move` gets a `timeout` effect, which is a pre-hook fault: the call is canceled before it runs.
- `get_pokemon` gets `truncate_fields` with `max_length` 200, which is a post-hook fault: the tool runs and its response is cut.
- Using two effects from different families in one config tests both failure modes in one run.
- The article warns that a mismatched key does not produce an error. The experiment reports success while doing nothing.

Pre-hook effects cancel the tool call: timeout, network_error, execution_error and validation_error. Post-hook effects corrupt the response: truncate_fields (with max_length), remove_fields (with remove_ratio) and corrupt_values (with corrupt_ratio).

### Write and stop ordering
The SSM Automation document writes fault_rate, fault_injections and execution_id first, then sets active=true last. The article says this means no invocation sees a half-written config. On stop, the document reverses that order and waits for the TTL cache to drain. The onCancel and onFailure handlers run the same clean disable path if a stop-condition alarm trips.

> ❌ **Anti-pattern — Mismatched tool names:** Writing fault_injections keys that do not match the registered tool names. **Why people do it:** not covered in the article. **Consequence:** the experiment completes "successfully" and tests nothing. **Fix:** copy the names from the agent's tool registration and check them against the parameters before starting.

## Running the experiment
Start the FIS experiment, check that chaos is active in Parameter Store, and send a burst of traffic:

```bash
aws fis start-experiment --experiment-template-id <YOUR_TEMPLATE_ID> --region <YOUR_REGION>

# confirm chaos is active
aws ssm get-parameters-by-path --path /chaos/<YOUR_RUNTIME_ID>/ --region <YOUR_REGION>

# send a burst of traffic
for i in $(seq 1 20); do
    agentcore invoke '{"prompt": "Which of these three has the strongest move?"}'
done
```

**What this does:**
- `start-experiment` launches the FIS template, which runs the SSM document that switches chaos on.
- `get-parameters-by-path` under `/chaos/<YOUR_RUNTIME_ID>/` confirms the values are in Parameter Store before traffic starts.
- The loop sends a burst of 20 invocations, the traffic pattern the article uses to exercise the faults.
- The prompt asks a question that needs `get_move` for its answer, so the faulted tool is actually on the path.

## What the fault reveals
The article ran the same fault against two versions of the agent that differ only in the system prompt. The fault was `get_move` set to `timeout` with `fault_rate=1.0`, with five runs per configuration. Configuration A had no failure-handling prompt. Configuration B had it.

> Figure 2: The agent with no failure-handling prompt (Configuration A) made 21 tool calls on the broken tool, then answered from training knowledge. The prompted agent (Configuration B) declined after 12 calls and named the data it was missing.

The prompt did two things consistently. It cut the retry count: Configuration A used about 1.35x the tool calls of the unfaulted baseline on average and rose to 2.5x in its worst run. More important, it changed what the agent did when retries ran out. When the question was asked as a real user would ask it, Configuration A answered from prior training knowledge in every run and presented that data as fact. Configuration B named the missing data in every run.

The article quotes one verbatim answer from Configuration A, given after every `get_move` call had failed:

> "I'm experiencing technical difficulties with the move lookup tool. However, based on standard Pokémon move data: Among Mamoswine, Nidoking, and Garchomp, the one that learns the single most powerful move is Nidoking, with Megahorn at 120 power…"

The author's reading is that this is one clause of hedging followed by a confident, detailed answer built on aged training data. With proprietary inventory levels, prices or account balances, where the model has no relevant data at all, the same behavior could produce fabricated numbers with the same authority. A user, or another agent, could act on them without anyone noticing.

### Three outcome classes
The same impairment produced three classes of outcome depending on configuration. **Fail safe** means the agent detects the failure, declines or adds a caveat, and shows the user no raw error. **Resilient work-around** means the agent retries or routes around the broken tool at a multiple of the normal tool calls, which costs latency, tokens and money. **Confidently wrong** means the agent falls back to trained data and presents it as if the tools had answered. The article says this last class erodes trust invisibly, and that the failure-handling prompt eliminated it.

### Corruption is harder
When the author switched to post-hook faults, fourteen out of fifteen unprompted runs produced a confident answer with no mention that the data was degraded. The agent caught corruption only where it had strong priors about what values should look like. For example, it recognized that 99999 is not a plausible Pokémon move power. On proprietary values there are no such priors. Adding the failure-handling prompt changed the result: all five prompted runs declined and named the invalid data.

### LLM-as-a-judge evaluators
For scoring beyond manual transcript review, the Strands Evals SDK ships three LLM-judge evaluators: FailureCommunicationEvaluator, RecoveryStrategyEvaluator and PartialCompletionEvaluator. Each rates these outcome classes on a 0 to 1 scale. This makes the comparison repeatable, though the article does not discuss how reliable the judges themselves are.

> ❌ **Anti-pattern — Reading one transcript as the result:** Judging the agent from a single run and treating it as proof. **Why people do it:** not covered in the article. **Consequence:** nondeterministic agent responses can make a lucky run look like a safe design. **Fix:** run each scenario multiple times and score the runs.

## Observing the fault
A tool-level fault that the agent handles gracefully is invisible to infrastructure monitoring. During the author's experiment, every Amazon CloudWatch error metric read exactly 0.0. That included invocations where every tool call was canceled or corrupted. Every invocation returned HTTP 200, because from the runtime's point of view nothing had failed. If your observability watches only availability, you will not see this class of problem.

> ❌ **Anti-pattern — Availability-only observability:** Monitoring whether the runtime responds and returns 200. **Why people do it:** not covered in the article. **Consequence:** a confidently wrong agent looks healthy in every dashboard. **Fix:** validate the agent's tool outputs and emit metrics from that validation, as in the next section.

### Silent failure in practice
The article's distinction is between a safe failure, which someone can see, and a silent failure, which looks like success. The metric that matters is therefore one that records bad tool data inside the agent, not whether the infrastructure stayed up.

## Mitigations
The article's main mitigation is to validate tool output inside the agent. The approach adds a tool-observability plugin on the Strands AfterToolCallEvent hook. The plugin validates the output against a Pydantic contract and emits an Embedded Metric Format (EMF) log line. The example model is:

```python
from pydantic import BaseModel, Field

class PokemonOutput(BaseModel):
    name: str = Field(min_length=4)  # catches truncate_fields
    pokedex_id: int
    types: list[str]
```

> 🔎 Added context: Pydantic is a Python library that checks data against classes whose attributes have type annotations. It raises a ValidationError when data does not match.

**What this does:**
- `BaseModel` turns each annotated attribute into a validation rule, and passing the tool's output through it raises a ValidationError when the data breaks the contract.
- Declaring `name`, `pokedex_id` and `types` without defaults makes them required, so `remove_fields` fails with a "field required" error.
- `pokedex_id: int` and `types: list[str]` check the shape of each value, which catches a `corrupt_values` fault that changes a value's type.
- `name: str = Field(min_length=4)` is a constraint on the value itself. Truncating "mamoswine" to "mam" leaves a valid string, so only the length rule catches it.
- To catch a value that has the right type but is nonsensical, such as a negative or absurdly large Pokédex id, the article suggests an explicit range check like `Field(ge=1)`.

### Type checks and constraint checks
The article draws a clear line between the two kinds of check. A type annotation checks the shape of a value. A constraint checks the value itself. Shape checks catch type changes and missing fields, and constraints catch plausible-looking but wrong values. You usually need both.

### Embedded Metric Format
With EMF, CloudWatch creates metrics directly from the log entry, and those metrics can be alarmed on. This is how the validation failures become something an operations team can be paged for.

### Amazon Bedrock Guardrails
The article recommends complementing this approach with Amazon Bedrock Guardrails, such as content filtering and contextual grounding checks, when running agents on Amazon Bedrock.

### Structured outputs do not check tool results
Structured outputs and strict tool use for recent Claude models guarantee that what the model emits matches a schema, both its final answer and the arguments it passes to tools. The article points out that they say nothing about whether the tool's returned data was correct. A schema-valid answer can still be built on a timed-out or corrupted tool call. The length and range constraints that caught truncation and corruption are also not enforced server-side.

> ❌ **Anti-pattern — Assuming schema-valid means correct:** Treating a schema-checked answer as verified. **Why people do it:** not covered in the article. **Consequence:** an answer built on corrupted tool data passes every check. **Fix:** add validation on the tool outputs themselves, as the Pydantic contract does.

## Best practices
The article closes its guidance with this list:

> 📋 **Checklist — Best practices**
> - [ ] Use a dedicated chaos runtime.
> - [ ] Engineer the behavior you want, then test it.
> - [ ] Run each scenario multiple times.
> - [ ] Validate agent output, not just availability.
> - [ ] Point stop conditions at customer impact, not at the fault you're injecting.

The article explains each item briefly. Do not inject faults into production, and verify that the production runtime omits chaos_plugins() and has no read access to /chaos/*. Write the failure-handling guardrail first, then use chaos to prove it holds. Agent responses are nondeterministic, so a single clean run is an anecdote rather than evidence. An agent that stays "up" while serving fabricated answers has failed the test. Stop conditions should alarm on task success rate, not on the tool-error metric the experiment is designed to move.

## Cleaning up
The experiment deletes its own parameters when it ends, but the surrounding resources persist. The article gives the commands to remove them:

```bash
# Delete FIS template and SSM document
aws fis delete-experiment-template --id <YOUR_TEMPLATE_ID> --region <YOUR_REGION>
aws ssm delete-document --name ChaosExperiment --region <YOUR_REGION>

# Delete the chaos runtime, container image, and log group
aws bedrock-agentcore-control delete-agent-runtime \
    --agent-runtime-id <YOUR_RUNTIME_ID> --region <YOUR_REGION>
aws ecr delete-repository --repository-name <YOUR_REPO> --force --region <YOUR_REGION>
aws logs delete-log-group --log-group-name <YOUR_RUNTIME_LOG_GROUP> --region <YOUR_REGION>
```

**What this does:**
- The first two commands delete the FIS experiment template and the ChaosExperiment SSM document, so the experiment cannot be started again.
- `delete-agent-runtime` removes the chaos runtime itself.
- `delete-repository --force` removes the ECR repository along with its container image.
- `delete-log-group` removes the runtime's log group.
- The article adds that the three IAM roles and any stop-condition alarm must be detached and deleted separately.

## Conclusion
The article's point is that the value of the experiment is not confirming that the agent keeps running when its tools fail. The value is finding out what the agent does with bad data, and whether you would notice. In the author's test, the same timeout produced a clean decline from one agent and a confident, wrong answer from another, and the two differed by one line in a prompt. The article recommends starting with a low fault rate and a single timeout in a non-production chaos runtime, fixing what breaks, then escalating to corrupted results and running these experiments regularly as the agent evolves.

## For more information
The article lists these references. Only the article's own URL was captured in the extracted text, so the others appear as titles:
- agentcore-strands-agent-faults experiment in the FIS template library — the source of the controller, SSM document, FIS template and IAM policies.
- Strands Agents SDK — the agent framework used in the example.
- Strands chaos-testing guide — the offline chaos testing that the article extends to live runtimes.
- Amazon Bedrock AgentCore documentation — the runtime the agent is deployed to.
- Amazon CloudWatch Embedded Metric Format — how EMF log lines become metrics.
- AWS Well-Architected Reliability Pillar: Test resiliency — the reliability guidance the experiment follows.

## About the authors
The extracted article text contains this heading only. No author biographies were captured.

## 🧠 Mental models
### Three outcome classes of a tool failure
**The model:** When a tool fails, the agent's response falls into one of three classes: fail safe, resilient work-around, or confidently wrong.
**How to apply it:** For every tool in your agent, decide which class you want, then test whether a fault produces it. Treat confidently wrong as the one to eliminate first.

### Shape checks versus constraint checks
**The model:** A type annotation checks the shape of a value, and a constraint checks the value itself. Each catches failures the other misses.
**How to apply it:** For each tool output field, add a type for shape and a constraint such as min_length or ge for the value. Test each rule against a truncated or corrupted sample.

### Availability is not correctness
**The model:** A system can stay up, return HTTP 200 and still serve fabricated answers. Availability metrics cannot show that.
**How to apply it:** Pair availability alarms with validation metrics that record bad tool data inside the agent, and use customer-impact measures as stop conditions.

## 🔑 Key takeaways
- A tool timeout produced a clean decline in one configuration and a confident, wrong answer from training data in another. The only difference was one line in the system prompt.
- Post-hook corruption was harder to catch. Fourteen of fifteen unprompted runs gave confident answers without flagging degraded data.
- A failure-handling prompt changed that result: all five prompted runs declined and named the invalid data.
- Graceful failures are invisible to infrastructure monitoring. CloudWatch error metrics read 0.0 while every tool call was failing and every invocation returned HTTP 200.
- Pydantic contracts catch truncation, missing fields and type changes, but only range or length constraints catch plausible-looking wrong values.
- Structured outputs guarantee the shape of the model's output, not the correctness of the tool data it was built on.
- Run each scenario several times, because a single clean run is not evidence, and alarm on task success rather than on the fault being injected.

## 🏛️ For Kenji
> 🔎 This is my interpretation, not the author's. For an architect, the useful part is the separation between deploying the fault code and activating it, which lets a production-like runtime be tested without a redeploy. It raises a question for review boards: which of your agent's tool outputs have a Pydantic-style contract, and which tools would your monitoring show as healthy if they returned plausible but wrong data?

## 📚 Reading plan
### From the article
- Resilience testing your agents on Amazon Bedrock AgentCore (the original article) — the full source, with the figures and the exact wording of the hypothesis template. [Resilience testing your agents on Amazon Bedrock AgentCore](https://aws.amazon.com/blogs/architecture/resilience-testing-your-agents-on-amazon-bedrock-agentcore) — read it to check the figures and the reference list, which this note does not reproduce in full.
- agentcore-strands-agent-faults experiment in the FIS template library — the working code, SSM document and IAM policies. (Link not present in the extracted text.)
- Strands chaos-testing guide — the offline version of the same faults, for comparison. (Link not present in the extracted text.)
- Supported models by AWS Region — for checking model availability before you deploy. (Link not present in the extracted text.)
- Amazon Bedrock Guardrails — the content and grounding controls the article recommends alongside its own validation. (Link not present in the extracted text.)
### To go further
- Chaos engineering — the general practice this experiment is an example of, for designing future fault experiments.
- Stop conditions for fault-injection experiments — how to choose alarms that protect customers without ending the test too early.

## 🔗 Concepts
- [well-architected](/concepts/well-architected.md)
- [llm-as-a-judge](/concepts/llm-as-a-judge.md)
- [silent-failure](/concepts/silent-failure.md)
- [chaos-engineering](/concepts/chaos-engineering.md)
