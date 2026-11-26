---
type: Concept
title: OpenID Connect
description: 'An identity layer on top of OAuth 2.0 that lets applications verify who the user is through a signed ID token from an identity provider (single sign-on).'
tags:
- security
- iam
domain: security
subdomain: iam
status: stable
generated:
  by: claude (seed from Notion tree)
  at: '2026-11-16T01:30:00-03:00'
sources:
- title: 'OpenID Foundation (2014). OpenID Connect Core 1.0'
  resource: 'https://openid.net/specs/openid-connect-core-1_0.html'
  kind: standard
- title: 'IETF (2015). RFC 7519 JSON Web Token (JWT)'
  resource: 'https://www.rfc-editor.org/rfc/rfc7519'
  kind: standard
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 4 Identity and Access Management (SSO with SAML and OIDC), p. 62'
relations:
- type: extends
  target: /concepts/security/oauth2.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# OpenID Connect

**Definition.** OpenID Connect is an identity layer on top of OAuth 2.0, finalised in 2014. Besides the access token, the identity provider returns an ID token - a signed JSON Web Token stating who the user is, who issued the token, for which application and when - and offers a UserInfo endpoint and discovery metadata. It is the common protocol for single sign-on in web and mobile apps.

Domain: [security](/domains/security.md) · subdomain `iam`. Expanded on 2026-11-26 with independent sources.

## How it fits

Where OAuth handles authorisation, OIDC handles authentication; together they replace much of SAML in newer systems, using JSON and JWTs instead of XML. Dotson notes that in practice the application never sees the user's password and the user does not log in again for every app.

## Example

'Sign in with Microsoft' on an internal portal: the portal validates the ID token's signature, issuer and audience, then reads the user's email and groups from its claims.

## Sources

1. OpenID Foundation (2014). OpenID Connect Core 1.0. <https://openid.net/specs/openid-connect-core-1_0.html> — ID token, flows and claims.
2. IETF (2015). RFC 7519 JSON Web Token (JWT). <https://www.rfc-editor.org/rfc/rfc7519> — The token format ID tokens use.
3. Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 4 Identity and Access Management (SSO with SAML and OIDC), p. 62. OIDC versus SAML for single sign-on.

## Relations
- **extends** [OAuth 2.0](/concepts/security/oauth2.md)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
