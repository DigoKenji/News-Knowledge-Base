---
type: Concept
title: Identity and access management (IAM)
description: The processes and systems that manage digital identities (people, services, devices) through their lifecycle and control what each identity can access - covering provisioning, authentication, authorization, federation and access reviews.
tags:
- security
- iam
- core
domain: security
subdomain: iam
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
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
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§4.5 RBAC p. 127; §4.6 ABAC p. 133; §4.7 Identity, Credential, and Access Management p. 139'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 4 Identity and Access Management, pp. 49-76'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 11 Identity and Access Management Security'
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Identity and access management (IAM)

**Definition.** IAM answers "who is this, and what may they do?" across an organisation. It includes the identity lifecycle (joiner, mover, leaver), an identity provider that authenticates users, single sign-on and federation with standards such as SAML and OpenID Connect, permission models (roles, policies), and governance (periodic access reviews, audit logs). Cloud providers have their own IAM services for controlling access to resources.

Domain: [security](/domains/security.md) · subdomain `iam`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

Identity has become the main security perimeter: with cloud and remote work, stolen credentials and over-broad permissions cause many breaches. Non-human identities (service accounts, API keys, AI agents) now outnumber people in many organisations and need the same lifecycle discipline.

## Example

When an employee leaves, disabling one account in the identity provider removes their access to email, GitHub and the cloud console at once, because all of them use single sign-on.

## Narrower concepts

- [OAuth 2.0](/concepts/security/oauth2.md)
- [OpenID Connect](/concepts/security/oidc.md)
- [Zero-trust architecture](/concepts/security/zero-trust-architecture.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, §4.5 RBAC p. 127; §4.6 ABAC p. 133; §4.7 Identity, Credential, and Access Management p. 139: Access-control models and the ICAM lifecycle.
- **Dotson, C. (2019)**, *Practical Cloud Security: A Guide for Secure Design and Deployment*, ch. 4 Identity and Access Management, pp. 49-76: IAM in the cloud: identities, federation, MFA, secrets and access reviews.
- **Chapple, M. and Seidl, D. (2017)**, *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*, ch. 11 Identity and Access Management Security: IAM threats and controls for analysts.

## Sources

1. NIST (2017). *SP 800-63-3 - Digital Identity Guidelines*. <https://csrc.nist.gov/pubs/sp/800/63/3/final> — Identity proofing, authentication and federation.
2. OpenID Foundation (2014). *OpenID Connect Core 1.0*. <https://openid.net/specs/openid-connect-core-1_0.html> — Federated identity standard used for single sign-on.
3. AWS. *What is IAM?* (AWS Identity and Access Management User Guide). <https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html> — Cloud IAM - identities, policies, least-privilege permissions.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §4.5 RBAC p. 127; §4.6 ABAC p. 133; §4.7 Identity, Credential, and Access Management p. 139. Access-control models and the ICAM lifecycle.
5. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 4 Identity and Access Management, pp. 49-76. IAM in the cloud: identities, federation, MFA, secrets and access reviews.
6. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 11 Identity and Access Management Security. IAM threats and controls for analysts.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
