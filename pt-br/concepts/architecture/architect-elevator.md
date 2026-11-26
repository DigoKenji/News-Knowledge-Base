---
type: Concept
title: 'O elevador do arquiteto (o papel do arquiteto)'
description: 'A imagem de Hohpe para o trabalho do arquiteto em uma grande organização - circular entre a cobertura (estratégia, liderança) e a casa de máquinas (código, operação) traduzindo nos dois sentidos - valorizado por quantos andares cobre, não por quão alto chega.'
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
  locator: 'ch. 1 The Architect Elevator; ch. 4 Enterprise Architect or Architect in the Enterprise?; ch. 5 An Architect Stands on Three Legs'
- title: 'Fowler, M. (2003). Who Needs an Architect? IEEE Software 20(5)'
  resource: 'https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf'
  kind: paper
- title: 'Bredemeyer, D. and Malan, R. (2002-2006). The Role of the Architect. Bredemeyer Consulting white paper'
  resource: 'https://www.bredemeyer.com/pdf_files/role.pdf'
  kind: practitioner
lang: pt-br
translation_of: /concepts/architecture/architect-elevator.md
---

# O elevador do arquiteto (o papel do arquiteto)

**Definição.** Em grandes organizações, quem tem o conhecimento técnico muitas vezes não decide, e quem decide não tem o conhecimento técnico. Hohpe descreve o arquiteto como o elemento de ligação: alguém que trabalha perto dos engenheiros em sistemas reais, consegue explicar temas técnicos a executivos sem perder a essência e transforma estratégia de negócio em decisões técnicas. Ele chama isso de andar no elevador do arquiteto. O elevador precisa descer tanto quanto sobe, porque o arquiteto só descobre se suas decisões funcionam vendo a implementação; quem fica na cobertura cai em "autoridade sem responsabilidade". Ele apoia o papel em três pernas: habilidade, impacto e liderança.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `architecture-practice`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-26); rascunho aguardando certificação do Kenji.

## Como se encaixa

A metáfora explica por que empresas digitais e planas precisam de menos arquitetos corporativos (vivem em um térreo) e por que empresas tradicionais precisam de vários arquitetos que se encontram no meio de um prédio alto. Complementa a visão do TOGAF da arquitetura corporativa como disciplina com a visão prática do que um arquiteto faz no dia a dia, e o modelo de competências de Bredemeyer e Malan (tecnologia, estratégia de negócio, política organizacional, consultoria, liderança).

## Exemplo

O conselho quer lançar produtos mais rápido. O arquiteto traduz isso para baixo como infraestrutura de nuvem elástica e serviços sem estado que escalam horizontalmente, e para cima explica por que a antiga integração em lote impede a meta e quanto custa substituí-la.

## Conceitos mais amplos

- [Arquitetura de software](/pt-br/concepts/architecture/software-architecture.md)

## Conceitos relacionados

- [Arquitetura como opções](/pt-br/concepts/architecture/architecture-options.md)
- [Registro de decisão de arquitetura](/pt-br/concepts/backend-systems/adr.md)

## Fontes

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 1 The Architect Elevator; ch. 4 Enterprise Architect or Architect in the Enterprise?; ch. 5 An Architect Stands on Three Legs. A metáfora do elevador, o ciclo de feedback da cobertura à casa de máquinas, e habilidade, impacto e liderança como as três pernas do papel.
2. Fowler, M. (2003). *Who Needs an Architect? IEEE Software 20(5)*. <https://martinfowler.com/ieeeSoftware/whoNeedsArchitect.pdf> — Ensaio de Fowler sobre o que um arquiteto deve ser: um guia que trabalha com o time, e não alguém que decide à distância.
3. Bredemeyer, D. and Malan, R. (2002-2006). The Role of the Architect. Bredemeyer Consulting white paper. <https://www.bredemeyer.com/pdf_files/role.pdf> — Modelo de competências do papel de arquiteto: tecnologia, estratégia de negócio, política organizacional, consultoria e liderança.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
