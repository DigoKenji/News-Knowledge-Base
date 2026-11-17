---
type: Article Summary
title: Governed Autonomous Intelligence for Mission-Critical Utility
description: Governed agentic operations keep AI reasoning separate from execution, so a policy gate and one tool gateway control every production change.
resource: https://www.infoq.com/articles/utility-governed-autonomous-intelligence-ai/
tags:
- ai-ml
- ml-governance
- rag
- multi-agent-orchestration
- concept-drift
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://www.infoq.com/articles/utility-governed-autonomous-intelligence-ai/
  title: Governed Autonomous Intelligence for Mission-Critical Utility
  author: Purnang Borah
status: stable
source_url: https://www.infoq.com/articles/utility-governed-autonomous-intelligence-ai/
source: InfoQ
source_type: blog
published: '2026-11-16'
domain: ai-ml
subdomain: ml-governance
concepts:
- rag
- multi-agent-orchestration
- concept-drift
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81ed941fc5270812f39f
---

# Governed Autonomous Intelligence for Mission-Critical Utility

**Source:** https://www.infoq.com/articles/utility-governed-autonomous-intelligence-ai/
**Author:** Purnang Borah
**Date:** 2026-11-16
**Concepts:** [Retrieval-augmented generation](/concepts/rag.md) · [Multi-agent orchestration](/concepts/multi-agent-orchestration.md) · [Concept drift](/concepts/concept-drift.md)

---

## 🎯 Core thesis
In a regulated utility, an agent may reason about operations and propose production changes, but only a deterministic policy gate and one governed tool gateway can authorize execution, and autonomy grows one action class at a time.

## 🧭 Why the author wrote it
Utility IT and platform teams already have more monitoring data than they can use, so the hard problem is connecting signals fast enough to act on them. Agentic AI can do that connecting, but a wrong production action can affect customer-facing services, safety-related workflows, or regulatory reporting. If the thesis holds, a model error becomes a rejected or reviewed proposal instead of an executed change.

## Key Takeaways
*(The author's own summary, reproduced close to the original.)*
- In a regulated utility environment, an agentic operations platform keeps reasoning separate from execution: agents correlate signals, assess business impact, and propose remediation, but every production action crosses a deterministic policy boundary that the model cannot override.
- That boundary is defined outside the model. Each proposed action maps to a risk class and approval requirement in a predefined, versioned action catalog, which keeps approval decisions predictable, testable, and reviewable before deployment.
- A governed tool gateway enforces the boundary as the only path from an agent proposal to a production change, with least privilege, independent authorization, approval verification, and complete action evidence attached to that single door.
- Because every action passes through one governed path, the platform produces the evidence needed to support the regulatory and governance requirements of each system, without claiming that the platform itself creates compliance.
- Autonomy is earned rather than assumed: the platform starts read-only and expands one action class at a time, guided by recommendation quality, operator acceptance, execution outcomes, and rollback safety, and that autonomy can be revoked as easily as it is granted.

## Introduction
Utility operations teams, meaning the IT and platform teams that support enterprise applications, integrations, and infrastructure, already have more monitoring data than they can comfortably consume. Application logs, infrastructure metrics, API telemetry, security events, job failures, service-management tickets, and business alerts each describe a piece of the same operating environment. The hard problem is not collecting another signal. It is connecting those signals fast enough to understand what is happening, what it may affect, and what should happen next.

Agentic AI is attractive because it can reason across fragmented evidence, retrieve operational knowledge, call tools, and coordinate multi-step work. A utility cannot treat such an agent like a conversational assistant. A wrong summary is inconvenient. A wrong production action can affect a customer-facing service, a safety-related workflow, regulatory reporting, or a system that participates in reliable energy delivery.

### Agentic AI in utility operations
In this article, an agent is a model that can retrieve context, call tools, and carry out multi-step work. The central concern is the step from answering to acting, because that step is where errors gain consequences.
> 🔎 Added context: "Agentic" here means the model can call tools, not just produce text. The rest of the article is about controlling that tool-calling power.

## Related Sponsors
Despite its label, this section holds the article's architecture statement rather than a product pitch. The author describes the pattern used for an agentic operations platform in a regulated utility environment. The reference implementation uses Amazon Bedrock AgentCore, Amazon Bedrock, retrieval over enterprise runbooks and incident history, and Model Context Protocol (MCP) tool interfaces.

The section states the principle in three sentences: "The model can propose anything. It can execute only what the policy gate authorizes, through the governed tool path, with human approval wherever policy requires it. Authority never lives inside the model." It also says prediction and autonomous remediation are intentionally enabled incrementally, and presents the ability to expand autonomy gradually and revoke it quickly as part of the architecture.

### Amazon Bedrock AgentCore
AgentCore is the AWS managed platform the reference implementation is built on. The author uses it as a concrete example of the pattern rather than as a requirement.

### Model Context Protocol (MCP)
MCP standardizes how tools are described and invoked at the tool boundary. One standard interface gives the platform a single place to constrain operations, validate arguments, enforce policy, and record evidence.
> 🔎 Added context: MCP is an open protocol for connecting models to external tools and data sources. The article uses it as the tool-description layer, not as a security control by itself.

## Why Utility Operations Need a Different Agentic Pattern
A large utility estate spans custom applications, SaaS products, cloud platforms, integration layers, databases, observability tools, field systems, and third-party services. Each domain can be monitored well in isolation, yet the end-to-end service stays hard to reason about. The author lists recurring gaps:

- Related signals appear in separate tools and are correlated manually.
- Technical severity is visible, but the business consequence is not.
- Runbooks and lessons learned are spread across wikis, ticket histories, shared drives, and the memory of experienced engineers.
- Thresholds report a known condition after it has occurred, while weaker precursors across dependent systems are easy to miss.
- The evidence linking a change to its rationale, authorization, approver, and outcome may be assembled after the fact.

Regulatory requirements also vary by sector, environment, and jurisdiction. The architecture therefore has to correlate across domains, reason with trusted operational knowledge, map technical events to business impact, support prediction without pretending it is certain, and keep an execution model that security, compliance, operators, and auditors can understand. Figure 1 in the original shows fragmented observability leaving teams to correlate signals by hand.

### NERC CIP and BES Cyber Systems
NERC is the North American Electric Reliability Corporation. Its Critical Infrastructure Protection (CIP) requirements apply to the Bulk Electric System (BES) and to defined in-scope BES Cyber Systems. Scope is set per system, not per company: other systems in the same utility may be governed by different rules.
> 🔎 Added context: The article links CIP-002-5.1a, where in-scope systems are defined. Read it before deciding which systems a platform like this may touch.

## The Control-Plane Pattern: Separate Reasoning from Execution
The organizing idea is an AI control plane, a term borrowed from networking and cloud infrastructure, where one plane decides what should happen and a separate plane carries it out. The separation serves safety, auditability, and blast-radius containment. Figure 2 in the original shows reasoning authority separated from execution authority.

In the reasoning plane, the orchestrator and domain agents analyze events, retrieve context, compare incidents with history, estimate business impact, predict failure modes, and propose remediation. None of them can change a production system directly. A proposed action crosses into the execution plane only through a deterministic policy gate and a governed tool gateway. The gate decides whether the action is permitted and what approval it needs. The gateway enforces final authorization, obtains narrowly scoped credentials, invokes the target, records the result, and performs or requests post-action validation.

The payoff: a hallucinated root cause in a conventional autonomous design can become a bad action, but here it becomes a bad proposal. Policy can reject it, a human can review it, or the gateway can deny authorization.

### Policy gate
The policy gate maps each proposed action to a risk class and an approval requirement. It is deterministic and sits outside the model, so the same proposal always gets the same classification.

### Governed tool gateway
The gateway is the single path from proposal to production change. It enforces authorization, approvals, scoped credentials, schema validation, and evidence capture. The author's principle: "one door to production, with every control attached to that door."

> ❌ **Anti-pattern — Optional gateway:** Agents reach target systems through direct API keys, side channels, or tools that skip the gateway. **Why people do it:** direct access is quicker for prototypes. **Consequence:** the controls on the gateway can be bypassed, so there is no single boundary to audit. **Fix:** make the gateway the only route, with no target credentials in the reasoning runtime and network paths that force traffic through it.

## Reference Implementation on AWS
The reference implementation runs on Amazon Bedrock AgentCore, which supplies managed agent runtime, memory, gateway, identity, and observability integrations. Amazon Bedrock provides model access, and Bedrock Knowledge Bases provide retrieval over enterprise knowledge.

The boundary is covered by AgentCore Policy, generally available since March 2026. Rules are written in natural language or policy-as-code, compiled to Cedar, and attached to an AgentCore Gateway, which evaluates every agent-tool request before allowing or denying it. The author advises teams starting today to evaluate this before building an equivalent. AgentCore Gateway is the AWS implementation of the tool gateway. The "one place" property matters most: the design has no target-system credentials in the reasoning runtime, deny-by-default identity policies, restricted network paths, and credentials exposed only to the tool implementation that needs them. Figure 3 maps the pattern onto AWS services.

### AgentCore Policy and Cedar
Cedar is the open-source authorization policy language AWS uses for compiled rules. Enforcement happens at the gateway, not in agent logic, so a reasoning error cannot talk its way past the rule.

### AgentCore Gateway as the tool gateway
AgentCore Gateway is where each agent-tool request is allowed or denied. It works with MCP tool interfaces, which give it a schema-defined boundary to check.

### AgentCore Evaluations
AgentCore Evaluations supports continuous and on-demand evaluation of agent behavior, used in the lifecycle controls described later.

### Equivalent Services on Other Platforms
The pattern needs a managed agent runtime, a retrieval layer, and a policy-enforcing tool gateway that agents cannot bypass. On Microsoft Azure, Microsoft Foundry Agent Service provides the runtime, Microsoft Entra supplies agent identities, Azure AI Search handles retrieval, and Azure API Management serves as the tool gateway and AI gateway for model and MCP traffic. Some of these governance features are still in preview. On Google Cloud, the Gemini Enterprise Agent Platform provides Agent Runtime and Agent Identity, with Agent Gateway as the tool gateway, enforcing IAM access policies and Model Armor screening for prompt injection.

### End-to-End Responsibilities
The boundary that matters sits between the agentic layer and the tool gateway. Everything above it reasons, and nothing above it can act. The author's table of the governed pipeline:

| Layer | Responsibility |
|---|---|
| Source | Alerts, logs, events, and telemetry from applications, infrastructure, cloud, monitoring, and security systems. |
| Integration | Ingests and normalizes signals through APIs, webhooks, and streaming into a common schema. |
| Queue | Buffers events for asynchronous processing and absorbs spikes without loss. |
| Orchestrator | Receives events, determines analysis needs, routes to domain agents, and assembles findings. |
| Agentic layer | Bounded agents for correlation, root-cause analysis, business impact, prediction, and remediation proposals. |
| Knowledge base | Runbooks, SOPs, architecture documents, incident history, and dependency maps for trusted context. |
| Model access | Reasoning, interpretation, and recommendations grounded in enterprise knowledge. |
| Policy gate | Maps proposed actions to risk classes and approval requirements outside the model. |
| Tool gateway (MCP; AgentCore Gateway on AWS) | Single governed production path enforcing authorization, approvals, scoped credentials, schema validation, and evidence capture. |
| Target systems | Applications, cloud services, databases, networks, ITSM, and observability platforms receiving remediation. |
| Human approval | Operator or owner review with recorded decision, identity, and timestamp. |
| Operations view | Health, predictions, business impact, approvals, remediation status, and validated outcomes. |

*Table 1 in the original: end-to-end responsibilities across the governed pipeline.*

## Multi-Agent Reasoning Grounded in Operational Knowledge
The reasoning plane uses a hierarchical multi-agent structure rather than one large agent with broad access. A narrow agent with a bounded responsibility is easier to test, constrain, evaluate, and explain. The Orchestrator Agent is the entry point for normalized events. It determines the analysis required, routes work to domain agents, and assembles findings, but it does not execute. The Application Reliability Agent analyzes application failures against known patterns and history. The Platform Intelligence Agent examines API and interface failures, latency, partial degradation, and dependencies. The Business Impact Agent translates technical state into consequences for customer service, billing, safety-related programs, reliability commitments, or reporting deadlines. The Remediation Agent is the only reasoning component allowed to propose a production-touching action, and that proposal has no authority until it passes policy and approval.

### Hierarchical multi-agent orchestration
The orchestrator coordinates and the domain agents specialize. Each agent is kept narrow so it can be tested against its own failure modes, and only one may propose production changes.

> ❌ **Anti-pattern — Monolithic super-agent:** One agent with access to every domain and every tool. **Why people do it:** it is simpler to wire up and demos well. **Consequence:** it is hard to test, constrain, or explain, and every tool becomes reachable from every reasoning path. **Fix:** split responsibilities into bounded agents routed by an orchestrator.

### Retrieval-grounded knowledge (RAG)
All agents share operational context retrieved from runbooks, SOPs, architecture documents, incident history, business rules, dependency maps, and recovery procedures. The author argues that a recommendation grounded in this environment's documented procedure is a very different artifact from a plausible answer based on general training. Retrieval is only as good as its content.

### Knowledge hygiene
Outdated, contradictory, or incomplete runbooks produce outdated, contradictory, or incomplete instructions, faithfully retrieved. Every runbook therefore carries a named owner and a last-review date, and retrieval returns both with each passage. Overdue runbooks are flagged to the owner, and operator rejections citing a faulty runbook go to that owner.

> ❌ **Anti-pattern — Unowned runbooks:** Runbooks with no named owner, review date, or version history. **Why people do it:** documentation is treated as a side project. **Consequence:** the agent retrieves stale steps with full confidence, and no one is responsible for fixing them. **Fix:** assign owners, record review dates, flag overdue items, and route rejections back to owners.

### Normalized event contract
Incoming signals are normalized into a small schema: affected service or component, environment, event type, timestamp, source, technical severity, dependency identifiers, correlation keys, business-service mapping, and a reference to the original evidence. The raw payload is preserved. Deterministic parsing and source adapters, not the LLM, establish basic facts such as which system emitted an event. The business-service mapping comes from the service catalog. The simplified example from the walkthrough scenario:

```json
{
  "event_id": "evt-000193",
  "timestamp": "2026-04-12T14:03:27Z",
  "source": "apm",
  "source_event_ref": "apm://alerts/88213",
  "service": "customer-payments-api",
  "environment": "prod",
  "event_type": "latency_degradation",
  "technical_severity": "warning",
  "dependency_ids": ["billing-integration", "payments-queue"],
  "correlation_keys": ["chain:bill-payment-flow"],
  "business_service": {
    "name": "Customer bill payment",
    "criticality": "high",
    "mapping_source": "service-catalog"
  }
}
```

**What this does:**
- `source_event_ref` points back to the original APM alert, so the evidence stays traceable.
- `service`, `environment`, and `event_type` are the facts deterministic parsing should establish.
- `dependency_ids` and `correlation_keys` link this event to related signals in the same dependency chain.
- `business_service` carries `mapping_source: "service-catalog"`, so an auditor can see the mapping came from a controlled source.

> ❌ **Anti-pattern — Model-established facts:** Letting the LLM decide which system emitted a log line, whether two identifiers name the same service, or which business service a component supports. **Why people do it:** free-text logs are messy, and the model handles them well. **Consequence:** a probabilistic guess drives correlation and risk with no reliable record of origin. **Fix:** parse deterministically, look up mappings in the catalog, and keep the raw payload.

### Walking Through a Governed Event
An API's latency is rising, a downstream integration retries more often, and a processing queue grows. Each signal alone is below a page-worthy threshold, but together they suggest a customer-facing workflow could degrade. The orchestrator sees the shared dependency chain and asks the Platform Intelligence Agent to analyze the interface. The Application Reliability Agent checks prior incidents for a similar pattern, and the Business Impact Agent maps the dependency to the business service. Retrieval pulls the current runbook and the most relevant incidents. The output is a structured hypothesis: the likely failure mode, evidence, dependent services, and documented options. Weak or contradictory evidence is carried forward, not hidden.

The runbook permits diagnostics and, if a condition is confirmed, a stateless restart. The catalog classifies diagnostics as low risk, so they run automatically, and maps the restart to human approval. The Remediation Agent can propose the restart but cannot downgrade its risk because the model is confident. When an operator approves, the gateway verifies the approval reference and policy, obtains the scoped identity, invokes only the permitted operation, and captures the response. The workflow then checks latency, retry behavior, queue trend, and service health. If they do not recover, the remediation is not marked successful just because the restart command completed.

## Prediction Without Granting Prediction Authority
Prediction moves the platform beyond faster summarization. It looks for combinations of weak signals that historically precede failure, such as rising latency, queue growth, retries, resource saturation, repeated warnings, or anomalies that matter only when correlated across a dependency chain. It can use historical incidents, time-series trends, dependency data, and semantic analysis of logs to state what may fail, on what horizon, through which mechanism, with what confidence, and with what business consequence. False positives are expected and should be measured and reduced over time.

### Predictive detection
Predictions enter the same governed path as observed incidents, carrying evidence, confidence, and business-weighted risk so operators can decide how to respond.

> ❌ **Anti-pattern — Overselling prediction:** Presenting forecasts as near-certain outcomes. **Why people do it:** confident forecasts impress stakeholders. **Consequence:** operators learn the alerts are unreliable and stop acting on them. **Fix:** report evidence, confidence, lead time, and false-positive rates. The author calls this one of the fastest ways to lose operator trust.

### Confidence is not authorization
A confidence score is not an authorization token. Prediction carries no permission to act, the same separation as the two planes, applied to a single number.

> ❌ **Anti-pattern — Confidence as permission:** Letting a high-confidence prediction trigger execution. **Why people do it:** if the model is usually right, skipping review seems efficient. **Consequence:** the rare confident error reaches production unchecked. **Fix:** keep predictions on the governed path, with execution gated by policy and approvals alone.

## Governed Remediation: Deterministic Risk and Human Approval
Human control should not be a sentence in a governance document. It should sit directly on the execution path. The platform maps every candidate action to a predefined risk class outside the model, driven by an approved action catalog that weighs operation, target environment, blast radius, reversibility, data impact, and dependency sensitivity. The policy is fixed, versioned, testable, and reviewable.

| Risk class | Examples | Governance |
|---|---|---|
| Low | Read-only diagnostics, health checks, additional telemetry | May auto-execute; fully logged |
| Medium | Reversible changes, such as approved stateless service restarts or scaling within limits | Approval per policy |
| High | Production configuration changes, failover, sensitive data operations, or broad-impact changes | Explicit approval; dual control where policy requires it |

The author calls this intentionally conservative. Even some low-risk actions may remain approval-only early in deployment. Figure 5 in the original shows policy mapping each action to a risk class before the gateway can execute it.

### Deterministic risk classification
Risk-driving facts come from controlled configuration, inventory, or verified context. The agent may supply parameters such as a service name or requested capacity, but policy decides whether they fall inside the authorized envelope. Each permitted operation has a policy record that can be tested without an LLM: tool identifier, target environment, allowed scope, risk class, approval type, execution timeout, preconditions, expected postconditions, and a rollback link where rollback is meaningful.

> ❌ **Anti-pattern — Self-labeled risk:** Asking the model to label its own proposal low, medium, or high. **Why people do it:** the model has the context, so asking seems efficient. **Consequence:** the authority decision moves to the model, which is what the design constrains. **Fix:** derive risk from the catalog and verified facts, and treat any model label as advisory only.

### Human approval on the execution path
The gateway rechecks required approvals at execution time instead of trusting an upstream component's claim. Each approval is recorded with identity and timestamp.

> ❌ **Anti-pattern — Trusting upstream approvals:** Accepting an approval flag from an earlier component without rechecking it. **Why people do it:** the upstream component is internal and trusted. **Consequence:** a stale or misapplied approval can authorize an action it never covered. **Fix:** verify the approval reference and policy at the gateway, at execution.

### Post-action validation
"API returned 200" is not the same as "the system is healthy." The workflow validates the outcome it was meant to produce and escalates if the expected state is not restored.

> ❌ **Anti-pattern — Success by exit status:** Marking remediation successful because the command completed. **Why people do it:** exit codes are immediate and easy to read. **Consequence:** latency or queue depth stays degraded while the incident is closed. **Fix:** check the signals that motivated the action, and escalate if they do not recover.

### Defense in depth and prompt injection
The gateway scopes least-privilege credentials to specific tools, authorizes each agent-and-action pair independently, validates arguments against schemas, and records identity, policy decision, approval reference, inputs, result, and health status for every call. Typed tools constrain what a manipulated agent can request, but they do not remove prompt injection, because an agent can still misuse an operation within its scope. The control is layered: untrusted-data handling, narrow tools, argument validation, least privilege, policy, approvals, network constraints, and post-action validation.

> ❌ **Anti-pattern — Typed tools as the injection fix:** Treating a schema-defined interface as sufficient protection against prompt injection. **Why people do it:** typed interfaces feel like a complete boundary. **Consequence:** injected instructions can drive a legitimate operation with harmful arguments or at the wrong time. **Fix:** stack the layers so no single control carries the load.

### Content inspection
Since June 2026, AgentCore Policy can evaluate Bedrock Guardrails on gateway inputs and outputs for prompt injection and sensitive-data exposure, initially in a subset of AWS Regions. The author treats this as one control on the door, not a solution to injection.

## Security, Auditability, and Compliance
For regulated operations, governance is why the architecture has this shape.

### Change discipline for agents
Model and agent changes follow production software discipline: version-controlled prompts and definitions, reviewed changes, evaluation before promotion, and rollback. Historical incidents and adversarial cases serve as regression tests. Production traces are sampled and scored for task completion, tool selection, groundedness, safety, and policy adherence. Confidence and recommendation quality are monitored for drift after deployment.

### Data minimization and OWASP guidance
Logs and tickets can contain customer, employee, or security-sensitive data irrelevant to the task. Teams should establish data classification, domain isolation, encryption, and retention rules before broad ingestion. The author points to the OWASP Top 10 for LLM Applications, which treats prompt injection and excessive agency as distinct risks, and whose agentic guidance adds goal hijacking, tool misuse, identity and privilege abuse, and multi-agent interactions.

### Audit evidence
Audit evidence should answer more than "what did the system do?" For each consequential action, the organization should reconstruct the originating signal, retrieved evidence, recommendation, applicable policy, human decision, executed operation, identity used, and validated result. Immutability depends on the storage control; at minimum the record should be durable, access-controlled, and tamper-evident.

### Mapping evidence to CIP requirements
For in-scope BES Cyber Systems, CIP-010-4 addresses configuration change management and vulnerability assessments, CIP-004-7 covers personnel and access management, CIP-011-3 addresses protection of BES Cyber System Information, and CIP-008-6 covers incident reporting and response planning. The platform can support the evidence these requirements need, but it does not create compliance. Customer-facing programs may face other jurisdictional obligations, and corporate applications outside CIP scope may fall under internal change control, privacy, or financial controls.

> ❌ **Anti-pattern — Blanket compliance label:** Describing the whole utility estate as "CIP compliant" because one platform is in use. **Why people do it:** a single label is easy to communicate. **Consequence:** systems outside scope inherit controls that do not fit their regime, and in-scope systems can be overlooked. **Fix:** map evidence to the regime governing each specific system.

### NIST AI RMF and ISO/IEC 42001
The NIST AI Risk Management Framework (Govern, Map, Measure, Manage) and ISO/IEC 42001:2023 can organize the broader AI governance program. They complement, rather than replace, industry-specific requirements.

## Measure Value and Earn Autonomy
Measure agentic AI value against operational baselines, not unsupported improvement percentages. The author tracks detection time, response time to validated resolution, prediction precision and lead time, engineering effort on manual work, and governance measures such as approval latency, policy denials, blocked unauthorized actions, evidence completeness, and audit reconstruction time.

### Phased autonomy
The author expanded autonomy one action class at a time, and no class advanced on model confidence alone. Each step needed agreement from the system owner and change-management reviewers.

| Phase | Action class | Execution mode | Evidence required to move on |
|---|---|---|---|
| 1 | All actions | Read-only: observe, correlate, retrieve, predict, and recommend | Operators consistently accepted recommendations, and rejection reasons were reviewed and addressed |
| 2 | Read-only diagnostics and health checks | Approval-only → automatic | No diagnostic ran outside its authorized scope; evidence records were complete |
| 3 | Reversible changes, such as stateless service restarts | Introduced as approval-only | Post-action validation confirmed recovery; rollback procedures were tested |
| 4 | Selected low-impact reversible changes, such as restarting an approved stateless service or scaling within predefined limits | Approval-only → automatic, within defined scope and time windows | Approved consistently without modification; post-action validation confirmed recovery; rollback succeeded when tested; no policy denials or out-of-scope requests |
| 5 | High-risk changes, such as configuration changes and failover | Always explicit approval | Not eligible for automatic execution |

*Table 3 in the original: phased autonomy progression and evidence requirements.*

> ❌ **Anti-pattern — Autonomy on model confidence alone:** Granting automatic execution because the model performed well in tests or conversation. **Why people do it:** strong early results create pressure to move faster. **Consequence:** autonomy outruns the evidence operators, owners, and auditors need. **Fix:** require the phase evidence, and keep high-risk changes under explicit approval.

### Unsupported improvement percentages
The article gives no performance numbers, and the author's point is that teams should set baselines before claiming gains.

> ❌ **Anti-pattern — Unsupported improvement percentages:** Quoting gains without a measured baseline. **Why people do it:** headline numbers persuade in planning and reporting. **Consequence:** the claim cannot be checked, and trust falls when it does not hold. **Fix:** measure against a baseline captured before the change.

## Conclusion: Lessons for Teams Adopting This Pattern
Agentic AI can correlate signals, assess impact, and recommend remediation, but in mission-critical environments its value depends on how well its authority is governed. The author's four recommendations:

- Establish governance first: Build the governed execution path, enforce least privilege, and define the action catalog before introducing agents.
- Enforce deterministic policies: Manage risk classification and approval requirements through versioned configuration, not model prompts.
- Validate outcomes: Confirm service recovery rather than relying on successful command execution.
- Earn autonomy gradually: Enable one action class at a time, with the ability to revoke authorization when needed.

The goal, in the author's words, "is not maximum autonomy, but evidence-based autonomy that can be safely governed and revoked."

## About the Author
Purnang Borah wrote the article, and Rafał Gancarz is listed as reviewer. The extracted page has no biography text beyond the name.

## 🧩 Mental models
### Control plane versus execution plane
**The model:** Deciding and doing are separate systems, as in networking, where the control plane decides and another plane carries out the decision.
**How to apply it:** Ask of any agent design which component decides and which component acts, and whether the acting side has its own deterministic controls.

### One door to production
**The model:** Every production change passes through a single gateway, and every control is attached to that door.
**How to apply it:** Audit for other routes to target systems, including credentials, network paths, and side tools, before trusting any control on the gateway.

### A bad proposal, not a bad action
**The model:** A model error should cost a rejected or reviewed proposal, never an executed change.
**How to apply it:** Design so that each reasoning output passes an independent check before any effect in production.

### Earned, revocable autonomy
**The model:** Autonomy is granted one action class at a time, backed by evidence, and can be withdrawn as easily as it was granted.
**How to apply it:** Write the evidence needed for each phase down before the phase starts, and define the revocation path too.

## 🔑 Key takeaways
- Authority comes from a deterministic policy gate and a single governed tool gateway, not from the model. The model can propose, but it cannot execute on its own judgment.
- The gateway is one door to production, and agents must have no other route to target systems.
- Risk classes and approvals live in a versioned catalog that can be tested without an LLM, and the model never labels its own proposals.
- Success means the signals that motivated the action recovered, not that a command returned successfully.
- Prediction is a hypothesis with evidence, confidence, and lead time. Confidence never works as permission.
- Compliance evidence is mapped system by system. The platform supports evidence but does not make a utility compliant.
- Autonomy expands one action class at a time with phase-specific evidence, and it can be revoked as easily as it was granted.

## 🏛️ For Kenji
*This is your interpretation, not the author's.* The pattern fits a Staff Architect's AI governance work: a policy-as-code gateway, a versioned action catalog, and phased autonomy are controls an AI governance program must specify and evidence. The open question is how you would show that a catalog's risk classes came from verified facts rather than from agent labels, and who owns that evidence when an auditor asks.

## 📚 Reading plan
### From the article
- [Amazon Bedrock AgentCore](https://aws.amazon.com/bedrock/agentcore/) — the reference platform; map each component to your own stack.
- [Model Context Protocol (MCP)](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation) — the tool interface standard the boundary is built on.
- [AgentCore Policy](https://aws.amazon.com/about-aws/whats-new/2026/03/policy-amazon-bedrock-agentcore-generally-available/) — the gateway-enforced policy layer the author advises evaluating first.
- [Cedar policy core concepts](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/policy-core-concepts.html) — how the compiled rules are structured.
- [AgentCore Evaluations](https://aws.amazon.com/about-aws/whats-new/2026/03/agentcore-evaluations-generally-available/) — continuous and on-demand evaluation of agent behavior.
- [Bedrock Guardrails](https://aws.amazon.com/bedrock/guardrails/) — the content screening the author treats as one control among several.
- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/) — the risks behind the injection and excessive-agency controls.
- [NIST AI RMF core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/) — the Govern, Map, Measure, Manage structure.
- [ISO/IEC 42001:2023](https://www.iso.org/standard/42001) — the AI management system standard the author pairs with NIST.
- [CIP-002-5.1a](https://www.nerc.com/standards/reliability-standards/cip/cip-002-5.1a) — where BES Cyber Systems are defined; read before scoping any system.
- [CIP-010-4](https://www.nerc.com/standards/reliability-standards/cip/cip-010-4) — configuration change management and vulnerability assessment.
- [CIP-004-7](https://www.nerc.com/globalassets/standards/reliability-standards/cip/cip-004-7.pdf) — personnel and access management.
- [CIP-011-3](https://prod.nerc.com/standards/reliability-standards/cip/cip-011-3) — protection of BES Cyber System Information.
- [CIP-008-6](https://www.nerc.com/globalassets/standards/reliability-standards/cip/cip-008-6.pdf) — incident reporting and response planning.

### To go further
- OWASP agentic security guidance — goal hijacking, tool misuse, identity and privilege abuse, multi-agent interactions.
- Policy-as-code authorization — writing, versioning, and testing deny-by-default rules for agent actions.
- Measuring predictive alerting — detection time, lead time, and false-positive rate against a baseline.
- Runbook content governance — ownership, review intervals, and versioning for documents agents retrieve.
