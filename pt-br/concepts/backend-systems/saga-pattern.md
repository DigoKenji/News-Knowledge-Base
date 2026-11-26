---
type: Concept
title: Padrão Saga
description: Padrão Saga (microservices).
tags:
- backend-systems
- microservices
domain: backend-systems
subdomain: microservices
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
relations:
- type: mitigates
  target: /concepts/backend-systems/cap-theorem.md
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 17 Your Coffee Shop Doesn''t Use Two-Phase Commit (Write-off; Retry; Compensating Action)'
lang: pt-br
translation_of: /concepts/backend-systems/saga-pattern.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Padrão Saga

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `microservices`.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (Architect Elevator adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Write-off; Retry; Compensating Action): Três alternativas ao two-phase commit quando um passo falha: absorver a perda, tentar de novo ou desfazer os passos concluídos com ações compensatórias.

## Fontes

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 17 Your Coffee Shop Doesn't Use Two-Phase Commit (Write-off; Retry; Compensating Action). Três alternativas ao two-phase commit quando um passo falha: absorver a perda, tentar de novo ou desfazer os passos concluídos com ações compensatórias.

## Relações
- **mitiga** [Teorema CAP](/pt-br/concepts/backend-systems/cap-theorem.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
