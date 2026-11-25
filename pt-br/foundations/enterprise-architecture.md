---
type: Foundation
title: Arquitetura Empresarial
description: Arquitetura no escopo de uma empresa inteira (ou de uma grande área dela), usada para alinhar processos, informação e tecnologia com a estratégia e para conduzir a mudança.
tags:
- foundation
- architecture
- enterprise-architecture
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
  section: Part I §1.3 Executive Overview, "Why is an Enterprise Architecture needed?"
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §1.1 Executive Overview, "Why is an Enterprise Architecture Needed?"
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §3 (Enterprise Agility), "Enterprise Architecture provides a framework for change"
  tier: B
- title: 'A Guide to the Business Architecture Body of Knowledge (BIZBOK Guide)'
  publisher: Business Architecture Guild
  edition: v9.0 (2020)
  section: 'Appendix A: Glossary, Enterprise Architecture (PDF p. 1030); FEAPO definition, adopted 2017'
  tier: B
- title: 'SEBoK - Guide to the Systems Engineering Body of Knowledge, glossary: Enterprise Architecture'
  publisher: 'BKCASE Editorial Board (INCOSE, IEEE Systems Council, Stevens Institute/SERC)'
  edition: 'v2.x (online, accessed 2026-11-25)'
  section: 'Glossary entry, quoting the standards listed'
  resource: 'https://sebokwiki.org/wiki/Enterprise_Architecture_(glossary)'
  tier: B
evidence:
  bodies:
  - The Open Group
  - Business Architecture Guild (BIZBOK Guide; FEAPO-adopted definitions)
  - US Federal CIO Council (FEAF, via SEBoK)
  - Gartner (tier D, via SEBoK)
  meets_standard: yes
  gaps:
  - 'TOGAF still has no formal entry; the KB now relies on FEAPO (via BIZBOK) and FEAF for formal definitions, with Gartner as supporting evidence'
related:
- enterprise
- architecture
- architecture-domain
- solution-architecture
domain: architecture
lang: pt-br
translation_of: /foundations/enterprise-architecture.md
---

# Arquitetura Empresarial

## Definição de trabalho
A Arquitetura Empresarial (EA) é a arquitetura da empresa como um todo: descreve como negócio, dados, aplicações e tecnologia se encaixam em toda a empresa, e é usada para conduzir a mudança, de modo que a empresa permaneça integrada e apoie sua estratégia. *Segue o TOGAF 9.2, Parte I §1.3, a referência do curso; o TOGAF dá um propósito, não uma definição formal, então esta frase é a síntese da KB e precisa de um segundo órgão.*

## Definições autoritativas
> The purpose of Enterprise Architecture is to optimize across the enterprise the often fragmented legacy of processes (both manual and automated) into an integrated environment that is responsive to change and supportive of the delivery of the business strategy.
> *Tradução livre:* O propósito da Arquitetura Empresarial é otimizar, em toda a empresa, o legado muitas vezes fragmentado de processos (manuais e automatizados), transformando-o em um ambiente integrado, responsivo a mudanças e que apoie a entrega da estratégia de negócio.
> — The Open Group, TOGAF Standard 9.2, Part I §1.3 Executive Overview

> The purpose of Enterprise Architecture is to optimize across the enterprise the often fragmented legacy of processes (both manual and automated) into an integrated environment that is responsive to change and supportive of the delivery of the business strategy.
> *Tradução livre:* O propósito da Arquitetura Empresarial é otimizar, em toda a empresa, o legado muitas vezes fragmentado de processos (manuais e automatizados), transformando-o em um ambiente integrado, responsivo a mudanças e que apoie a entrega da estratégia de negócio.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §1.1 Executive Overview

> Enterprise Architecture provides a framework for change, linked to both strategic direction and business value.
> *Tradução livre:* A Arquitetura Empresarial oferece um framework para a mudança, vinculado tanto à direção estratégica quanto ao valor de negócio.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), section on enterprise agility

> Enterprise architecture represents the holistic planning, analysis, design, and implementation for the development and execution of strategy by applying principles and practices to guide organizations through the integration and interoperation of all other architecture domains.
> — Business Architecture Guild®, BIZBOK® Guide v9.0 (2020), Appendix A: Glossary, *Enterprise Architecture* (FEAPO definition, adopted 2017)

> A strategic information asset base, which defines the business, the information necessary to operate the business, the technologies necessary to support the business operations, and the transitional processes necessary for implementing new technologies in response to the changing business needs. It is a representation or blueprint.
> — US Federal CIO Council, FEAF (1999), as reproduced in the [SEBoK glossary: Enterprise Architecture](https://sebokwiki.org/wiki/Enterprise_Architecture_(glossary))

> A discipline for proactively and holistically leading enterprise responses to disruptive forces by identifying and analyzing the execution of change toward desired business vision and outcomes.
> — Gartner (2013), as reproduced in the SEBoK glossary (Gartner's own glossary page blocks automated access)

## Em que diferem
- **As definições formais agora vêm de fora do The Open Group.** A FEAPO (via BIZBOK) e o FEAF federal dos EUA dão definições formais, com a Gartner como evidência de apoio. Elas se dividem em duas linhas: o FEAF e a declaração de propósito do TOGAF descrevem a EA como um *blueprint* de negócio, informação e tecnologia, enquanto a FEAPO e a Gartner a descrevem como uma *disciplina* que conduz a execução da estratégia e a mudança.
- **O BIZBOK v9 (redação da FEAPO) traz a definição formal que falta ao TOGAF**: a arquitetura corporativa como a disciplina que transforma estratégia em execução integrando todos os outros domínios de arquitetura. Ela combina com a declaração de propósito do TOGAF (9.2 §1.3, 10 §1.1), mas agora há um verbete citável de um segundo órgão.
- A frase de propósito é palavra por palavra a mesma no TOGAF 9.2 §1.3 e no TOGAF 10 §1.1.
- O TOGAF descreve a Arquitetura Empresarial pelo seu **propósito** (otimizar, integrar, possibilitar a mudança) em vez de definir o que ela **é**. A definição de trabalho preenche essa lacuna combinando §4.8 Architecture com §4.43 Enterprise; essa combinação é a interpretação da KB, não uma citação.
- O TOGAF 10 enfatiza que EA não é "apenas outro nome para arquitetura de TI" (§1.1).
- **Baseline.** A KB segue o TOGAF 9.2 (orientação do curso, 2026-11-19: a 9.2 está consolidada, enquanto o TOGAF 10 ainda está sendo atualizado). A redação do TOGAF 10 é mantida aqui para mostrar o que mudou.

## Onde se encaixa
Termo de escopo no domínio [Arquitetura](/pt-br/domains/architecture.md), acima da [Arquitetura de Solução](/pt-br/foundations/solution-architecture.md). O TOGAF 10 também nomeia níveis intermediários: Arquitetura Estratégica (§4.78) e Arquitetura de Segmento (§4.68).

## No curso
Técnicas de Arquitetura (TOGAF). Aulas ainda não ingeridas.

## Questões em aberto
- Aceitar uma definição de trabalho baseada em propósito até que um segundo órgão seja citado?
