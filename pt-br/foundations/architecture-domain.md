---
type: Foundation
title: Domínio de arquitetura
description: 'Uma das áreas em que uma arquitetura empresarial é dividida; o TOGAF usa quatro domínios primários: negócio, dados, aplicação e tecnologia.'
tags:
- foundation
- architecture
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
  section: Part I §3.11 Architecture Domain
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.12 Architecture Domain
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - 'A second body: ArchiMate 3.2 layers (same body), ISO/IEC/IEEE 42010:2022 (uses ''architecture aspects'' instead), or Zachman Framework (columns/rows; tier D)'
related:
- business-architecture
- data-architecture
- application-architecture
- technology-architecture
- enterprise-architecture
domain: architecture
lang: pt-br
translation_of: /foundations/architecture-domain.md
---

# Domínio de arquitetura

## Definição de trabalho
Um domínio de arquitetura é uma área de preocupação em que uma arquitetura empresarial é dividida. O TOGAF tem quatro primários (negócio, dados, aplicação e tecnologia); outros domínios, como segurança, também podem ser considerados. *Segue o TOGAF 9.2 §3.11, a referência do curso.*

## Definições autoritativas
> The architectural area being considered. The TOGAF framework has four primary architecture domains: business, data, application, and technology. Other domains may also be considered (e.g., security).
> *Tradução livre:* A área arquitetural considerada. O framework TOGAF tem quatro domínios de arquitetura primários: negócio, dados, aplicação e tecnologia. Outros domínios também podem ser considerados (por exemplo, segurança).
> — The Open Group, TOGAF Standard 9.2, Part I §3.11 Architecture Domain

> The architectural area being considered. The TOGAF framework follows the tradition of dividing Enterprise Architecture into four primary architecture domains: business, data, application, and technology. Other domains (motivation, security, governance, etc.) may span those four primary domains.
> *Tradução livre:* A área arquitetural considerada. O framework TOGAF segue a tradição de dividir a Arquitetura Empresarial em quatro domínios de arquitetura primários: negócio, dados, aplicação e tecnologia. Outros domínios (motivação, segurança, governança etc.) podem atravessar esses quatro domínios primários.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.12 Architecture Domain

## Em que diferem
- O TOGAF 10 reescreve a 9.2: ele “segue a tradição de dividir a Arquitetura Empresarial” nos quatro domínios e nomeia motivação, segurança e governança como domínios que podem atravessá-los; a 9.2 dá apenas a segurança como exemplo.
- Empresa e solução **não** são domínios no TOGAF: são escopos ou níveis (ver [Arquitetura Empresarial](/pt-br/foundations/enterprise-architecture.md), [Arquitetura de Solução](/pt-br/foundations/solution-architecture.md)). Dados e aplicação às vezes são agrupados como *Arquitetura de Sistemas de Informação* (TOGAF ADM, Fase C).
- **Baseline.** A KB segue o TOGAF 9.2 (orientação do curso, 2026-11-19: a 9.2 está consolidada, enquanto o TOGAF 10 ainda está sendo atualizado). A redação do TOGAF 10 é mantida aqui para mostrar o que mudou.

## Onde se encaixa
Organiza os subdomínios (BDAT) do domínio [Arquitetura](/pt-br/domains/architecture.md).

## No curso
Técnicas de Arquitetura; o curso de Arquitetura de Dados cobre o domínio de dados. Aulas ainda não ingeridas.

## Questões em aberto
- Tratar a arquitetura de segurança como um quinto subdomínio, ou como uma preocupação transversal, como sugere o TOGAF?
