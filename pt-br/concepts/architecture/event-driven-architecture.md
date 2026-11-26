---
type: Concept
title: Arquitetura orientada a eventos
description: Um estilo arquitetural em que componentes se comunicam produzindo e reagindo a eventos - registros de que algo aconteceu - geralmente por meio de um broker, para que produtores não precisem saber quem os consome.
tags:
- architecture
- core
domain: architecture
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Fowler, M. (2017). What do you mean by "Event-Driven"?
  resource: 'https://martinfowler.com/articles/201701-event-driven.html'
  kind: practitioner
- title: Hohpe, G. and Woolf, B. (2003). Enterprise Integration Patterns
  resource: 'https://www.enterpriseintegrationpatterns.com/'
  kind: book
- title: CNCF. CloudEvents specification
  resource: 'https://cloudevents.io/'
  kind: standard
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 11 Stream Processing: Messaging Systems; Change Data Capture; Event Sourcing'
lang: pt-br
translation_of: /concepts/architecture/event-driven-architecture.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Arquitetura orientada a eventos

**Definição.** Um evento é um fato imutável ("PedidoRealizado às 10:42"). Fowler distingue quatro padrões que costumam ser confundidos: notificação de evento, transferência de estado carregada no evento, event sourcing (guardar o estado como o log de eventos) e CQRS. Brokers como Kafka, RabbitMQ ou barramentos de eventos na nuvem entregam os eventos; o CloudEvents padroniza seu formato.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Desenhos orientados a eventos desacoplam serviços no tempo e no conhecimento, absorvem picos de tráfego e facilitam acrescentar novos consumidores (análise, notificações). O preço: raciocínio e depuração ponta a ponta mais difíceis, consistência eventual, entrega duplicada e fora de ordem (os consumidores precisam ser idempotentes) e a necessidade de tratamento de dead letters e rastreamento.

## Exemplo

Quando um pedido é feito, o serviço de pedidos publica PedidoRealizado; os serviços de estoque, faturamento e e-mail reagem cada um por conta própria, e acrescentar uma checagem de fraude depois não exige mudança no serviço de pedidos.

## Conceitos mais específicos

- [Dead-letter queue](/pt-br/concepts/backend-systems/dead-letter-queue.md)
- [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md)
- [Fila de mensagens e mensageria assíncrona](/pt-br/concepts/backend-systems/message-queue.md)
- [Padrão Saga](/pt-br/concepts/backend-systems/saga-pattern.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 11 Stream Processing: Messaging Systems; Change Data Capture; Event Sourcing: Fluxos de eventos, brokers, CDC e event sourcing como a base de dados dos sistemas orientados a eventos.

## Fontes

1. Fowler, M. (2017). *What do you mean by "Event-Driven"?*. <https://martinfowler.com/articles/201701-event-driven.html> — Os quatro padrões orientados a eventos e seus trade-offs.
2. Hohpe, G. and Woolf, B. (2003). *Enterprise Integration Patterns*. <https://www.enterpriseintegrationpatterns.com/> — Canais de mensagens, publish-subscribe, mensagens de evento.
3. CNCF. *CloudEvents specification*. <https://cloudevents.io/> — Formato padrão de eventos, neutro em relação a fornecedores.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Messaging Systems; Change Data Capture; Event Sourcing. Fluxos de eventos, brokers, CDC e event sourcing como a base de dados dos sistemas orientados a eventos.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
