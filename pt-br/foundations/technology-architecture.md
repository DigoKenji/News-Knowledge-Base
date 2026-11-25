---
type: Foundation
title: Arquitetura de Tecnologia
description: Uma descrição da estrutura e da interação dos serviços de tecnologia e dos componentes de tecnologia.
tags:
- foundation
- architecture
- technology-architecture
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
  section: Part I §3.77 Technology Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.81 Technology Architecture
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Technical Architecture (PDF p. 1038); FEAPO definition, adopted 2017'
  tier: B
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  meets_standard: yes
  gaps:
  - 'Terminology: the KB keeps TOGAF''s ''technology architecture'' as the title and lists ''technical architecture'' as the BIZBOK/FEAPO synonym'
related:
- architecture-domain
- application-architecture
domain: architecture
lang: pt-br
translation_of: /foundations/technology-architecture.md
---

# Arquitetura de Tecnologia

## Definição de trabalho
A arquitetura de tecnologia descreve os serviços de tecnologia (como hospedagem, redes, armazenamento e plataformas) e os componentes de tecnologia que os provêm, e como eles interagem. *Segue o TOGAF 9.2 §3.77, a referência do curso; idêntica no TOGAF 10.*

## Definições autoritativas
> A description of the structure and interaction of the technology services and technology components.
> *Tradução livre:* Uma descrição da estrutura e da interação dos serviços de tecnologia e dos componentes de tecnologia.
> — The Open Group, TOGAF Standard 9.2, Part I §3.77 Technology Architecture

> A description of the structure and interaction of the technology services and technology components.
> *Tradução livre:* Uma descrição da estrutura e da interação dos serviços de tecnologia e dos componentes de tecnologia.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.81 Technology Architecture

> Technical architecture represents the logical and physical interconnection of infrastructure elements to enable the deployment and management of data architecture, application architecture, business architecture, and strategy.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Technical Architecture* (FEAPO definition, adopted 2017)

## Em que diferem
- **Nome diferente, mesmo domínio.** O BIZBOK v9 (redação da FEAPO) usa *technical* architecture (seu histórico de versões registra que substituiu "technology architecture") e a define como a interconexão de elementos de infraestrutura que permite implantar e gerir os outros três domínios; o TOGAF descreve a mesma camada como a estrutura e a interação de serviços e componentes de tecnologia.
- Idêntica no 9.2 e no 10.
- **Baseline.** A KB segue o TOGAF 9.2 (orientação do curso, 2026-11-19: a 9.2 está consolidada, enquanto o TOGAF 10 ainda está sendo atualizado). A redação do TOGAF 10 é mantida aqui para mostrar o que mudou.

## Onde se encaixa
Subdomínio `technology-architecture` do domínio; sobrepõe-se ao domínio Nuvem e Infraestrutura da KB, que traz o detalhe de engenharia.

## No curso
O curso de FinOps (gestão financeira de TI) aplica visões de custo à arquitetura de tecnologia. Aulas ainda não ingeridas.

## Questões em aberto
- Nenhuma.
