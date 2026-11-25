---
type: Foundation
title: Arquitetura de Negócio
description: Uma representação do negócio — capacidades, entrega de valor, informação e organização — e de como esses elementos se relacionam com estratégias, produtos, políticas, processos, iniciativas e stakeholders (TOGAF 9.2).
tags:
- foundation
- architecture
- business-architecture
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
  section: Part I §3.24 Business Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.27 Business Architecture
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Business Architecture (PDF p. 1025); FEAPO definition, adopted 2017'
  tier: B
- title: 'Business Architecture: A Practical Guide'
  publisher: 'Whelan, J. and Meaden, G. (2012). Gower'
  edition: 1st (2012)
  section: 'ch. 1 About Business Architecture, pp. 2-3; Glossary, pp. 254-255'
  tier: C
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: yes
  gaps:
  - 'BIZBOK v9.0 (2020) is not the current Guild version (v15+); re-check the wording if Kenji gets the current glossary'
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
A arquitetura de negócio é uma visão estruturada do próprio negócio — o que ele é capaz de fazer (capacidades), como entrega valor de ponta a ponta, de que informação depende e como se organiza — e de como esses elementos se relacionam com estratégias, produtos, políticas, iniciativas e stakeholders. *Segue o TOGAF 9.2 §3.24, a referência do curso (o TOGAF 10 acrescenta processos a essa lista); aguardando o glossário do BIZBOK.*

## Definições autoritativas
> A representation of holistic, multi-dimensional business views of: capabilities, end-to-end value delivery, information, and organizational structure; and the relationships among these business views and strategies, products, policies, initiatives, and stakeholders.
> *Tradução livre:* Uma representação de visões de negócio holísticas e multidimensionais sobre: capacidades, entrega de valor de ponta a ponta, informação e estrutura organizacional; e as relações entre essas visões de negócio e estratégias, produtos, políticas, iniciativas e stakeholders.
> — The Open Group, TOGAF Standard 9.2, Part I §3.24 Business Architecture

> A representation of holistic, multi-dimensional business views of: capabilities, end-to-end value delivery, information, and organizational structure; and the relationships among these business views and strategies, products, policies, processes, initiatives, and stakeholders.
> *Tradução livre:* Uma representação de visões de negócio holísticas e multidimensionais sobre: capacidades, entrega de valor de ponta a ponta, informação e estrutura organizacional; e as relações entre essas visões de negócio e estratégias, produtos, políticas, processos, iniciativas e stakeholders.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.27 Business Architecture

> Business architecture represents holistic, multidimensional business views of capabilities, end-to-end value delivery, information, and organizational structure as well as the relationships among these business views and strategies, products, policies, initiatives, and stakeholders.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Business Architecture* (FEAPO definition, adopted 2017)

## Em que diferem
- **O BIZBOK v9 (Business Architecture Guild, redação da FEAPO) coincide quase palavra por palavra com o TOGAF 9.2**: as mesmas quatro visões e a mesma lista de elementos relacionados, com "represents" no lugar de "a representation of". Isso confirma o trecho de busca anotado antes. Whelan e Meaden (2012, p. 2) ressaltam que não existe uma definição única aceita e tratam a arquitetura de negócio como "uma coleção de ativos, métodos, processos e recursos" que viabilizam os objetivos da organização (nível C).
- O TOGAF 10 acrescenta **processos** à lista de coisas às quais as visões de negócio se relacionam; fora isso, é idêntico ao 9.2.
- As quatro visões centrais (capacidades, entrega de valor, informação, organização) correspondem aos domínios centrais do Guia BIZBOK (confirmado pelo verbete do glossário do BIZBOK v9 acima).
- **Baseline.** A KB segue o TOGAF 9.2 (orientação do curso, 2026-11-19: a 9.2 está consolidada, enquanto o TOGAF 10 ainda está sendo atualizado). A redação do TOGAF 10 é mantida aqui para mostrar o que mudou.

## Onde se encaixa
Subdomínio `business-architecture`. Blocos de construção centrais: [Capacidade de negócio](/pt-br/foundations/business-capability.md), fluxo de valor (TOGAF 10 §4.85), organização e informação. Processos são modelados com BPMN (OMG).

## No curso
Arquitetura de Negócio no MBA (2026): arquitetura de negócio, blueprints, BPMN, estratégia digital — `Materiais/Business Archictecture/MBA_Business_Architecture_2026_v2.pdf`. Aulas ainda não ingeridas.

## Questões em aberto
- O fluxo de valor deve ser a próxima entrada de fundamento (TOGAF 10 §4.85 + Series Guide G178)?
