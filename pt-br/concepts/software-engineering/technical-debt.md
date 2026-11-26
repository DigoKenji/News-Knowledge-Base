---
type: Concept
title: Dívida técnica
description: O custo futuro criado ao escolher agora uma solução rápida ou conveniente em vez de uma melhor - como uma dívida financeira, cobra "juros" na forma de esforço extra em cada mudança posterior até ser paga com refatoração.
tags:
- software-engineering
- design
- core
domain: software-engineering
subdomain: design
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Cunningham, W. (1992). The WyCash Portfolio Management System. OOPSLA 1992 experience report
  resource: 'https://c2.com/doc/oopsla92.html'
  kind: paper
- title: Kruchten, P., Nord, R. L. and Ozkaya, I. (2012). Technical Debt - From Metaphor to Theory and Practice. IEEE Software 29(6)
  resource: 'https://doi.org/10.1109/MS.2012.167'
  kind: paper
- title: Fowler, M. (2009). TechnicalDebtQuadrant
  resource: 'https://martinfowler.com/bliki/TechnicalDebtQuadrant.html'
  kind: practitioner
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 12 If You Never Kill Anything, You Will Live Among Zombies'
lang: pt-br
translation_of: /concepts/software-engineering/technical-debt.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Dívida técnica

**Definição.** Ward Cunningham criou a metáfora em 1992: entregar código de primeira é como contrair uma dívida, o que é aceitável se ela for paga logo com uma reescrita; o perigo é quando nunca é paga. O quadrante de Fowler separa dívida deliberada de inadvertida e prudente de imprudente.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `design`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Dívida técnica é um conceito de gestão: permite que engenheiros expliquem a stakeholders de negócio por que a velocidade cai com o tempo e por que tempo para refatoração é investimento. Ela se acumula no código, na arquitetura, nos testes, na documentação e na infraestrutura. Parte da dívida é racional (para pegar uma janela de mercado); dívida sem gestão atrasa entregas e causa incidentes.

## Exemplo

Fixar no código a lista de fontes de notícias para lançar mais rápido é dívida deliberada e prudente; movê-las depois para config/sources.yaml a quita.

## Conceitos mais específicos

- [Migração strangler fig](/pt-br/concepts/architecture/strangler-fig.md)
- [Monólito modular](/pt-br/concepts/architecture/modular-monolith.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (Architect Elevator adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 12 If You Never Kill Anything, You Will Live Among Zombies: Sistemas legados como "zumbis" mantidos vivos pelo medo de mudar; a cura é tornar a mudança rotineira e aposentar sistemas de forma deliberada.

## Fontes

1. Cunningham, W. (1992). *The WyCash Portfolio Management System*. OOPSLA 1992 experience report. <https://c2.com/doc/oopsla92.html> — Origem da metáfora da dívida técnica.
2. Kruchten, P., Nord, R. L. and Ozkaya, I. (2012). *Technical Debt - From Metaphor to Theory and Practice*. IEEE Software 29(6). <https://doi.org/10.1109/MS.2012.167> — Visão de pesquisa sobre dívida técnica e seus tipos.
3. Fowler, M. (2009). *TechnicalDebtQuadrant*. <https://martinfowler.com/bliki/TechnicalDebtQuadrant.html> — Dívida deliberada vs inadvertida, prudente vs imprudente.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 12 If You Never Kill Anything, You Will Live Among Zombies. Sistemas legados como "zumbis" mantidos vivos pelo medo de mudar; a cura é tornar a mudança rotineira e aposentar sistemas de forma deliberada.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
