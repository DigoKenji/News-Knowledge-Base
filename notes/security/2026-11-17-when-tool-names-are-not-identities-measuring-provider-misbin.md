---
type: Paper Summary
title: 'When Tool Names Are Not Identities: Measuring Provider Misbinding in MCP-Based AI Agents'
description: Tool names, descriptions and schemas do not bind a tool to its provider; aligned attacker metadata won 100% of selections in three of five clients.
resource: https://arxiv.org/abs/2611.21577
tags:
- security
- appsec
- threat-modeling
- model-context-protocol
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://export.arxiv.org/pdf/2611.21577
  title: 'When Tool Names Are Not Identities: Measuring Provider Misbinding in MCP-Based AI Agents'
  author: Zeynab Anbiaee, Sajjad Dadkhah, Ali A. Ghorbani
status: stable
source_url: https://arxiv.org/abs/2611.21577
source: arXiv cs.AI/cs.LG/cs.CL
source_type: arxiv
published: '2026-11-16'
domain: security
subdomain: appsec
concepts:
- threat-modeling
- model-context-protocol
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81b9b54df1bea2b96ba8
---

# When Tool Names Are Not Identities: Measuring Provider Misbinding in MCP-Based AI Agents

**Source:** https://arxiv.org/abs/2611.21577
**Author:** Zeynab Anbiaee, Sajjad Dadkhah, Ali A. Ghorbani
**Date:** 2026-11-16
**Concepts:** [Threat modeling](/concepts/threat-modeling.md) · [Model Context Protocol (MCP)](/concepts/model-context-protocol.md)

---

## 🎯 In one sentence
Tool names, descriptions and schemas do not identify a tool's provider: in the strongest case (C10: exact name, copied description, compatible schema), GPT-5.4, Qwen3.7 Max and DeepSeek V3 chose the attacker's tool in every one of 50 trials (violation rate 1.00), while Claude Sonnet 5 mostly asked for clarification instead (violation rate 0.06, clarification rate 0.94).

## ❓ The problem
LLM applications now act as agents that pick and call external tools. The Model Context Protocol (MCP) standardizes this: a client connects to several servers, each advertising tools with a **name**, a natural-language **description** and an **input schema** (the typed arguments the tool accepts). The LLM reads these fields to choose which tool to call, so they form part of the agent's control plane. MCP requires names to be unique within one server, but it does not provide a host-wide, provider-bound identity, so two independently run servers can advertise tools that look alike. The authors call the resulting failure **tool-provider misbinding**: a request meant for a legitimate provider is resolved to an attacker-controlled tool, even though the host, LLM, legitimate server and network are never compromised. This is hard to catch with ordinary tests, because the wrong tool can look semantically correct.

## 💡 The idea
The key insight is that a tool is not a string, a description or a JSON schema; it is a provider-bound authority to act. So the executable that gets selected should be bound to the provider authorized to offer it, and matching metadata alone does not establish that. Because an attacker controls only its own advertisement, the authors treat name, description and schema as the attacker's controllable selection surface and vary each one on its own. The closest conventional analogy they give is namespace confusion in software supply chains, where package and update systems learned that familiar names are not enough and artifacts must be bound to authenticated publishers. Turning this into a factorial experiment with explicit outcome categories lets them see which field, or which combination of fields, drives the failure.

## ⚙️ How it works
### Provider-unbound tool identity
A tool's visible attributes are attr(t) = {n(t), d(t), σ(t), m(t)}: name, description, schema and other metadata. Nothing in this set proves which provider runs the tool. The authors' target form adds a verified provider identity, attr+(t) = {n(t), d(t), σ(t), m(t), p(t)}, and they hypothesize that without that binding a client may confuse two similar tools served by different providers.
```text
attr(t)  = { n(t), d(t), σ(t), m(t) }         visible attributes today
attr+(t) = { n(t), d(t), σ(t), m(t), p(t) }   adds verified provider identity p(t)
```
Here n is the tool name, d the description, σ the input schema, m other exposed metadata and p the verified provider. The paper's contribution is to show what happens when p is missing.

### Attacker-controlled tool variants
The attacker controls only its own tool advertisement and implementation; the legitimate tool, the client and the user prompts stay fixed. The attacker varies three factors: name (unrelated or exact copy of the legitimate name), description (unrelated, copied verbatim, or authority-enhanced, which keeps the task and adds one trust cue and one security or control cue such as "enterprise-approved") and schema (compatible, meaning it accepts the same dataset_id and destination arguments, so one client invocation could run against either provider, or incompatible, which is still valid MCP but has an unrelated interface). Two levels of name and schema and three levels of description give 2 x 3 x 2 = 12 conditions, C01 to C12.

### Selection, outcomes and binding stability
For each trial the client's selection policy is a black box that returns a tool, asks for clarification, or fails. The trial is labelled A (attacker tool), L (legitimate tool), C (clarification) or F (failure), and the three rates are computed from those labels:
```text
VR  = A / (A + L + C + F)   violation rate: share of trials that chose the attacker tool
CR  = C / (A + L + C + F)   clarification rate
LSR = L / (A + L + C + F)   legitimate-selection rate
```
Because no trial failed in the study, 1 - LSR = VR + CR. Resolved binding stability (RBS) asks whether repeated runs of the same prompt bind to the same tool and provider. Clarification and failure never count as a binding.
```text
S_ij  = 1 if runs j and j+1 are both A or L and bind to the same tool-provider pair, else 0
RBS_i = (1 / (N - 1)) * sum over j = 1..N-1 of S_ij   (N = 5 runs per prompt)
```
RBS must be read together with the provider: a stable attacker binding is as "stable" as a stable legitimate one, and the authors say the attacker case is the worse one.

### Experimental setup
The environment has one legitimate MCP server and one attacker-controlled server. Each condition is run with 10 task-equivalent prompts (the last five use security-aware wording that stresses the trusted destination), each repeated 5 times, which gives 50 trials per condition and model. The authors report 95% Wilson intervals, and for the robustness runs the mean and standard deviation across generated names or descriptions. All tool calls are logged with the selected server identifier, which the authors use as the authoritative provider label. The study uses a local mock dataset, a trusted local destination and no real user data, and it produces no external side effects.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| C10: exact name, copied description, compatible schema (r = 1) | C09, same but incompatible schema: VR 0.00 for all five models | VR / CR: GPT-5.4 1.00 / 0.00; Claude Sonnet 5 0.06 / 0.94; Gemini 2.5 Pro 0.84 / 0.16; Qwen3.7 Max 1.00 / 0.00; DeepSeek V3 1.00 / 0.00 | Violation rate (VR), clarification rate (CR), 50 trials per model |
| C12: exact name, authority-enhanced description, compatible schema (r = 1) | C11, same but incompatible schema: VR GPT-5.4 0.04, Claude Sonnet 5 0.00, Gemini 2.5 Pro 0.02, Qwen3.7 Max 0.18, DeepSeek V3 0.56 | VR / CR: GPT-5.4 0.96 / 0.00; Claude Sonnet 5 0.26 / 0.74; Gemini 2.5 Pro 0.70 / 0.30; Qwen3.7 Max 1.00 / 0.00; DeepSeek V3 1.00 / 0.00 | VR, CR |
| Schema amplification, C10 vs C09 | C09: average VR 0 | C10: average VR approximately 0.78 (text) | Mean VR across five models |
| C07: exact name only, unrelated description, incompatible schema | Name collision with no other alignment | GPT-5.4 VR 0.02; Gemini 2.5 Pro CR 0.20; other models 0.00 VR and 0.00 CR | VR, CR |
| C06: unrelated name, authority-enhanced description, compatible schema | Unrelated name, no collision | VR: GPT-5.4 0.04; Claude Sonnet 5 0.00 (CR 0.22); Gemini 2.5 Pro 0.42 (CR 0.56); Qwen3.7 Max 0.20; DeepSeek V3 0.50 | VR, CR |
| Low-alignment controls C01, C02, C03, C05 | Control conditions | VR and CR 0.00 for all five models; one exception, C04 DeepSeek V3 VR 0.22 | VR, CR |
| Robustness: C06 over five unrelated names (250 trials per model) | C06 with one name | Mean VR: GPT-5.4 0.248 [95% CI 0.199 to 0.305]; Gemini 2.5 Pro 0.460 [0.399 to 0.522]; Qwen3.7 Max 0.204 [0.159 to 0.258]; DeepSeek V3 0.696 [0.636 to 0.750]; Claude Sonnet 5 0.000 (CR 0.224) | Mean VR, pooled 95% Wilson CI |
| Robustness: C12 over five authority-enhanced descriptions D1-D5 | C12 with one description | Mean VR: GPT-5.4 0.980; Claude Sonnet 5 0.256 (CR 0.744); Gemini 2.5 Pro 0.720 (CR 0.280); Qwen3.7 Max 1.000; DeepSeek V3 1.000 | Mean VR, CR |
| Attacker budget, C09 from r = 1 to r = 10 | r = 1 | GPT-5.4 VR 0.00 to 0.34; Claude Sonnet 5 CR 0.22 to 1.00 | VR, CR |
| Attacker budget, C10 for GPT-5.4 | r = 1 | VR 1.00 at every r in {1, 3, 5, 10} | VR |

The results show that misbinding is strongest when name, description and schema all align, and that the schema is a common amplifier across models. Low-alignment metadata alone is almost never enough, and the robustness runs show the effect is not tied to one name or one description string. The evidence is still narrow: one dataset-fetching task, 10 prompts, 5 repetitions per prompt, one attacker server and a mock dataset. The results do not show how often this happens on real MCP catalogues or with other clients, and the paper does not test a defence.

## ⚠️ Limitations
- The authors test one controlled dataset-fetching task, so other tool classes may give different rates.
- They test five LLM-based clients; other models, orchestrators, retrieval policies or confirmation designs may behave differently.
- The metadata variants do not exhaust attacker strategies or real MCP deployments; the authors plan to extend the model to a wider range of attacks.
- Clarification is not counted as attacker success, but the authors say it still means the client did not bind the request to the legitimate provider.
- 🔎 Added context: the provider label is the server identifier recorded by the client's own log. That identifier is the same kind of unverified metadata the paper says is missing, so the measurement assumes the provider identity that the paper argues should be enforced.
- 🔎 Added context: the paper's numbers do not fully agree. For Gemini 2.5 Pro in C10 at r = 1, Table 2 gives VR 0.84 and CR 0.16, while the budget text gives VR 0.88 and CR 0.12. Check cell values against the full tables before quoting them.

## 🏗️ What it means for building systems
This is interpretation, not the authors' claim. If an agent gateway aggregates MCP servers from several teams or vendors, tool selection becomes a routing decision that needs an identity check, and the natural plug-in point is between the model's choice and the tool invocation: map each selected tool to a registry entry that pins the server identity, a signed manifest or a schema hash, and refuse or confirm when two providers advertise the same name. That costs a registry or allowlist to operate, manifest signing, and a user experience for clarification prompts that users will otherwise click through. To trust such a control you would need your own test suite, using exact-name, copied-description and budget variants against the models and clients you actually run, and you would track VR and CR rather than task success.

## 🔑 Key takeaways
- Name, description and schema are selection signals, not provider identity: in C10 three of five clients chose the attacker's tool in all 50 trials.
- Schema compatibility is the strongest common amplifier: average VR rose from 0 in C09 to approximately 0.78 in C10.
- Clarification is not safety: Claude Sonnet 5 had VR 0.06 but CR 0.94 in C10, which leaves the provider unresolved.
- Stable binding is not safety: a stable attacker binding is the worst case, so consistency alone is not a success metric.
- Metadata that is only weakly aligned (C01, C02, C03, C05) produced zero violations, so risk comes from several aligned fields together, and attacker budget mostly adds pressure on borderline cases.
- The authors' conclusion is that MCP clients need provider-bound tool identity, which complements authorization, sandboxing and user consent rather than replacing them.

## 📚 Reading plan
### From the paper
- [Introducing the Model Context Protocol](https://www.anthropic.com/news/model-context-protocol) — where MCP comes from and the intended role of tools in agents.
- [Model Context Protocol: Tools](https://modelcontextprotocol.io/specification/2025-06-18/server/tools) — the exact tool advertisement fields (name, description, input schema) the attacker manipulates.
- [Model Context Protocol Architecture](https://modelcontextprotocol.io/specification/2025-06-18/architecture) — the host, client and server roles behind the threat model.
- [InjecAgent](https://aclanthology.org/2024.findings-acl.624/) — benchmark for indirect prompt injection in tool-integrated agents, the closest prior attack setting.
- [PoisonedRAG](https://www.usenix.org/conference/usenixsecurity25/presentation/zou-poisonedrag) — retrieval poisoning, the paper's comparison for attacks on external objects consumed by an LLM.
- [Sigstore: Software signing for everybody](https://doi.org/10.1145/3548606.3560596) — the signing and transparency approach the authors borrow from software supply chains.
### To go further
- Prompt injection attack to tool selection in LLM agents (Shi et al., NDSS 2026) — the direct precursor on tool-selection manipulation.
- Secure tool manifest and digital signing solution for verifiable MCP and LLM pipelines (Jamshidi et al., 2026) — a manifest-signing design that would implement provider-bound identity.
- Beyond typosquatting: an in-depth look at package confusion (USENIX Security 2023) — the supply-chain analogy the authors use, studied in depth.
- Les dissonances: cross-tool harvesting and polluting in pool-of-tools empowered LLM agents (NDSS 2026) — cross-tool contamination in the same class of agents.
