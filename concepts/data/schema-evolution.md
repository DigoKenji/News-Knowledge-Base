---
type: Concept
title: 'Schema evolution and data compatibility'
description: 'Changing the structure of data over time without breaking readers or writers - keeping backward compatibility (new code reads old data) and forward compatibility (old code reads new data), usually with schema-based encodings such as Protocol Buffers, Thrift or Avro.'
tags:
- data
- data-engineering
domain: data
subdomain: data-engineering
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 4 Encoding and Evolution: Formats for Encoding Data (Thrift and Protocol Buffers; Avro); Modes of Dataflow'
- title: 'Google. Protocol Buffers - Updating a Message Type (proto3 language guide)'
  resource: 'https://protobuf.dev/programming-guides/proto3/#updating'
  kind: docs
- title: 'Apache Software Foundation. Apache Avro Specification - Schema Resolution'
  resource: 'https://avro.apache.org/docs/1.11.1/specification/#schema-resolution'
  kind: docs
---

# Schema evolution and data compatibility

**Definition.** Kleppmann defines backward compatibility as newer code being able to read data written by older code, and forward compatibility as older code being able to read data written by newer code. Both matter because in a running system old and new versions coexist: rolling deployments, mobile clients that do not update, and data stored years ago. Text formats such as JSON and XML are flexible but vague about types; binary schema-based formats (Protocol Buffers, Thrift, Avro) make the rules explicit: give fields tags or names, only add optional fields or fields with defaults, never reuse a removed tag. The same rules apply wherever data flows: through databases, through service calls (REST/RPC) and through message brokers.

Domain: [data](/domains/data.md) · subdomain `data-engineering`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-26); draft waiting for Kenji to certify.

## How it fits

Schema evolution is the data-level side of API versioning and the reason event-driven systems need a schema registry. It also connects to change data capture and event sourcing, where old events must stay readable forever. Breaking compatibility is one of the most common causes of failed deployments in distributed systems.

## Example

A team adds an optional "discount" field to the order event. Old consumers ignore the unknown field (forward compatible), and new consumers treat a missing discount as zero when reading old events (backward compatible), so producers and consumers can deploy in any order.

## Broader concepts

- [Data model (relational, document, graph)](/concepts/data/data-model.md)

## Related concepts

- [API versioning](/concepts/backend-systems/api-versioning.md)
- [gRPC](/concepts/backend-systems/grpc.md)

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 4 Encoding and Evolution: Formats for Encoding Data (Thrift and Protocol Buffers; Avro); Modes of Dataflow. Definitions of backward and forward compatibility and how each encoding supports schema evolution across databases, services and messages.
2. Google. Protocol Buffers - Updating a Message Type (proto3 language guide). <https://protobuf.dev/programming-guides/proto3/#updating> — Official guide to updating message types safely.
3. Apache Software Foundation. Apache Avro Specification - Schema Resolution. <https://avro.apache.org/docs/1.11.1/specification/#schema-resolution> — Avro specification, schema resolution rules.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
