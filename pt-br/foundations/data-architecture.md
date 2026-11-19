---
type: Foundation
title: Arquitetura de Dados
description: Uma descrição da estrutura e da interação dos principais tipos e fontes de dados de uma empresa, de seus ativos de dados lógicos e físicos e de seus recursos de gerenciamento de dados.
tags:
- foundation
- architecture
- data-architecture
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
  section: Part I §3.36 Data Architecture
  tier: B
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.39 Data Architecture
  tier: B
evidence:
  bodies:
  - The Open Group
  meets_standard: partial
  gaps:
  - DAMA-DMBOK2 (DAMA International), chapter on Data Architecture — book, Kenji to provide
  - Data Architecture course slides (FIAP, Aula02–04) as tier E
related:
- architecture-domain
- application-architecture
domain: architecture
lang: pt-br
translation_of: /foundations/data-architecture.md
---

# Arquitetura de Dados

## Definição de trabalho
A arquitetura de dados descreve a estrutura e a interação dos principais tipos e fontes de dados da empresa, seus ativos de dados lógicos e físicos e os recursos que os gerenciam. *Segue o TOGAF 9.2 §3.36, a referência do curso; aguardando o DAMA-DMBOK2.*

## Definições autoritativas
> A description of the structure and interaction of the enterprise's major types and sources of data, logical data assets, physical data assets, and data management resources.
> *Tradução livre:* Uma descrição da estrutura e da interação dos principais tipos e fontes de dados da empresa, dos ativos de dados lógicos, dos ativos de dados físicos e dos recursos de gerenciamento de dados.
> — The Open Group, TOGAF Standard 9.2, Part I §3.36 Data Architecture

> A description of the structure of the enterprise's major types and sources of data, logical data assets, physical data assets, and data management resources.
> *Tradução livre:* Uma descrição da estrutura dos principais tipos e fontes de dados da empresa, dos ativos de dados lógicos, dos ativos de dados físicos e dos recursos de gerenciamento de dados.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.39 Data Architecture

## Em que diferem
- O TOGAF 10 retira **"and interaction"** ("e interação"): a arquitetura de dados descreve apenas a estrutura, e as interações ficam a cargo da arquitetura de aplicações.
- Padrões de plataforma de dados (data lake, streaming) do curso de Arquitetura de Dados ficam no nível de solução; a definição do TOGAF está no nível de empresa.
- **Baseline.** A KB segue o TOGAF 9.2 (orientação do curso, 2026-11-19: a 9.2 está consolidada, enquanto o TOGAF 10 ainda está sendo atualizado). A redação do TOGAF 10 é mantida aqui para mostrar o que mudou.

## Onde se encaixa
Subdomínio `data-architecture` do domínio; junto com a arquitetura de aplicações, forma a Arquitetura de Sistemas de Informação do TOGAF (ADM, Fase C).

## No curso
Arquitetura de Dados (FIAP): data lake, streaming, sistemas de visão computacional — `Materiais/Data Archictecture/`. Aulas ainda não ingeridas.

## Questões em aberto
- Nenhuma.
