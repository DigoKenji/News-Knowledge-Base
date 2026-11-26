---
type: Concept
title: Estilo arquitetural
description: Uma família nomeada de arquiteturas que compartilham os mesmos tipos de componentes, conectores e restrições - como camadas, monólito, microservices, orientada a eventos ou cliente-servidor - cada uma com forças e fraquezas conhecidas.
tags:
- architecture
- software-architecture
- core
domain: architecture
subdomain: software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Garlan, D. and Shaw, M. (1994). An Introduction to Software Architecture. CMU-CS-94-166
  resource: 'https://www.cs.cmu.edu/afs/cs/project/able/ftp/intro_softarch/intro_softarch.pdf'
  kind: paper
- title: Fielding, R. T. (2000). Architectural Styles and the Design of Network-based Software Architectures. PhD dissertation, UC Irvine
  resource: 'https://ics.uci.edu/~fielding/pubs/dissertation/top.htm'
  kind: paper
- title: Richards, M. and Ford, N. (2020). Fundamentals of Software Architecture. O'Reilly
  resource: 'https://www.oreilly.com/library/view/fundamentals-of-software/9781492043447/'
  kind: book
lang: pt-br
translation_of: /concepts/architecture/architectural-style.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Estilo arquitetural

**Definição.** Garlan e Shaw descreveram estilos arquiteturais como vocabulários de tipos de componentes e conectores mais restrições sobre como podem ser combinados. A tese de Fielding, que definiu o REST, trata um estilo como um conjunto coordenado de restrições. Richards e Ford agrupam os estilos modernos em monolíticos (camadas, monólito modular, microkernel) e distribuídos (baseado em serviços, orientado a eventos, microservices, space-based).

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `software-architecture`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Escolher um estilo é uma das primeiras decisões de arquitetura e deve seguir os atributos de qualidade: microservices compram deploy e escala independentes ao custo de complexidade operacional; um monólito modular compra simplicidade ao custo de escala independente. Sistemas reais muitas vezes misturam estilos.

## Exemplo

O Project_News segue principalmente um estilo de pipeline (buscar → selecionar → resumir → arquivar → digest) disparado por agenda, com uma passagem orientada a eventos do Hermes pela pasta inbox.

## Conceitos mais específicos

- [Microservices](/pt-br/concepts/architecture/microservices.md)
- [Arquitetura orientada a eventos](/pt-br/concepts/architecture/event-driven-architecture.md)
- [Monólito modular](/pt-br/concepts/architecture/modular-monolith.md)

## Fontes

1. Garlan, D. and Shaw, M. (1994). *An Introduction to Software Architecture*. CMU-CS-94-166. <https://www.cs.cmu.edu/afs/cs/project/able/ftp/intro_softarch/intro_softarch.pdf> — Definição clássica e catálogo de estilos arquiteturais.
2. Fielding, R. T. (2000). *Architectural Styles and the Design of Network-based Software Architectures*. PhD dissertation, UC Irvine. <https://ics.uci.edu/~fielding/pubs/dissertation/top.htm> — Estilos como conjuntos de restrições; derivação do REST.
3. Richards, M. and Ford, N. (2020). *Fundamentals of Software Architecture*. O'Reilly. <https://www.oreilly.com/library/view/fundamentals-of-software/9781492043447/> — Catálogo moderno de estilos com avaliação de trade-offs.

## Relações
- **related** [Padrão de projeto (design pattern)](/pt-br/concepts/software-engineering/design-pattern.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
