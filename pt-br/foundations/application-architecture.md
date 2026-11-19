---
type: Foundation
title: Arquitetura de Aplicações
description: Uma descrição da estrutura e da interação das aplicações, como grupos de capacidades que provêm funções de negócio-chave e gerenciam os ativos de dados (TOGAF 9.2).
tags:
- foundation
- architecture
- application-architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
  updated: '2026-11-19T19:00:00-03:00'
baseline: TOGAF 9.2
sources:
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.3 Application Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.3 Application Architecture
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - 'A second body: ISO/IEC/IEEE 42010 (no domain-specific definition), or a textbook such as Fowler, Patterns of Enterprise Application Architecture (2002, tier C, in Materiais)'
related:
- architecture-domain
- data-architecture
- technology-architecture
- business-capability
domain: architecture
lang: pt-br
translation_of: /foundations/application-architecture.md
---

# Arquitetura de Aplicações

## Definição de trabalho
A arquitetura de aplicações descreve a estrutura e a interação das aplicações da empresa, vistas como grupos de capacidades que provêm funções de negócio-chave e gerenciam os ativos de dados. *Segue o TOGAF 9.2 §3.3, a referência do curso (o TOGAF 10 ancora as aplicações em capacidades de negócio, e não em funções).*

## Definições autoritativas
> A description of the structure and interaction of the applications as groups of capabilities that provide key business functions and manage the data assets.
> *Tradução livre:* Uma descrição da estrutura e da interação das aplicações como grupos de capacidades que provêm funções de negócio-chave e gerenciam os ativos de dados.
> — The Open Group, TOGAF Standard 9.2, Part I §3.3 Application Architecture

> A description of the structure and interaction of the applications that provide key business capabilities and manage the data assets.
> *Tradução livre:* Uma descrição da estrutura e da interação das aplicações que provêm capacidades de negócio-chave e gerenciam os ativos de dados.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.3 Application Architecture

## Em que diferem
- Na 9.2, as aplicações eram enquadradas como *grupos de capacidades que provêm funções de negócio-chave*; na 10, elas *provêm capacidades de negócio-chave*. A capacidade, e não a função, passou a ser a âncora de negócio, em consonância com o foco em capacidades da arquitetura de negócio.
- Não é o mesmo que arquitetura de software: a arquitetura de aplicações do TOGAF trata do portfólio e das interações entre aplicações, e não do interior de uma aplicação.
- **Baseline.** A KB segue o TOGAF 9.2 (orientação do curso, 2026-11-19: a 9.2 está consolidada, enquanto o TOGAF 10 ainda está sendo atualizado). A redação do TOGAF 10 é mantida aqui para mostrar o que mudou.

## Onde se encaixa
Subdomínio `application-architecture` do domínio [Arquitetura](/pt-br/domains/architecture.md). O desenho interno de uma aplicação (padrões, microsserviços) fica em `integration-and-software-architecture`.

## No curso
Técnicas de Arquitetura (Fowler PoEAA, Enterprise Integration Patterns, material sobre microsserviços). Aulas ainda não ingeridas.

## Questões em aberto
- Nenhuma.
