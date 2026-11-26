---
type: Concept
title: 'Transaction isolation levels'
description: 'How far a database shields concurrent transactions from each other - from read committed and snapshot isolation to full serializability - each level preventing more anomalies (dirty reads, lost updates, write skew) at a higher cost.'
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
---

# Transaction isolation levels

**Definition.** Isolation is the I in ACID: ideally each transaction behaves as if it ran alone (serializability). Because that is expensive, databases offer weaker levels. Kleppmann walks through them by the anomalies they allow: read committed prevents dirty reads and dirty writes; snapshot isolation (often called repeatable read) gives each transaction a consistent snapshot, usually via multi-version concurrency control, but still allows lost updates and write skew; serializability prevents all races and is implemented by actual serial execution, two-phase locking, or serializable snapshot isolation (SSI). He stresses that level names are used inconsistently across databases, so you must check what yours actually guarantees.

Domain: [data](/domains/data.md) · subdomain `core`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-26); draft waiting for Kenji to certify.

## How it fits

Most production databases default to a weak level (read committed in PostgreSQL and Oracle, repeatable read in MySQL), so concurrency bugs such as double bookings are an application concern unless you raise the level or lock explicitly. Isolation is about concurrent transactions on one database; linearizability is a different guarantee about replicas, and Kleppmann contrasts the two in ch. 9.

## Example

Two doctors go off call at the same moment; each transaction checks that another doctor is still on call and then removes itself. Under snapshot isolation both succeed and nobody is on call (write skew); under serializable isolation one of them is aborted.

## Broader concepts

- [Transaction and ACID](/concepts/data/transactions-acid.md)

## Related concepts

- [Linearizability](/concepts/backend-systems/linearizability.md)
- [Consistency models](/concepts/backend-systems/consistency-models.md)

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 7 Transactions: The Meaning of ACID; Weak Isolation Levels; Snapshot Isolation and Repeatable Read; Serializability (2PL, SSI); ch. 9 Linearizability Versus Serializability. Isolation levels explained through the anomalies they allow, with the doctors on-call write-skew example.
2. Berenson, H. et al. (1995). *A Critique of ANSI SQL Isolation Levels*. SIGMOD 1995. <https://doi.org/10.1145/223784.223785> — A Critique of ANSI SQL Isolation Levels.
3. PostgreSQL Global Development Group. PostgreSQL Documentation - Transaction Isolation. <https://www.postgresql.org/docs/current/transaction-iso.html> — PostgreSQL documentation on transaction isolation.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
