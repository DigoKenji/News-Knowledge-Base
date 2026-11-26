---
type: Concept
title: Fila de mensagens e mensageria assíncrona
description: Um componente que permite aos serviços se comunicarem enviando mensagens a uma fila ou tópico em vez de se chamarem diretamente, desacoplando remetentes e destinatários no tempo, na taxa e na localização.
tags:
- backend-systems
- core
domain: backend-systems
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Hohpe, G. and Woolf, B. (2003). Enterprise Integration Patterns (Messaging patterns catalogue)
  resource: 'https://www.enterpriseintegrationpatterns.com/patterns/messaging/'
  kind: book
- title: Apache Software Foundation. Apache Kafka documentation - Introduction and Design
  resource: 'https://kafka.apache.org/documentation/'
  kind: docs
- title: Amazon Web Services. Amazon SQS Developer Guide - What is Amazon Simple Queue Service?
  resource: 'https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html'
  kind: docs
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 11 Stream Processing: Messaging Systems (message brokers vs log-based brokers); Transmitting Event Streams'
lang: pt-br
translation_of: /concepts/backend-systems/message-queue.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Fila de mensagens e mensageria assíncrona

**Definição.** Hohpe e Woolf descrevem a mensageria como comunicação assíncrona por canais: uma fila ponto a ponto entrega cada mensagem a um consumidor (distribuição de trabalho), enquanto um tópico publish-subscribe a entrega a cada assinante. Brokers como RabbitMQ ou Amazon SQS guardam mensagens até serem consumidas; sistemas baseados em log como o Apache Kafka mantêm um log ordenado e reproduzível por partição. A entrega é tipicamente pelo menos uma vez, então os consumidores precisam ser idempotentes.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Filas absorvem picos, permitem que trabalho lento aconteça em segundo plano e evitam que a falha de um serviço derrube imediatamente quem o chama. Os custos são consistência eventual, depuração mais difícil entre saltos, mensagens venenosas (tratadas por dead-letter queues) e a necessidade de monitorar a profundidade da fila como sinal de problema.

## Exemplo

Quando um usuário envia um vídeo, a API o armazena e publica uma mensagem "video.uploaded". Um conjunto de workers de transcodificação consome da fila no próprio ritmo; num pico de dia de lançamento a fila cresce para 50.000 mensagens e esvazia ao longo da hora seguinte em vez de derrubar a API.

## Conceitos mais específicos

- [Dead-letter queue](/pt-br/concepts/backend-systems/dead-letter-queue.md)
- [Tempestade de retentativas](/pt-br/concepts/backend-systems/retry-storm.md)
- [Captura de alterações de dados (CDC)](/pt-br/concepts/data/change-data-capture.md)
- [Padrão Saga](/pt-br/concepts/backend-systems/saga-pattern.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 11 Stream Processing: Messaging Systems (message brokers vs log-based brokers); Transmitting Event Streams: Brokers no estilo AMQP versus brokers baseados em log, como o Kafka, e o que cada um garante.

## Fontes

1. Hohpe, G. and Woolf, B. (2003). *Enterprise Integration Patterns* (Messaging patterns catalogue). <https://www.enterpriseintegrationpatterns.com/patterns/messaging/> — Canais de mensagem, ponto a ponto versus publish-subscribe, padrões de mensageria.
2. Apache Software Foundation. *Apache Kafka documentation - Introduction and Design*. <https://kafka.apache.org/documentation/> — Mensageria baseada em log, partições, grupos de consumidores e semântica de entrega.
3. Amazon Web Services. *Amazon SQS Developer Guide - What is Amazon Simple Queue Service?*. <https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html> — Semântica de fila gerenciada, entrega pelo menos uma vez e dead-letter queues.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Messaging Systems (message brokers vs log-based brokers); Transmitting Event Streams. Brokers no estilo AMQP versus brokers baseados em log, como o Kafka, e o que cada um garante.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
