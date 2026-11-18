---
type: Foundation
title: Arquitetura de Negócio
description: Uma representação do negócio — capacidades, entrega de valor, informação e organização — e de como esses elementos se relacionam com estratégia, produtos, políticas, processos, iniciativas e stakeholders.
tags:
- foundation
- architecture
- business-architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
sources:
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.27 Business Architecture
  tier: B
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.24 Business Architecture
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - 'BIZBOK Guide v15 public glossary (Appendix A), Business Architecture Guild: Kenji downloads the PDF from businessarchitectureguild.org (cloud fetch is blocked) into FIAP-MBA/Referencias; a search snippet suggests the Guild''s wording matches TOGAF''s, but it is not verified'
related:
- business-capability
- architecture-domain
- enterprise-architecture
domain: architecture
lang: pt-br
translation_of: /foundations/business-architecture.md
---

# Arquitetura de Negócio

## Definição de trabalho
A arquitetura de negócio é uma visão estruturada do próprio negócio — o que ele é capaz de fazer (capacidades), como entrega valor (fluxos de valor), de que informação depende e como se organiza — vinculada à sua estratégia, produtos, políticas, processos, iniciativas e stakeholders. *Segue o TOGAF 10 §4.27, aguardando o glossário público do BIZBOK.*

## Definições autoritativas
> A representation of holistic, multi-dimensional business views of: capabilities, end-to-end value delivery, information, and organizational structure; and the relationships among these business views and strategies, products, policies, processes, initiatives, and stakeholders.
> *Tradução livre:* Uma representação de visões de negócio holísticas e multidimensionais sobre: capacidades, entrega de valor de ponta a ponta, informação e estrutura organizacional; e as relações entre essas visões de negócio e estratégias, produtos, políticas, processos, iniciativas e stakeholders.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.27 Business Architecture

> A representation of holistic, multi-dimensional business views of: capabilities, end-to-end value delivery, information, and organizational structure; and the relationships among these business views and strategies, products, policies, initiatives, and stakeholders.
> *Tradução livre:* Uma representação de visões de negócio holísticas e multidimensionais sobre: capacidades, entrega de valor de ponta a ponta, informação e estrutura organizacional; e as relações entre essas visões de negócio e estratégias, produtos, políticas, iniciativas e stakeholders.
> — The Open Group, TOGAF Standard 9.2, Part I §3.24 Business Architecture

## Em que diferem
- O TOGAF 10 acrescenta **processos** à lista de coisas às quais as visões de negócio se relacionam; fora isso, é idêntico ao 9.2.
- As quatro visões centrais (capacidades, entrega de valor, informação, organização) correspondem aos domínios centrais do Guia BIZBOK; a confirmar com o texto do glossário do BIZBOK.

## Onde se encaixa
Subdomínio `business-architecture` do domínio. Blocos de construção centrais: [Capacidade de negócio](/pt-br/foundations/business-capability.md), fluxo de valor (TOGAF 10 §4.85), organização e informação. Processos são modelados com BPMN (OMG).

## No curso
Arquitetura de Negócio no MBA (Prof. Rogério Kobayashi, 2026): arquitetura de negócio, blueprints, BPMN, estratégia digital — `Materiais/Business Archictecture/MBA_Business_Architecture_2026_v2.pdf`. Aulas ainda não ingeridas.

## Questões em aberto
- O fluxo de valor deve ser a próxima entrada de fundamento (TOGAF 10 §4.85 + Series Guide G178)?
