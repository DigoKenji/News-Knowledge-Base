---
type: Concept
title: Deterministic simulation testing
description: Testing a distributed system by running all of its nodes in one process against simulated time, network, disk and randomness controlled by a seed, injecting faults, so any failure can be replayed exactly.
tags:
- backend-systems
- distributed-systems
domain: backend-systems
subdomain: distributed-systems
also_in:
- software-engineering/testing
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
- title: Zhou, J. et al. (2021). FoundationDB - A Distributed Unbundled Transactional Key Value Store. SIGMOD 2021
  resource: 'https://www.foundationdb.org/files/fdb-paper.pdf'
  kind: paper
- title: FoundationDB. Simulation and Testing
  resource: 'https://apple.github.io/foundationdb/testing.html'
  kind: docs
- title: Antithesis. Deterministic simulation testing (docs and resources)
  resource: 'https://antithesis.com/docs/introduction/welcome/'
  kind: practitioner
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Deterministic simulation testing

**Definition.** Deterministic simulation testing (DST) removes every source of non-determinism from a system under test (threads, clocks, network timing, I/O, random numbers) by routing them through a simulator driven by one pseudo-random seed. The simulator then explores millions of interleavings and fault combinations, and a failing seed reproduces the exact same run every time.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `distributed-systems`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

FoundationDB pioneered it: the database was written so that the whole cluster runs single-threaded inside a simulator with fake network and disks, and engineers ran the equivalent of years of operation per night with injected machine crashes, partitions, disk errors and clock jumps ("buggify" hooks). Because time is simulated, tests run faster than real time. TigerBeetle (VOPR), Turso and Antithesis (a hypervisor that makes arbitrary software deterministic) applied the idea further. It requires designing for it: all I/O behind interfaces the simulator can control, no hidden threads or system clock calls.

## Why it matters and trade-offs

Distributed bugs that appear once in a million runs become findable and debuggable. FoundationDB credits it for very few bugs reaching users. The cost is architectural: retrofitting determinism into an existing system is hard, third-party libraries may break it, and the simulator only finds faults it models.

## Example

Seed 48213 drops one message during leader election and the cluster loses a committed write; rerunning seed 48213 with a debugger reproduces the bug step by step.

## Sources

1. Zhou, J. et al. (2021). *FoundationDB - A Distributed Unbundled Transactional Key Value Store*. SIGMOD 2021. <https://www.foundationdb.org/files/fdb-paper.pdf> — Simulation testing framework, fault injection, buggify, results.
2. FoundationDB. *Simulation and Testing*. <https://apple.github.io/foundationdb/testing.html> — How the deterministic simulator works and what it injects.
3. Antithesis. *Deterministic simulation testing* (docs and resources). <https://antithesis.com/docs/introduction/welcome/> — Independent, general-purpose implementation of the approach.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
