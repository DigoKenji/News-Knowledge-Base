---
type: Concept
title: Idempotência
description: 'A propriedade de que repetir uma operação tem o mesmo efeito que executá-la uma vez, o que torna seguras as novas tentativas e as mensagens duplicadas.'
tags:
- backend-systems
- messaging-and-workflows
domain: backend-systems
subdomain: messaging-and-workflows
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 11 Stream Processing: Idempotence (Fault Tolerance); ch. 12 Exactly-once execution of an operation'
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 17 Your Coffee Shop Doesn''t Use Two-Phase Commit (Retry)'
- title: 'IETF (2022). RFC 9110 HTTP Semantics, §9.2.2 Idempotent Methods'
  resource: 'https://www.rfc-editor.org/rfc/rfc9110#section-9.2.2'
  kind: standard
- title: 'Stripe. Idempotent requests (API reference)'
  resource: 'https://docs.stripe.com/api/idempotent_requests'
  kind: docs
lang: pt-br
translation_of: /concepts/backend-systems/idempotency.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Idempotência

**Definição.** Uma operação é idempotente quando executá-la várias vezes tem o mesmo efeito que executá-la uma vez. Algumas operações são naturalmente idempotentes (definir um valor, apagar por id; o HTTP define PUT e DELETE como métodos idempotentes); outras ficam assim ao dar a cada requisição uma chave de idempotência única, que o servidor guarda com o resultado e verifica antes de repetir o trabalho.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `messaging-and-workflows`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Redes e brokers de mensagens entregam pelo menos uma vez, então novas tentativas e duplicatas são normais; receptores idempotentes transformam isso em processamento efetivamente único sem transações distribuídas. É a base de novas tentativas seguras, sagas e processamento de streams exatamente uma vez.

## Exemplo

Um cliente envia um pagamento com Idempotency-Key 7f3a; a conexão cai e ele tenta de novo; a API de pagamentos encontra a chave e devolve a primeira cobrança em vez de cobrar duas vezes.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 11 Stream Processing: Idempotence (Fault Tolerance); ch. 12 Exactly-once execution of an operation: Idempotência como alternativa mais barata a transações distribuídas, por exemplo gravando o offset da mensagem junto com cada escrita.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (Architect Elevator adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Retry): Repetir um grupo de operações fica simples quando todo receptor é idempotente e ignora duplicatas.

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Idempotence (Fault Tolerance); ch. 12 Exactly-once execution of an operation. Idempotência como alternativa mais barata a transações distribuídas, por exemplo gravando o offset da mensagem junto com cada escrita.
2. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Retry). Repetir um grupo de operações fica simples quando todo receptor é idempotente e ignora duplicatas.
3. IETF (2022). RFC 9110 HTTP Semantics, §9.2.2 Idempotent Methods. <https://www.rfc-editor.org/rfc/rfc9110#section-9.2.2> — Definição de métodos HTTP idempotentes.
4. Stripe. Idempotent requests (API reference). <https://docs.stripe.com/api/idempotent_requests> — Chaves de idempotência em uma API de pagamentos em produção.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
