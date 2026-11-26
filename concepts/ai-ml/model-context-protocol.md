---
type: Concept
title: Model Context Protocol (MCP)
description: An open protocol (JSON-RPC based) that standardises how AI applications connect to external tools, data sources and prompts, so any compatible client can use any compatible server.
tags:
- ai-ml
- ai-agents
domain: ai-ml
subdomain: ai-agents
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Model Context Protocol. Specification 2025-06-18
  resource: 'https://modelcontextprotocol.io/specification/2025-06-18'
  kind: standard
- title: Anthropic (2024). Introducing the Model Context Protocol
  resource: 'https://www.anthropic.com/news/model-context-protocol'
  kind: practitioner
- title: 'Hou, X. et al. (2025). Model Context Protocol (MCP) - Landscape, Security Threats, and Future Research Directions. arXiv:2503.23278'
  resource: 'https://arxiv.org/abs/2503.23278'
  kind: paper
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Model Context Protocol (MCP)

**Definition.** The Model Context Protocol (MCP) defines a client-server contract between an AI application (the host, such as Claude Desktop or an IDE) and servers that expose capabilities. Anthropic introduced it in November 2024 as an open standard; it is often described as "USB-C for AI apps" because it replaces one custom integration per app-tool pair with one protocol.

Domain: [ai-ml](/domains/ai-ml.md) · subdomain `ai-agents`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Messages are JSON-RPC 2.0 over stdio (local servers) or streamable HTTP (remote servers). Servers expose three primitives: tools (functions the model may call), resources (data the app may read into context) and prompts (reusable templates). Clients can offer sampling (letting a server ask the host's model for a completion) and elicitation (asking the user for input). Remote servers authenticate with OAuth 2.1. A capability negotiation at connection time tells each side what the other supports.

## Why it matters and trade-offs

MCP was adopted quickly by OpenAI, Google, Microsoft and most agent tools, so one server works across many clients. Security is the main concern: tool descriptions and returned content are untrusted input that can carry prompt injection ("tool poisoning"), servers may over-request permissions, and a malicious or compromised server is a supply-chain risk. Hosts must show and gate what tools do.

## Example

The Notion and GitHub connectors used in this project are MCP servers: the same server works in Claude, in an IDE or in another agent.

## Sources

1. Model Context Protocol. *Specification 2025-06-18*. <https://modelcontextprotocol.io/specification/2025-06-18> — Architecture, transports, primitives, authorization.
2. Anthropic (2024). *Introducing the Model Context Protocol*. <https://www.anthropic.com/news/model-context-protocol> — Origin and motivation (replace per-integration connectors with one standard).
3. Hou, X. et al. (2025). *Model Context Protocol (MCP) - Landscape, Security Threats, and Future Research Directions*. arXiv:2503.23278. <https://arxiv.org/abs/2503.23278> — Independent analysis of the protocol lifecycle and its security threats.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
