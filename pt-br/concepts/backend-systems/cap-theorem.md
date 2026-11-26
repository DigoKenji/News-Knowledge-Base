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
- title: 'Gilbert, S. and Lynch, N. (2002). Brewer''s Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services. ACM SIGACT News 33(2)'
  resource: 'https://doi.org/10.1145/564585.564601'
  kind: paper
- title: 'Brewer, E. (2012). CAP Twelve Years Later: How the "Rules" Have Changed. IEEE Computer 45(2)'
  resource: 'https://doi.org/10.1109/MC.2012.37'
  kind: paper
lang: pt-br
translation_of: /concepts/backend-systems/cap-theorem.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio; descrição real'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'stub expandido: definição, encaixe, exemplo e fontes independentes'
---

# Teorema CAP

**Definição.** O teorema CAP, conjecturado por Eric Brewer em 2000 e provado por Gilbert e Lynch em 2002, diz que um armazenamento de dados replicado não consegue garantir ao mesmo tempo consistência (linearizabilidade) e disponibilidade (toda requisição a um nó que não falhou recebe resposta) quando a rede está particionada. Como partições acontecem queira-se ou não, a escolha real é o que o sistema faz durante uma: recusar algumas requisições para continuar consistente, ou respondê-las arriscando dados desatualizados.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `distributed-systems`. Expandido em 2026-11-26 com fontes independentes.

## Como se encaixa

O próprio Brewer depois alertou que 'escolha dois de três' simplifica demais: a escolha é por operação e só durante partições, e os trade-offs de latência importam o tempo todo (PACELC). Kleppmann considera o CAP pouco útil para projetar e prefere modelos de consistência precisos.

## Exemplo

Durante uma falha de link entre dois data centers, o razão contábil de um banco rejeita escritas no lado minoritário (consistente), enquanto um carrinho de compras continua aceitando itens nos dois lados e mescla depois (disponível).

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem: Argumenta que "escolha 2 de 3" engana, porque partições não são opcionais; melhor ler como "consistente ou disponível quando particionado".

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 9 Consistency and Consensus: The Cost of Linearizability; The CAP theorem; The Unhelpful CAP Theorem. Argumenta que "escolha 2 de 3" engana, porque partições não são opcionais; melhor ler como "consistente ou disponível quando particionado".
2. Gilbert, S. and Lynch, N. (2002). Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services. ACM SIGACT News 33(2). <https://doi.org/10.1145/564585.564601> — A prova formal.
3. Brewer, E. (2012). CAP Twelve Years Later: How the "Rules" Have Changed. IEEE Computer 45(2). <https://doi.org/10.1109/MC.2012.37> — A correção do próprio Brewer à leitura 'dois de três'.

## Relações
- [Padrão Saga](/pt-br/concepts/backend-systems/saga-pattern.md) **mitiga** este

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
