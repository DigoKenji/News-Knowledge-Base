---
type: Foundation
title: Arquitetura de Dados
description: Uma descrição da estrutura dos principais tipos e fontes de dados de uma empresa, de seus ativos de dados lógicos e físicos e de seus recursos de gerenciamento de dados.
tags:
- foundation
- architecture
- data-architecture
status: draft
generated:
  by: claude-opus (interactive foundation session)
  at: '2026-11-18T18:30:00-03:00'
sources:
- title: TOGAF Standard — Introduction and Core Concepts (C220 Part 0)
  publisher: The Open Group
  edition: 10th Edition, 2025 update (evaluation copy)
  section: §4.39 Data Architecture
  tier: B
- title: The TOGAF Standard, Version 9.2
  publisher: The Open Group
  edition: 9.2 (2018)
  section: Part I §3.36 Data Architecture
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
A arquitetura de dados descreve quais dados a empresa possui e de onde vêm, como são organizados lógica e fisicamente e quais recursos os gerenciam. *Segue o TOGAF 10 §4.39, aguardando o DAMA-DMBOK2.*

## Definições autoritativas
> A description of the structure of the enterprise's major types and sources of data, logical data assets, physical data assets, and data management resources.
> *Tradução livre:* Uma descrição da estrutura dos principais tipos e fontes de dados da empresa, dos ativos de dados lógicos, dos ativos de dados físicos e dos recursos de gerenciamento de dados.
> — The Open Group, TOGAF Standard 10th Ed. (C220 Part 0), §4.39 Data Architecture

> A description of the structure and interaction of the enterprise's major types and sources of data, logical data assets, physical data assets, and data management resources.
> *Tradução livre:* Uma descrição da estrutura e da interação dos principais tipos e fontes de dados da empresa, dos ativos de dados lógicos, dos ativos de dados físicos e dos recursos de gerenciamento de dados.
> — The Open Group, TOGAF Standard 9.2, Part I §3.36 Data Architecture

## Em que diferem
- O TOGAF 10 retira **"and interaction"** ("e interação"): a arquitetura de dados descreve apenas a estrutura, e as interações ficam a cargo da arquitetura de aplicações.
- Padrões de plataforma de dados (data lake, streaming) do curso de Arquitetura de Dados ficam no nível de solução; a definição do TOGAF está no nível de empresa.

## Onde se encaixa
Subdomínio `data-architecture` do domínio; junto com a arquitetura de aplicações, forma a Arquitetura de Sistemas de Informação do TOGAF (ADM, Fase C).

## No curso
Arquitetura de Dados (FIAP): data lake, streaming, sistemas de visão computacional — `Materiais/Data Archictecture/`. Aulas ainda não ingeridas.

## Questões em aberto
- Nenhuma.
