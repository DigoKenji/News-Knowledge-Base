---
type: Concept
title: Migração strangler fig
description: 'Migrar um sistema legado aos poucos, redirecionando funcionalidades uma a uma para uma nova implementação ao redor dele, até que o sistema antigo possa ser desligado.'
tags:
- architecture
- software-architecture
domain: architecture
subdomain: software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Fowler, M. (2004). StranglerFigApplication'
  resource: 'https://martinfowler.com/bliki/StranglerFigApplication.html'
  kind: practitioner
- title: 'Microsoft. Strangler Fig pattern (Azure Architecture Center)'
  resource: 'https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig'
  kind: docs
- title: 'AWS. Strangler fig pattern (Prescriptive Guidance: cloud design patterns)'
  resource: 'https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/strangler-fig.html'
  kind: docs
relations:
- type: uses
  target: /concepts/architecture/bounded-context.md
lang: pt-br
translation_of: /concepts/architecture/strangler-fig.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; movido para architecture; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Migração strangler fig

**Definição.** O padrão strangler fig substitui um sistema legado aos poucos: uma fachada ou camada de roteamento fica na frente dele, funcionalidades novas ou reescritas são construídas em um sistema novo, e o tráfego de cada funcionalidade é trocado uma por vez até que o sistema antigo não atenda mais nada e possa ser removido. Martin Fowler deu o nome em 2004, inspirado nas figueiras que crescem em volta de uma árvore hospedeira e acabam por substituí-la.

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `software-architecture`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

Evita o risco de uma reescrita total de uma vez: entrega valor cedo e cada passo pode ser revertido. Seus custos são a camada de roteamento, manter os dados sincronizados entre o antigo e o novo durante a transição, e a disciplina de realmente terminar. Contextos delimitados são um guia comum para escolher a próxima fatia a migrar.

## Exemplo

Um API gateway envia /invoices para um serviço novo enquanto todos os outros caminhos ainda chegam ao monólito; meses depois /payments segue o mesmo caminho, e o módulo de cobrança do monólito é apagado.

## Fontes

1. Fowler, M. (2004). StranglerFigApplication. <https://martinfowler.com/bliki/StranglerFigApplication.html> — Origem e fundamento do padrão.
2. Microsoft. Strangler Fig pattern (Azure Architecture Center). <https://learn.microsoft.com/en-us/azure/architecture/patterns/strangler-fig> — Fachada, problemas e quando não usar.
3. AWS. Strangler fig pattern (Prescriptive Guidance: cloud design patterns). <https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/strangler-fig.html> — Migração passo a passo com uma camada de proxy.

## Relações
- **usa** [Contexto delimitado](/pt-br/concepts/architecture/bounded-context.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
