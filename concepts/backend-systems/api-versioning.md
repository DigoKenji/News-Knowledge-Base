---
type: Concept
title: API versioning
description: 'Changing an API without breaking existing clients - by keeping changes backward compatible or by running explicit versions side by side until clients move.'
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
  locator: 'ch. 4 Encoding and Evolution: backward and forward compatibility; Data encoding and evolution for RPC'
- title: 'Google. AIP-185: API Versioning'
  resource: 'https://google.aip.dev/185'
  kind: standard
- title: 'Preston-Werner, T. Semantic Versioning 2.0.0'
  resource: 'https://semver.org/'
  kind: standard
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

# API versioning

**Definition.** API versioning is how a provider evolves an interface without breaking its consumers. The first tool is compatibility: additive changes (new optional fields, new endpoints) keep old clients working. When a breaking change is unavoidable, a new major version is published - in the URL path, a header or the package name - and the old one runs in parallel until clients migrate and it is deprecated.

Domain: [backend-systems](/domains/backend-systems.md) · subdomain `api-design`. Expanded on 2026-11-26 with independent sources.

## How it fits

It applies schema evolution to service contracts: servers must stay backward compatible with old clients, which cannot be upgraded on the server's schedule. Semantic versioning gives the vocabulary (major means breaking), and Google's API guidelines make major versions part of the path and forbid breaking changes within one.

## Example

Renaming customer_name to full_name would break mobile apps already installed, so /v1 keeps the old field while /v2 introduces the new one; v1 is retired a year later after usage drops to zero.

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (DDIA added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Kleppmann, M. (2017)**, *Designing Data-Intensive Applications*, ch. 4 Encoding and Evolution: backward and forward compatibility; Data encoding and evolution for RPC: Compatibility rules underneath API versioning: servers must stay backward compatible with old clients.

## Sources

1. Kleppmann, M. (2017). *Designing Data-Intensive Applications*. O'Reilly. <https://dataintensive.net/> — ch. 4 Encoding and Evolution: backward and forward compatibility; Data encoding and evolution for RPC. Compatibility rules underneath API versioning: servers must stay backward compatible with old clients.
2. Google. AIP-185: API Versioning. <https://google.aip.dev/185> — Major versions in the path; no breaking changes within a version.
3. Preston-Werner, T. Semantic Versioning 2.0.0. <https://semver.org/> — MAJOR.MINOR.PATCH and what counts as breaking.

## Notes
Notes tagged with this concept are listed in the concept index after each run.
