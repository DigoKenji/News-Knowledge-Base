---
type: Concept
title: Idempotência
description: Idempotência (system-design).
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
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
lang: pt-br
translation_of: /concepts/backend-systems/idempotency.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Idempotência

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `system-design`.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 11 Stream Processing: Idempotence (Fault Tolerance); ch. 12 Exactly-once execution of an operation: Idempotência como alternativa mais barata a transações distribuídas, por exemplo gravando o offset da mensagem junto com cada escrita.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (Architect Elevator adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Retry): Repetir um grupo de operações fica simples quando todo receptor é idempotente e ignora duplicatas.

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 11 Stream Processing: Idempotence (Fault Tolerance); ch. 12 Exactly-once execution of an operation. Idempotência como alternativa mais barata a transações distribuídas, por exemplo gravando o offset da mensagem junto com cada escrita.
2. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Retry). Repetir um grupo de operações fica simples quando todo receptor é idempotente e ignora duplicatas.

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
