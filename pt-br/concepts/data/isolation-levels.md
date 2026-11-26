---
type: Concept
title: 'Níveis de isolamento de transações'
description: 'O quanto um banco protege transações concorrentes umas das outras - de read committed e snapshot isolation até a serializabilidade completa - com cada nível evitando mais anomalias (leituras sujas, atualizações perdidas, write skew) a um custo maior.'
tags:
- data
- core
domain: data
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 7 Transactions: The Meaning of ACID; Weak Isolation Levels; Snapshot Isolation and Repeatable Read; Serializability (2PL, SSI); ch. 9 Linearizability Versus Serializability'
- title: 'Berenson, H. et al. (1995). A Critique of ANSI SQL Isolation Levels. SIGMOD 1995'
  resource: 'https://doi.org/10.1145/223784.223785'
  kind: paper
- title: 'PostgreSQL Global Development Group. PostgreSQL Documentation - Transaction Isolation'
  resource: 'https://www.postgresql.org/docs/current/transaction-iso.html'
  kind: docs
lang: pt-br
translation_of: /concepts/data/isolation-levels.md
---

# Níveis de isolamento de transações

**Definição.** O isolamento é o I do ACID: idealmente, cada transação se comporta como se rodasse sozinha (serializabilidade). Como isso é caro, os bancos oferecem níveis mais fracos. Kleppmann os percorre pelas anomalias que permitem: read committed evita leituras sujas e escritas sujas; snapshot isolation (muitas vezes chamado de repeatable read) dá a cada transação um retrato consistente, geralmente via controle de concorrência multiversão, mas ainda permite atualizações perdidas e write skew; a serializabilidade evita todas as condições de corrida e é implementada por execução serial de fato, bloqueio em duas fases (2PL) ou serializable snapshot isolation (SSI). Ele ressalta que os nomes dos níveis são usados de forma inconsistente entre bancos, então é preciso verificar o que o seu realmente garante.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `core`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-26); rascunho aguardando certificação do Kenji.

## Como se encaixa

A maioria dos bancos em produção usa por padrão um nível fraco (read committed no PostgreSQL e no Oracle, repeatable read no MySQL), então bugs de concorrência, como reservas duplicadas, são problema da aplicação, a menos que se eleve o nível ou se bloqueie explicitamente. Isolamento trata de transações concorrentes em um banco; linearizabilidade é outra garantia, sobre réplicas, e Kleppmann compara as duas no cap. 9.

## Exemplo

Dois médicos saem do plantão no mesmo instante; cada transação verifica que outro médico ainda está de plantão e então se remove. Sob snapshot isolation as duas têm sucesso e ninguém fica de plantão (write skew); sob isolamento serializável uma delas é abortada.

## Conceitos mais amplos

- [Transação e ACID](/pt-br/concepts/data/transactions-acid.md)

## Conceitos relacionados

- [Linearizabilidade](/pt-br/concepts/backend-systems/linearizability.md)
- [Modelos de consistência](/pt-br/concepts/backend-systems/consistency-models.md)

## Fontes

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 7 Transactions: The Meaning of ACID; Weak Isolation Levels; Snapshot Isolation and Repeatable Read; Serializability (2PL, SSI); ch. 9 Linearizability Versus Serializability. Níveis de isolamento explicados pelas anomalias que permitem, com o exemplo de write skew dos médicos de plantão.
2. Berenson, H. et al. (1995). *A Critique of ANSI SQL Isolation Levels*. SIGMOD 1995. <https://doi.org/10.1145/223784.223785> — Uma crítica aos níveis de isolamento do ANSI SQL.
3. PostgreSQL Global Development Group. PostgreSQL Documentation - Transaction Isolation. <https://www.postgresql.org/docs/current/transaction-iso.html> — Documentação do PostgreSQL sobre isolamento de transações.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
