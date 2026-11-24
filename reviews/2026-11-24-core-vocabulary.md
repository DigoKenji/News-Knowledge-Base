---
type: Candidate List
title: Core vocabulary — seed list
description: About 70 core terms per domain (what AI, ML, a distributed system… are) to be written top-down as short entries, with existing concepts linked under them. Approved by Kenji 2026-11-24.
status: approved
generated:
  by: claude (project thread)
  at: '2026-11-24T20:15:00-03:00'
---

# Core vocabulary — seed list

**Status:** approved by Kenji on 2026-11-24. Entries are written in batches (AI/ML and Security first): short definition, how it fits, 2–3 sources, EN + pt-br. Terms marked **F** go to `foundations/` when two independent bodies define them; the rest go to `concepts/`.

**Legend**
- **F** = candidate *foundation*: a standards body or recognised reference defines it, so it can meet the two-independent-sources rule (ADR-0010).
- **C** = short *concept* with sources.
- *Under it:* existing KB concepts that will link up to this term (`is-a` / `part-of`).

About 70 terms, 8–12 per domain, short entries.

---

## AI/ML (12)

| # | Term | Type | Main references | Under it |
|---|---|---|---|---|
| A1 | Artificial intelligence | F | ISO/IEC 22989, OECD AI definition, NIST AI RMF | everything in AI/ML |
| A2 | Machine learning | F | ISO/IEC 22989, ISO/IEC 23053, Mitchell (1997) | A3–A5 |
| A3 | Supervised learning | F | ISO/IEC 22989, ISO/IEC 23053 | svm, calibration |
| A4 | Unsupervised / self-supervised learning | F | ISO/IEC 22989, LeCun | jepa, semantic-search (embeddings) |
| A5 | Reinforcement learning | F | ISO/IEC 22989, Sutton & Barto | (RLHF later) |
| A6 | Neural network and deep learning | F | ISO/IEC 22989, Goodfellow et al. *Deep Learning* | rbf-network, mixture-of-experts |
| A7 | Training vs inference | C | ISO/IEC 23053, Goodfellow | inference-tradeoffs, kv-cache, speculative-decoding, prefill-decode-disaggregation |
| A8 | Overfitting and generalisation | C | Goodfellow, Hastie et al. *ESL* | concept-drift, data-drift |
| A9 | Model evaluation and metrics | C | ISO/IEC 23053, scikit-learn docs | llm-as-a-judge, position-bias, calibration |
| A10 | Transformer and attention | C | Vaswani et al. (2017) | rope, multi-head-latent-attention |
| A11 | Large language model (LLM) and foundation model | C | Bommasani et al. (2021), NIST AI 600-1 | in-context-learning, lora, distillation, rag |
| A12 | Embedding | C | Mikolov et al., Sentence-BERT | semantic-search, rag, graph-rag |

## Software engineering (10)

| # | Term | Type | Main references | Under it |
|---|---|---|---|---|
| S1 | Software engineering | F | ISO/IEC/IEEE 24765, SWEBOK v4 | all of it |
| S2 | Software development life cycle (SDLC) | F | ISO/IEC/IEEE 12207, SWEBOK | verification-tax |
| S3 | Requirements (functional / non-functional, quality attributes) | F | ISO/IEC 25010, SWEBOK | atam |
| S4 | Software testing (unit, integration, end-to-end) | F | ISO/IEC/IEEE 29119, SWEBOK | deterministic-simulation-testing |
| S5 | Technical debt | C | Cunningham (1992), Kruchten et al. | strangler-fig |
| S6 | Version control and code review | C | SWEBOK, Bacchelli & Bird | verification-tax |
| S7 | Continuous integration / delivery (CI/CD) | C | Humble & Farley, DORA | canary-deploy |
| S8 | DevOps | C | DORA / *Accelerate*, Kim et al. | slo-error-budget |
| S9 | Agile and Scrum | C | Agile Manifesto, Scrum Guide | |
| S10 | Design pattern | C | Gamma et al. (GoF), Alexander | saga-pattern, strangler-fig |

## Architecture (8) — complements the 10 TOGAF foundations

| # | Term | Type | Main references | Under it |
|---|---|---|---|---|
| R1 | Software architecture | F | ISO/IEC/IEEE 42010, Bass et al. *SA in Practice* | adr, atam |
| R2 | Architectural style (monolith, microservices, event-driven) | C | Richards & Ford, Fowler | modular-monolith |
| R3 | Microservices | C | Lewis & Fowler (2014), Newman | saga-pattern, bounded-context |
| R4 | Coupling and cohesion | C | Stevens, Myers & Constantine (1974), Parnas | modular-monolith |
| R5 | Domain-driven design | C | Evans (2003), Vernon | bounded-context |
| R6 | API and API design | C | OpenAPI spec, Fielding (REST) | api-versioning, grpc, idempotency |
| R7 | Quality attribute and trade-off | F | ISO/IEC 25010, Bass et al. | atam, well-architected |
| R8 | Event-driven architecture | C | Hohpe & Woolf, Fowler | dead-letter-queue, change-data-capture |

## Backend and distributed systems (11)

| # | Term | Type | Main references | Under it |
|---|---|---|---|---|
| B1 | Distributed system | C | Tanenbaum & van Steen, Kleppmann *DDIA* | everything below |
| B2 | Latency, throughput and tail latency | C | Dean & Barroso "Tail at Scale", Google SRE | goodput, gc-pressure |
| B3 | Scalability (vertical / horizontal) | C | Kleppmann, Bondi (2000) | |
| B4 | Availability and reliability | F | ISO/IEC 25010, IEC 60050-192, Google SRE | slo-error-budget, correlated-failure |
| B5 | Fault tolerance and redundancy | C | Avizienis et al. (2004) taxonomy | cascading-failure, silent-failure |
| B6 | Consistency models | C | Kleppmann, Jepsen | linearizability, cap-theorem |
| B7 | Replication and partitioning (sharding) | C | Kleppmann | |
| B8 | Consensus | C | Lamport (Paxos), Ongaro & Ousterhout (Raft) | control-plane |
| B9 | Caching | C | Kleppmann, Hennessy & Patterson | hybrid-memory-disk-cache, kv-cache |
| B10 | Message queue and asynchronous messaging | C | Hohpe & Woolf | dead-letter-queue, retry-storm |
| B11 | Load balancing and backpressure | C | Google SRE, AWS Builders' Library | adaptive-concurrency-limit |

## Data (9)

| # | Term | Type | Main references | Under it |
|---|---|---|---|---|
| D1 | Data model (relational, document, graph) | F | ISO/IEC 9075 (SQL), Codd (1970), DAMA-DMBOK | knowledge-graph |
| D2 | Database and DBMS | F | ISO/IEC 2382, DAMA-DMBOK | |
| D3 | Transaction and ACID | C | Gray & Reuter, Härder & Reuter (1983) | saga-pattern, idempotency |
| D4 | OLTP vs OLAP | C | Kimball, Kleppmann | |
| D5 | Data warehouse, data lake, lakehouse | C | Inmon, Kimball, Armbrust et al. (2021) | change-data-capture |
| D6 | ETL / ELT and data pipeline | C | DAMA-DMBOK, Kleppmann | change-data-capture |
| D7 | Batch vs stream processing | C | Kleppmann, Akidau et al. (Dataflow model) | change-data-capture |
| D8 | Data governance and data quality | F | DAMA-DMBOK, ISO 8000 | silent-failure |
| D9 | Metadata and data catalog | C | DAMA-DMBOK | feature-store |

## Cloud and infrastructure (9)

| # | Term | Type | Main references | Under it |
|---|---|---|---|---|
| I1 | Cloud computing | F | NIST SP 800-145, ISO/IEC 22123 | all of it |
| I2 | Service models (IaaS, PaaS, SaaS) | F | NIST SP 800-145, ISO/IEC 22123 | |
| I3 | Deployment models (public, private, hybrid) | F | NIST SP 800-145, ISO/IEC 22123 | |
| I4 | Virtualisation and containers | C | Popek & Goldberg (1974), Docker / OCI spec | |
| I5 | Container orchestration (Kubernetes) | C | Kubernetes docs, Burns et al. "Borg, Omega, Kubernetes" | control-plane |
| I6 | Infrastructure as code | C | Morris *IaC*, Terraform docs | |
| I7 | Observability (logs, metrics, traces) | C | OpenTelemetry spec, Google SRE | silent-failure |
| I8 | Serverless | C | Jonas et al. "Cloud Programming Simplified" (2019), CNCF | |
| I9 | FinOps and cloud cost | C | FinOps Foundation framework | ai-gateway (chargeback) |

## Security (10)

| # | Term | Type | Main references | Under it |
|---|---|---|---|---|
| X1 | CIA triad (confidentiality, integrity, availability) | F | ISO/IEC 27000, NIST SP 800-12 | all of it |
| X2 | Threat, vulnerability and risk | F | ISO/IEC 27005, NIST SP 800-30 | threat-modeling |
| X3 | Authentication vs authorisation | F | NIST SP 800-63, ISO/IEC 27000 | oauth2, oidc |
| X4 | Least privilege and defence in depth | F | NIST SP 800-53, Saltzer & Schroeder (1975) | zero-trust-architecture |
| X5 | Encryption (symmetric, asymmetric, at rest / in transit) | F | NIST SP 800-175B, ISO/IEC 18033 | remote-attestation |
| X6 | Identity and access management (IAM) | C | NIST SP 800-63, ISO/IEC 24760 | oauth2, oidc |
| X7 | Attack surface | C | Manadhata & Wing (2011), OWASP | supply-chain-attack |
| X8 | Secure software development (SSDLC) | F | NIST SP 800-218 (SSDF), OWASP SAMM | supply-chain-attack |
| X9 | Common vulnerability classes (injection, XSS, …) | C | OWASP Top 10, CWE | indirect-prompt-injection |
| X10 | AI security and adversarial ML | F | NIST AI 100-2, OWASP LLM Top 10 | indirect-prompt-injection, memory-poisoning, information-flow-control |
