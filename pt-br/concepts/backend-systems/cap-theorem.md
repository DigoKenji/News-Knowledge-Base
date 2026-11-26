---
type: Concept
title: Teorema CAP
description: 'O resultado de que um armazenamento de dados replicado não pode ser ao mesmo tempo linearizavelmente consistente e totalmente disponível enquanto a rede está particionada - precisa escolher um durante a partição.'
tags:
- backend-systems
- distributed-systems
domain: backend-systems
subdomain: distributed-systems
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem'
lang: pt-br
translation_of: /concepts/backend-systems/cap-theorem.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
---

# Teorema CAP

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `distributed-systems`.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem: Argumenta que "escolha 2 de 3" engana, porque partições não são opcionais; melhor ler como "consistente ou disponível quando particionado".

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem. Argumenta que "escolha 2 de 3" engana, porque partições não são opcionais; melhor ler como "consistente ou disponível quando particionado".

## Relações
- [Padrão Saga](/pt-br/concepts/backend-systems/saga-pattern.md) **mitiga** este

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
