---
type: Concept
title: API and API design
description: An API (application programming interface) is the contract through which software components or systems talk to each other; API design is choosing its resources, operations, data formats, errors and versioning so it is easy to use and safe to evolve.
tags:
- architecture
- core
domain: architecture
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Fielding, R. T. (2000). Architectural Styles and the Design of Network-based Software Architectures, ch. 5 REST
  resource: 'https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm'
  kind: paper
- title: OpenAPI Initiative (2021). OpenAPI Specification v3.1.0
  resource: 'https://spec.openapis.org/oas/v3.1.0'
  kind: standard
- title: Google Cloud. API Design Guide
  resource: 'https://docs.cloud.google.com/apis/design'
  kind: practitioner
---

# API and API design

**Definition.** Web APIs mostly follow REST (Fielding, 2000): resources identified by URLs, manipulated with standard HTTP methods, stateless requests. Alternatives include RPC styles such as gRPC, GraphQL, and asynchronous event APIs. The OpenAPI Specification is the standard way to describe HTTP APIs as machine-readable contracts.

Domain: [architecture](/domains/architecture.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

APIs are the boundaries between services, teams and companies, so their design has long-lived consequences: once published, breaking changes hurt clients, hence versioning, backward compatibility and idempotent operations for safe retries. Good practice: design the contract first, consistent naming, clear error model, pagination, authentication with OAuth.

## Example

GET /articles?domain=security&since=2026-11-01 returns a page of articles; POST /articles with an Idempotency-Key header can be retried without creating duplicates.

## Narrower concepts

- [API versioning](/concepts/backend-systems/api-versioning.md)
- [gRPC](/concepts/backend-systems/grpc.md)
- [Idempotency](/concepts/backend-systems/idempotency.md)
- [OAuth 2.0](/concepts/security/oauth2.md)

## Sources

1. Fielding, R. T. (2000). *Architectural Styles and the Design of Network-based Software Architectures*, ch. 5 REST. <https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm> — Definition of the REST architectural style.
2. OpenAPI Initiative (2021). *OpenAPI Specification v3.1.0*. <https://spec.openapis.org/oas/v3.1.0> — Standard machine-readable description of HTTP APIs.
3. Google Cloud. *API Design Guide*. <https://docs.cloud.google.com/apis/design> — Resource-oriented design, naming, errors, versioning at scale.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
