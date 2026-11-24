---
type: Concept
title: Batch vs stream processing
description: Batch processing runs over a bounded set of data collected over time (for example last night's logs); stream processing handles an unbounded flow of events continuously, producing results within seconds of each event.
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
- title: Dean, J. and Ghemawat, S. (2004). MapReduce - Simplified Data Processing on Large Clusters. OSDI 2004
  resource: 'https://www.usenix.org/conference/osdi-04/mapreduce-simplified-data-processing-large-clusters'
  kind: paper
- title: Akidau, T. et al. (2015). The Dataflow Model - A Practical Approach to Balancing Correctness, Latency, and Cost in Massive-Scale, Unbounded, Out-of-Order Data Processing. PVLDB 8(12)
  resource: 'https://doi.org/10.14778/2824032.2824076'
  kind: paper
- title: Kleppmann, M. (2017). Designing Data-Intensive Applications, ch. 10 "Batch Processing" and ch. 11 "Stream Processing"
  resource: 'https://dataintensive.net/'
  kind: book
---

# Batch vs stream processing

**Definition.** MapReduce popularised large-scale batch processing: split a big, finite input, process pieces in parallel, combine the results. Stream processing treats data as an unbounded sequence of events; Akidau et al.'s Dataflow model frames the questions every pipeline must answer - what is computed, where in event time (windows), when results are emitted (watermarks and triggers) and how late data refines them - and shows batch as a special case of streaming.

Domain: [data](/domains/data.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Batch is simpler, cheaper and easier to re-run; streaming gives freshness (fraud detection, live dashboards, alerting) at the cost of handling out-of-order events, state and exactly-once semantics. Many platforms use streaming for ingestion (Kafka, change data capture) and batch for heavy recomputation.

## Example

A bank recomputes customer risk scores in a nightly batch job over all transactions, while a stream job scores each card payment within 200 ms and blocks it if it looks fraudulent.

## Narrower concepts

- [Change data capture](/concepts/data/change-data-capture.md)
- [Message queue and asynchronous messaging](/concepts/backend-systems/message-queue.md)

## Sources

1. Dean, J. and Ghemawat, S. (2004). *MapReduce - Simplified Data Processing on Large Clusters*. OSDI 2004. <https://www.usenix.org/conference/osdi-04/mapreduce-simplified-data-processing-large-clusters> — Large-scale batch processing model.
2. Akidau, T. et al. (2015). *The Dataflow Model - A Practical Approach to Balancing Correctness, Latency, and Cost in Massive-Scale, Unbounded, Out-of-Order Data Processing*. PVLDB 8(12). <https://doi.org/10.14778/2824032.2824076> — Unbounded data, event time, windows, watermarks and triggers.
3. Kleppmann, M. (2017). *Designing Data-Intensive Applications*, ch. 10 "Batch Processing" and ch. 11 "Stream Processing". <https://dataintensive.net/> — Comparison of batch and stream systems and their trade-offs.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
