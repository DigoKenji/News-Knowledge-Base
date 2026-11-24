# Conceitos

`status: stable` = vocabulário aceito · `status: draft` = proposto pelo agente, aguardando Kenji. Kenji certifica um conceito adicionando uma entrada `verified`.

## Engenharia de IA/ML

- [Inteligência artificial](/pt-br/concepts/artificial-intelligence.md) — core · **rascunho**
- [Machine learning (aprendizado de máquina)](/pt-br/concepts/machine-learning.md) — core · **rascunho**
- [Aprendizado supervisionado](/pt-br/concepts/supervised-learning.md) — core · **rascunho**
- [Aprendizado não supervisionado e auto-supervisionado](/pt-br/concepts/unsupervised-learning.md) — core · **rascunho**
- [Aprendizado por reforço](/pt-br/concepts/reinforcement-learning.md) — core · **rascunho**
- [Redes neurais e deep learning](/pt-br/concepts/deep-learning.md) — core · **rascunho**
- [Treino vs inferência](/pt-br/concepts/training-vs-inference.md) — core · **rascunho**
- [Overfitting e generalização](/pt-br/concepts/overfitting.md) — core · **rascunho**
- [Avaliação de modelos e métricas](/pt-br/concepts/model-evaluation.md) — core · **rascunho**
- [Transformer e atenção](/pt-br/concepts/transformer.md) — core · **rascunho**
- [Grande modelo de linguagem (LLM) e modelo de fundação](/pt-br/concepts/large-language-model.md) — core · **rascunho**
- [Embedding](/pt-br/concepts/embedding.md) — core · **rascunho**
- [Geração aumentada por recuperação](/pt-br/concepts/rag.md) — rag
- [GraphRAG](/pt-br/concepts/graph-rag.md) — rag
- [RAG sem vetores](/pt-br/concepts/vectorless-rag.md) — rag
- [KV cache](/pt-br/concepts/kv-cache.md) — llm-engineering
- [Throughput/latência/custo de inferência](/pt-br/concepts/inference-tradeoffs.md) — llm-engineering
- [Orquestração multiagente](/pt-br/concepts/multi-agent-orchestration.md) — ai-agents
- [Feature store](/pt-br/concepts/feature-store.md) — mlops
- [Drift de conceito](/pt-br/concepts/concept-drift.md) — ml-governance
- [Drift de dados](/pt-br/concepts/data-drift.md) — ml-governance
- [Máquina de vetores de suporte](/pt-br/concepts/svm.md) — classical-ml
- [Rede RBF](/pt-br/concepts/rbf-network.md) — classical-ml
- [LLM como juiz](/pt-br/concepts/llm-as-a-judge.md) — mlops
- [Arquitetura preditiva de embedding conjunto (JEPA)](/pt-br/concepts/jepa.md) — deep-learning
- [Decodificação especulativa](/pt-br/concepts/speculative-decoding.md) — llm-engineering
- [Desagregação prefill-decode](/pt-br/concepts/prefill-decode-disaggregation.md) — llm-engineering
- [Codificação posicional rotatória (RoPE)](/pt-br/concepts/rope.md) — llm-engineering
- [Model Context Protocol (MCP)](/pt-br/concepts/model-context-protocol.md) — ai-agents
- [Calibração](/pt-br/concepts/calibration.md) — mlops
- [Modelo de decisão estruturado](/pt-br/concepts/structured-decision-model.md) — llm-engineering
- [Viés de posição](/pt-br/concepts/position-bias.md) — mlops
- [Chunking pai-filho](/pt-br/concepts/parent-child-chunking.md) — rag
- [Harness de agente](/pt-br/concepts/agent-harness.md) — ai-agents
- [Busca semântica](/pt-br/concepts/semantic-search.md) — rag
- [Destilação](/pt-br/concepts/distillation.md) — llm-engineering
- [Gateway de IA](/pt-br/concepts/ai-gateway.md) — llm-engineering
- [Goodput](/pt-br/concepts/goodput.md) — llm-engineering
- [Agente de IA](/pt-br/concepts/ai-agent.md) — ai-agents
- [Aprendizado em contexto](/pt-br/concepts/in-context-learning.md) — llm-engineering
- [Memória de agente](/pt-br/concepts/agent-memory.md) — ai-agents
- [Mistura de especialistas](/pt-br/concepts/mixture-of-experts.md) — ai-ml
- [Atenção latente multicabeça](/pt-br/concepts/multi-head-latent-attention.md) — ai-ml
- [Engenharia de contexto](/pt-br/concepts/context-engineering.md) — ai-ml
- [Adaptação de baixo posto (LoRA)](/pt-br/concepts/lora.md) — ai-ml

## Engenharia de Backend e Sistemas

- [Idempotência](/pt-br/concepts/idempotency.md) — system-design
- [Teorema CAP](/pt-br/concepts/cap-theorem.md) — system-design
- [Padrão Saga](/pt-br/concepts/saga-pattern.md) — microservices
- [Migração strangler fig](/pt-br/concepts/strangler-fig.md) — microservices
- [Contexto delimitado](/pt-br/concepts/bounded-context.md) — microservices
- [Registro de decisão de arquitetura](/pt-br/concepts/adr.md) — system-design
- [Análise de trade-offs ATAM](/pt-br/concepts/atam.md) — system-design
- [gRPC](/pt-br/concepts/grpc.md) — api-design
- [Versionamento de API](/pt-br/concepts/api-versioning.md) — api-design
- [Linearizabilidade](/pt-br/concepts/linearizability.md) — system-design
- [Teste de simulação determinística](/pt-br/concepts/deterministic-simulation-testing.md) — system-design
- [Tempestade de retentativas](/pt-br/concepts/retry-storm.md) — system-design
- [Limite adaptativo de concorrência](/pt-br/concepts/adaptive-concurrency-limit.md) — system-design
- [Falha silenciosa](/pt-br/concepts/silent-failure.md) — system-design
- [Engenharia do caos](/pt-br/concepts/chaos-engineering.md) — system-design
- [Plano de controle](/pt-br/concepts/control-plane.md) — system-design
- [Falha correlacionada](/pt-br/concepts/correlated-failure.md) — system-design
- [Dead-letter queue](/pt-br/concepts/dead-letter-queue.md) — microservices
- [Cache híbrido memória-disco](/pt-br/concepts/hybrid-memory-disk-cache.md) — system-design
- [Pressão de GC](/pt-br/concepts/gc-pressure.md) — system-design
- [Falha em cascata](/pt-br/concepts/cascading-failure.md) — backend-systems

## Nuvem e Infraestrutura

- [Well-Architected Framework](/pt-br/concepts/well-architected.md) — cloud-platforms
- [Deploy canary / blue-green](/pt-br/concepts/canary-deploy.md) — devops-cicd
- [Orçamento de erros de SLO](/pt-br/concepts/slo-error-budget.md) — cloud-infra

## Cibersegurança

- [Tríade CIA (confidencialidade, integridade, disponibilidade)](/pt-br/concepts/cia-triad.md) — core · **rascunho**
- [Ameaça, vulnerabilidade e risco](/pt-br/concepts/threat-vulnerability-risk.md) — core · **rascunho**
- [Autenticação vs autorização](/pt-br/concepts/authentication-authorization.md) — core · **rascunho**
- [Privilégio mínimo e defesa em profundidade](/pt-br/concepts/least-privilege.md) — core · **rascunho**
- [Criptografia](/pt-br/concepts/encryption.md) — core · **rascunho**
- [Gestão de identidades e acessos (IAM)](/pt-br/concepts/identity-access-management.md) — core · **rascunho**
- [Superfície de ataque](/pt-br/concepts/attack-surface.md) — core · **rascunho**
- [Desenvolvimento seguro de software (SSDLC)](/pt-br/concepts/secure-software-development.md) — core · **rascunho**
- [Classes comuns de vulnerabilidade](/pt-br/concepts/common-vulnerability-classes.md) — core · **rascunho**
- [Segurança de IA e machine learning adversarial](/pt-br/concepts/ai-security.md) — core · **rascunho**
- [OAuth 2.0](/pt-br/concepts/oauth2.md) — iam
- [OpenID Connect](/pt-br/concepts/oidc.md) — iam
- [Modelagem de ameaças](/pt-br/concepts/threat-modeling.md) — appsec
- [Prompt injection indireta](/pt-br/concepts/indirect-prompt-injection.md) — appsec
- [Arquitetura zero-trust](/pt-br/concepts/zero-trust-architecture.md) — appsec
- [Controle de fluxo de informação](/pt-br/concepts/information-flow-control.md) — appsec
- [Atestação remota](/pt-br/concepts/remote-attestation.md) — appsec
- [Envenenamento de memória](/pt-br/concepts/memory-poisoning.md) — security
- [Ataque à cadeia de suprimentos](/pt-br/concepts/supply-chain-attack.md) — security

## Dados e Ciência de Dados

- [Grafo de conhecimento](/pt-br/concepts/knowledge-graph.md) — data-engineering
- [Captura de mudanças de dados (CDC)](/pt-br/concepts/change-data-capture.md) — data

## Práticas de Engenharia de Software
- [Imposto de verificação](/pt-br/concepts/verification-tax.md) — ai-assisted-development

## Arquitetura
- [Monólito modular](/pt-br/concepts/modular-monolith.md) — architecture
