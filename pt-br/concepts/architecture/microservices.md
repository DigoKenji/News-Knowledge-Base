---
type: Concept
title: Microservices
description: Um estilo arquitetural que constrói uma aplicação como um conjunto de serviços pequenos e implantados de forma independente, cada um dono de seus dados e de uma capacidade de negócio, comunicando-se pela rede.
tags:
- architecture
- software-architecture
- core
domain: architecture
subdomain: software-architecture
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Lewis, J. and Fowler, M. (2014). Microservices. martinfowler.com
  resource: 'https://martinfowler.com/articles/microservices.html'
  kind: practitioner
- title: 'Dragoni, N. et al. (2017). Microservices - yesterday, today, and tomorrow. arXiv:1606.04036'
  resource: 'https://arxiv.org/abs/1606.04036'
  kind: paper
- title: Newman, S. (2021). Building Microservices, 2nd ed. O'Reilly
  resource: 'https://samnewman.io/books/building_microservices_2nd_edition/'
  kind: book
lang: pt-br
translation_of: /concepts/architecture/microservices.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Microservices

**Definição.** Lewis e Fowler (2014) descrevem microservices como uma abordagem para desenvolver uma única aplicação como um conjunto de pequenos serviços, cada um rodando em seu próprio processo e se comunicando por mecanismos leves, construídos em torno de capacidades de negócio e implantados de forma independente por automação completa. Cada serviço é dono do seu banco de dados ("gestão descentralizada de dados").

Domínio: [architecture](/pt-br/domains/architecture.md) · subdomínio `software-architecture`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Microservices permitem que muitas equipes façam deploy e escalem de forma independente, por isso grandes organizações os adotam. Os custos são os de sistemas distribuídos: falhas de rede, consistência eventual, transações distribuídas (sagas), observabilidade e sobrecarga operacional. Muitas equipes hoje começam com um monólito modular e só extraem serviços quando necessário.

## Exemplo

Uma loja online se divide em serviços de catálogo, carrinho, pagamento e entrega; a equipe de pagamento pode entregar três vezes por dia sem coordenar com a equipe de catálogo.

## Conceitos mais específicos

- [Padrão Saga](/pt-br/concepts/backend-systems/saga-pattern.md)
- [Contexto delimitado](/pt-br/concepts/architecture/bounded-context.md)
- [Versionamento de API](/pt-br/concepts/backend-systems/api-versioning.md)
- [Migração strangler fig](/pt-br/concepts/architecture/strangler-fig.md)

## Fontes

1. Lewis, J. and Fowler, M. (2014). *Microservices*. martinfowler.com. <https://martinfowler.com/articles/microservices.html> — A descrição que definiu o termo e suas características.
2. Dragoni, N. et al. (2017). *Microservices - yesterday, today, and tomorrow*. arXiv:1606.04036. <https://arxiv.org/abs/1606.04036> — Revisão acadêmica do estilo e problemas em aberto.
3. Newman, S. (2021). *Building Microservices*, 2nd ed. O'Reilly. <https://samnewman.io/books/building_microservices_2nd_edition/> — Orientação prática, incluindo quando não usar microservices.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
