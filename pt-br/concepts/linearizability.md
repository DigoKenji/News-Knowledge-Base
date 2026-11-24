---
type: Concept
title: Linearizabilidade
description: O modelo de consistência mais forte para um único objeto - toda operação parece acontecer instantaneamente num ponto entre seu início e seu fim, então todos os clientes veem uma única ordem atualizada das operações, coerente com o tempo real.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Herlihy, M. and Wing, J. (1990). Linearizability - A Correctness Condition for Concurrent Objects. ACM TOPLAS 12(3)
  resource: 'https://dl.acm.org/doi/10.1145/78969.78972'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 9 Consistency and Consensus. O'Reilly
  resource: 'https://dataintensive.net/'
  kind: book
- title: Jepsen. Consistency Models - Linearizability
  resource: 'https://jepsen.io/consistency/models/linearizable'
  kind: practitioner
- title: Bailis, P. (2014). Linearizability versus Serializability
  resource: 'https://www.bailis.org/blog/linearizability-versus-serializability/'
  kind: practitioner
lang: pt-br
translation_of: /concepts/linearizability.md
---

# Linearizabilidade

**Definição.** Um sistema é linearizável se, para toda execução, existe uma ordem total das operações que (1) obedece à especificação sequencial de cada objeto e (2) respeita o tempo real: se a operação A terminou antes de B começar, A vem antes de B. Informalmente, o sistema se comporta como se houvesse uma única cópia dos dados, mesmo que esteja replicado.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `system-design`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

Definida por Herlihy e Wing (1990). Assim que qualquer leitura devolve um valor novo, todas as leituras posteriores precisam devolvê-lo também: nada de voltar no tempo. Implementações precisam de coordenação: replicação com líder único e leituras no líder (com leases do líder), ou protocolos de consenso (Raft, Paxos) usados pelo etcd, pelas escritas do ZooKeeper e pelo Spanner (que adiciona o TrueTime para consistência externa). O teorema CAP diz que um sistema linearizável não consegue continuar disponível durante uma partição de rede. É diferente de serializabilidade, que trata de transações sobre muitos objetos e não exige tempo real; strict serializability combina as duas.

## Por que importa e trade-offs

Torna sistemas distribuídos fáceis de raciocinar e é exigida para locks, eleição de líder, restrições de unicidade e contadores que não podem voltar. Custa latência (idas e voltas de coordenação, muitas vezes entre regiões) e disponibilidade durante partições, por isso muitos sistemas oferecem modelos mais fracos (sequencial, causal, eventual) por padrão.

## Exemplo

Alice atualiza a foto de perfil e a chamada retorna. Se Bob em seguida carrega a página e vê a foto antiga vinda de uma réplica atrasada, o sistema não é linearizável.

## Fontes

1. Herlihy, M. and Wing, J. (1990). *Linearizability - A Correctness Condition for Concurrent Objects*. ACM TOPLAS 12(3). <https://dl.acm.org/doi/10.1145/78969.78972> — Definição formal.
2. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 9 Consistency and Consensus. O'Reilly. <https://dataintensive.net/> — Explicação prática, implementações, custo, relação com o CAP.
3. Jepsen. *Consistency Models - Linearizability*. <https://jepsen.io/consistency/models/linearizable> — Posição na hierarquia de modelos de consistência; o que garante e o que não garante.
4. Bailis, P. (2014). *Linearizability versus Serializability*. <https://www.bailis.org/blog/linearizability-versus-serializability/> — A diferença entre os dois termos e strict serializability.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
