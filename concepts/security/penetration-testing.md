---
type: Concept
title: 'Penetration testing and vulnerability assessment'
description: 'Authorised, scoped attempts to find and safely exploit weaknesses in a system the way an attacker would, to prove real risk and drive fixes - distinct from an automated vulnerability scan, which only lists known weaknesses.'
tags:
- security
- appsec
domain: security
subdomain: appsec
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Easttom, W. (2018). Penetration Testing Fundamentals: A Hands-On Guide to Reliable Security Audits. Pearson IT Certification'
  resource: 'https://books.google.com/books?vid=ISBN9780789759375'
  kind: book
  locator: 'ch. 1 Introduction to Penetration Testing (vulnerability scans vs penetration tests); ch. 2 Standards (PCI DSS, NIST 800-115, PTES, CREST); ch. 19 A Sample Pen Test Project'
- title: 'Beaver, K. (2018). Hacking For Dummies, 6th edition. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9781119485476'
  kind: book
  locator: 'ch. 1 Introduction to Vulnerability and Penetration Testing, pp. 7-24 (testing versus auditing p. 10; the testing process p. 18)'
- title: 'Allen, L., Heriyanto, T. and Ali, S. (2014). Kali Linux - Assuring Security by Penetration Testing, 2nd edition. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781849519489'
  kind: book
  locator: 'ch. 2 Penetration Testing Methodology, pp. 51-72 (OSSTMM p. 56); ch. 3 Target Scoping p. 73; ch. 12 Documentation and Reporting, pp. 365-380'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 8 Vulnerability Scanning p. 356 and Penetration Testing p. 359'
- title: 'NIST (2008). SP 800-115 - Technical Guide to Information Security Testing and Assessment'
  resource: 'https://csrc.nist.gov/pubs/sp/800/115/final'
  kind: standard
- title: 'OWASP. Web Security Testing Guide'
  resource: 'https://owasp.org/www-project-web-security-testing-guide/'
  kind: docs
---

# Penetration testing and vulnerability assessment

**Definition.** Easttom defines a penetration test as using actual hacking techniques to test the security of a target system, and stresses that pen testing and hacking are not synonyms: the difference is written permission, an agreed scope and rules of engagement, and a report. A vulnerability scan detects known vulnerabilities automatically and should run regularly; a penetration test goes further, chaining findings to show what an attacker could actually reach. NIST SP 800-115 describes four phases (planning, discovery, attack, reporting); methodologies such as PTES, OSSTMM and the OWASP Web Security Testing Guide give more detailed steps. Engagements may be black-box (no inside knowledge), grey-box or white-box, and may include social engineering if the scope allows.

Domain: [security](/domains/security.md) · subdomain `appsec`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Pen testing is the offensive check on everything defensive in this domain: it validates controls, feeds vulnerability management with prioritised findings and is required by some standards (PCI DSS). Its output is only as good as its report: executive, management and technical views with reproducible evidence and remediation advice (Allen et al., ch. 12). Results are a point-in-time snapshot, which is why continuous scanning and periodic tests complement each other. Several books in the MBA library are offensive-tool manuals; this KB keeps only the concepts, not attack procedures.

## Example

Before launching a customer portal, a company hires testers with a signed scope covering the web app and its API only. They find an access-control flaw that lets one customer read another's invoices, demonstrate it with a test account, and the report ranks it critical with a fix.

## Broader concepts

- [Threat, vulnerability and risk](/concepts/security/threat-vulnerability-risk.md)

## Related concepts

- [Vulnerability management](/concepts/security/vulnerability-management.md)
- [Common vulnerability classes](/concepts/security/common-vulnerability-classes.md)

## Sources

1. Easttom, W. (2018). *Penetration Testing Fundamentals: A Hands-On Guide to Reliable Security Audits*. Pearson IT Certification. <https://books.google.com/books?vid=ISBN9780789759375> — ch. 1 Introduction to Penetration Testing (vulnerability scans vs penetration tests); ch. 2 Standards (PCI DSS, NIST 800-115, PTES, CREST); ch. 19 A Sample Pen Test Project. Definitions, standards and a full sample engagement; the most methodical of the pen-test books.
2. Beaver, K. (2018). *Hacking For Dummies, 6th edition*. Wiley. <https://books.google.com/books?vid=ISBN9781119485476> — ch. 1 Introduction to Vulnerability and Penetration Testing, pp. 7-24 (testing versus auditing p. 10; the testing process p. 18). Plain-language view of ethical testing, permission and process.
3. Allen, L., Heriyanto, T. and Ali, S. (2014). *Kali Linux - Assuring Security by Penetration Testing, 2nd edition*. Packt. <https://books.google.com/books?vid=ISBN9781849519489> — ch. 2 Penetration Testing Methodology, pp. 51-72 (OSSTMM p. 56); ch. 3 Target Scoping p. 73; ch. 12 Documentation and Reporting, pp. 365-380. Methodologies, scoping and how to write the three report types.
4. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 8 Vulnerability Scanning p. 356 and Penetration Testing p. 359. Exam-level contrast between scans and pen tests (SY0-501, 2017).
5. NIST (2008). *SP 800-115 - Technical Guide to Information Security Testing and Assessment*. <https://csrc.nist.gov/pubs/sp/800/115/final> — NIST's testing and assessment guide (four-phase model).
6. OWASP. Web Security Testing Guide. <https://owasp.org/www-project-web-security-testing-guide/> — OWASP methodology for web application testing.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
