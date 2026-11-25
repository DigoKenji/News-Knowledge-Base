---
type: Concept
title: Authentication vs authorization
description: Authentication verifies who someone (or something) is; authorization decides what that verified identity is allowed to do - two separate steps that are often confused.
tags:
- security
- core
domain: security
subdomain: core
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
- title: Hardt, D. (ed.) (2012). RFC 6749 - The OAuth 2.0 Authorization Framework. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc6749'
  kind: standard
- title: OWASP. Authentication Cheat Sheet and Authorization Cheat Sheet
  resource: 'https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html'
  kind: practitioner
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 3 User Authentication, pp. 72-112; ch. 4 Access Control, pp. 113-154'
- title: 'Stuttard, D. and Pinto, M. (2008). The Web Application Hacker''s Handbook. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9780470170779'
  kind: book
  locator: 'ch. 6 Attacking Authentication p. 133; ch. 8 Attacking Access Controls p. 217'
foundation_candidate: true
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'added MBA library citations; definition unchanged'
---

# Authentication vs authorization

**Definition.** Authentication (authn) proves an identity claim using factors: something you know (password), have (security key, phone) or are (biometrics); multi-factor authentication combines two or more. Authorization (authz) applies a policy to the authenticated identity to grant or deny access to a resource or action, using models such as role-based (RBAC) or attribute-based (ABAC) access control.

Domain: [security](/domains/security.md) · subdomain `core`. Core vocabulary entry (approved seed list, 2026-11-24); certified by Kenji on 2026-11-24.

## How it fits

In web systems, OpenID Connect handles authentication (who logged in) and OAuth 2.0 handles delegated authorization (what an app may do on the user's behalf); mixing them up is a common source of vulnerabilities. HTTP even reflects the split: 401 means "not authenticated", 403 "authenticated but not allowed". Broken access control, an authorization failure, is the top item in the OWASP Top 10 (2021).

## Example

Logging into GitHub with a password and security key is authentication; GitHub allowing you to push to your repository but not to someone else's is authorization.

## Narrower concepts

- [OAuth 2.0](/concepts/security/oauth2.md)
- [OpenID Connect](/concepts/security/oidc.md)
- [Identity and access management (IAM)](/concepts/security/identity-access-management.md)

## Review notes for Kenji

Foundation candidate: a standards body defines this term (see sources). It moves to `foundations/` once verbatim quotes from two independent issuing bodies are collected (ADR-0010).

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (2026-11-24). Summaries with locators; nothing copied beyond short phrases.

- **Stallings, W. and Brown, L. (2015)**, *Computer Security: Principles and Practice, 3rd edition*, ch. 3 User Authentication, pp. 72-112; ch. 4 Access Control, pp. 113-154: Passwords, tokens, biometrics and remote authentication; DAC, RBAC and ABAC.
- **Stuttard, D. and Pinto, M. (2008)**, *The Web Application Hacker's Handbook*, ch. 6 Attacking Authentication p. 133; ch. 8 Attacking Access Controls p. 217: How authentication and authorization fail in web apps, read as a list of design mistakes to avoid.

## Sources

1. NIST (2017). *SP 800-63-3 - Digital Identity Guidelines*. <https://csrc.nist.gov/pubs/sp/800/63/3/final> — Authentication, authenticators, assurance levels.
2. Hardt, D. (ed.) (2012). *RFC 6749 - The OAuth 2.0 Authorization Framework*. IETF. <https://www.rfc-editor.org/rfc/rfc6749> — Delegated authorization as distinct from authentication.
3. OWASP. *Authentication Cheat Sheet* and *Authorization Cheat Sheet*. <https://cheatsheetseries.owasp.org/cheatsheets/Authentication_Cheat_Sheet.html> — Practical guidance and common mistakes for both.
4. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 3 User Authentication, pp. 72-112; ch. 4 Access Control, pp. 113-154. Passwords, tokens, biometrics and remote authentication; DAC, RBAC and ABAC.
5. Stuttard, D. and Pinto, M. (2008). *The Web Application Hacker's Handbook*. Wiley. <https://books.google.com/books?vid=ISBN9780470170779> — ch. 6 Attacking Authentication p. 133; ch. 8 Attacking Access Controls p. 217. How authentication and authorization fail in web apps, read as a list of design mistakes to avoid.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
