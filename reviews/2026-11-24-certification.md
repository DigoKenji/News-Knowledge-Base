---
type: Review Checklist
title: Certification checklist — 2026-11-24
description: Every draft concept (45) and draft foundation (10) in one list, for Kenji to approve or fix in one pass.
status: closed
generated:
  by: claude (project thread, from the repo state at be3ddae)
  at: '2026-11-24T19:45:00-03:00'
---

# Certification checklist — 2026-11-24

Everything in the KB that is still `status: draft`: **45 concepts** and **10 foundations**. Nothing here has been certified by Claude; only you certify.

**Updated 2026-11-24 (evening):** at your request the 45 concepts were rewritten from one-liners into full entries, each grounded in 3 or more independent sources (papers, standards, official docs, books). Every link was checked: all 61 arXiv papers match their cited title and first author, and 3 sites that block automated checks (Netflix Tech Blog on Medium, CISA, the Stanford IR book) are well-known pages.

## Outcome

**2026-11-24:** Kenji approved all 45 concepts (C01–C45) in the project thread; they are now `stable` with `verified: Kenji, 2026-11-24`. The 10 foundations (F01–F10) stay `draft` until each has a second independent source.

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

Vocabulary entries the daily run proposed from the articles it summarized, expanded on 2026-11-24 into full entries (definition, how it works, trade-offs, example) backed by 3 or more independent sources each. The line below is the summary; click the title to read the full entry. ⚠️ marks entries where I changed the scope of the original draft or where the term itself is shaky.

### AI/ML (22)

- [x] **C01 · [Agent harness](/concepts/agent-harness.md)** — The software around a language model that turns it into an agent - it builds the context, exposes tools, runs the loop, enforces rules and keeps state between steps.
  - Seen in: [Self-Improvement Memory Enables Continual Recursive Self-Improvement](/notes/ai-ml/2026-11-17-self-improvement-memory-enables-continual-recursive-self-imp.md); [Building an Agentic Knowledge Graph for Billions of Events](/notes/ai-ml/2026-11-18-building-an-agentic-knowledge-graph-for-billions-of-events.md); [Introducing Claude Fable 5.2 and Claude Mythos 5.2](/notes/ai-ml/2026-11-18-introducing-claude-fable-5-2-and-claude-mythos-5-2.md); +6 more
  - Sources: 3 (2 practitioner, 1 paper)
  - Fix: 
- [x] **C02 · [Agent memory](/concepts/agent-memory.md)** — Information an AI agent stores outside the model and retrieves in later steps or sessions - facts, past events and learned procedures - because the context window alone forgets everything between calls.
  - Seen in: [MemFolder: Structure-Guided Memory Folding for Efficient Agent Memory Management](/notes/ai-ml/2026-11-21-memfolder-structure-guided-memory-folding-for-efficient-agen.md); [Backtracking Agent Applications Security Incidents](/notes/security/2026-11-22-backtracking-agent-applications-security-incidents.md)
  - Sources: 3 (3 paper)
  - Relations: part-of `ai-agent`
  - Fix: 
- [x] **C03 · [AI agent](/concepts/ai-agent.md)** — A system in which a language model decides, step by step, which actions to take with tools to reach a goal, observing results and adapting, with limited human supervision.
  - Seen in: [You Think AI Is Overhyped? Just Wait Until You See What Comes Next.](/notes/ai-ml/2026-11-20-you-think-ai-is-overhyped-just-wait-until-you-see-what-comes.md); [A Capability-First Approach to Modernizing Retirement Services Technology Platforms](/notes/architecture/2026-11-21-a-capability-first-approach-to-modernizing-retirement-servic.md); [Fragments: November 20](/notes/software-engineering/2026-11-21-fragments-november-20.md); +9 more
  - Sources: 4 (1 book, 2 paper, 1 practitioner)
  - Fix: 
- [x] **C04 · [AI gateway](/concepts/ai-gateway.md)** — A shared proxy between applications and LLM providers that centralizes routing, authentication, cost tracking, rate limits, caching, logging and policy for every model call.
  - Seen in: [How AUMOVIO centralized AI access with Databricks Unity Gateway](/notes/architecture/2026-11-19-how-aumovio-centralized-ai-access-with-databricks-unity-gate.md)
  - Sources: 3 (3 docs)
  - ⚠️ Note: The term is vendor-driven and has no standards-body definition yet; the three sources are independent vendors that agree on the feature set.
  - Fix: 
- [x] **C05 · [Calibration](/concepts/calibration.md)** — How well a model's stated confidence matches its actual accuracy - of all answers given with 80% confidence, a calibrated model is right about 80% of the time.
  - Seen in: [$0.0001 Decision Model That Will Replace Millions of LLM Calls](/notes/ai-ml/2026-11-17-0-0001-decision-model-that-will-replace-millions-of-llm-call.md); [I Tested TypeSafe's Jev, a 478× Cheaper Decision Model, Against Claude, GPT-6, Kimi, MiniMax and DeepSeek](/notes/ai-ml/2026-11-17-i-tested-typesafe-s-jev-a-478-cheaper-decision-model-against.md); [Jev killed LLM-as-a-Judge?](/notes/ai-ml/2026-11-17-jev-killed-llm-as-a-judge.md); +2 more
  - Sources: 3 (2 paper, 1 docs)
  - Fix: 
- [x] **C06 · [Context engineering](/concepts/context-engineering.md)** — Designing and managing everything a model sees at each call - instructions, retrieved documents, memory, tool definitions and results, history - so it has exactly the information it needs, in a form it can use, within a finite window.
  - Seen in: [Netflix at QConSF 2026: Retrofitting the SDLC for AI Coding Agents](/notes/software-engineering/2026-11-24-netflix-at-qconsf-2026-retrofitting-the-sdlc-for-ai-coding-a.md)
  - Sources: 4 (2 practitioner, 2 paper)
  - ⚠️ Note: The draft definition was tied to coding agents; it now covers any LLM application.
  - Relations: part-of `ai-agent`
  - Fix: 
- [x] **C07 · [Distillation](/concepts/distillation.md)** — Training a smaller or cheaper student model to reproduce the outputs of a larger teacher model, so the student keeps much of the teacher's ability at a fraction of the cost.
  - Seen in: [Introducing Claude Fable 5.2 and Claude Mythos 5.2](/notes/ai-ml/2026-11-18-introducing-claude-fable-5-2-and-claude-mythos-5-2.md); [Combatting adversarial distillation](/notes/security/2026-11-21-combatting-adversarial-distillation.md)
  - Sources: 4 (4 paper)
  - ⚠️ Note: The draft ("extract capabilities") described the adversarial case only; the definition is now the general technique, with the adversarial use as a caveat.
  - Fix: 
- [x] **C08 · [Goodput](/concepts/goodput.md)** — The rate of useful work a system delivers - requests (or tokens, or training steps) that complete correctly and within their latency target - as opposed to raw throughput, which also counts work that was late, failed or repeated.
  - Seen in: [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
  - Sources: 3 (1 standard, 1 paper, 1 practitioner)
  - ⚠️ Note: The draft limited goodput to "requests that finish within their timeout"; it is now the general idea with the networking origin and the LLM-serving definition.
  - Fix: 
- [x] **C09 · [In-context learning](/concepts/in-context-learning.md)** — A language model's ability to pick up a task from instructions or examples placed in its prompt and perform it on new inputs, without any change to its weights.
  - Seen in: [AI on Excel Sheet: NVIDIA's Kumo Runs 23x Faster Than Google's TabFM](/notes/data/2026-11-20-ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-googles.md)
  - Sources: 4 (4 paper)
  - Fix: 
- [x] **C10 · [Joint-embedding predictive architecture (JEPA)](/concepts/jepa.md)** — Joint-Embedding Predictive Architecture - a self-supervised design that learns by predicting the abstract representation of a hidden part of the input from a visible part, instead of reconstructing raw pixels or tokens.
  - Seen in: [Two JEPAs, one world model: the Platonic Representation Hypothesis](/notes/ai-ml/2026-11-16-platonic-representation-jepa.md)
  - Sources: 3 (3 paper)
  - ⚠️ Note: All three sources come from Meta researchers; independent replication exists but is less prominent. Good enough for a concept, not for a foundation.
  - Fix: 
- [x] **C11 · [LLM-as-a-judge](/concepts/llm-as-a-judge.md)** — Using a strong language model, with a rubric, to score or compare the outputs of another model - a scalable stand-in for human evaluation that must itself be validated against human judgments.
  - Seen in: [Building strong safety evaluations](/notes/ai-ml/2026-11-16-building-strong-safety-evaluations.md); [LLM-as-a-Judge has a scoring problem (JevEval)](/notes/ai-ml/2026-11-16-jeveval-llm-as-a-judge.md); [Do Verifier Judgments Survive Abstraction? Measuring Verification Transport for LLM Agent Evaluation](/notes/ai-ml/2026-11-16-verifier-judgments-survive-abstraction.md); +12 more
  - Sources: 3 (3 paper)
  - Fix: 
- [x] **C12 · [Low-rank adaptation (LoRA)](/concepts/lora.md)** — Low-Rank Adaptation - a parameter-efficient fine-tuning method that freezes a model's weights and trains small low-rank matrices added to some layers, cutting trainable parameters by orders of magnitude with little quality loss.
  - Seen in: [LiveLoRA: Exact KV Cache Migration Across LoRA Updates for LLM Agents](/notes/ai-ml/2026-11-24-livelora-exact-kv-cache-migration-across-lora-updates-for-ll.md)
  - Sources: 4 (3 paper, 1 docs)
  - Fix: 
- [x] **C13 · [Mixture of experts](/concepts/mixture-of-experts.md)** — A neural-network layer made of many expert sub-networks plus a router that activates only a few experts per token, so total parameters grow without a matching growth in compute per token.
  - Seen in: [The US-China Open-Weight Feedback Loop](/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md)
  - Sources: 4 (4 paper)
  - Fix: 
- [x] **C14 · [Model Context Protocol (MCP)](/concepts/model-context-protocol.md)** — An open protocol (JSON-RPC based) that standardises how AI applications connect to external tools, data sources and prompts, so any compatible client can use any compatible server.
  - Seen in: [10 MCP Servers Every Local AI Developer Should Install](/notes/ai-ml/2026-11-17-10-mcp-servers-every-local-ai-developer-should-install.md); [GTIG AI Threat Tracker: Agentic Threat Activity Crosses a Significant Threshold](/notes/security/2026-11-17-gtig-ai-threat-tracker-agentic-threat-activity-crosses-a-sig.md); [How Rakbank built centralized observability at scale with Amazon OpenSearch Service](/notes/cloud-infra/2026-11-17-how-rakbank-built-centralized-observability-at-scale-with-am.md); +13 more
  - Sources: 3 (1 standard, 1 practitioner, 1 paper)
  - Fix: 
- [x] **C15 · [Multi-head latent attention](/concepts/multi-head-latent-attention.md)** — An attention variant introduced in DeepSeek-V2 that compresses each token's keys and values into one small shared latent vector, shrinking the KV cache several-fold while keeping quality close to standard multi-head attention.
  - Seen in: [The US-China Open-Weight Feedback Loop](/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md)
  - Sources: 4 (4 paper)
  - Fix: 
- [x] **C16 · [Parent-child chunking](/concepts/parent-child-chunking.md)** — A RAG retrieval pattern that indexes small child chunks for precise matching but returns the larger parent section they belong to, so the model gets enough surrounding context.
  - Seen in: [How to Architect Semantic Chunking Pipelines for High-Performance RAG](/notes/ai-ml/2026-11-17-how-to-architect-semantic-chunking-pipelines-for-high-perfor.md)
  - Sources: 3 (2 docs, 1 paper)
  - ⚠️ Note: This is an engineering pattern documented by frameworks, not a research result; the sources are two independent framework docs plus the original RAG paper for the underlying retrieval idea.
  - Relations: part-of `rag`
  - Fix: 
- [x] **C17 · [Position bias](/concepts/position-bias.md)** — A systematic tendency of a model, especially when acting as a judge or ranker, to favour an option because of where it appears (first, last, or in a given slot) rather than because of its content.
  - Seen in: [Jev killed LLM-as-a-Judge?](/notes/ai-ml/2026-11-17-jev-killed-llm-as-a-judge.md)
  - Sources: 3 (3 paper)
  - Fix: 
- [x] **C18 · [Prefill-decode disaggregation](/concepts/prefill-decode-disaggregation.md)** — Running the two phases of LLM inference - prefill (processing the prompt) and decode (generating tokens one by one) - on separate GPU pools, transferring the KV cache between them, so each can be sized and tuned for its own bottleneck.
  - Seen in: [Callo: Serving Cache-Miss Requests with Speculative Decoding on Invalidated KV under Prefill-Decode Disaggregation](/notes/ai-ml/2026-11-16-callo-speculative-decoding-invalidated-kv.md); [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
  - Sources: 4 (3 paper, 1 docs)
  - Relations: uses `kv-cache`, part-of `inference-tradeoffs`
  - Fix: 
- [x] **C19 · [Rotary position embedding (RoPE)](/concepts/rope.md)** — Rotary Position Embedding - a way to encode token positions by rotating query and key vectors by position-dependent angles, so attention scores depend on the relative distance between tokens.
  - Seen in: [Understanding Positional Embeddings in Modern LLMs](/notes/ai-ml/2026-11-16-understanding-positional-embeddings.md); [The US-China Open-Weight Feedback Loop](/notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md); [LiveLoRA: Exact KV Cache Migration Across LoRA Updates for LLM Agents](/notes/ai-ml/2026-11-24-livelora-exact-kv-cache-migration-across-lora-updates-for-ll.md)
  - Sources: 4 (3 paper, 1 practitioner)
  - Fix: 
- [x] **C20 · [Semantic search](/concepts/semantic-search.md)** — Search that matches the meaning of a query rather than its exact words, usually by turning queries and documents into embedding vectors and returning the nearest ones.
  - Seen in: [txtai: an all-in-one open-source framework for semantic search, RAG and agents](/notes/ai-ml/2026-11-17-txtai-an-all-in-one-open-source-framework-for-semantic-searc.md); [10 AI Tools Everyone Will Be Using in Less Than Three Months](/notes/ai-ml/2026-11-20-10-ai-tools-everyone-will-be-using-in-less-than-three-months.md)
  - Sources: 4 (3 paper, 1 book)
  - Fix: 
- [x] **C21 · [Speculative decoding](/concepts/speculative-decoding.md)** — A lossless way to speed up LLM generation - a fast draft model guesses several next tokens and the large model verifies them all in one forward pass, keeping the ones it agrees with.
  - Seen in: [Callo: Serving Cache-Miss Requests with Speculative Decoding on Invalidated KV under Prefill-Decode Disaggregation](/notes/ai-ml/2026-11-16-callo-speculative-decoding-invalidated-kv.md)
  - Sources: 3 (3 paper)
  - Relations: part-of `inference-tradeoffs`
  - Fix: 
- [x] **C22 · [Structured decision model](/concepts/structured-decision-model.md)** — A model used to make a decision from a fixed set of typed outputs - a label, a score, a yes/no, a probability - rather than to write free text; in practice a classifier or a constrained-output LLM call.
  - Seen in: [I Tested TypeSafe's Jev, a 478× Cheaper Decision Model, Against Claude, GPT-6, Kimi, MiniMax and DeepSeek](/notes/ai-ml/2026-11-17-i-tested-typesafe-s-jev-a-478-cheaper-decision-model-against.md); [Jev AI Explained: The New Model Making AI Decisions Ridiculously Cheap](/notes/ai-ml/2026-11-17-jev-ai-explained-the-new-model-making-ai-decisions-ridiculou.md); [Jev vs LLMs: Do AI Agents Really Need a Language Model for Every Decision?](/notes/ai-ml/2026-11-17-jev-vs-llms-do-ai-agents-really-need-a-language-model-for-ev.md); +2 more
  - Sources: 3 (1 paper, 2 docs)
  - ⚠️ Note: "Structured decision model" is not an established term; it comes from vendor articles (TypeSafe Jev). Consider rejecting it, or renaming to "structured outputs" or "classifier instead of LLM call". The definition above describes the real underlying idea.
  - Fix: 

### Backend systems (12)

- [x] **C23 · [Adaptive concurrency limit](/concepts/adaptive-concurrency-limit.md)** — A limit on how many requests a service handles at once that adjusts itself from observed latency or errors - shrinking quickly when the service shows congestion and growing back cautiously when it recovers - instead of a fixed, hand-tuned number.
  - Seen in: [How we raised Databricks cluster rate limits by 7.5×](/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md); [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
  - Sources: 3 (1 paper, 1 practitioner, 1 docs)
  - Fix: 
- [x] **C24 · [Cascading failure](/concepts/cascading-failure.md)** — A failure that spreads because the reaction to one component failing - extra load, retries, failover traffic - overloads other components, which then fail and pass their load on in turn.
  - Seen in: [Microsoft outage: A cascading failure that started with an undetected bug](/notes/cloud-infra/2026-11-22-microsoft-outage-a-cascading-failure-that-started-with-an-un.md); [The FAA outage is a warning about technical debt in the AI era](/notes/software-engineering/2026-11-24-the-faa-outage-is-a-warning-about-technical-debt-in-the-ai-e.md)
  - Sources: 3 (1 book, 1 paper, 1 practitioner)
  - Fix: 
- [x] **C25 · [Chaos engineering](/concepts/chaos-engineering.md)** — The practice of running controlled experiments that inject real faults into a system (often in production) to test a hypothesis about how it keeps working, so weaknesses are found before they cause outages.
  - Seen in: [Resilience testing your agents on Amazon Bedrock AgentCore](/notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md); [Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover](/notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md)
  - Sources: 3 (1 practitioner, 1 paper, 1 docs)
  - Fix: 
- [x] **C26 · [Control plane](/concepts/control-plane.md)** — The part of a system that decides and configures what should happen - placement, routing rules, desired state, scheduling - as opposed to the data plane, which does the actual work on each request or packet.
  - Seen in: [OpenAI Just Open-Sourced Symphony. Every AI Startup Should Pay Attention](/notes/ai-ml/2026-11-20-openai-just-open-sourced-symphony-every-ai-startup-should-pa.md); [How Google Traced GKE API Timeouts to a Linux Kernel TCP Bug](/notes/cloud-infra/2026-11-21-how-google-traced-gke-api-timeouts-to-a-linux-kernel-tcp-bug.md); [Probing DNS at Cloudflare](/notes/cloud-infra/2026-11-24-probing-dns-at-cloudflare.md)
  - Sources: 3 (1 standard, 1 docs, 1 practitioner)
  - ⚠️ Note: The draft narrowed this to job orchestration ("decides what work runs, when"); it is now the general control/data-plane split, of which job scheduling is one case.
  - Fix: 
- [x] **C27 · [Correlated failure](/concepts/correlated-failure.md)** — Several components that were meant to fail independently fail together because they share a hidden cause - a dependency, a configuration push, a software bug, a power or network domain - which defeats redundancy designed on the assumption of independence.
  - Seen in: [Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover](/notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md); [Microsoft outage: A cascading failure that started with an undetected bug](/notes/cloud-infra/2026-11-22-microsoft-outage-a-cascading-failure-that-started-with-an-un.md); [More control, fewer dependencies: Why DORA is forcing a connectivity rethink](/notes/architecture/2026-11-24-more-control-fewer-dependencies-why-dora-is-forcing-a-connec.md)
  - Sources: 3 (1 paper, 2 practitioner)
  - Fix: 
- [x] **C28 · [Dead-letter queue](/concepts/dead-letter-queue.md)** — A separate queue where a messaging system puts messages it could not deliver or process after a set number of attempts, so they stop blocking or looping in the main queue and can be inspected, fixed and replayed.
  - Seen in: [How Country Road Group built a pattern-driven integration solution on AWS](/notes/architecture/2026-11-20-how-country-road-group-built-a-pattern-driven-integration-so.md); [How United Airlines streams database changes to Apache Iceberg with Amazon Data Firehose](/notes/data/2026-11-22-how-united-airlines-streams-database-changes-to-apache-icebe.md)
  - Sources: 3 (1 book, 2 docs)
  - Fix: 
- [x] **C29 · [Deterministic simulation testing](/concepts/deterministic-simulation-testing.md)** — Testing a distributed system by running all of its nodes in one process against simulated time, network, disk and randomness controlled by a seed, injecting faults, so any failure can be replayed exactly.
  - Seen in: [Deterministic Simulation Testing](/notes/backend-systems/2026-11-17-deterministic-simulation-testing.md)
  - Sources: 3 (1 paper, 1 docs, 1 practitioner)
  - Fix: 
- [x] **C30 · [GC pressure](/concepts/gc-pressure.md)** — The load a program puts on its garbage collector - mainly through allocation rate and the size and pointer-density of the live heap - which shows up as CPU spent collecting, longer or more frequent pauses, and higher tail latency.
  - Seen in: [Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS](/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md)
  - Sources: 3 (1 book, 2 docs)
  - ⚠️ Note: The draft defined it only as "CPU cost of GC"; it now covers allocation rate, live heap and pause latency.
  - Fix: 
- [x] **C31 · [Hybrid memory-disk cache](/concepts/hybrid-memory-disk-cache.md)** — A cache that keeps its index and hottest items in RAM and the larger, colder part on SSD, so it holds far more data per server than memory alone at a cost of slower reads for items that live on disk.
  - Seen in: [Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS](/notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md)
  - Sources: 3 (2 paper, 1 docs)
  - Fix: 
- [x] **C32 · [Linearizability](/concepts/linearizability.md)** — The strongest single-object consistency model - every operation appears to happen instantly at one point between its start and its end, so all clients see one up-to-date order of operations consistent with real time.
  - Seen in: [Deterministic Simulation Testing](/notes/backend-systems/2026-11-17-deterministic-simulation-testing.md)
  - Sources: 4 (1 paper, 1 book, 2 practitioner)
  - Fix: 
- [x] **C33 · [Retry storm](/concepts/retry-storm.md)** — A surge of retried requests that hits a struggling dependency all at once - often multiplied across layers - adding load exactly when it can least cope and keeping it overloaded after the original problem is gone.
  - Seen in: [How we raised Databricks cluster rate limits by 7.5×](/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md); [Cap-and-Track: Optimizing LLM Inference Request Dispatch Across Shifting Capacity](/notes/backend-systems/2026-11-19-cap-and-track-optimizing-llm-inference-request-dispatch-acro.md)
  - Sources: 4 (1 practitioner, 1 book, 1 standard, 1 paper)
  - ⚠️ Note: The draft wording ("retries from accepted work") was unclear; rewritten around the standard explanation.
  - Fix: 
- [x] **C34 · [Silent failure](/concepts/silent-failure.md)** — A failure that produces no error - the system returns output that looks normal but is wrong, incomplete or stale - so monitoring based on errors and uptime does not see it and damage accumulates until someone notices the results.
  - Seen in: [Resilience testing your agents on Amazon Bedrock AgentCore](/notes/ai-ml/2026-11-19-resilience-testing-your-agents-on-amazon-bedrock-agentcore.md); [Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover](/notes/cloud-infra/2026-11-20-building-reliability-into-the-cloud-observability-as-the-con.md); [Microsoft outage: A cascading failure that started with an undetected bug](/notes/cloud-infra/2026-11-22-microsoft-outage-a-cascading-failure-that-started-with-an-un.md)
  - Sources: 4 (3 paper, 1 book)
  - Fix: 

### Security (6)

- [x] **C35 · [Indirect prompt injection](/concepts/indirect-prompt-injection.md)** — An attack in which instructions hidden in content an AI system reads - a web page, email, document, tool result or code comment - are treated by the model as commands, letting a third party who never talks to the system steer what it does.
  - Seen in: [Safe Resemblance, Silent Propagation: Limits of Text-Similarity Guardrails for AI Agent Safety](/notes/security/2026-11-17-safe-resemblance-silent-propagation-limits-of-text-similarit.md); [Agent Trust Infrastructure (ATI): Federated Identity and Attestation for Cross-Organization AI Agents](/notes/security/2026-11-18-agent-trust-infrastructure-ati-federated-identity-and-attest.md); [Kaer: Security-Aware Replanning for Multi-Agent Systems](/notes/security/2026-11-19-kaer-security-aware-replanning-for-multi-agent-systems.md); +2 more
  - Sources: 4 (2 paper, 2 standard)
  - Fix: 
- [x] **C36 · [Information flow control](/concepts/information-flow-control.md)** — A security approach that labels data by its confidentiality or integrity (for example "secret" or "untrusted") and tracks those labels as data moves through a program, blocking flows that would violate a policy - such as untrusted input deciding a privileged action.
  - Seen in: [Kaer: Security-Aware Replanning for Multi-Agent Systems](/notes/security/2026-11-19-kaer-security-aware-replanning-for-multi-agent-systems.md); [Replication of Agentic Security Events: Audit and Investigation (ROGUE-AI)](/notes/security/2026-11-21-replication-of-agentic-security-events-audit-and-investigati.md); [Backtracking Agent Applications Security Incidents](/notes/security/2026-11-22-backtracking-agent-applications-security-incidents.md)
  - Sources: 4 (4 paper)
  - Relations: mitigates `indirect-prompt-injection`
  - Fix: 
- [x] **C37 · [Memory poisoning](/concepts/memory-poisoning.md)** — An attack that plants false or malicious entries in an AI agent's long-term memory or knowledge store, so that the agent later retrieves them as trusted context and acts on them, possibly long after and for other users.
  - Seen in: [Backtracking Agent Applications Security Incidents](/notes/security/2026-11-22-backtracking-agent-applications-security-incidents.md)
  - Sources: 4 (1 standard, 3 paper)
  - Fix: 
- [x] **C38 · [Remote attestation](/concepts/remote-attestation.md)** — A protocol in which a machine proves to a remote verifier, with evidence signed by hardware it cannot forge, exactly what software and configuration it is running, so the verifier can decide whether to trust it before sending secrets or work.
  - Seen in: [How Bee implemented Zero Operator Access for personal AI on AWS](/notes/security/2026-11-21-how-bee-implemented-zero-operator-access-for-personal-ai-on.md)
  - Sources: 3 (1 standard, 1 paper, 1 docs)
  - Fix: 
- [x] **C39 · [Supply-chain attack](/concepts/supply-chain-attack.md)** — An attack that compromises something a target depends on - a third-party library, package registry, build system, update channel, vendor or developer tool - so the malicious code or access reaches the target through a trusted path instead of a direct attack.
  - Seen in: [Preflight: Agentic Last-Mile Verification for AI Software Supply Chains](/notes/security/2026-11-24-preflight-agentic-last-mile-verification-for-ai-software-sup.md)
  - Sources: 4 (3 standard, 1 paper)
  - ⚠️ Note: The draft limited this to "a dependency release"; it now covers build systems, update channels and vendors too, matching NIST and MITRE.
  - Fix: 
- [x] **C40 · [Zero-trust architecture](/concepts/zero-trust-architecture.md)** — A security model that grants no implicit trust based on network location - every request to a resource is authenticated, authorised and evaluated against policy (identity, device health, context) at the time it is made, with least-privilege access.
  - Seen in: [Agent Trust Infrastructure (ATI): Federated Identity and Attestation for Cross-Organization AI Agents](/notes/security/2026-11-18-agent-trust-infrastructure-ati-federated-identity-and-attest.md); [SoK: Assessing the Evolution of LLM Capability in Software Vulnerability Research](/notes/security/2026-11-20-sok-assessing-the-evolution-of-llm-capability-in-software-vu.md); [How Bee implemented Zero Operator Access for personal AI on AWS](/notes/security/2026-11-21-how-bee-implemented-zero-operator-access-for-personal-ai-on.md); +1 more
  - Sources: 3 (2 standard, 1 paper)
  - Fix: 

### Data (2)

- [x] **C41 · [Change data capture](/concepts/change-data-capture.md)** — Capturing every insert, update and delete committed in a database - usually by reading its transaction log - and publishing them as an ordered stream of change events for other systems to consume.
  - Seen in: [How United Airlines streams database changes to Apache Iceberg with Amazon Data Firehose](/notes/data/2026-11-22-how-united-airlines-streams-database-changes-to-apache-icebe.md)
  - Sources: 3 (1 book, 1 paper, 1 docs)
  - Fix: 
- [x] **C42 · [Knowledge graph](/concepts/knowledge-graph.md)** — A graph that represents knowledge as entities (nodes) connected by typed, meaningful relations (edges), often with a schema or ontology, so that facts can be queried, linked across sources and reasoned over.
  - Seen in: [Building an Agentic Knowledge Graph for Billions of Events](/notes/ai-ml/2026-11-18-building-an-agentic-knowledge-graph-for-billions-of-events.md); [10 AI Tools Everyone Will Be Using in Less Than Three Months](/notes/ai-ml/2026-11-20-10-ai-tools-everyone-will-be-using-in-less-than-three-months.md); [What is a semantic knowledge graph, and does your AI need one?](/notes/data/2026-11-22-what-is-a-semantic-knowledge-graph-and-does-your-ai-need-one.md)
  - Sources: 3 (1 paper, 1 standard, 1 practitioner)
  - ⚠️ Note: The draft said knowledge graphs are "stored as triples"; that is only the RDF model. The definition now covers property graphs too.
  - Fix: 

### Cloud and infra (1)

- [x] **C43 · [SLO error budget](/concepts/slo-error-budget.md)** — An SLO is a target for a service's reliability measured by an indicator (for example 99.9% of requests succeed over 30 days); the error budget is the failure that target still allows (0.1%), which teams spend on releases and risk and protect when it runs low.
  - Seen in: [Probing DNS at Cloudflare](/notes/cloud-infra/2026-11-24-probing-dns-at-cloudflare.md)
  - Sources: 4 (3 book, 1 standard)
  - ⚠️ Note: The draft covered only the budget and burn rate; the entry now defines SLI and SLO too. Three of four sources are Google's SRE books; OpenSLO is the independent one.
  - Fix: 

### Software engineering (1)

- [x] **C44 · [Verification tax](/concepts/verification-tax.md)** — The growing share of engineering time spent reviewing, testing and correcting AI-generated code, which can cancel out the speed gained by generating it - a recent, informal term rather than an established one.
  - Seen in: [What developers and QA need from each other when AI writes the code](/notes/software-engineering/2026-11-21-what-developers-and-qa-need-from-each-other-when-ai-writes-t.md)
  - Sources: 3 (1 paper, 2 practitioner)
  - ⚠️ Note: "Verification tax" is not an established term; no standard or peer-reviewed source defines it. The sources support the underlying effect. Consider renaming to something more standard (for example "AI code review burden") or keeping it with this caveat.
  - Fix: 

### Architecture (1)

- [x] **C45 · [Modular monolith](/concepts/modular-monolith.md)** — An application deployed as a single unit but internally divided into modules with strong, enforced boundaries - each owning its data and exposing an explicit interface - giving much of the structure of microservices without the cost of a distributed system.
  - Seen in: [Agoda Reduces API Code Review Time by 64% with Modular Architecture](/notes/architecture/2026-11-24-agoda-reduces-api-code-review-time-by-64-with-modular-archit.md)
  - Sources: 4 (1 paper, 2 practitioner, 1 docs)
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

