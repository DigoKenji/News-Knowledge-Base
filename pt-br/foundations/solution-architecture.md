---
type: Foundation
title: Arquitetura de Solução
description: Uma descrição de uma operação ou atividade de negócio discreta e de como sistemas de informação e TI a apoiam — normalmente restrita a um único projeto ou release.
tags:
- foundation
- architecture
- solution-architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
sources:
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.73 Solution Architecture; §4.78 Strategic Architecture; §4.68 Segment Architecture; Appendix B.41 System
  tier: B
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.69 Solution Architecture
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - 'A second body. TOGAF 10 has no definition of ''solution'' itself; candidates: ISO/IEC/IEEE 15288 (system, via TOGAF B.41), or a tier C source on solution architecture'
related:
- enterprise-architecture
- architecture
- architecture-domain
domain: architecture
lang: pt-br
translation_of: /foundations/solution-architecture.md
---

# Arquitetura de Solução

## Definição de trabalho
A arquitetura de solução descreve uma operação ou mudança de negócio focada — normalmente um projeto ou release — e como a TI a apoia, traduzindo requisitos em uma visão de solução, especificações de alto nível e tarefas de implementação. Uma *solução* é a combinação de elementos de negócio e de TI entregue para atender a essa necessidade. *Segue o TOGAF 9.2 §3.69 (que inclui a nota sobre o nível de projeto) e o TOGAF 10 §4.73; a definição de "solução" é a interpretação da KB, já que o TOGAF não define o termo.*

## Definições autoritativas
> A description of a discrete and focused business operation or activity and how IS/IT supports that operation.
> *Tradução livre:* Uma descrição de uma operação ou atividade de negócio discreta e focada e de como IS/TI apoia essa operação.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.73 Solution Architecture

> A description of a discrete and focused business operation or activity and how IS/IT supports that operation.
> Note: A Solution Architecture typically applies to a single project or project release, assisting in the translation of requirements into a solution vision, high-level business and/or IT system specifications, and a portfolio of implementation tasks.
> *Tradução livre:* Uma descrição de uma operação ou atividade de negócio discreta e focada e de como IS/TI apoia essa operação. Nota: Uma Arquitetura de Solução normalmente se aplica a um único projeto ou release de projeto, auxiliando na tradução de requisitos em uma visão de solução, em especificações de sistemas de negócio e/ou de TI de alto nível e em um portfólio de tarefas de implementação.
> — The Open Group, TOGAF Standard 9.2, Part I §3.69 Solution Architecture

> A combination of interacting elements organized to achieve one or more stated purposes. (Source: ISO/IEC/IEEE 15288:2015)
> *Tradução livre:* Uma combinação de elementos interagentes organizada para alcançar um ou mais propósitos declarados. (Fonte: ISO/IEC/IEEE 15288:2015)
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), Appendix B.41 System

## Em que diferem
- O TOGAF 10 mantém a definição, mas retira a nota da 9.2 que a vinculava a um único projeto ou release.
- O TOGAF 10 a coloca numa hierarquia de níveis: Arquitetura Estratégica (§4.78, executiva, de longo prazo), Arquitetura de Segmento (§4.68, programa ou portfólio) e Arquitetura de Solução (projeto).
- Nenhuma das edições define *solução*. O uso no mercado varia (um produto, um entregável de projeto, um conjunto de sistemas); a KB adota a interpretação da definição de trabalho e a sinaliza.

## Onde se encaixa
Termo de escopo: o nível abaixo da [Arquitetura Empresarial](/pt-br/foundations/enterprise-architecture.md). Pode tocar os quatro [domínios de arquitetura](/pt-br/foundations/architecture-domain.md).

## No curso
Técnicas de Arquitetura (ASO desenho, solution design deck). Aulas ainda não ingeridas.

## Questões em aberto
- Kenji perguntou "o que é uma solução?": aceitar a interpretação da KB, ou tornar *solução* um fundamento próprio quando for encontrada uma definição tier A/B?
