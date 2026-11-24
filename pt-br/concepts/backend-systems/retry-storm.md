---
type: Concept
title: Tempestade de retentativas
description: Uma enxurrada de requisições repetidas que atinge de uma vez uma dependência em dificuldade - muitas vezes multiplicada entre camadas - adicionando carga justamente quando ela menos aguenta e mantendo-a sobrecarregada depois que o problema original passou.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Amazon Builders' Library (AWS Builder Center). Timeouts, retries, and backoff with jitter
  resource: 'https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter'
  kind: practitioner
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 22 Addressing Cascading Failures. O'Reilly / Google
  resource: 'https://sre.google/sre-book/addressing-cascading-failures/'
  kind: book
- title: gRPC. A6 - gRPC Retry Design (proposal)
  resource: 'https://github.com/grpc/proposal/blob/master/A6-client-retries.md'
  kind: standard
- title: Huang, L. et al. (2022). Metastable Failures in the Wild. OSDI 2022
  resource: 'https://www.usenix.org/conference/osdi22/presentation/huang-lexiang'
  kind: paper
lang: pt-br
translation_of: /concepts/backend-systems/retry-storm.md
---

# Tempestade de retentativas

**Definição.** Um retry storm acontece quando muitos clientes respondem a erros ou timeouts repetindo a chamada, então o serviço com problema recebe um múltiplo do tráfego normal. Retentativas em várias camadas se multiplicam: se cada uma de 3 camadas tenta 3 vezes, uma ação do usuário pode virar 27 chamadas ao serviço do fundo.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `system-design`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Retentativas ajudam com falhas transitórias e independentes, mas durante sobrecarga as falhas são correlacionadas e retentar só aumenta a carga. Clientes sincronizados pioram, porque todos repetem nos mesmos momentos. Defesas: backoff exponencial com jitter (atraso aleatório) para espalhar as retentativas; orçamentos de retentativa ou token buckets que limitam retentativas a uma fração do tráfego normal (por exemplo 10%, como no retry throttling do gRPC e na orientação de SRE do Google); retentar em uma só camada; não retentar em erros de sobrecarga; circuit breakers; e descarte de carga no servidor. Retry storms são um gatilho clássico de falhas metaestáveis.

## Por que importa e trade-offs

Código de resiliência bem-intencionado pode transformar um soluço breve numa queda longa. Limitar retentativas significa que algumas requisições falham quando uma retentativa as salvaria; o trade-off é aceito porque protege o sistema todo.

## Exemplo

Um banco de dados fica lento por 10 segundos; 10.000 clientes repetem 5 vezes cada um sem jitter, enviando 50.000 consultas extras que mantêm o banco saturado por 20 minutos.

## Notas de revisão para Kenji

A redação do rascunho ("retentativas de trabalho aceito") era confusa; reescrita em torno da explicação padrão.

## Fontes

1. Amazon Builders' Library (AWS Builder Center). *Timeouts, retries, and backoff with jitter*. <https://builder.aws.com/content/3EumjoZascWd1oZiEgL8ORlv3qE/timeouts-retries-and-backoff-with-jitter> — Amplificação de retentativas entre camadas, backoff, jitter, limite de retentativas por token bucket.
2. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 22 Addressing Cascading Failures. O'Reilly / Google. <https://sre.google/sre-book/addressing-cascading-failures/> — Retentativas como causa de falha em cascata; orçamentos de retentativa por requisição e por cliente.
3. gRPC. *A6 - gRPC Retry Design* (proposal). <https://github.com/grpc/proposal/blob/master/A6-client-retries.md> — Retry throttling embutido num framework de RPC muito usado.
4. Huang, L. et al. (2022). *Metastable Failures in the Wild*. OSDI 2022. <https://www.usenix.org/conference/osdi22/presentation/huang-lexiang> — Retentativas como efeito sustentador em incidentes metaestáveis reais.

## Relações
- (nenhuma ainda)

## Notas
- [Como elevamos os limites de taxa dos clusters do Databricks em 7.5×](/pt-br/notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md)
