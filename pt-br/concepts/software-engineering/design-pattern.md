---
type: Concept
title: Padrão de projeto (design pattern)
description: Uma solução nomeada e reutilizável para um problema recorrente num dado contexto, descrita com seus trade-offs - de padrões orientados a objetos no nível do código a padrões de arquitetura e de integração.
tags:
- software-engineering
- design
- core
domain: software-engineering
subdomain: design
also_in:
- architecture/software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Gamma, E., Helm, R., Johnson, R. and Vlissides, J. (1994). Design Patterns - Elements of Reusable Object-Oriented Software. Addison-Wesley
  resource: 'https://www.oreilly.com/library/view/design-patterns-elements/0201633612/'
  kind: book
- title: Fowler, M. (2002). Patterns of Enterprise Application Architecture - catalog
  resource: 'https://martinfowler.com/eaaCatalog/'
  kind: book
- title: Hohpe, G. and Woolf, B. (2003). Enterprise Integration Patterns
  resource: 'https://www.enterpriseintegrationpatterns.com/'
  kind: book
lang: pt-br
translation_of: /concepts/software-engineering/design-pattern.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Padrão de projeto (design pattern)

**Definição.** A ideia vem do arquiteto Christopher Alexander e entrou no software com o livro da "Gang of Four" (1994), que catalogou 23 padrões orientados a objetos em três grupos: criacionais (Factory, Builder, Singleton), estruturais (Adapter, Facade, Decorator) e comportamentais (Observer, Strategy, Command). Cada padrão tem um nome, o problema, a solução e as consequências.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `design`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Padrões dão às equipes um vocabulário comum ("use um facade aqui") e capturam experiência. O mesmo formato foi usado depois para padrões de aplicações empresariais (Fowler), padrões de integração empresarial (Hohpe e Woolf) e padrões de sistemas distribuídos como saga, strangler fig e dead-letter queue. Usar padrões demais onde uma solução simples funciona é um antipadrão conhecido.

## Exemplo

Strategy - o resumidor escolhe um prompt por tipo de fonte (artigo, paper, post do LinkedIn) atrás de uma interface única, então acrescentar um tipo novo não muda o código que chama.

## No trabalho de arquitetura

A mesma ideia em escala de sistema é um [estilo arquitetural](/pt-br/concepts/architecture/architectural-style.md) (microsserviços, orientado a eventos, em camadas). Padrões de projeto ficam no nível de código e módulos; padrões de integração, como os de Hohpe e Woolf, ficam entre os dois. O conceito aparece também no tema Arquitetura, em arquitetura de software.

## Conceitos mais específicos

- [Padrão Saga](/pt-br/concepts/backend-systems/saga-pattern.md)
- [Migração strangler fig](/pt-br/concepts/architecture/strangler-fig.md)
- [Dead-letter queue](/pt-br/concepts/backend-systems/dead-letter-queue.md)

## Fontes

1. Gamma, E., Helm, R., Johnson, R. and Vlissides, J. (1994). *Design Patterns - Elements of Reusable Object-Oriented Software*. Addison-Wesley. <https://www.oreilly.com/library/view/design-patterns-elements/0201633612/> — O catálogo original de 23 padrões orientados a objetos.
2. Fowler, M. (2002). *Patterns of Enterprise Application Architecture* - catalog. <https://martinfowler.com/eaaCatalog/> — Padrões aplicados a aplicações empresariais.
3. Hohpe, G. and Woolf, B. (2003). *Enterprise Integration Patterns*. <https://www.enterpriseintegrationpatterns.com/> — Linguagem de padrões para mensageria e integração.

## Relações
- **related** [Estilo arquitetural](/pt-br/concepts/architecture/architectural-style.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
