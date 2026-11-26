---
type: Concept
title: AI gateway
description: A shared proxy between applications and LLM providers that centralizes routing, authentication, cost tracking, rate limits, caching, logging and policy for every model call.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Cloudflare. AI Gateway documentation
  resource: 'https://developers.cloudflare.com/ai-gateway/'
  kind: docs
- title: Databricks. AI Gateway documentation
  resource: 'https://docs.databricks.com/aws/en/ai-gateway/'
  kind: docs
- title: LiteLLM. Proxy Server (LLM Gateway) documentation
  resource: 'https://docs.litellm.ai/docs/simple_proxy'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 10 Step 3. Add Model Router and Gateway, pp. 456-460'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# AI gateway

**Definition.** An AI gateway is an API gateway specialised for model traffic. Applications call one endpoint; the gateway forwards each request to a model provider (OpenAI, Anthropic, a self-hosted model) and applies organisation-wide controls on the way: who may use which model, how much they may spend, what may be logged, and what to do when a provider fails.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `llm-engineering`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Common features: a unified API across providers; credential management so teams never hold provider keys; per-team quotas, token-based rate limits and cost attribution (chargeback); fallback and load balancing across models or regions; response caching, including semantic caching; request/response logging for audit and evaluation; and guardrails such as PII redaction or prompt-injection filters. Products include Cloudflare AI Gateway, Kong AI Gateway, Databricks Mosaic AI Gateway and the open-source LiteLLM proxy.

## Why it matters and trade-offs

Without a gateway each team integrates providers separately, so spending, data exposure and model choice are invisible to the organisation. A gateway gives one place for governance and makes switching providers cheap. It adds a hop of latency, becomes a critical dependency (it must be highly available), and concentrates sensitive prompts in one log store that must itself be protected.

## Example

AUMOVIO (from the 2026-11-19 note) routes all internal LLM use through Databricks Unity AI Gateway to enforce permissions and charge costs back to business units.

## Review notes for Kenji

The term is vendor-driven and has no standards-body definition yet; the three sources are independent vendors that agree on the feature set.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 10 Step 3. Add Model Router and Gateway, pp. 456-460: Routers that pick a model per query and gateways that unify access, keys, fallbacks and cost control.

## Sources

1. Cloudflare. *AI Gateway documentation*. <https://developers.cloudflare.com/ai-gateway/> — Caching, rate limiting, analytics, logging and fallback for model requests.
2. Databricks. *AI Gateway documentation*. <https://docs.databricks.com/aws/en/ai-gateway/> — Governance features - permissions, usage tracking, guardrails, traffic routing.
3. LiteLLM. *Proxy Server (LLM Gateway) documentation*. <https://docs.litellm.ai/docs/simple_proxy> — Open-source implementation - unified API, virtual keys, budgets, load balancing.
4. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 10 Step 3. Add Model Router and Gateway, pp. 456-460. Routers that pick a model per query and gateways that unify access, keys, fallbacks and cost control.

## Relations
- (none yet)

## Notes
- [How AUMOVIO centralized AI access with Databricks Unity Gateway](/notes/architecture/2026-11-19-how-aumovio-centralized-ai-access-with-databricks-unity-gate.md)
