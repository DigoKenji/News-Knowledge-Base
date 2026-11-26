---
type: Concept
title: Transaction and ACID
description: A transaction groups several reads and writes into one unit that either fully happens or not at all; ACID names its guarantees - atomicity, consistency, isolation and durability.
tags:
- data
- databases
- core
domain: data
subdomain: databases
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
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Transaction and ACID

**Definition.** Härder and Reuter coined ACID in 1983. Atomicity: all of a transaction's changes are applied or none are. Consistency: a transaction moves the database from one valid state to another (invariants hold). Isolation: concurrent transactions do not see each other's partial work; the strongest level is serializability, and weaker levels (read committed, snapshot isolation) allow specific anomalies. Durability: once committed, changes survive crashes, typically via a write-ahead log.

Domain: [data](/domains/data.md) · subdomain `databases`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Transactions let developers ignore whole classes of concurrency and crash bugs. Across services or databases a single ACID transaction is usually impractical (two-phase commit is slow and blocking), so distributed designs use sagas with compensating actions and idempotent operations instead. Knowing which isolation level your database really uses by default prevents subtle bugs.

## Example

Transferring R$100 debits account A and credits account B in one transaction. If the server crashes after the debit, atomicity rolls it back on restart; isolation stops a concurrent balance report from seeing the money missing from both accounts.

## Narrower concepts

- [Saga pattern](/concepts/backend-systems/saga-pattern.md)
- [Idempotency](/concepts/backend-systems/idempotency.md)
- [Linearizability](/concepts/backend-systems/linearizability.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 7 Transactions: The Meaning of ACID; Weak Isolation Levels; Serializability: What each ACID letter really promises, and why "consistency" is an application property.

## Sources

1. Härder, T. and Reuter, A. (1983). *Principles of Transaction-Oriented Database Recovery*. ACM Computing Surveys 15(4). <https://doi.org/10.1145/289.291> — Origin of the ACID acronym and recovery principles.
2. Gray, J. and Reuter, A. (1992). *Transaction Processing - Concepts and Techniques*. Morgan Kaufmann. <https://shop.elsevier.com/books/transaction-processing/gray/978-0-08-051955-5> — Reference text on transactions, logging and concurrency control.
3. Berenson, H. et al. (1995). *A Critique of ANSI SQL Isolation Levels*. SIGMOD 1995. <https://doi.org/10.1145/223784.223785> — Isolation levels, anomalies and snapshot isolation.
4. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 7 Transactions: The Meaning of ACID; Weak Isolation Levels; Serializability. What each ACID letter really promises, and why "consistency" is an application property.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
