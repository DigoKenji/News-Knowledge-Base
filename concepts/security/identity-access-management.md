---
type: Concept
title: Identity and access management (IAM)
description: The processes and systems that manage digital identities (people, services, devices) through their lifecycle and control what each identity can access - covering provisioning, authentication, authorization, federation and access reviews.
tags:
- security
- core
domain: security
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T20:45:00-03:00'
sources:
- title: NIST (2017). SP 800-63-3 - Digital Identity Guidelines
  resource: 'https://csrc.nist.gov/pubs/sp/800/63/3/final'
  kind: standard
- title: OpenID Foundation (2014). OpenID Connect Core 1.0
  resource: 'https://openid.net/specs/openid-connect-core-1_0.html'
  kind: standard
- title: AWS. What is IAM? (AWS Identity and Access Management User Guide)
  resource: 'https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html'
  kind: docs
---

# Identity and access management (IAM)

**Definition.** IAM answers "who is this, and what may they do?" across an organisation. It includes the identity lifecycle (joiner, mover, leaver), an identity provider that authenticates users, single sign-on and federation with standards such as SAML and OpenID Connect, permission models (roles, policies), and governance (periodic access reviews, audit logs). Cloud providers have their own IAM services for controlling access to resources.

Domain: [security](/domains/security.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Identity has become the main security perimeter: with cloud and remote work, stolen credentials and over-broad permissions cause many breaches. Non-human identities (service accounts, API keys, AI agents) now outnumber people in many organisations and need the same lifecycle discipline.

## Example

When an employee leaves, disabling one account in the identity provider removes their access to email, GitHub and the cloud console at once, because all of them use single sign-on.

## Narrower concepts

- [OAuth 2.0](/concepts/security/oauth2.md)
- [OpenID Connect](/concepts/security/oidc.md)
- [Zero-trust architecture](/concepts/security/zero-trust-architecture.md)

## Sources

1. NIST (2017). *SP 800-63-3 - Digital Identity Guidelines*. <https://csrc.nist.gov/pubs/sp/800/63/3/final> — Identity proofing, authentication and federation.
2. OpenID Foundation (2014). *OpenID Connect Core 1.0*. <https://openid.net/specs/openid-connect-core-1_0.html> — Federated identity standard used for single sign-on.
3. AWS. *What is IAM?* (AWS Identity and Access Management User Guide). <https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html> — Cloud IAM - identities, policies, least-privilege permissions.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
