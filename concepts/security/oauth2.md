---
type: Concept
title: OAuth 2.0
description: 'An authorisation framework that lets an application get limited access to a user''s resources on another service through access tokens, without handling the user''s password.'
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
- title: 'IETF (2012). RFC 6749 The OAuth 2.0 Authorization Framework'
  resource: 'https://www.rfc-editor.org/rfc/rfc6749'
  kind: standard
- title: 'IETF (2025). RFC 9700 Best Current Practice for OAuth 2.0 Security'
  resource: 'https://www.rfc-editor.org/rfc/rfc9700'
  kind: standard
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 4 Identity and Access Management (SSO, OIDC on OAuth 2.0), p. 62'
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy; real description'
- by: claude-opus (interactive, project thread; stub expansion)
  at: '2026-11-26T22:00:00-03:00'
  change: 'expanded stub: definition, fit, example and independent sources'
---

# OAuth 2.0

**Definition.** OAuth 2.0 (RFC 6749) is an authorisation framework: it lets a client application obtain an access token to call an API on behalf of a resource owner (usually a user), with limited scope and lifetime, without ever seeing the user's password. An authorisation server issues the tokens after the user consents; grant types such as authorisation code (with PKCE) and client credentials cover different kinds of clients.

Domain: [security](/domains/security.md) · subdomain `iam`. Expanded on 2026-11-26 with independent sources.

## How it fits

OAuth answers 'what may this app do?', not 'who is the user?' - authentication is added by OpenID Connect on top. The 2025 security best current practice (RFC 9700) retires the implicit and password grants and requires PKCE and exact redirect matching.

## Example

A calendar app asks to read your Google Calendar; you consent on Google's page, and the app receives a token with scope calendar.readonly, not your Google password.

## Sources

1. IETF (2012). RFC 6749 The OAuth 2.0 Authorization Framework. <https://www.rfc-editor.org/rfc/rfc6749> — The specification: roles, grants, tokens.
2. IETF (2025). RFC 9700 Best Current Practice for OAuth 2.0 Security. <https://www.rfc-editor.org/rfc/rfc9700> — Current security requirements (PKCE, no implicit grant).
3. Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 4 Identity and Access Management (SSO, OIDC on OAuth 2.0), p. 62. OAuth and OIDC in cloud identity and SSO.

## Relations
- [OpenID Connect](/concepts/security/oidc.md) **extends** this

## Notes
Notes tagged with this concept are listed in the concept index after each run.
