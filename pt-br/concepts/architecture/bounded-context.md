---
type: Concept
title: Contexto delimitado
description: 'Uma fronteira do DDD dentro da qual um modelo de domínio e sua linguagem valem de forma consistente; o mesmo termo pode significar outra coisa em outro contexto delimitado.'
tags:
- architecture
- software-architecture
domain: architecture
subdomain: software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley'
  resource: 'https://books.google.com/books?vid=ISBN9780321125217'
  kind: book
- title: 'Fowler, M. (2014). BoundedContext'
  resource: 'https://martinfowler.com/bliki/BoundedContext.html'
  kind: practitioner
lang: pt-br
translation_of: /concepts/architecture/bounded-context.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; movido para architecture; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Contexto delimitado

**Definição.** Um contexto delimitado é a fronteira explícita dentro da qual um modelo de domínio vale e seus termos têm um único significado. Eric Evans o introduziu no Domain-Driven Design: sistemas grandes inevitavelmente contêm vários modelos, então, em vez de forçar um modelo único, cada contexto mantém o seu, e um mapa de contextos descreve como eles se relacionam e traduzem nas bordas.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `software-architecture`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

É o núcleo estratégico do DDD e um guia comum para traçar fronteiras de microsserviços ou módulos: um contexto delimitado por serviço ou módulo mantém a linguagem consistente dentro e explicita a integração fora. Fowler destaca que a mesma palavra ('cliente', 'produto') legitimamente significa coisas diferentes em contextos diferentes.

## Exemplo

Em Vendas um 'produto' tem preço e promoções; em Logística tem peso e dimensões. Dois contextos delimitados têm cada um seu modelo de Produto e trocam apenas o id do produto.

## Fontes

1. Evans, E. (2003). Domain-Driven Design: Tackling Complexity in the Heart of Software. Addison-Wesley. <https://books.google.com/books?vid=ISBN9780321125217> — Parte IV Design Estratégico, cap. 14 Mantendo a integridade do modelo (contexto delimitado, mapa de contextos).
2. Fowler, M. (2014). BoundedContext. <https://martinfowler.com/bliki/BoundedContext.html> — Explicação curta com o exemplo de múltiplos significados.

## Relações
- [Migração strangler fig](/pt-br/concepts/architecture/strangler-fig.md) **usa** este

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
