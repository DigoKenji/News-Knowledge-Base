---
type: Foundation
title: Arquitetura
description: Os conceitos ou propriedades fundamentais de algo em seu ambiente, somados aos princípios que regem como isso é construído e como evolui.
tags:
- foundation
- architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
sources:
- title: ISO/IEC/IEEE 42010 Software, systems and enterprise — Architecture description
  publisher: ISO/IEC/IEEE
  edition: '2022'
  section: §3 (quoted in TOGAF 10 §4.8, definition 1)
  tier: A
  via: TOGAF 10 §4.8
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.8 Architecture
  tier: B
- title: ISO/IEC/IEEE 42010 Systems and software engineering — Architecture description
  publisher: ISO/IEC/IEEE
  edition: 2011 (superseded)
  section: §3 (quoted in TOGAF 9.2 §3.7, definition 1)
  tier: A
  via: TOGAF 9.2 §3.7
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.7 Architecture
  tier: B
evidence:
  bodies:
  - ISO/IEC/IEEE
  - The Open Group
  meets_standard: partial
  gaps:
  - ISO/IEC/IEEE 42010:2022 original text (paywalled at iso.org); the ISO quote is via TOGAF 10 until checked
related:
- architecture-domain
- enterprise-architecture
- solution-architecture
domain: architecture
lang: pt-br
translation_of: /foundations/architecture.md
---

# Arquitetura

## Definição de trabalho
Uma arquitetura é o conjunto de conceitos ou propriedades fundamentais de um sistema ou empresa em seu ambiente, junto com os princípios que regem como ela é realizada e como evolui. *Segue a ISO/IEC/IEEE 42010:2022 (via TOGAF 10), por ser a norma internacional e por o TOGAF adotá-la como seu primeiro significado.*

## Definições autoritativas
> 1. The fundamental concepts or properties of an entity in its environment and governing principles for the realization and evolution of this entity and its related lifecycle processes (Source: ISO/IEC/IEEE 42010:2022)
> 2. The structure of components, their inter-relationships, and the principles and guidelines governing their design and evolution over time
> *Tradução livre:* 1. Os conceitos ou propriedades fundamentais de uma entidade em seu ambiente e os princípios reguladores para a realização e a evolução dessa entidade e de seus processos de ciclo de vida relacionados (Fonte: ISO/IEC/IEEE 42010:2022) 2. A estrutura de componentes, suas inter-relações e os princípios e diretrizes que regem seu desenho e sua evolução ao longo do tempo
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.8 Architecture

> 1. The fundamental concepts or properties of a system in its environment embodied in its elements, relationships, and in the principles of its design and evolution. (Source: ISO/IEC/IEEE 42010: 2011)
> 2. The structure of components, their inter-relationships, and the principles and guidelines governing their design and evolution over time.
> *Tradução livre:* 1. Os conceitos ou propriedades fundamentais de um sistema em seu ambiente, incorporados em seus elementos, relações e nos princípios de seu desenho e evolução. (Fonte: ISO/IEC/IEEE 42010: 2011) 2. A estrutura de componentes, suas inter-relações e os princípios e diretrizes que regem seu desenho e sua evolução ao longo do tempo.
> — The Open Group, TOGAF Standard 9.2, Part I §3.7 Architecture

## Em que diferem
- **Sistema → entidade.** A ISO 42010:2011 definia a arquitetura de um *sistema*; a edição de 2022 (como citada pelo TOGAF 10) fala em *entidade*, de modo que a definição agora abrange empresas, sistemas de sistemas e outras coisas que não são sistemas de software.
- **Incorporados em elementos → princípios reguladores.** Em 2011, os conceitos são *incorporados em seus elementos e relações*; em 2022, essa expressão sai e entram *princípios reguladores para a realização e evolução* e *processos de ciclo de vida relacionados*. Arquitetura é menos "as partes" e mais "as decisões e princípios".
- **Dois significados no TOGAF.** As duas edições do TOGAF mantêm um segundo significado, mais estrutural (componentes, inter-relações, princípios). A definição 2 não muda entre o 9.2 e o 10.

## Onde se encaixa
Termo-raiz do domínio [Arquitetura](/pt-br/domains/architecture.md). Especializado pelo [Domínio de arquitetura](/pt-br/foundations/architecture-domain.md) (negócio, dados, aplicação, tecnologia) e por escopo ([Arquitetura Empresarial](/pt-br/foundations/enterprise-architecture.md), [Arquitetura de Solução](/pt-br/foundations/solution-architecture.md)).

## No curso
Coberto em *Técnicas de Arquitetura* (material do TOGAF 9.2 em `Materiais/Tecnicas de Arquitetura/`). Aulas ainda não ingeridas.

## Questões em aberto
- Adotar a definição 1 (ISO, abstrata) como significado da KB e tratar a definição 2 (estrutural) como uma descrição do conteúdo de uma arquitetura?
