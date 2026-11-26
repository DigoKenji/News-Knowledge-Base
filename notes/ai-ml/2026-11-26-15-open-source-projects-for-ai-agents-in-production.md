---
type: Article Summary
title: 15 Open Source Projects for AI Agents in Production
description: Choose production agent tools by the job each component performs, and separate named deployments from mere vendor
  claims.
resource: https://medium.com/@unicodeveloper/15-open-source-projects-for-ai-agents-in-production-fd8a329d00da
tags:
- ai-ml
- ai-agent
- agent-harness
- model-context-protocol
- ai-gateway
- mlops-llmops
- model-evaluation
- observability
- idempotency
- least-privilege
- indirect-prompt-injection
- ai-security
- software-testing
- durable-execution
- sandboxed-execution
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-26T06:47:00-03:00'
sources:
- resource: https://medium.com/@unicodeveloper/15-open-source-projects-for-ai-agents-in-production-fd8a329d00da
  title: 15 Open Source Projects for AI Agents in Production
  author: unicodeveloper
status: stable
source_url: https://medium.com/@unicodeveloper/15-open-source-projects-for-ai-agents-in-production-fd8a329d00da
source: Medium (unicodeveloper)
source_type: medium
published: '2026-11-02'
domain: ai-ml
concepts:
- ai-agent
- agent-harness
- model-context-protocol
- ai-gateway
- mlops-llmops
- model-evaluation
- observability
- idempotency
- least-privilege
- indirect-prompt-injection
- ai-security
- software-testing
- durable-execution
- sandboxed-execution
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/424baf74172d817f967dd049a93b9fd9
---

## 🎯 Core thesis
Choosing tools for production AI agents should start from the job each component must do (execution, durable work, tool access, observability, evaluation, model routing), not from a search for the single "best" framework.

## 🧭 Why the author wrote it
The author starts from a market that moves fast: the article states that two out of five developers now run AI agents in production, and the tools change nearly as fast as the agents. If the right question is "what does this agent need to do its job reliably?", the answer differs by company, so a list of open source projects has to be grouped by responsibility and backed by evidence about where each project is actually used.

## Introduction (untitled in the article)
The opening section makes three moves. First, it says "which tool is best?" is the wrong starting point. Second, it gives two contrasting examples. Genspark's agents need to execute code, work with files, and keep state across multi-step tasks; the article says Genspark's published implementation account describes using E2B for those execution environments. Canva's support agents need tracing, prompt management, and evaluations, which its team runs through Langfuse. Third, it separates the jobs: the software that runs an agent's code does not explain why the agent made a mistake, and an observability platform does not provide an execution environment.

The article then sets its scope. The infrastructure entries rest on named deployments and direct dependencies. The applications (cybersecurity monitoring, geopolitical research, prediction-market analysis) show complete workflows; the article says they are not presented as dependencies used by a majority of agents.

> 🔎 Added context: A "dependency" here means a package the project declares as required, found in its manifest. A "deployment account" means a company publicly describing how it uses the tool.

## TL;DR
The article's summary gives one line per responsibility:

- **Build the agent:** LangGraph and Pydantic AI offer approaches to stateful control flow, typed tools, and application integration.
- **Keep work running:** Temporal handles durable background workflows, with a documented role in Sanity's Content Agent architecture.
- **Connect tools and execute tasks:** Pydantic, the MCP Python SDK, Playwright, Browser Use, and E2B provide data contracts, tool connectivity, browser control, and execution environments.
- **Operate and improve the system:** Langfuse, promptfoo, and LiteLLM cover observability, testing, and model access. The article stresses they serve different roles and are not interchangeable.
- **Add research and study complete applications:** the Valyu TypeScript SDK provides research APIs; Global Hack Map, Global Threat Map, and Polyseer show domain-specific interfaces and outputs.

The article says the list is grouped by responsibility, not ranked by popularity.

## Agent frameworks

### 1. LangGraph: explicit state and control flow
LangGraph is a framework for stateful agent orchestration. It models the agent's work as a graph of steps, so transitions are explicit, and it supports checkpointing, which saves execution state so work can resume, and interruptions that wait for human input. Checkpointing matters because an agent task that spans several tool calls and approvals is not a single model response; the state between steps is what must survive.

The article's published deployment examples include LinkedIn's recruiting assistant, Uber's code-testing workflows, and AppFolio's property-management copilot. The article describes these as applications with multiple stages and tools, where execution state matters beyond the latest model response.

The article says LangGraph is worth evaluating when an application must branch, pause for approval, or continue a partly completed job. Its advice is to define the state that must survive and make transitions visible in application code.

> ❌ **Anti-pattern — restoring state as if it were the whole recovery:** assuming that because graph state is checkpointed, a resumed run is safe. **Why people do it:** checkpointing is the visible feature, and it looks like complete recovery. **Consequence:** the article warns that restoring graph state does not, by itself, prevent a repeated payment or duplicate message. **Fix:** use persistent checkpoint storage and design external actions for retries, so that repeating a step does not repeat its effect.

### 2. Pydantic AI: typed Python agents
Pydantic AI provides typed tools, structured outputs, and dependency injection for Python agents. Dependency injection means the components an agent needs (clients, configuration, database handles) are passed in from outside rather than created inside the agent, which makes it easier to test. The article notes that Pydantic AI is a separate project from the Pydantic validation library covered later in the list.

The article's evidence is a published migration account from MindsDB, which describes adopting Pydantic AI to gain programmatic control over agent state and to validate intermediate outputs. It also cites public downstream code: marimo's dependency manifest includes Pydantic AI Slim for its AI features.

The practical fit, in the article's terms, is a Python application where one component's output becomes another component's input. Typed results let ordinary application code decide what happens next, instead of asking another model call to interpret a paragraph.

The article warns to keep business validation separate from structural validation. A correctly typed supplier ID can still identify the wrong supplier, and a valid action schema does not authorize the action.

## Durable execution and background work

### 3. Temporal: recover long-running workflows
Temporal separates durable workflow coordination from the activities that perform external work. It records the workflow's history, so execution can be recovered after interruptions such as a crashed worker. In this model, the workflow code decides the sequence of steps, and activities are the individual calls that touch the outside world; activities are where retries and failures are handled.

The article's Sanity deployment account describes using Temporal to compute a content schema graph and run parallel mutation tasks in the background. These jobs support the agent without requiring the whole operation to stay inside a live chat request.

The article says Temporal is worth considering when a task crosses service boundaries, waits for an event or approval, or must survive worker failures. Its example: a user submits a job, leaves the application, and returns later to check progress or the result.

The article's cautions are that teams need to understand deterministic workflow code, activity retry behavior, and workflow versioning. External side effects still need appropriate idempotency. It also notes that running the Temporal service yourself is a different commitment from using Temporal Cloud.

## Data contracts and tool connectivity

### 4. Pydantic: a shared dependency across model providers
Pydantic supplies typed data models, validation, serialization (converting objects to and from a transport format such as JSON), and JSON Schema generation. JSON Schema is a standard description of what a JSON document may contain, which is how many tool definitions tell a model what arguments it may send.

The article's adoption evidence is direct: Pydantic is a declared dependency in the official Python SDKs from OpenAI, Anthropic, and Google, as well as in OpenAI's Agents SDK.

For an agent application, the article argues that explicit data contracts are useful wherever a tool request, model response, or API result enters ordinary code. They make accepted fields and invalid values testable and give downstream components a predictable representation.

The article's caution is to decide what validation means at each boundary. Shape, business correctness, and authorization are different checks. It also notes that these SDK dependencies do not reveal how the providers' private inference services are implemented.

### 5. MCP Python SDK: a reusable tool interface
The official Model Context Protocol Python SDK provides software for building MCP clients and servers. The article is explicit that the project in this entry is the SDK, not the protocol in the abstract. MCP is a JSON-RPC based protocol that standardizes how AI applications connect to tools, data sources, and prompts.

The article cites downstream use: OpenAI's Agents SDK and Browser Use both declare the Python `mcp` package as a runtime dependency. The article treats these manifests as evidence of use in agent software, rather than as proof that the products are merely compatible.

The argument is that a standard tool interface lets one service expose its operations to different clients. The application can reuse the tool implementation instead of writing a separate adapter for every agent framework.

The article's caution: implement authentication, authorization, input validation, and tenant isolation yourself. MCP describes communication; it does not decide which customer records a caller may access. Pin compatible SDK versions and review security updates.

> 🔎 Added context: "Tenant isolation" means ensuring one customer's data cannot be reached through another customer's requests in a multi-customer system.

## Browser control and code execution

### 6. Playwright: programmed browser automation
Playwright provides browser automation primitives for navigation, page interaction, downloads, and inspection. A primitive is a basic operation the library exposes, from which larger scripts are built.

The article cites two concrete downstream dependencies: OpenClaw's manifest lists `playwright-core` as a runtime dependency, and Microsoft's Playwright MCP implementation depends on Playwright. The article adds that these do not mean every agent task opens a browser.

The argument: for a known portal workflow, programmed browser actions can be easier to test than asking a model to choose every click. Navigate the expected pages, download the document, and verify the result before passing it to the next component.

The article's cautions are to isolate browser sessions, protect authenticated state, and use approval gates for consequential actions. It also notes that not every browser agent uses Playwright; some use their own Chrome DevTools Protocol drivers.

### 7. Browser Use: task-driven browser interaction
Browser Use provides a browser-agent library and related browser tooling, with separate cloud services. The article's evidence is first-party downstream documentation from Hermes Agent, which identifies Browser Use's browser-harness as the driver behind its browser execution tool and lists it as a regular Python dependency. Hermes explains how model-written code operates a local or configured cloud browser.

The article presents this as useful when an agent must navigate an unfamiliar site, inspect a changing interface, or complete a task whose route cannot be fully scripted in advance. The driver executes the operations; the surrounding application determines what the agent is allowed to attempt.

The article's cautions: treat page content as untrusted input, restrict destinations, and bound execution. Separate read-only research from purchases or account changes. Hosted browser use and local library use are different deployment choices.

> ❌ **Anti-pattern — treating page content as instructions:** letting text on a visited page direct the agent's next actions. **Why people do it:** a browser agent reads pages in order to act, so the page's text can look like part of the task. **Consequence:** the agent may follow instructions an attacker placed in the page; this is the indirect prompt-injection risk the article's guidance addresses. **Fix:** treat page content as untrusted input, keep the agent's permitted actions defined by the application, and gate consequential ones.

### 8. E2B: execution environments for agents
E2B provides SDKs for creating and controlling isolated execution environments, including code-interpreter capabilities. An isolated environment is a separate place to run code, so generated code does not run with the web server's own access.

The article's deployment evidence is Genspark's published account of using E2B for agents that execute code, work with files, and complete multi-step tasks. It also names E2B customer accounts involving Gumloop and Lindy.

The article presents the concrete requirement this answers: generated code needs somewhere to run without inheriting the authority of the web server. Its example: a data-analysis agent receives a file, runs analysis in a separate environment, and returns artifacts for the user to inspect.

The article's cautions: decide what credentials, data, and network access enter each environment, and set resource and lifetime limits. It also notes that the customer evidence concerns E2B's service and does not establish that those customers self-host E2B's open-source components.

## Observability, testing, and model operations

### 9. Langfuse: trace and evaluate agent behavior
Langfuse provides tracing, prompt management, datasets, and evaluations for model-based applications. A trace records the steps of one request, such as retrieval, model calls, and tool operations, so a failure can be located.

The article's Canva account describes using Langfuse across a Java Help Assistant and a Python asynchronous support agent. The team traces execution, versions prompts, and runs online and offline evaluations (online meaning on live traffic, offline meaning on stored test sets). The account also explains Canva's move from self-hosting to Langfuse Cloud.

The article's operating practice: when an agent returns a bad result, trace the retrieval, model calls, and tool operations to find the failure. Preserve representative failures as test cases so the next release can be checked against them.

The article's cautions: plan redaction, access, and retention before logging customer conversations or tool responses. Instrument application logic as well as model calls, because a trace containing only the final generation may miss the problem.

> ❌ **Anti-pattern — tracing only the final model output:** logging the last generation and nothing else. **Why people do it:** the final answer is the visible artifact, and it is the easiest thing to capture. **Consequence:** the root cause, often an earlier retrieval or tool result, is absent from the record. **Fix:** instrument the surrounding application logic as well as the model calls.

### 10. promptfoo: evaluations and adversarial testing
promptfoo is a CLI and library for evaluating and red-teaming model-based applications, with support for automated development workflows. Red-teaming means deliberately trying to make the system fail or misbehave, such as by pushing it toward prohibited actions.

The article reports that promptfoo's published adoption statements identify OpenAI and Anthropic as users. It labels this a vendor-published claim, not a public dependency manifest or a detailed account of a specific internal deployment. It adds that testing software can support a production product without being present in its request path.

The practical workflow: turn failures into repeatable tests. Keep examples of valid requests, unsupported claims, and prohibited actions, and run them whenever a prompt, model, or tool implementation changes.

The article's cautions: test observable behavior, including tool actions where the integration exposes them. A convincing final answer is not enough. Passing an evaluation suite establishes performance on those tests, not protection against every attack. Review where configured evaluators send data.

> ❌ **Anti-pattern — equating a passing suite with safety:** treating a green evaluation run as proof the system is protected. **Why people do it:** a suite gives a clear, countable result. **Consequence:** the suite only covers the cases it contains, so untested attacks and failures pass unnoticed. **Fix:** treat the suite as evidence on named cases, keep growing it from real failures, and test actions as well as text.

### 11. LiteLLM: shared model access and routing
LiteLLM provides a unified model interface and a proxy server with routing, virtual keys, spend tracking, and load balancing. A virtual key is an application-level credential that the gateway maps to real provider keys.

The article's customer testimony includes NVIDIA, which describes a consistent interface to model endpoints across cloud providers, open-source deployments, and internal services. The article says this supports LiteLLM's role as shared model-access infrastructure, and makes no claim about every NVIDIA agent.

The argument: for a team operating several applications, a common gateway makes usage attributable and provider configuration easier to manage. It also gives operators one place to configure approved routing and to investigate failures, instead of searching several unrelated client implementations.

The article's cautions: evaluate fallback models on the tasks they will actually receive. Compatible API formats do not guarantee equivalent tool use or output quality. Check feature-specific licensing, and test budget enforcement rather than treating a dashboard as a hard spending limit.

## Research tooling and reference applications

### 12. Valyu TypeScript SDK: research inside an existing agent
The Valyu TypeScript SDK, published as `valyu-js`, exposes search, content extraction, and asynchronous DeepResearch through typed JavaScript and TypeScript interfaces. Asynchronous here means the research task is submitted, runs for some time, and is retrieved later by its ID.

The article identifies a concrete implementation to inspect: Global Threat Map's server-side client imports the SDK to retrieve events and run research. The article presents this as evidence of use in a Valyu-powered application, not as an independent claim about the SDK's market share.

The argument: for a research assistant preparing a company brief, the SDK lets application code request source material for its own reasoning, or create a longer research task and retrieve the completed output. Keep the task ID, status, and source links together with the job, so a user can inspect the evidence and reopen the result.

The article's cautions: keep API keys server-side, bound usage, and handle incomplete or failed tasks. The SDK's license does not make the hosted Valyu services free or self-hostable.

### 13. Global Hack Map: a security-monitoring interface
Global Hack Map presents vulnerabilities, exploits, breaches, and other security developments on a map-based interface. Its dashboard separates new critical vulnerabilities from reports of exploitation, and distinguishes confirmed, reported, and claimed incidents.

The article's framing is triage. A security team operating agent infrastructure should review developments that affect the products in its stack, follow the linked advisory or report, and decide whether the item warrants further investigation. The article says the interface shows how a monitoring product can organize research around an operational question rather than return an undifferentiated news summary.

The article's caution: verify affected versions and exploitation claims against the underlying sources. A reported event does not establish that your environment is vulnerable, and a geographic pin does not prove attribution.

### 14. Global Threat Map: geographic context for research
Global Threat Map is a situational-awareness application that combines a map, an event feed, country research, and entity dossiers. The article says its repository documents source-linked research and exportable report artifacts.

The article's example: a regional-risk analyst uses the event feed to identify a development, inspects its sources, and requests a dossier on an organization or country before preparing a briefing. The article presents the implementation as showing how search results, locations, and longer-running research can appear in one interface, with outputs a colleague can review outside the application.

The article frames the repository as an application reference, not as evidence that other production agents depend on it.

The article's cautions: review geocoding, source freshness, and API integrations against current service contracts. It notes that the documented self-hosted mode has no authentication and bills calls to the operator's key, so access controls and usage limits are needed before a shared deployment. Map rendering also requires a Mapbox token.

### 15. Polyseer: structured research for prediction markets
Polyseer accepts a Polymarket or Kalshi URL, retrieves the market's question, prices, close date, and resolution rules, and submits a research task to Valyu DeepResearch.

The article describes its server-side implementation as separating market-data retrieval from research creation and status tracking. The documented output includes a structured forecast, evidence for and against it, and downloadable report artifacts. A researcher can compare the analysis with the market's actual resolution terms before deciding which assumptions deserve further checking.

The article presents this as a useful application pattern: external facts define a bounded research question, an asynchronous task performs the investigation, and a structured result stays available under a stable task ID.

The article's caution: evaluate forecast quality on resolved markets, because a generated probability is not evidence of calibration. The documented self-hosted mode has no authentication and uses the operator's Valyu API key, so review access and spending controls before exposing it to others.

## Frequently asked questions

### Which open-source agent framework should I start with?
The article suggests evaluating Pydantic AI for typed Python application code, and LangGraph when explicit stateful control flow is central. It advises running a representative task and testing its failure paths before committing to a framework. It adds that a research SDK or reference application addresses a different layer and does not necessarily require adopting another agent framework.

### Does production adoption mean a project is safe to deploy as-is?
No, according to the article. Adoption establishes a reported use or dependency, not a security guarantee. A deployment still needs appropriate permissions, isolation, resource limits, and operational testing. Managed services and self-hosted components also carry different responsibilities.

### Are these projects free to operate?
Open-source licenses do not cover operating costs such as model inference, hosting, storage, or browser and sandbox services. The article notes the Valyu-powered applications also depend on hosted APIs. Check the license declarations and source availability for the exact project you plan to use; the article notes the Global Hack Map source-access limitation in its entry.

## 🧠 Mental models

### Choose by job, not by brand
**The model:** each component in an agent system has a distinct responsibility (running code, keeping work alive, validating data, operating the system), and the right tool follows from which responsibility is missing.
**How to apply it:** before evaluating a project, write down the job it must do in your system and the part of the application that needs support.

### Evidence has grades
**The model:** the article separates named deployment accounts, direct dependency manifests, downstream documentation, and vendor-published adoption statements. Each supports a different claim.
**How to apply it:** when a project is said to be "used in production," ask which kind of evidence is cited, and only claim what that evidence supports.

### Validate in layers
**The model:** structural validation (the value has the right shape), business validation (the value is correct for this situation), and authorization (this caller may do this) are separate checks, and passing one says nothing about the others.
**How to apply it:** place a check for each layer at each boundary where data enters ordinary code or triggers an action.

### Turn failures into tests
**The model:** a production failure, once traced, becomes a stored case that the next release must pass.
**How to apply it:** keep representative valid requests, unsupported claims, and prohibited actions as a suite, and run it whenever a prompt, model, or tool changes.

## 🔑 Key takeaways
- The right starting question is what a particular agent needs to do its job reliably; the article says the answer can differ widely between companies.
- Agent infrastructure splits into distinct jobs: stateful orchestration, durable background work, data contracts, tool connectivity, isolated execution, observability and evaluation, and shared model access.
- Durable workflows are for work that crosses service boundaries, waits for events or approvals, or must survive worker failures, and external side effects still need idempotency.
- Pydantic, the MCP Python SDK, Playwright, and Browser Use are each cited with concrete downstream dependencies, and the article notes that none of these show every agent uses them.
- Observability and evaluation are different roles: traces locate a failure, and evaluations check that a fix holds on preserved cases; a passing suite is not a security guarantee.
- Hosted APIs, self-hosted services, and open-source licenses carry separate responsibilities, costs, and access-control needs.

## 🏛️ For Kenji
For an architect, the useful move is the article's separation of responsibilities: the same review that asks whether an agent needs durable workflows, isolated execution, or a model gateway also asks where each boundary's authorization and idempotency must live. The question it raises for your work is which layer of an agent system you would expect to fail first in a production incident, and whether your traces would show it. *This is your interpretation, not the author's.*

## 📚 Reading plan
### From the article
- The extracted text of the article cites no hyperlinks, so no links are listed here. Read the article for the named deployment accounts (LinkedIn, Uber, AppFolio, Sanity, MindsDB, Canva, Genspark, Hermes Agent, NVIDIA) as the primary sources for each claim.

### To go further
- LangGraph documentation: to see how checkpointing and interrupts are configured for a stateful agent.
- Temporal workflow and activity model: to understand determinism, retries, and workflow versioning before running durable jobs.
- Model Context Protocol specification and Python SDK: to implement authentication and authorization around tools you expose.
- OWASP guidance on prompt injection: to design defenses for browser and document-reading agents.
- Observability practice for LLM applications (traces, prompt versions, online and offline evaluation): to plan redaction, retention, and instrumentation of application logic.

## Concepts
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [agent-harness](/concepts/ai-ml/agent-harness.md)
- [model-context-protocol](/concepts/ai-ml/model-context-protocol.md)
- [ai-gateway](/concepts/ai-ml/ai-gateway.md)
- [mlops-llmops](/concepts/ai-ml/mlops-llmops.md)
- [model-evaluation](/concepts/ai-ml/model-evaluation.md)
- [observability](/concepts/cloud-infra/observability.md)
- [idempotency](/concepts/backend-systems/idempotency.md)
- [least-privilege](/concepts/security/least-privilege.md)
- [indirect-prompt-injection](/concepts/security/indirect-prompt-injection.md)
- [ai-security](/concepts/security/ai-security.md)
- [software-testing](/concepts/software-engineering/software-testing.md)
- [durable-execution](/concepts/backend-systems/durable-execution.md)
- [sandboxed-execution](/concepts/security/sandboxed-execution.md)
