---
type: Article Summary
title: How AUMOVIO centralized AI access with Databricks Unity Gateway
description: AUMOVIO consolidated scattered LLM access into one governed Databricks gateway, buying the platform and building only its differentiating layers.
resource: https://www.databricks.com/blog/how-aumovio-centralized-ai-access-databricks-unity-gateway
tags:
- architecture
- technology-architecture
- model-context-protocol
- oidc
- oauth2
- ai-gateway
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://www.databricks.com/blog/how-aumovio-centralized-ai-access-databricks-unity-gateway
  title: How AUMOVIO centralized AI access with Databricks Unity Gateway
  author: Databricks
status: stable
source_url: https://www.databricks.com/blog/how-aumovio-centralized-ai-access-databricks-unity-gateway
source: Databricks
source_type: blog
published: '2026-11-18'
domain: architecture
subdomain: technology-architecture
concepts:
- model-context-protocol
- oidc
- oauth2
- ai-gateway
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d8141978bf9ba64382c08
---

## 🎯 Core thesis
Consolidating scattered LLM access behind one centrally governed gateway pays off when you buy the gateway and build only the layers that encode your own operating model.

## 🧭 Why the author wrote it
AUMOVIO's teams had built several different ways to reach LLMs, and that variety made operations, cost attribution and consistent governance harder. If the thesis holds, one shared gateway lets the company manage providers, spend and approvals in one place instead of rebuilding that work in every team.

## Introduction
The article opens without a heading. As AI adoption grew, AUMOVIO teams developed different ways to access large language models, and Databricks Unity Gateway now provides a shared entry point for the company's AI workloads. The post explains how those approaches were brought together.

### Databricks Unity Gateway
This is the Databricks gateway product at the centre of the article. The later sections describe what it does for AUMOVIO: it routes model calls to approved providers, applies approvals, access policies, usage tracking, logging and guardrails, and gives the company one place to manage them.

## The challenge: Managing AI access across multiple providers
Teams had built provider integrations and gateways for specific use cases. Each approach met a real need, but together they added complexity to operations, cost attribution and consistent governance. The article lists four opportunities for improvement.

### Shared visibility
The goal is to bring model usage into one place so reporting is consistent. This comes first in the article's list, before cost and operations.

### Cost transparency
The goal is to attribute spend to a user, team or use case. Attribution is what makes the cost of AI visible to the people responsible for it.

### Simplified operations
The goal is to manage provider updates, incidents and support through one shared service instead of separately in each team.

### Consistent governance
The goal is to apply model approvals, logging and guardrails through a common approach. The article combines the four opportunities into "a shared operating model for managing our AI workloads with a single AI gateway solution."

## The decision: One centralized AI gateway
AUMOVIO's cross-functional AI taskforce brought together security, data protection, business areas and IT. It decided on one central AI Gateway as the shared route for LLM access and governance across the company. The team's remaining job was to make that central path useful to teams and their applications.

### Cross-functional AI taskforce
The taskforce is the governance mechanism the article describes. Its members came from security, data protection, business areas and IT, so the decision reflected all four concerns. The result was a common direction for model access across the whole company.

## Why we chose Databricks Unity Gateway
The article's core point is that building an AI gateway is straightforward, but operating one as an enterprise service is a permanent commitment. Five arguments decided the choice over buying another AI gateway.

### Operating a gateway as a permanent commitment
Provider APIs change, dependencies need patching, security requirements evolve and users need support. Once applications depend on the gateway, it needs a roadmap, an on-call rotation and a support process.

> ❌ **Anti-pattern — Pricing the prototype:** Treating the gateway as a simple build because the first version is straightforward. **Why people do it:** the first version really is straightforward. **Consequence:** the gateway becomes a permanent commitment that needs a roadmap, on-call rotation and support process. **Fix:** price the decade, not the prototype.

### 1. Databricks was already our data platform
The One AUMOVIO Data Platform (1ADP) runs on Databricks. Putting model access next to governed enterprise data meant no new vendor, no new network path to review and no isolated stack to operate.

### 2. Corporate identity integration
Databricks is integrated with the corporate Microsoft Entra ID tenant, and other AUMOVIO applications authenticate against the same tenant. An application presents its Entra ID token and gets access through token exchange, with no separate account, no API key to distribute and no bespoke integration. Onboarding a new AI-enabled application takes a permission grant, and access to every model ends automatically when the corporate identity does.

> 🔎 Added context: Token exchange means trading one token for another, usually one scoped to a different service. Microsoft Entra ID is an identity provider that issues OpenID Connect and OAuth 2.0 tokens.

> ❌ **Anti-pattern — Per-application credentials:** Each application gets its own account and API key, plus a bespoke integration. **Why people do it:** it is the quickest way to connect one application to one provider; the article does not say this, it is implied by its contrast. **Consequence (implied):** keys to distribute and revoke for every application, and access that does not end when the identity does. **Fix:** authenticate applications against the corporate identity and use token exchange.

### 3. Governance and usage tracking
Model approvals, access policies, usage tracking and logging sit in one place instead of being reimplemented per application. Payload logging through inference tables is off by default and enabled only with an explicit, approved justification, which the article says is the posture its data protection agreements require.

### 4. We stayed free to choose providers
The article calls this the most important argument. AUMOVIO publishes a curated catalog of 1adp-* model aliases, and Unity Gateway routes each alias to whichever provider serves it: Databricks-hosted foundation models, Google Vertex AI, Azure OpenAI, AWS Bedrock or others. Users code against a stable, vendor-neutral name, and the company can re-point that name to another provider or a newer model without any consumer changing a line of code.

> ❌ **Anti-pattern — Hard-coding provider model names:** Application code names a specific vendor model directly. **Why people do it:** it works immediately for the one provider in use. **Consequence:** switching provider or model means changing every consumer, which the aliases avoid. **Fix:** code against a 1adp-* alias.

### 5. The setup is infrastructure as code
Terraform defines everything Unity Gateway does for AUMOVIO: the endpoints behind each 1adp-* alias, the provider each model routes to, the fallback order when a provider degrades, rate limits and the guardrails applied per endpoint. Adding a model, re-pointing an alias or tightening a safety control is a pull request that is reviewed, versioned and reproducible across environments. When production misbehaves, version control explains the configuration that caused it, and the Gateway is one more module for engineers who already run the platform.

## How Unity Gateway helps us with AI cost management and model access
Centralizing model access was the first step. The company then built three capabilities on top of Unity Gateway.

### Cost attribution and chargeback
Unity Gateway reports how many tokens were consumed. Every user assigns that consumption to a personal cost center or to one of the projects they work in. Pipelines join those assignments with Databricks system tables and charge usage to the right cost center. The results are published as dashboards showing which models are used, by whom, how consumption develops and what it costs, so budget owners can see their spend and spot changes in consumption.

> 🔎 Added context: Chargeback means billing consumption back to the business unit that uses it, instead of absorbing it in a central IT budget.

### A curated model catalog
The catalog is the single source of truth for which models are approved, what they map to and which consumption tier they run in. Teams still test their application against the model they choose, but they no longer build infrastructure to reach it.

> ❌ **Anti-pattern — Treating standardized access as model equivalence:** Assuming that one route makes all models interchangeable. **Why people do it:** a single endpoint can make models look like drop-in replacements (an implication of the article's point). **Consequence:** the article says standardized access "does not make models interchangeable", so behaviour can differ across models. **Fix:** keep testing the application against the chosen model, and use the catalog to check what each model is approved for.

### An interface for employees
The 1ADP AI App runs in production on the same gateway and gives employees a governed way to use AI without writing code. Developers get an OpenAI-compatible endpoint and can point compatible agentic coding tools at the central models. The article's summary: same infrastructure, two audiences. The company standardizes model access and leaves the choice of development interface open.

> 🔎 Added context: An OpenAI-compatible endpoint accepts requests in the format of OpenAI's API, so tools built for that format can be pointed at it.

## The results so far: Centralized AI governance, Cost visibility, and developer access
Unity Gateway provides a shared route for LLM access. The 1adp-* aliases connect applications to models, guardrails apply centrally, and usage is attributed to cost centers in near real time.

### Next phase: agents and tools
The next phase moves up the AI platform stack: sharing agents inside 1ADP projects and connecting knowledge sources, web search and other tools through managed and bring-your-own MCP (Model Context Protocol) connections. Model access stays centralized, but the range of tools an agent can reach grows. That raises three questions: what an agent may retrieve, which tools it may call and whose permissions it acts under. The article says one framework to answer them is why the company consolidated in the first place.

> 🔎 Added context: MCP is an open standard for connecting AI applications to external tools, files, databases and APIs through one common interface.

## Lessons learned: Building vs. buying an enterprise AI Gateway
If you are bringing different model access approaches together, involve security, data protection, business areas and IT in the operating model, then make the shared service useful to the teams expected to use it.

### Price the decade, not the prototype
When comparing build with buy, ask who patches the provider integration, who answers the support ticket, who handles the incident, and who does it again next year.

> 📋 **Checklist — Build vs. buy questions**
> - [ ] Who patches the provider integration
> - [ ] Who answers the support ticket
> - [ ] Who handles the incident
> - [ ] Who does it again next year

### Build what is genuinely yours
Decide what is genuinely yours to build. The gateway was not. Cost attribution, the model catalog and the employee-facing AI App were, because they encode how AUMOVIO works and no vendor was going to hand them over.

> 📋 **Checklist — Building vs. buying an AI gateway**
> - [ ] Involve security, data protection, business areas and IT in the operating model
> - [ ] Make the shared service useful to the teams expected to use it
> - [ ] Price the decade rather than the prototype
> - [ ] Decide what is genuinely yours to build

### Accountability stays with the company
The article says AUMOVIO remains accountable for its architecture, its security decisions and the quality of its applications. Unity Gateway provides the shared model access service, while teams keep choosing the applications they build.

## Frequently Asked Questions
### Why not build its own gateway
The company chose Unity Gateway to centralize model access without the ongoing maintenance, security updates and operational overhead of building its own. Because Databricks already powered its data platform, it could reuse existing infrastructure, Entra ID integration and governance controls. That left teams free to build company-specific capabilities such as cost attribution, a model catalog and an employee AI application.

### Governance across multiple providers
Unity Gateway gives a single governed entry point to providers such as Azure OpenAI, Google Vertex AI, AWS Bedrock and Databricks-hosted models. Central approvals, access policies, logging and guardrails apply the same governance across applications. Curated aliases let administrators change the underlying model or provider without application code changes.

### Tracking and allocating costs
Unity Gateway provides token usage data. AUMOVIO combines it with Databricks system tables and project and cost center assignments to attribute spend to business owners. Dashboards give near-real-time visibility into usage, trends and costs, which supports chargeback and budget oversight.

### Developer access without managing API keys
Developers reach approved models through an OpenAI-compatible endpoint, so they do not manage separate provider integrations. Applications authenticate through Entra ID token exchange, which removes the need to distribute individual API keys. IT manages permissions and provider connections centrally.

### Benefits of a central gateway for agents
A central gateway puts access to multiple models, governance, cost visibility and operational management into one shared service. The same infrastructure serves employee applications and developer tools. The next phase extends this to shared agents and MCP connections, with a focus on governing tool access and permissions.

## 🧠 Mental models
### Build what encodes your operating model, buy the rest
**The model:** The article draws the build-or-buy line by asking whether a component encodes how the company works. The gateway did not, so it was bought. Cost attribution, the model catalog and the AI App did, so they were built.
**How to apply it:** For each candidate component, ask whether a vendor could sell it without knowing how your company works. If not, build it; if so, buy it.

### Price the decade, not the prototype
**The model:** The real cost of a shared service is the recurring work of keeping it running for years, not the cost of the first version.
**How to apply it:** Before choosing build over buy, list who patches, supports, responds to incidents and repeats the work next year, and estimate that cost over several years.

## 🔑 Key takeaways
- AUMOVIO consolidated model access into one gateway to pursue four goals: shared visibility, cost transparency, simplified operations and consistent governance.
- The company bought the gateway because operating one as an enterprise service is a permanent commitment. It built only cost attribution, the model catalog and the AI App.
- Five arguments decided the choice: an existing data platform, corporate identity integration, governance and usage tracking, provider freedom, and infrastructure as code.
- Stable 1adp-* aliases let the company re-point a name to another provider or newer model without changing consumer code.
- Token exchange with the corporate identity removes the need to distribute API keys, and access to every model ends when the corporate identity does.
- Payload logging is off by default and enabled only with an explicit, approved justification, which the article ties to its data protection agreements.
- The next phase, agents and MCP tool connections, shifts the governance question to which tools an agent can reach and whose permissions it acts under.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Business (cost centers, budget owners, the cross-functional taskforce); Data (usage and system tables, the payload logging policy); Application (the shared gateway service, the 1ADP AI App, developer access); Technology (Databricks, Microsoft Entra ID, Terraform, several model providers). The article does not use TOGAF terms; this mapping is a classification of its content. |
| Scope | Enterprise: the taskforce set one central gateway as the route for LLM access across the company. |
| Stakeholders and concerns | Taskforce members from security, data protection, business areas and IT; budget owners (spend visibility); developers (API access without key distribution); employees (governed AI use); the engineers who run the platform (reproducible configuration, support load). Concerns: governance, data protection, cost attribution, provider change without code change, and operational support. |
| Frameworks and notations | None named. TOGAF, ArchiMate, BPMN, BIZBOK and C4 do not appear. Named technologies only: Databricks, Microsoft Entra ID, Terraform, OpenAI-compatible endpoints and MCP. |

### Decisions and trade-offs
- **Buy a gateway rather than build one:** The options were building an AI gateway or buying another one. The company chose Databricks Unity Gateway, which it already had through its data platform. Cost: the article says operating a gateway is a permanent commitment, and it does not discuss the cost of depending on Databricks beyond the reuse benefits.
- **One central route rather than team-level access:** The taskforce chose one central AI gateway for LLM access and governance. Cost: applications come to depend on a shared service that needs a roadmap, on-call rotation and support process.
- **Vendor-neutral 1adp-* aliases rather than direct provider names:** Applications code against aliases that Unity Gateway maps to providers. Cost (implied): the company curates and maintains the catalog and the mapping itself.
- **Payload logging off by default:** Logging is enabled only with an explicit, approved justification. Cost (implied, not discussed): less payload data is available when troubleshooting.
- **Terraform for all gateway configuration:** Changes are reviewed pull requests, versioned and reproducible. Cost (implied): changes move through review, so they are slower than direct edits.
- **Build the differentiators, buy the gateway:** The company built cost attribution, the catalog and the AI App. Cost: the article says it remains accountable for those components and for its architecture and security decisions.

### Foundations it rests on
- [Enterprise Architecture](/foundations/enterprise-architecture.md) — The article applies it at company scope: one taskforce-chosen gateway aligns LLM access and governance across the company.
- [Technology Architecture](/foundations/technology-architecture.md) — The gateway, Databricks, Entra ID, Terraform and the model providers are the technology services and components it describes.
- [Application Architecture](/foundations/application-architecture.md) — The article describes the AI App and developer tools using the shared gateway, but treats the gateway itself as a shared service rather than an application.
- [Architecture domain](/foundations/architecture-domain.md) — The article touches business, data, application and technology, though it does not name the domains.
- [Business Capability](/foundations/business-capability.md) — The article says "capabilities" for three built features (cost attribution, catalog, employee interface), which are product features rather than what the business does.
- [Data Architecture](/foundations/data-architecture.md) — The article covers usage data, system tables and inference-table logging, but as data flows and policy rather than a data architecture.

## 🏛️ For Kenji
*This is your interpretation, not the author's.* The article's tests, "price the decade" and "build what encodes how you work", apply directly to make-or-buy decisions for shared platforms and to the business case and cost allocation work of an MBA in architecture. Once tools and agents sit behind the same gateway, which permission model should decide what an agent may retrieve or call?

## 📚 Reading plan
### From the article
- No links are cited in the extracted text, so none can be listed here.
### To go further
- Terraform and infrastructure as code — how reviewed, versioned configuration changes work for shared platforms.
- Microsoft Entra ID token exchange and OAuth 2.0 — the mechanism behind access without distributed API keys.
- Chargeback and showback in IT cost management — how usage is attributed to cost centers and budget owners.
- Model Context Protocol — the standard behind the MCP connections the next phase depends on.
- Build-versus-buy total cost of ownership for internal platforms — how to price years of operating a shared service.

## 🔗 Concepts
- [model-context-protocol](/concepts/ai-ml/model-context-protocol.md)
- [oidc](/concepts/security/oidc.md)
- [oauth2](/concepts/security/oauth2.md)
- [ai-gateway](/concepts/ai-ml/ai-gateway.md)
