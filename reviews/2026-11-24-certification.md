---
type: Review Checklist
title: Certification checklist — 2026-11-24
description: Every draft concept (45) and draft foundation (10) in one list, for Kenji to approve or fix in one pass.
status: open
generated:
  by: claude (project thread, from the repo state at be3ddae)
  at: '2026-11-24T19:45:00-03:00'
---

# Certification checklist — 2026-11-24

Everything in the KB that is still `status: draft`: **45 concepts** and **10 foundations**. Nothing here has been certified by Claude; only you certify.

## How to review

1. Read each definition (click the title to open the full entry and the notes it came from).
2. If it's right, tick the box: `- [ ]` → `- [x]`.
3. If it needs a change, leave the box empty and write what to fix after **Fix:**. A rough note is enough ("too narrow, it's not only for agents").
4. Skip anything you're unsure about; it stays a draft and shows up in the next checklist.

You can do this by editing this file on GitHub (pencil icon on the pull request branch), or just reply in the project thread by number, for example *"approve all except C06, C40; C06: broaden to any LLM app"*.

## What happens to your answers

- **Ticked concept** → set to `status: stable` with `verified: [{by: Kenji, at: <date>}]`, its Portuguese twin in `pt-br/` gets the same, and `concepts/index.md` is updated. The daily run never edits a verified concept again.
- **Concept with a Fix** → Claude rewrites the definition as you described and leaves it `draft`. It comes back in the next checklist for a quick yes.
- **Ticked foundation** → the wording is approved (recorded under `evidence` as `wording_approved`), but it stays `draft` until it has quotes from a second, independent issuing body, as ADR-0010 requires. Tell me if you want any of them certified anyway with `meets_standard: partial`.
- When all answers are applied, this file is marked `status: closed` and kept as the record of what you approved and when.

## Part 1 — Draft concepts (45)

Short vocabulary entries the daily run (Claude Haiku) proposed from the articles it summarized. Check that the one-line definition is right and not too narrow for the article it came from.

### AI/ML (22)

- [ ] **C01 · [Agent harness](/concepts/agent-harness.md)** — The code around a fixed model that manages its context, tools and execution flow.
  - From: [Self-Improvement Memory Enables Continual Recursive Self-Improvement](/notes/ai-ml/2026-11-17-self-improvement-memory-enables-continual-recursive-self-imp.md); [Building an Agentic Knowledge Graph for Billions of Events](/notes/ai-ml/2026-11-18-building-an-agentic-knowledge-graph-for-billions-of-events.md); [Introducing Claude Fable 5.2 and Claude Mythos 5.2](/notes/ai-ml/2026-11-18-introducing-claude-fable-5-2-and-claude-mythos-5-2.md); +6 more
  - Fix: 
- [ ] **C02 · [Agent memory](/concepts/agent-memory.md)** — The external, persistent store of records an agent accumulates across sessions and retrieves from.
  - From: [MemFolder: Structure-Guided Memory Folding for Efficient Agent Memory Management](/notes/ai-ml/2026-11-21-memfolder-structure-guided-memory-folding-for-efficient-agen.md); [Backtracking Agent Applications Security Incidents](/notes/security/2026-11-22-backtracking-agent-applications-security-incidents.md)
  - Relations: part-of `ai-agent`
  - Fix: 
- [ ] **C03 · [AI agent](/concepts/ai-agent.md)** — A system that plans a task, uses software tools and completes multi-step work with limited supervision.
  - From: [You Think AI Is Overhyped? Just Wait Until You See What Comes Next.](/notes/ai-ml/2026-11-20-you-think-ai-is-overhyped-just-wait-until-you-see-what-comes.md); [A Capability-First Approach to Modernizing Retirement Services Technology Platforms](/notes/architecture/2026-11-21-a-capability-first-approach-to-modernizing-retirement-servic.md); [Fragments: November 20](/notes/software-engineering/2026-11-21-fragments-november-20.md); +9 more
  - Fix: 
- [ ] **C04 · [AI gateway](/concepts/ai-gateway.md)** — A shared service that routes application LLM calls to model providers under central governance.
  - From: [How AUMOVIO centralized AI access with Databricks Unity Gateway](/notes/architecture/2026-11-19-how-aumovio-centralized-ai-access-with-databricks-unity-gate.md)
  - Fix: 
- [ ] **C05 · [Calibration](/concepts/calibration.md)** — How closely a model's stated confidence or probabilities match how often it is actually right.
  - From: [$0.0001 Decision Model That Will Replace Millions of LLM Calls](/notes/ai-ml/2026-11-17-0-0001-decision-model-that-will-replace-millions-of-llm-call.md); [I Tested TypeSafe's Jev, a 478× Cheaper Decision Model, Against Claude, GPT-6, Kimi, MiniMax and DeepSeek](/notes/ai-ml/2026-11-17-i-tested-typesafe-s-jev-a-478-cheaper-decision-model-against.md); [Jev killed LLM-as-a-Judge?](/notes/ai-ml/2026-11-17-jev-killed-llm-as-a-judge.md); +2 more
  - Fix: 
- [ ] **C06 · [Context engineering](/concepts/context-engineering.md)** — Deciding what information an agent sees, and in what form, so it can act like an engineer who knows the codebase.
  - From: [Netflix at QConSF 2026: Retrofitting the SDLC for AI Coding Agents](/notes/software-engineering/2026-11-24-netflix-at-qconsf-2026-retrofitting-the-sdlc-for-ai-coding-a.md)
  - Relations: part-of `ai-agent`
  - Fix: 
- [ ] **C07 · [Distillation](/concepts/distillation.md)** — Training a model on another model's outputs to extract its capabilities.
  - From: [Introducing Claude Fable 5.2 and Claude Mythos 5.2](/notes/ai-ml/2026-11-18-introducing-claude-fable-5-2-and-claude-mythos-5-2.md); [Combatting adversarial distillation](/notes/security/2026-11-21-combatting-adversarial-distillation.md)
  - Fix: 
- [ ] **C08 · [Goodput](/concepts/goodput.md)** — The rate of requests that finish within their timeout.
  - From: [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
  - Fix: 
- [ ] **C09 · [In-context learning](/concepts/in-context-learning.md)** — A model learns a task from labeled examples included in its input, with no weight updates.
  - From: [AI on Excel Sheet: NVIDIA's Kumo Runs 23x Faster Than Google's TabFM](/notes/data/2026-11-20-ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-googles.md)
  - Fix: 
- [ ] **C10 · [Joint-embedding predictive architecture (JEPA)](/concepts/jepa.md)** — Self-supervised models that predict representations of one view from another in embedding space, not raw tokens or pixels.
  - From: [Two JEPAs, one world model: the Platonic Representation Hypothesis](/notes/ai-ml/2026-11-16-platonic-representation-jepa.md)
  - Fix: 
- [ ] **C11 · [LLM-as-a-judge](/concepts/llm-as-a-judge.md)** — Using a language model to grade or score another model's outputs against stated criteria.
  - From: [Building strong safety evaluations](/notes/ai-ml/2026-11-16-building-strong-safety-evaluations.md); [LLM-as-a-Judge has a scoring problem (JevEval)](/notes/ai-ml/2026-11-16-jeveval-llm-as-a-judge.md); [Do Verifier Judgments Survive Abstraction? Measuring Verification Transport for LLM Agent Evaluation](/notes/ai-ml/2026-11-16-verifier-judgments-survive-abstraction.md); +12 more
  - Fix: 
- [ ] **C12 · [Low-rank adaptation (LoRA)](/concepts/lora.md)** — Trainable low-rank matrices added to frozen model weights, so a model can be adapted cheaply.
  - From: [LiveLoRA: Exact KV Cache Migration Across LoRA Updates for LLM Agents](/notes/ai-ml/2026-11-24-livelora-exact-kv-cache-migration-across-lora-updates-for-ll.md)
  - Fix: 
- [ ] **C13 · [Mixture of experts](/concepts/mixture-of-experts.md)** — A layer with many expert subnetworks and a router that sends each token to a few of them.
  - From: [The US-China Open-Weight Feedback Loop](/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md)
  - Fix: 
- [ ] **C14 · [Model Context Protocol (MCP)](/concepts/model-context-protocol.md)** — An open standard that lets AI applications connect to external tools, files, databases and APIs through one common interface.
  - From: [10 MCP Servers Every Local AI Developer Should Install](/notes/ai-ml/2026-11-17-10-mcp-servers-every-local-ai-developer-should-install.md); [GTIG AI Threat Tracker: Agentic Threat Activity Crosses a Significant Threshold](/notes/security/2026-11-17-gtig-ai-threat-tracker-agentic-threat-activity-crosses-a-sig.md); [How Rakbank built centralized observability at scale with Amazon OpenSearch Service](/notes/cloud-infra/2026-11-17-how-rakbank-built-centralized-observability-at-scale-with-am.md); +13 more
  - Fix: 
- [ ] **C15 · [Multi-head latent attention](/concepts/multi-head-latent-attention.md)** — Attention that caches keys and values as one shared compressed representation, which shrinks the KV cache.
  - From: [The US-China Open-Weight Feedback Loop](/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md)
  - Fix: 
- [ ] **C16 · [Parent-child chunking](/concepts/parent-child-chunking.md)** — Retrieves small child chunks for precise matching, then returns their larger parent chunk as context for the LLM.
  - From: [How to Architect Semantic Chunking Pipelines for High-Performance RAG](/notes/ai-ml/2026-11-17-how-to-architect-semantic-chunking-pipelines-for-high-perfor.md)
  - Relations: part-of `rag`
  - Fix: 
- [ ] **C17 · [Position bias](/concepts/position-bias.md)** — A judge's verdict changes when the order of the candidate answers is swapped, so presentation order alone affects the result.
  - From: [Jev killed LLM-as-a-Judge?](/notes/ai-ml/2026-11-17-jev-killed-llm-as-a-judge.md)
  - Fix: 
- [ ] **C18 · [Prefill-decode disaggregation](/concepts/prefill-decode-disaggregation.md)** — Serving LLM prefill and decode phases on separate GPU pools, moving the KV cache between them.
  - From: [Callo: Serving Cache-Miss Requests with Speculative Decoding on Invalidated KV under Prefill-Decode Disaggregation](/notes/ai-ml/2026-11-16-callo-speculative-decoding-invalidated-kv.md); [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
  - Relations: uses `kv-cache`, part-of `inference-tradeoffs`
  - Fix: 
- [ ] **C19 · [Rotary position embedding (RoPE)](/concepts/rope.md)** — Encodes token position by rotating query and key vectors, so attention depends on relative distance.
  - From: [Understanding Positional Embeddings in Modern LLMs](/notes/ai-ml/2026-11-16-understanding-positional-embeddings.md); [The US-China Open-Weight Feedback Loop](/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md); [LiveLoRA: Exact KV Cache Migration Across LoRA Updates for LLM Agents](/notes/ai-ml/2026-11-24-livelora-exact-kv-cache-migration-across-lora-updates-for-ll.md)
  - Fix: 
- [ ] **C20 · [Semantic search](/concepts/semantic-search.md)** — Finding information by the meaning of a query rather than by matching keywords.
  - From: [txtai: an all-in-one open-source framework for semantic search, RAG and agents](/notes/ai-ml/2026-11-17-txtai-an-all-in-one-open-source-framework-for-semantic-searc.md); [10 AI Tools Everyone Will Be Using in Less Than Three Months](/notes/ai-ml/2026-11-20-10-ai-tools-everyone-will-be-using-in-less-than-three-months.md)
  - Fix: 
- [ ] **C21 · [Speculative decoding](/concepts/speculative-decoding.md)** — A cheap draft model proposes several tokens that the target model verifies in one pass, cutting decode latency.
  - From: [Callo: Serving Cache-Miss Requests with Speculative Decoding on Invalidated KV under Prefill-Decode Disaggregation](/notes/ai-ml/2026-11-16-callo-speculative-decoding-invalidated-kv.md)
  - Relations: part-of `inference-tradeoffs`
  - Fix: 
- [ ] **C22 · [Structured decision model](/concepts/structured-decision-model.md)** — A model that answers discrete questions with typed outputs such as choices, scores and probabilities instead of generating free text.
  - From: [I Tested TypeSafe's Jev, a 478× Cheaper Decision Model, Against Claude, GPT-6, Kimi, MiniMax and DeepSeek](/notes/ai-ml/2026-11-17-i-tested-typesafe-s-jev-a-478-cheaper-decision-model-against.md); [Jev AI Explained: The New Model Making AI Decisions Ridiculously Cheap](/notes/ai-ml/2026-11-17-jev-ai-explained-the-new-model-making-ai-decisions-ridiculou.md); [Jev vs LLMs: Do AI Agents Really Need a Language Model for Every Decision?](/notes/ai-ml/2026-11-17-jev-vs-llms-do-ai-agents-really-need-a-language-model-for-ev.md); +2 more
  - Fix: 

### Backend systems (12)

- [ ] **C23 · [Adaptive concurrency limit](/concepts/adaptive-concurrency-limit.md)** — A cap on active operations that shrinks on congestion signals and grows back cautiously as health recovers.
  - From: [How we raised Databricks cluster rate limits by 7.5×](/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md); [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
  - Fix: 
- [ ] **C24 · [Cascading failure](/concepts/cascading-failure.md)** — A failure in one component shifts load onto its neighbours until they also fail, spreading through the system.
  - From: [Microsoft outage: A cascading failure that started with an undetected bug](/notes/cloud-infra/2026-11-22-microsoft-outage-a-cascading-failure-that-started-with-an-un.md); [The FAA outage is a warning about technical debt in the AI era](/notes/software-engineering/2026-11-24-the-faa-outage-is-a-warning-about-technical-debt-in-the-ai-e.md)
  - Fix: 
- [ ] **C25 · [Chaos engineering](/concepts/chaos-engineering.md)** — Deliberately injecting faults into a running system to see how it behaves under failure.
  - From: [Resilience testing your agents on Amazon Bedrock AgentCore](/notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md); [Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover](/notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md)
  - Fix: 
- [ ] **C26 · [Control plane](/concepts/control-plane.md)** — The layer that decides what work runs, when, and in what state, separate from the workers that execute it.
  - From: [OpenAI Just Open-Sourced Symphony. Every AI Startup Should Pay Attention](/notes/ai-ml/2026-11-20-openai-just-open-sourced-symphony-every-ai-startup-should-pa.md); [How Google Traced GKE API Timeouts to a Linux Kernel TCP Bug](/notes/cloud-infra/2026-11-21-how-google-traced-gke-api-timeouts-to-a-linux-kernel-tcp-bug.md); [Probing DNS at Cloudflare](/notes/cloud-infra/2026-11-24-probing-dns-at-cloudflare.md)
  - Fix: 
- [ ] **C27 · [Correlated failure](/concepts/correlated-failure.md)** — A failure that hits several redundant regions at once because they share a dependency, which defeats regional failover.
  - From: [Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover](/notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md); [Microsoft outage: A cascading failure that started with an undetected bug](/notes/cloud-infra/2026-11-22-microsoft-outage-a-cascading-failure-that-started-with-an-un.md); [More control, fewer dependencies: Why DORA is forcing a connectivity rethink](/notes/architecture/2026-11-24-more-control-fewer-dependencies-why-dora-is-forcing-a-connec.md)
  - Fix: 
- [ ] **C28 · [Dead-letter queue](/concepts/dead-letter-queue.md)** — A queue that holds messages that failed processing so they can be inspected, corrected and redriven.
  - From: [How Country Road Group built a pattern-driven integration solution on AWS](/notes/architecture/2026-11-20-how-country-road-group-built-a-pattern-driven-integration-so.md); [How United Airlines streams database changes to Apache Iceberg with Amazon Data Firehose](/notes/data/2026-11-22-how-united-airlines-streams-database-changes-to-apache-icebe.md)
  - Fix: 
- [ ] **C29 · [Deterministic simulation testing](/concepts/deterministic-simulation-testing.md)** — Testing a distributed system on one thread against a simulated clock, network and disk so every run replays exactly from a seed.
  - From: [Deterministic Simulation Testing](/notes/backend-systems/2026-11-17-deterministic-simulation-testing.md)
  - Fix: 
- [ ] **C30 · [GC pressure](/concepts/gc-pressure.md)** — The CPU cost of garbage collection, driven by the number of heap objects and references.
  - From: [Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS](/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md)
  - Fix: 
- [ ] **C31 · [Hybrid memory-disk cache](/concepts/hybrid-memory-disk-cache.md)** — A cache that keeps keys and hot values in RAM and spills the rest to disk.
  - From: [Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS](/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md)
  - Fix: 
- [ ] **C32 · [Linearizability](/concepts/linearizability.md)** — A strict consistency guarantee where each operation appears to take effect atomically at some point between its invocation and its response.
  - From: [Deterministic Simulation Testing](/notes/backend-systems/2026-11-17-deterministic-simulation-testing.md)
  - Fix: 
- [ ] **C33 · [Retry storm](/concepts/retry-storm.md)** — Retries from accepted work that add pressure to a degraded dependency and sustain the overload.
  - From: [How we raised Databricks cluster rate limits by 7.5×](/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md); [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
  - Fix: 
- [ ] **C34 · [Silent failure](/concepts/silent-failure.md)** — A failure that returns output looking successful, so monitoring and users miss it.
  - From: [Resilience testing your agents on Amazon Bedrock AgentCore](/notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md); [Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover](/notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md); [Microsoft outage: A cascading failure that started with an undetected bug](/notes/cloud-infra/2026-11-22-microsoft-outage-a-cascading-failure-that-started-with-an-un.md)
  - Fix: 

### Security (6)

- [ ] **C35 · [Indirect prompt injection](/concepts/indirect-prompt-injection.md)** — Instructions hidden in retrieved content, such as web pages or documents, that an assistant treats as authority over its behavior.
  - From: [Safe Resemblance, Silent Propagation: Limits of Text-Similarity Guardrails for AI Agent Safety](/notes/security/2026-11-17-safe-resemblance-silent-propagation-limits-of-text-similarit.md); [Agent Trust Infrastructure (ATI): Federated Identity and Attestation for Cross-Organization AI Agents](/notes/security/2026-11-18-agent-trust-infrastructure-ati-federated-identity-and-attest.md); [Kaer: Security-Aware Replanning for Multi-Agent Systems](/notes/security/2026-11-19-kaer-security-aware-replanning-for-multi-agent-systems.md); +2 more
  - Fix: 
- [ ] **C36 · [Information flow control](/concepts/information-flow-control.md)** — Tracking where untrusted data propagates so it cannot alter control decisions.
  - From: [Kaer: Security-Aware Replanning for Multi-Agent Systems](/notes/security/2026-11-19-kaer-security-aware-replanning-for-multi-agent-systems.md); [Replication of Agentic Security Events: Audit and Investigation (ROGUE-AI)](/notes/security/2026-11-21-replication-of-agentic-security-events-audit-and-investigati.md); [Backtracking Agent Applications Security Incidents](/notes/security/2026-11-22-backtracking-agent-applications-security-incidents.md)
  - Relations: mitigates `indirect-prompt-injection`
  - Fix: 
- [ ] **C37 · [Memory poisoning](/concepts/memory-poisoning.md)** — An attack that writes malicious content into an agent's persistent memory or knowledge store so it is retrieved and acted on later.
  - From: [Backtracking Agent Applications Security Incidents](/notes/security/2026-11-22-backtracking-agent-applications-security-incidents.md)
  - Fix: 
- [ ] **C38 · [Remote attestation](/concepts/remote-attestation.md)** — Hardware-signed proof of what software is running on a machine, checked by a verifier before that machine is trusted.
  - From: [How Bee implemented Zero Operator Access for personal AI on AWS](/notes/security/2026-11-21-how-bee-implemented-zero-operator-access-for-personal-ai-on.md)
  - Fix: 
- [ ] **C39 · [Supply-chain attack](/concepts/supply-chain-attack.md)** — An attack that compromises a dependency release so malicious code runs inside applications that install it.
  - From: [Preflight: Agentic Last-Mile Verification for AI Software Supply Chains](/notes/security/2026-11-24-preflight-agentic-last-mile-verification-for-ai-software-sup.md)
  - Fix: 
- [ ] **C40 · [Zero-trust architecture](/concepts/zero-trust-architecture.md)** — Never trust a request by default; check every request wherever it arrives, regardless of network location.
  - From: [Agent Trust Infrastructure (ATI): Federated Identity and Attestation for Cross-Organization AI Agents](/notes/security/2026-11-18-agent-trust-infrastructure-ati-federated-identity-and-attest.md); [SoK: Assessing the Evolution of LLM Capability in Software Vulnerability Research](/notes/security/2026-11-20-sok-assessing-the-evolution-of-llm-capability-in-software-vu.md); [How Bee implemented Zero Operator Access for personal AI on AWS](/notes/security/2026-11-21-how-bee-implemented-zero-operator-access-for-personal-ai-on.md); +1 more
  - Fix: 

### Data (2)

- [ ] **C41 · [Change data capture](/concepts/change-data-capture.md)** — Capturing row-level database changes from the transaction log and streaming them to downstream consumers.
  - From: [How United Airlines streams database changes to Apache Iceberg with Amazon Data Firehose](/notes/data/2026-11-22-how-united-airlines-streams-database-changes-to-apache-icebe.md)
  - Fix: 
- [ ] **C42 · [Knowledge graph](/concepts/knowledge-graph.md)** — A graph of entities and typed relations, stored as triples that can be queried and reasoned over.
  - From: [Building an Agentic Knowledge Graph for Billions of Events](/notes/ai-ml/2026-11-18-building-an-agentic-knowledge-graph-for-billions-of-events.md); [10 AI Tools Everyone Will Be Using in Less Than Three Months](/notes/ai-ml/2026-11-20-10-ai-tools-everyone-will-be-using-in-less-than-three-months.md); [What is a semantic knowledge graph, and does your AI need one?](/notes/data/2026-11-22-what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.md)
  - Fix: 

### Cloud and infra (1)

- [ ] **C43 · [SLO error budget](/concepts/slo-error-budget.md)** — The allowed share of failures under an SLO, and the burn rate at which that allowance is consumed.
  - From: [Probing DNS at Cloudflare](/notes/cloud-infra/2026-11-24-probing-dns-at-cloudflare.md)
  - Fix: 

### Software engineering (1)

- [ ] **C44 · [Verification tax](/concepts/verification-tax.md)** — The extra time spent reviewing generated code as its volume rises.
  - From: [What developers and QA need from each other when AI writes the code](/notes/software-engineering/2026-11-21-what-developers-and-qa-need-from-each-other-when-ai-writes-t.md)
  - Fix: 

### Architecture (1)

- [ ] **C45 · [Modular monolith](/concepts/modular-monolith.md)** — One deployable application split into modules with enforced internal boundaries.
  - From: [Agoda Reduces API Code Review Time by 64% with Modular Architecture](/notes/architecture/2026-11-24-agoda-reduces-api-code-review-time-by-64-with-modular-archit.md)
  - Relations: uses `bounded-context`
  - Fix: 

## Part 2 — Draft foundations (10)

The golden rules, drafted by Claude Opus from the standards (baseline TOGAF 9.2). **None meets the evidence standard yet:** each needs quotes from two *different* issuing bodies, and TOGAF 9.2 and TOGAF 10 are both The Open Group, so they count as one. Approving a foundation here means "the wording is right". It becomes certified once its second source is added (unless you tell me to certify it anyway).

- [ ] **F01 · [Architecture](/foundations/architecture.md)** — An architecture is the set of fundamental concepts or properties of a system in its environment, embodied in its elements, their relationships, and the principles of its design and evolution.
  - Sources: ISO/IEC/IEEE 42010:2011 §3 (second-hand, via TOGAF 9.2 §3.7); TOGAF 9.2 Part I §3.7 Architecture; ISO/IEC/IEEE 42010:2022 §3 (second-hand, via TOGAF 10 §4.8); TOGAF 10 §4.8 Architecture
  - ⚠️ Evidence: partial. Bodies quoted: ISO/IEC/IEEE, The Open Group. Missing: ISO/IEC/IEEE 42010:2022 original text (paywalled at iso.org); the ISO quote is via TOGAF 10 until checked
  - Fix: 
- [ ] **F02 · [Enterprise](/foundations/enterprise.md)** — An enterprise is any collection of organizations that share common goals: a company, a division, a government department or a partnership.
  - Sources: TOGAF 9.2 Part I §3.38 Enterprise; §1.3 Executive Overview, "What is an enterprise?"; TOGAF 10 §4.43 Enterprise; §1.1 Executive Overview, "What is an Enterprise?"
  - ⚠️ Evidence: partial. Bodies quoted: The Open Group. Missing: A second body: e.g. ISO 15704 (enterprise modelling) or ArchiMate 3.2 glossary (same body, does not count); a peer-reviewed EA text such as Ross, Weill & Robertson, Enterprise Architecture as Strategy (2006)
  - Fix: 
- [ ] **F03 · [Enterprise Architecture](/foundations/enterprise-architecture.md)** — Enterprise Architecture is the architecture of an enterprise as a whole: it describes how business, data, applications and technology fit together across the enterprise, and it is used to steer change so that the enterprise stays integrated and supports its strategy.
  - Sources: TOGAF 9.2 Part I §1.3 Executive Overview, "Why is an Enterprise Architecture needed?"; TOGAF 10 §1.1 Executive Overview, "Why is an Enterprise Architecture Needed?"; TOGAF 10 §3 (Enterprise Agility), "Enterprise Architecture provides a framework for change"
  - ⚠️ Evidence: **no**. Bodies quoted: The Open Group. Missing: TOGAF 10 has no formal entry for Enterprise Architecture in its Definitions chapter (§4); it explains it in §1.1 instead / A second body: Gartner IT glossary (tier D, blocked from cloud; Kenji can open it), FEAF v2 (US federal, public), or Ross, Weill & Robertson (2006)
  - Fix: 
- [ ] **F04 · [Architecture domain](/foundations/architecture-domain.md)** — An architecture domain is one area of concern into which an enterprise architecture is divided. TOGAF has four primary ones (business, data, application, technology); other domains, such as security, may also be considered.
  - Sources: TOGAF 9.2 Part I §3.11 Architecture Domain; TOGAF 10 §4.12 Architecture Domain
  - ⚠️ Evidence: partial. Bodies quoted: The Open Group. Missing: A second body: ArchiMate 3.2 layers (same body), ISO/IEC/IEEE 42010:2022 (uses 'architecture aspects' instead), or Zachman Framework (columns/rows; tier D)
  - Fix: 
- [ ] **F05 · [Business Architecture](/foundations/business-architecture.md)** — Business architecture is a structured view of the business itself — what it can do (capabilities), how it delivers value end to end, what information it relies on and how it is organized — and of how these relate to its strategies, products, policies, initiatives and stakeholders.
  - Sources: TOGAF 9.2 Part I §3.24 Business Architecture; TOGAF 10 §4.27 Business Architecture
  - ⚠️ Evidence: partial. Bodies quoted: The Open Group. Missing: BIZBOK Guide v15 public glossary (Appendix A), Business Architecture Guild: Kenji downloads the PDF from businessarchitectureguild.org (cloud fetch is blocked) into FIAP-MBA/Referencias; a search snippet suggests the Guild's wording matches TOGAF's, but it is not verified
  - Fix: 
- [ ] **F06 · [Data Architecture](/foundations/data-architecture.md)** — Data architecture describes the structure and interaction of the enterprise's major types and sources of data, its logical and physical data assets, and the resources that manage them.
  - Sources: TOGAF 9.2 Part I §3.36 Data Architecture; TOGAF 10 §4.39 Data Architecture
  - ⚠️ Evidence: partial. Bodies quoted: The Open Group. Missing: DAMA-DMBOK2 (DAMA International), chapter on Data Architecture — book, Kenji to provide / Data Architecture course slides (FIAP, Aula02–04) as tier E
  - Fix: 
- [ ] **F07 · [Application Architecture](/foundations/application-architecture.md)** — Application architecture describes the structure and interaction of the enterprise's applications, seen as groups of capabilities that provide key business functions and manage the data assets.
  - Sources: TOGAF 9.2 Part I §3.3 Application Architecture; TOGAF 10 §4.3 Application Architecture
  - ⚠️ Evidence: partial. Bodies quoted: The Open Group. Missing: A second body: ISO/IEC/IEEE 42010 (no domain-specific definition), or a textbook such as Fowler, Patterns of Enterprise Application Architecture (2002, tier C, in Materiais)
  - Fix: 
- [ ] **F08 · [Technology Architecture](/foundations/technology-architecture.md)** — Technology architecture describes the technology services (such as hosting, networking, storage, platforms) and the technology components that provide them, and how they interact.
  - Sources: TOGAF 9.2 Part I §3.77 Technology Architecture; TOGAF 10 §4.81 Technology Architecture
  - ⚠️ Evidence: partial. Bodies quoted: The Open Group. Missing: A second body, e.g. ArchiMate 3.2 Technology Layer (same body), or NIST SP 500-292 Cloud Computing Reference Architecture (tier A/B, public) for cloud technology services
  - Fix: 
- [ ] **F09 · [Solution Architecture](/foundations/solution-architecture.md)** — Solution architecture describes one focused business operation or change — usually one project or release — and how IT supports it, translating requirements into a solution vision, high-level specifications and implementation tasks. A *solution* is the combination of business and IT elements delivered to meet that need.
  - Sources: TOGAF 9.2 Part I §3.69 Solution Architecture; TOGAF 10 §4.73 Solution Architecture; §4.78 Strategic Architecture; §4.68 Segment Architecture; Appendix B.41 System
  - ⚠️ Evidence: partial. Bodies quoted: The Open Group. Missing: A second body. TOGAF 10 has no definition of 'solution' itself; candidates: ISO/IEC/IEEE 15288 (system, via TOGAF B.41), or a tier C source on solution architecture
  - Fix: 
- [ ] **F10 · [Business capability](/foundations/business-capability.md)** — A business capability is something the business is able to do to achieve a specific purpose (for example *manage customer orders*), described independently of the organization, processes and systems that deliver it.
  - Sources: TOGAF 9.2 Part I §3.25 Business Capability; §3.30 Capability; TOGAF 10 §4.28 Business Capability; §4.33 Capability
  - ⚠️ Evidence: partial. Bodies quoted: The Open Group. Missing: BIZBOK Guide v15 public glossary (Appendix A), Business Architecture Guild: Kenji downloads the PDF from businessarchitectureguild.org (cloud fetch is blocked) into FIAP-MBA/Referencias / TOGAF Series Guide G211 Business Capabilities v2 (same body; useful for the 'what not how' principle)
  - Fix: 

