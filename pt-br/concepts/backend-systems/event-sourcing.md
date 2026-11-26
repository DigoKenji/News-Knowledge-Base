---
type: Concept
title: 'Event sourcing'
description: 'Guardar o estado de uma aplicação como um log somente de acréscimo de eventos de domínio imutáveis ("pedido feito", "item enviado") e derivar o estado atual reproduzindo-os, em vez de sobrescrever registros no lugar.'
tags:
- backend-systems
- messaging-and-workflows
domain: backend-systems
subdomain: messaging-and-workflows
also_in:
- architecture/software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 11 Stream Processing: Change Data Capture; Event Sourcing; State, Streams, and Immutability; ch. 12 The Future of Data Systems'
- title: 'Fowler, M. (2005). Event Sourcing'
  resource: 'https://martinfowler.com/eaaDev/EventSourcing.html'
  kind: practitioner
- title: 'Microsoft. Event Sourcing pattern (Azure Architecture Center)'
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/event-sourcing.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Event sourcing

**Definição.** Kleppmann coloca o event sourcing ao lado do change data capture: os dois guardam todas as mudanças de estado da aplicação como um log de eventos, mas em níveis diferentes. No CDC a aplicação atualiza um banco livremente e o log de mudanças é extraído por baixo; no event sourcing a aplicação é construída explicitamente sobre eventos imutáveis gravados em um event store somente de acréscimo, onde atualizações e exclusões são desencorajadas ou proibidas, e cada evento registra algo que aconteceu no nível da aplicação, e não uma mudança de estado de baixo nível. O estado atual é uma visão derivada, calculada reproduzindo os eventos (muitas vezes com snapshots para ganhar velocidade). A técnica veio da comunidade de domain-driven design e costuma andar junto com CQRS, que separa o modelo de escrita (eventos) dos modelos de leitura.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `messaging-and-workflows`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-26); rascunho aguardando certificação do Kenji.

## Como se encaixa

O event sourcing oferece uma trilha de auditoria completa, permite reconstruir ou criar novos modelos de leitura depois e se encaixa naturalmente na arquitetura orientada a eventos. Os custos são reais: os eventos precisam continuar legíveis conforme os schemas evoluem, excluir dados pessoais exige tratamento especial, e consultar o estado atual requer projeções. O ponto mais amplo de Kleppmann (caps. 11-12) é que um log de eventos imutáveis pode ser o registro oficial do qual bancos, caches e índices são derivados.

## Exemplo

Um carrinho de compras é guardado como eventos (item adicionado, item removido, checkout iniciado). Um bug no cálculo de preço é corrigido e os totais dos carrinhos são recalculados reproduzindo os eventos, sem perder nada.

## Conceitos mais amplos

- [Arquitetura orientada a eventos](/pt-br/concepts/architecture/event-driven-architecture.md)

## Conceitos relacionados

- [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md)
- [Evolução de schema e compatibilidade de dados](/pt-br/concepts/data/schema-evolution.md)
- [Domain-driven design (DDD)](/pt-br/concepts/architecture/domain-driven-design.md)

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Change Data Capture; Event Sourcing; State, Streams, and Immutability; ch. 12 The Future of Data Systems. Event sourcing comparado ao CDC, e o log como origem dos dados derivados.
2. Fowler, M. (2005). *Event Sourcing*. <https://martinfowler.com/eaaDev/EventSourcing.html> — A descrição original do padrão por Fowler.
3. Microsoft. Event Sourcing pattern (Azure Architecture Center). <https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing> — Página do padrão no Azure Architecture Center, com trade-offs.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
