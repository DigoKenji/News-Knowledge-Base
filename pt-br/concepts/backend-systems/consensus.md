---
type: Concept
title: Consenso
description: O problema de fazer vários nós concordarem sobre um valor (ou um log ordenado de comandos) mesmo quando alguns nós caem ou mensagens atrasam - resolvido na prática por algoritmos como Paxos e Raft.
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
- title: Lamport, L. (2001). Paxos Made Simple. ACM SIGACT News 32(4)
  resource: 'https://lamport.azurewebsites.net/pubs/paxos-simple.pdf'
  kind: paper
- title: Ongaro, D. and Ousterhout, J. (2014). In Search of an Understandable Consensus Algorithm. USENIX ATC 2014
  resource: 'https://raft.github.io/raft.pdf'
  kind: paper
- title: Fischer, M. J., Lynch, N. A. and Paterson, M. S. (1985). Impossibility of Distributed Consensus with One Faulty Process. Journal of the ACM 32(2)
  resource: 'https://doi.org/10.1145/3149.214121'
  kind: paper
lang: pt-br
translation_of: /concepts/backend-systems/consensus.md
---

# Consenso

**Definição.** Um algoritmo de consenso permite que um grupo de processos concorde sobre um valor de forma que todos os processos sem falha decidam o mesmo valor, o valor tenha sido proposto por alguém e eles acabem decidindo. Fischer, Lynch e Paterson provaram que nenhum algoritmo determinístico garante isso em um sistema totalmente assíncrono com sequer uma queda, então algoritmos práticos (Paxos, Raft) são sempre seguros e progridem quando a rede está calma o suficiente. Eles precisam de uma maioria (quórum) de nós ativos.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

O consenso está por trás de eleição de líder, locks distribuídos, armazenamentos de configuração (etcd, ZooKeeper) e bancos replicados fortemente consistentes. Executá-lo em toda escrita custa latência, então os sistemas costumam reservá-lo ao plano de controle ou a metadados, e usam replicação mais barata para os dados em massa.

## Exemplo

Um cluster Kubernetes guarda seu estado em um cluster etcd de 3 nós rodando Raft. Se um nó do etcd morre, os outros dois ainda formam maioria e continuam aceitando mudanças; se dois morrem, o cluster recusa escritas em vez de arriscar duas versões conflitantes da verdade.

## Conceitos mais específicos

- [Plano de controle](/pt-br/concepts/backend-systems/control-plane.md)
- [Linearizabilidade](/pt-br/concepts/backend-systems/linearizability.md)

## Fontes

1. Lamport, L. (2001). *Paxos Made Simple*. ACM SIGACT News 32(4). <https://lamport.azurewebsites.net/pubs/paxos-simple.pdf> — O algoritmo Paxos e os requisitos de segurança do consenso.
2. Ongaro, D. and Ousterhout, J. (2014). *In Search of an Understandable Consensus Algorithm*. USENIX ATC 2014. <https://raft.github.io/raft.pdf> — Raft - eleição de líder, replicação de log e quóruns de maioria.
3. Fischer, M. J., Lynch, N. A. and Paterson, M. S. (1985). *Impossibility of Distributed Consensus with One Faulty Process*. Journal of the ACM 32(2). <https://doi.org/10.1145/3149.214121> — Resultado de impossibilidade FLP em sistemas assíncronos.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
