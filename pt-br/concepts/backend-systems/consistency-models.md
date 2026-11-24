---
type: Concept
title: Modelos de consistência
description: As regras que um armazenamento de dados distribuído promete sobre quais valores as leituras podem retornar quando os dados são copiados entre nós - do forte (toda leitura vê a última escrita) ao eventual (as réplicas convergem se as escritas pararem).
tags:
- backend-systems
- core
domain: backend-systems
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Jepsen. Consistency Models (map and definitions)
  resource: 'https://jepsen.io/consistency'
  kind: docs
- title: Vogels, W. (2009). Eventually Consistent. Communications of the ACM 52(1)
  resource: 'https://doi.org/10.1145/1435417.1435432'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 5 and ch. 9 "Consistency and Consensus"
  resource: 'https://dataintensive.net/'
  kind: book
lang: pt-br
translation_of: /concepts/backend-systems/consistency-models.md
---

# Modelos de consistência

**Definição.** Um modelo de consistência é um contrato entre um armazenamento de dados e seus clientes sobre a ordem e a visibilidade das operações. Modelos fortes como a linearizabilidade fazem o sistema se comportar como uma única cópia; modelos mais fracos como consistência causal, ler-as-próprias-escritas ou consistência eventual permitem leituras desatualizadas em troca de menor latência e maior disponibilidade. O Jepsen mantém um mapa muito usado desses modelos e de como se relacionam.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Escolher um modelo é um trade-off entre garantias de correção, latência e disponibilidade durante partições de rede (CAP, e PACELC para o caso sem partição). Consistência de banco de dados (o C de ACID, sobre invariantes) é uma ideia diferente de consistência entre réplicas; confundi-las é uma fonte comum de confusão.

## Exemplo

Uma rede social guarda curtidas com consistência eventual: um amigo pode ver 41 curtidas enquanto você vê 42 por um segundo. O serviço de transferência bancária usa um armazenamento linearizável, então dois saques nunca podem ler o mesmo saldo antigo.

## Conceitos mais específicos

- [Linearizabilidade](/pt-br/concepts/backend-systems/linearizability.md)
- [Teorema CAP](/pt-br/concepts/backend-systems/cap-theorem.md)
- [Replicação e particionamento (sharding)](/pt-br/concepts/backend-systems/replication-partitioning.md)
- [Transação e ACID](/pt-br/concepts/data/transactions-acid.md)

## Fontes

1. Jepsen. *Consistency Models* (map and definitions). <https://jepsen.io/consistency> — Hierarquia dos modelos de consistência e suas definições.
2. Vogels, W. (2009). *Eventually Consistent*. Communications of the ACM 52(1). <https://doi.org/10.1145/1435417.1435432> — Consistência eventual e variantes do lado do cliente, de um operador de grande escala.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 5 and ch. 9 "Consistency and Consensus". <https://dataintensive.net/> — Anomalias de atraso de replicação e o espectro do eventual ao linearizável.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
