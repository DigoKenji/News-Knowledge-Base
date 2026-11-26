---
type: Concept
title: 'Arquitetura como opções'
description: 'Ver a arquitetura como a compra de opções - o direito, mas não a obrigação, de fazer uma mudança depois a um custo conhecido - para que o time possa adiar decisões irreversíveis até saber mais; opções valem mais quando a incerteza é alta.'
tags:
- architecture
- architecture-practice
domain: architecture
subdomain: architecture-practice
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 9 Architecture Is Selling Options (Reversing Irreversible Decision Making; Deferring Decisions with Options; An Architecture Option: Elasticity; Strike Prices; Uncertainty Increases an Option''s Value)'
- title: 'Fowler, M. (2003). Who Needs an Architect? IEEE Software 20(5)'
  resource: 'https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf'
  kind: paper
- title: 'Sullivan, K. J. et al. (2001). The Structure and Value of Modularity in Software Design. ESEC/FSE 2001'
  resource: 'https://doi.org/10.1145/503209.503224'
  kind: paper
lang: pt-br
translation_of: /concepts/architecture/architecture-options.md
---

# Arquitetura como opções

**Definição.** Muitas definições descrevem a arquitetura como as decisões difíceis de reverter, tomadas cedo. Hohpe argumenta que o início do projeto é quando menos se sabe e segue o argumento de Fowler de que o arquiteto deve trabalhar para eliminar a irreversibilidade. Ele toma emprestada a opção financeira: pagar algo agora pelo direito de decidir depois em condições fixas. Uma camada de indireção, uma fronteira modular ou um desenho que escala horizontalmente custam esforço antecipado (o prêmio da opção) e reduzem o custo de uma mudança futura (o preço de exercício). Como nas opções financeiras, quanto mais incerto o futuro, mais a opção vale.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `architecture-practice`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-26); rascunho aguardando certificação do Kenji.

## Como se encaixa

O enquadramento dá ao arquiteto um vocabulário de negócio para explicar por que vale pagar por flexibilidade, e quando não vale: uma opção que ninguém vai exercer em um contexto estável é desperdício. Liga-se à pesquisa sobre modularidade, que avalia desenhos pelas opções que criam, e a atributos de qualidade como elasticidade, que é uma opção de escalar com preço de exercício quase zero em uma nuvem automatizada (enquanto o preço de exercício para trocar de provedor continua alto).

## Exemplo

Um time que não sabe de quais provedores de pagamento vai precisar coloca pagamentos atrás de uma interface interna. Isso custa alguns dias agora; incluir ou trocar um provedor depois vira uma mudança pequena, e não uma reescrita.

## Conceitos mais amplos

- [Arquitetura de software](/pt-br/concepts/architecture/software-architecture.md)

## Conceitos relacionados

- [Atributos de qualidade e trade-offs](/pt-br/concepts/architecture/quality-attributes.md)
- [Acoplamento e coesão](/pt-br/concepts/architecture/coupling-cohesion.md)
- [Registro de decisão de arquitetura](/pt-br/concepts/backend-systems/adr.md)

## Fontes

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 9 Architecture Is Selling Options (Reversing Irreversible Decision Making; Deferring Decisions with Options; An Architecture Option: Elasticity; Strike Prices; Uncertainty Increases an Option's Value). Arquitetura como venda de opções ao negócio, com elasticidade como exemplo e Black-Scholes como intuição de por que a incerteza aumenta o valor da opção.
2. Fowler, M. (2003). *Who Needs an Architect? IEEE Software 20(5)*. <https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf> — Origem da ideia de que a tarefa principal do arquiteto é eliminar a irreversibilidade no design de software.
3. Sullivan, K. J. et al. (2001). *The Structure and Value of Modularity in Software Design*. ESEC/FSE 2001. <https://doi.org/10.1145/503209.503224> — Aplica a teoria de opções reais ao design modular de software (independente de Hohpe).

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
