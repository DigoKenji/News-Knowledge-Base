---
type: Concept
title: Dead-letter queue
description: Uma fila separada para onde um sistema de mensageria manda mensagens que não conseguiu entregar ou processar após um número definido de tentativas, para que parem de bloquear ou circular na fila principal e possam ser inspecionadas, corrigidas e reprocessadas.
tags:
- backend-systems
- messaging-and-workflows
domain: backend-systems
subdomain: messaging-and-workflows
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Hohpe, G. and Woolf, B. (2003). Enterprise Integration Patterns - Dead Letter Channel. Addison-Wesley
  resource: 'https://www.enterpriseintegrationpatterns.com/patterns/messaging/DeadLetterChannel.html'
  kind: book
- title: AWS. Using dead-letter queues in Amazon SQS
  resource: 'https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html'
  kind: docs
- title: RabbitMQ. Dead Letter Exchanges
  resource: 'https://www.rabbitmq.com/docs/dlx'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/dead-letter-queue.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Dead-letter queue

**Definição.** Uma dead-letter queue (DLQ), ou dead letter channel nos Enterprise Integration Patterns, isola mensagens "venenosas": malformadas, expiradas, rejeitadas ou que continuam falhando no consumidor. Separá-las mantém o fluxo principal saudável e preserva as mensagens com falha em vez de descartá-las.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `messaging-and-workflows`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

No Amazon SQS, uma redrive policy move a mensagem para a DLQ após maxReceiveCount recebimentos com falha; um redrive depois a move de volta. O RabbitMQ usa dead-letter exchanges para mensagens rejeitadas, expiradas ou que estouraram a fila. O Kafka não tem DLQ embutida; frameworks como Kafka Connect e Spring Kafka escrevem registros com falha num tópico separado. Boa prática: guardar o payload original mais metadados do erro (exceção, número de tentativas, horários), alarmar sobre o tamanho da DLQ, definir retenção maior que a da fila principal e tornar o reprocessamento idempotente.

## Por que importa e trade-offs

Sem DLQ, uma única mensagem ruim pode bloquear uma fila ordenada para sempre ou gastar recursos com retentativas. Com ela, você troca isso por trabalho operacional: alguém precisa vigiar a DLQ, e mensagens paradas ali simplesmente não são processadas. Uma DLQ que ninguém monitora vira uma falha silenciosa.

## Exemplo

Um evento de pedido sem ID de cliente falha na validação três vezes e vai para a DLQ; um alarme dispara, o time corrige o bug no produtor, ajusta o evento e o reprocessa.

## Fontes

1. Hohpe, G. and Woolf, B. (2003). *Enterprise Integration Patterns* - Dead Letter Channel. Addison-Wesley. <https://www.enterpriseintegrationpatterns.com/patterns/messaging/DeadLetterChannel.html> — A definição do padrão.
2. AWS. *Using dead-letter queues in Amazon SQS*. <https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html> — maxReceiveCount, redrive policy, orientação sobre retenção.
3. RabbitMQ. *Dead Letter Exchanges*. <https://www.rabbitmq.com/docs/dlx> — Broker independente - gatilhos de rejeição, expiração e limite de tamanho.

## Relações
- (nenhuma ainda)

## Notas
- [Como a Country Road Group construiu uma solução de integração orientada a padrões na AWS](/pt-br/notes/architecture/2026-11-20-how-country-road-group-built-a-pattern-driven-integration-so.md)
