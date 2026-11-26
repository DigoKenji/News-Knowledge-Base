---
type: Concept
title: Registro de decisão de arquitetura
description: 'Um documento curto que registra uma decisão de arquitetura importante - contexto, opções consideradas, a escolha e suas consequências - mantido junto ao código.'
tags:
- architecture
- architecture-practice
domain: architecture
subdomain: architecture-practice
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 6 Making Decisions; ch. 8 Is This Architecture? (Architectural Decisions)'
- title: 'Nygard, M. (2011). Documenting Architecture Decisions. Cognitect blog'
  resource: 'https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions'
  kind: practitioner
- title: 'ADR GitHub organization. Architectural Decision Records (templates and tooling)'
  resource: 'https://adr.github.io/'
  kind: docs
- title: 'ISO/IEC/IEEE 42010 - Architecture description (overview site)'
  resource: 'http://www.iso-architecture.org/42010/'
  kind: standard
lang: pt-br
translation_of: /concepts/architecture/adr.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; movido para architecture; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Registro de decisão de arquitetura

**Definição.** Um registro de decisão de arquitetura captura uma decisão arquiteturalmente relevante em um arquivo de texto curto e numerado: título, status (proposta, aceita, substituída), contexto, decisão e consequências. Michael Nygard propôs o formato em 2011 para que os motivos por trás da forma de um sistema sobrevivam às mudanças de time; os registros não são editados depois de aceitos, apenas substituídos por novos.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `architecture-practice`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

ADRs são a forma leve e versionada do que a ISO/IEC/IEEE 42010 chama de justificativa de arquitetura, e passam no teste de Hohpe de que a documentação de arquitetura precisa conter decisões. Guardados no repositório junto ao código, são revisados como código. O próprio News-Automation registra seu design em docs/adr/.

## Exemplo

O ADR-0006 do News-Automation registra que o Claude nunca busca o Medium por conta própria e que o Hermes entrega o texto dos artigos no inbox, com as alternativas rejeitadas e o motivo.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (Architect Elevator adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 6 Making Decisions; ch. 8 Is This Architecture? (Architectural Decisions): Avaliar uma decisão pela informação e pelo risco do momento, e não pelo resultado; documentos de arquitetura valem a pena quando registram decisões e justificativas.

## Fontes

1. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 6 Making Decisions; ch. 8 Is This Architecture? (Architectural Decisions). Avaliar uma decisão pela informação e pelo risco do momento, e não pelo resultado; documentos de arquitetura valem a pena quando registram decisões e justificativas.
2. Nygard, M. (2011). Documenting Architecture Decisions. Cognitect blog. <https://cognitect.com/blog/2011/11/15/documenting-architecture-decisions> — Origem do formato ADR.
3. ADR GitHub organization. Architectural Decision Records (templates and tooling). <https://adr.github.io/> — Modelos (Nygard, MADR) e orientação prática.
4. ISO/IEC/IEEE 42010 - Architecture description (overview site). <http://www.iso-architecture.org/42010/> — Decisões e justificativas de arquitetura como parte de uma descrição de arquitetura.

## Relações
- [Análise de trade-offs ATAM](/pt-br/concepts/architecture/atam.md) **parte-de** este

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
