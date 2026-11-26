---
type: Concept
title: Transação e ACID
description: Uma transação agrupa várias leituras e escritas em uma unidade que acontece por inteiro ou não acontece; ACID nomeia suas garantias - atomicidade, consistência, isolamento e durabilidade.
tags:
- data
- core
domain: data
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Härder, T. and Reuter, A. (1983). Principles of Transaction-Oriented Database Recovery. ACM Computing Surveys 15(4)
  resource: 'https://doi.org/10.1145/289.291'
  kind: paper
- title: Gray, J. and Reuter, A. (1992). Transaction Processing - Concepts and Techniques. Morgan Kaufmann
  resource: 'https://shop.elsevier.com/books/transaction-processing/gray/978-0-08-051955-5'
  kind: book
- title: Berenson, H. et al. (1995). A Critique of ANSI SQL Isolation Levels. SIGMOD 1995
  resource: 'https://doi.org/10.1145/223784.223785'
  kind: paper
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 7 Transactions: The Meaning of ACID; Weak Isolation Levels; Serializability'
lang: pt-br
translation_of: /concepts/data/transactions-acid.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Transação e ACID

**Definição.** Härder e Reuter cunharam ACID em 1983. Atomicidade: todas as mudanças de uma transação são aplicadas ou nenhuma é. Consistência: a transação leva o banco de um estado válido a outro (os invariantes se mantêm). Isolamento: transações concorrentes não veem o trabalho parcial umas das outras; o nível mais forte é a serializabilidade, e níveis mais fracos (read committed, snapshot isolation) permitem anomalias específicas. Durabilidade: depois de confirmadas, as mudanças sobrevivem a quedas, tipicamente via log de escrita antecipada.

Domínio: [data](/pt-br/domains/data.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Transações permitem aos desenvolvedores ignorar classes inteiras de bugs de concorrência e de queda. Entre serviços ou bancos, uma única transação ACID costuma ser impraticável (o commit em duas fases é lento e bloqueante), então projetos distribuídos usam sagas com ações compensatórias e operações idempotentes. Saber qual nível de isolamento seu banco usa de fato por padrão evita bugs sutis.

## Exemplo

Transferir R$100 debita a conta A e credita a conta B numa só transação. Se o servidor cair após o débito, a atomicidade o desfaz ao reiniciar; o isolamento impede que um relatório de saldo concorrente veja o dinheiro faltando nas duas contas.

## Conceitos mais específicos

- [Padrão Saga](/pt-br/concepts/backend-systems/saga-pattern.md)
- [Idempotência](/pt-br/concepts/backend-systems/idempotency.md)
- [Linearizabilidade](/pt-br/concepts/backend-systems/linearizability.md)

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (DDIA adicionado em 2026-11-26). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 7 Transactions: The Meaning of ACID; Weak Isolation Levels; Serializability: O que cada letra do ACID realmente promete, e por que "consistência" é uma propriedade da aplicação.

## Fontes

1. Härder, T. and Reuter, A. (1983). *Principles of Transaction-Oriented Database Recovery*. ACM Computing Surveys 15(4). <https://doi.org/10.1145/289.291> — Origem do acrônimo ACID e princípios de recuperação.
2. Gray, J. and Reuter, A. (1992). *Transaction Processing - Concepts and Techniques*. Morgan Kaufmann. <https://shop.elsevier.com/books/transaction-processing/gray/978-0-08-051955-5> — Texto de referência sobre transações, logging e controle de concorrência.
3. Berenson, H. et al. (1995). *A Critique of ANSI SQL Isolation Levels*. SIGMOD 1995. <https://doi.org/10.1145/223784.223785> — Níveis de isolamento, anomalias e snapshot isolation.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 7 Transactions: The Meaning of ACID; Weak Isolation Levels; Serializability. O que cada letra do ACID realmente promete, e por que "consistência" é uma propriedade da aplicação.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
