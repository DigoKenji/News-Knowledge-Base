# Conceitos — Engenharia de Backend e Sistemas

Agrupados por subdomínio, conforme a taxonomia da [página do domínio](/pt-br/domains/backend-systems.md). **core** marca o vocabulário central; ↗ marca um conceito cuja origem é outro tema.

## Sistemas distribuídos

- [Consenso](/pt-br/concepts/backend-systems/consensus.md) — core · **rascunho**
- [Modelos de consistência](/pt-br/concepts/backend-systems/consistency-models.md) — core · **rascunho**
- [Sistema distribuído](/pt-br/concepts/backend-systems/distributed-system.md) — core · **rascunho**
- [Replicação e particionamento (sharding)](/pt-br/concepts/backend-systems/replication-partitioning.md) — core · **rascunho**
- [Teorema CAP](/pt-br/concepts/backend-systems/cap-theorem.md)
- [Teste de simulação determinística](/pt-br/concepts/backend-systems/deterministic-simulation-testing.md)
- [Linearizabilidade](/pt-br/concepts/backend-systems/linearizability.md)
- ↗ [Plano de controle](/pt-br/concepts/cloud-infra/control-plane.md) — origem: Nuvem e Infraestrutura

## Confiabilidade e resiliência

- [Disponibilidade e confiabilidade](/pt-br/concepts/backend-systems/availability-reliability.md) — core · **rascunho**
- [Tolerância a falhas e redundância](/pt-br/concepts/backend-systems/fault-tolerance.md) — core · **rascunho**
- [Balanceamento de carga e contrapressão (backpressure)](/pt-br/concepts/backend-systems/load-balancing-backpressure.md) — core · **rascunho**
- [Limite adaptativo de concorrência](/pt-br/concepts/backend-systems/adaptive-concurrency-limit.md)
- [Falha em cascata](/pt-br/concepts/backend-systems/cascading-failure.md)
- [Engenharia do caos](/pt-br/concepts/backend-systems/chaos-engineering.md)
- [Falha correlacionada](/pt-br/concepts/backend-systems/correlated-failure.md)
- [Tempestade de retentativas](/pt-br/concepts/backend-systems/retry-storm.md)
- [Falha silenciosa](/pt-br/concepts/backend-systems/silent-failure.md)
- [Circuit breaker](/pt-br/concepts/backend-systems/circuit-breaker.md) — **draft**
- ↗ [Observabilidade (logs, métricas, traces)](/pt-br/concepts/cloud-infra/observability.md) — origem: Nuvem e Infraestrutura
- ↗ [Orçamento de erros de SLO](/pt-br/concepts/cloud-infra/slo-error-budget.md) — origem: Nuvem e Infraestrutura

## Desempenho e escalabilidade

- [Cache (caching)](/pt-br/concepts/backend-systems/caching.md) — core · **rascunho**
- [Latência, vazão e latência de cauda](/pt-br/concepts/backend-systems/latency-throughput.md) — core · **rascunho**
- [Escalabilidade (vertical e horizontal)](/pt-br/concepts/backend-systems/scalability.md) — core · **rascunho**
- [Pressão de GC](/pt-br/concepts/backend-systems/gc-pressure.md)
- [Cache híbrido memória-disco](/pt-br/concepts/backend-systems/hybrid-memory-disk-cache.md)

## Mensageria e fluxos

- [Fila de mensagens e mensageria assíncrona](/pt-br/concepts/backend-systems/message-queue.md) — core · **rascunho**
- [Dead-letter queue](/pt-br/concepts/backend-systems/dead-letter-queue.md)
- [Execução Durável](/pt-br/concepts/backend-systems/durable-execution.md)
- [Event sourcing](/pt-br/concepts/backend-systems/event-sourcing.md) — **rascunho**
- [Idempotência](/pt-br/concepts/backend-systems/idempotency.md)
- [Padrão Saga](/pt-br/concepts/backend-systems/saga-pattern.md)
- [Visibility timeout](/pt-br/concepts/backend-systems/visibility-timeout.md) — **draft**
- ↗ [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md) — origem: Dados e Ciência de Dados
- ↗ [Arquitetura orientada a eventos](/pt-br/concepts/architecture/event-driven-architecture.md) — origem: Arquitetura

## APIs e protocolos

- [Versionamento de API](/pt-br/concepts/backend-systems/api-versioning.md)
- [gRPC](/pt-br/concepts/backend-systems/grpc.md)
- ↗ [Evolução de schema e compatibilidade de dados](/pt-br/concepts/data/schema-evolution.md) — origem: Dados e Ciência de Dados

## Plataforma web

- [Proxy Reverso](/pt-br/concepts/backend-systems/reverse-proxy.md)
