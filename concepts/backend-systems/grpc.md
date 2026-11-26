---
type: Concept
title: gRPC
description: 'An open-source RPC framework that uses HTTP/2 and Protocol Buffers for typed, efficient service-to-service calls, including streaming.'
tags:
- backend-systems
- api-design
domain: backend-systems
subdomain: api-design
also_in:
- architecture/software-architecture
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'Kleppmann, M. (2017). Designing Data-Intensive Applications. O''Reilly'
  resource: 'https://dataintensive.net/'
  kind: book
  locator: 'ch. 4 Encoding and Evolution: Dataflow Through Services: REST and RPC (Current directions for RPC; Data encoding and evolution for RPC)'
- title: 'gRPC Authors. Introduction to gRPC'
  resource: 'https://grpc.io/docs/what-is-grpc/introduction/'
  kind: docs
- title: 'Google. Protocol Buffers overview'
  resource: 'https://protobuf.dev/overview/'
  kind: docs
updated:
- by: claude-opus (interactive, project thread; MBA library digest, DDIA)
  at: '2026-11-26T10:00:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# gRPC

**Definition.** gRPC is an open-source remote procedure call framework, originally from Google, in which services are defined in Protocol Buffers .proto files and client and server code is generated for many languages. Calls run over HTTP/2 with binary Protobuf messages, and gRPC supports unary calls plus client, server and bidirectional streaming, with deadlines and cancellation built in.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `api-design`. Expanded on 2026-11-26 with independent sources.

## How it fits

It is the common choice for internal service-to-service APIs where typed contracts and efficiency matter more than browser reach; public APIs more often use REST/JSON. Its evolution rules come from Protobuf field numbering, which keeps old and new clients compatible.

## Example

An order service calls inventory.Reserve(items) with a 200 ms deadline; if inventory is slow the call is cancelled and the order service falls back instead of waiting.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 4 Encoding and Evolution: Dataflow Through Services: REST and RPC (Current directions for RPC; Data encoding and evolution for RPC): Why RPC is not like a local call, and how gRPC/Protocol Buffers handle evolution.

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 4 Encoding and Evolution: Dataflow Through Services: REST and RPC (Current directions for RPC; Data encoding and evolution for RPC). Why RPC is not like a local call, and how gRPC/Protocol Buffers handle evolution.
2. gRPC Authors. Introduction to gRPC. <https://grpc.io/docs/what-is-grpc/introduction/> — Service definition, code generation and call types.
3. Google. Protocol Buffers overview. <https://protobuf.dev/overview/> — The interface definition language and wire format gRPC uses.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
