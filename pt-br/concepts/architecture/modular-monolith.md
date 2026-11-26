---
type: Concept
title: Monólito modular
description: Uma aplicação implantada como uma única unidade mas dividida internamente em módulos com fronteiras fortes e garantidas - cada um dono de seus dados e expondo uma interface explícita - que dá boa parte da estrutura de microservices sem o custo de um sistema distribuído.
tags:
- architecture
- software-architecture
domain: architecture
subdomain: software-architecture
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Parnas, D. L. (1972). On the Criteria to Be Used in Decomposing Systems into Modules. Communications of the ACM 15(12)
  resource: 'https://dl.acm.org/doi/10.1145/361598.361623'
  kind: paper
- title: Fowler, M. (2015). MonolithFirst
  resource: 'https://martinfowler.com/bliki/MonolithFirst.html'
  kind: practitioner
- title: Shopify Engineering (2019). Deconstructing the Monolith - Designing Software that Maximizes Developer Productivity
  resource: 'https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity'
  kind: practitioner
- title: Spring. Spring Modulith Reference Documentation
  resource: 'https://docs.spring.io/spring-modulith/reference/'
  kind: docs
relations:
- type: uses
  target: bounded-context
lang: pt-br
translation_of: /concepts/architecture/modular-monolith.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Monólito modular

**Definição.** Um monólito modular é um monólito projetado em torno de módulos, geralmente alinhados a capacidades de negócio ou bounded contexts. Os módulos conversam só por interfaces públicas (chamadas no mesmo processo ou eventos internos), não acessam o interior nem as tabelas uns dos outros, e as fronteiras são verificadas por ferramentas, não só por convenção.

Domínio: [architecture](/pt-br/domains/architecture.md). Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O princípio vem de Parnas (1972): decompor sistemas escondendo decisões de projeto atrás de interfaces de módulos. Na prática: um módulo por domínio (pedidos, faturamento, catálogo); schemas separados ou posse de tabelas por módulo; regras de dependência aplicadas por ferramentas de build (módulos Java, Spring Modulith, ArchUnit, Packwerk da Shopify para Rails); e comunicação por interfaces ou eventos no mesmo processo, para que um módulo possa depois ser extraído como serviço. A Shopify roda um dos maiores monólitos Rails assim. O "Monolith First" de Martin Fowler defende que a maioria dos sistemas comece como um monólito bem estruturado e só se divida quando houver necessidade comprovada.

## Por que importa e trade-offs

Você ganha deploy simples, chamadas no mesmo processo, transações locais e refatoração fácil, além de responsabilidades claras e um caminho para microservices. Não ganha deploy ou escala independentes por módulo, uma falha num módulo pode derrubar o processo inteiro, e as fronteiras se desgastam se não forem verificadas automaticamente.

## Exemplo

Um app de e-commerce é entregue como um único serviço Spring Boot, mas o módulo de faturamento só pode chamar a API pública do módulo de pedidos, e um teste quebra o build se o faturamento importar classes internas de pedidos ou consultar suas tabelas.

## Fontes

1. Parnas, D. L. (1972). *On the Criteria to Be Used in Decomposing Systems into Modules*. Communications of the ACM 15(12). <https://dl.acm.org/doi/10.1145/361598.361623> — Ocultação de informação como base para fronteiras de módulos.
2. Fowler, M. (2015). *MonolithFirst*. <https://martinfowler.com/bliki/MonolithFirst.html> — Começar com um monólito bem estruturado e dividir depois.
3. Shopify Engineering (2019). *Deconstructing the Monolith - Designing Software that Maximizes Developer Productivity*. <https://shopify.engineering/deconstructing-monolith-designing-software-maximizes-developer-productivity> — Caso de produção em larga escala de um monólito dividido em componentes.
4. Spring. *Spring Modulith Reference Documentation*. <https://docs.spring.io/spring-modulith/reference/> — Ferramenta que verifica fronteiras de módulos e suporta eventos no mesmo processo.

## Relações
- **uses** [bounded-context](/pt-br/concepts/architecture/bounded-context.md)

## Notas
- [Agoda reduz em 64% o tempo de revisão de código de API com arquitetura modular](/pt-br/notes/architecture/2026-11-24-agoda-reduces-api-code-review-time-by-64-with-modular-archit.md)
