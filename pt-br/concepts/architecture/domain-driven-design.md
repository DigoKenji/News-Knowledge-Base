---
type: Concept
title: Domain-driven design (DDD)
description: Uma abordagem de projeto de software que modela o código bem próximo do domínio de negócio, usando uma linguagem compartilhada com especialistas do domínio e dividindo sistemas grandes em bounded contexts com fronteiras explícitas.
tags:
- architecture
- software-architecture
- core
domain: architecture
subdomain: software-architecture
also_in:
- software-engineering/design
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Evans, E. (2003). Domain-Driven Design - Tackling Complexity in the Heart of Software. Addison-Wesley
  resource: 'https://www.domainlanguage.com/ddd/'
  kind: book
- title: Fowler, M. (2014). BoundedContext
  resource: 'https://martinfowler.com/bliki/BoundedContext.html'
  kind: practitioner
- title: Microsoft. Using domain analysis to model microservices (Azure Architecture Center)
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis'
  kind: docs
lang: pt-br
translation_of: /concepts/architecture/domain-driven-design.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Domain-driven design (DDD)

**Definição.** Eric Evans introduziu o DDD em 2003. O projeto estratégico divide um domínio complexo em bounded contexts, cada um com seu próprio modelo e linguagem ubíqua, e mapeia como eles se relacionam (mapas de contexto). O projeto tático fornece blocos de construção dentro de um contexto: entidades, objetos de valor, agregados, eventos de domínio, repositórios e serviços.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `software-architecture`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O DDD é a forma mais comum de encontrar boas fronteiras para microservices ou módulos de um monólito modular, e liga o projeto de software à arquitetura de negócio (capacidades). Compensa em lógica de negócio complexa e é exagero para aplicações CRUD simples.

## Exemplo

"Cliente" significa um lead com dados de contato no contexto de Vendas mas uma conta com limite de crédito no contexto de Faturamento; o DDD mantém dois modelos em vez de uma classe compartilhada confusa.

## Conceitos mais específicos

- [Contexto delimitado](/pt-br/concepts/architecture/bounded-context.md)
- [Microservices](/pt-br/concepts/architecture/microservices.md)
- [Monólito modular](/pt-br/concepts/architecture/modular-monolith.md)

## Fontes

1. Evans, E. (2003). *Domain-Driven Design - Tackling Complexity in the Heart of Software*. Addison-Wesley. <https://www.domainlanguage.com/ddd/> — Origem do DDD, padrões estratégicos e táticos.
2. Fowler, M. (2014). *BoundedContext*. <https://martinfowler.com/bliki/BoundedContext.html> — Bounded contexts e relações entre contextos explicados.
3. Microsoft. *Using domain analysis to model microservices* (Azure Architecture Center). <https://learn.microsoft.com/en-us/azure/architecture/microservices/model/domain-analysis> — Aplicar DDD para encontrar fronteiras de serviços.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
