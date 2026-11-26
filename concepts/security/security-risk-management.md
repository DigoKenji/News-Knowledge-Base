---
type: Concept
title: 'Security risk management and business impact analysis'
description: 'The continuous cycle of identifying assets, threats and vulnerabilities, assessing likelihood and impact (including a business impact analysis of critical processes), choosing a treatment - mitigate, transfer, avoid or accept - and monitoring the result.'
tags:
- security
- governance-risk
domain: security
subdomain: governance-risk
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 14 IT Security Management and Risk Assessment, pp. 485-514 (security risk assessment p. 492; detailed risk analysis p. 495); ch. 15 Monitoring Risks, p. 526'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 8 Understanding Risk Management, pp. 344-356; ch. 9 Business Impact Analysis Concepts, pp. 406-412'
- title: 'Course handout (n.d.). Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)'
  resource: 'FIAP-MBA/Materiais/Referencias (local copy, no public URL)'
  kind: handout
  locator: 'pp. 1-23 (whole handout)'
- title: 'NIST (2012). SP 800-30 Rev. 1 - Guide for Conducting Risk Assessments'
  resource: 'https://csrc.nist.gov/pubs/sp/800/30/r1/final'
  kind: standard
- title: 'NIST (2010). SP 800-34 Rev. 1 - Contingency Planning Guide for Federal Information Systems'
  resource: 'https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final'
  kind: standard
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Security risk management and business impact analysis

**Definition.** Stallings and Brown present IT security management as a cycle: set the organisational context and policy, assess risk, select and implement controls, then monitor (ch. 14-15). Risk assessment can be baseline, informal, detailed or combined; a detailed analysis identifies assets, threats and vulnerabilities, estimates likelihood and consequence, and records the resulting risk level in a risk register. Each risk then gets a response: accept, avoid, mitigate (reduce with controls) or share/transfer (for example insurance). A business impact analysis (BIA) asks how the loss of each business process would hurt the organisation over time, and sets recovery targets such as RTO (how fast) and RPO (how much data can be lost) (Gibson, ch. 9). The FIAP risk handout adds the audit view: controls must be effective and cost-justified, gaps are found by gap analysis, and the ISMS improves through plan-do-check-act.

Domain: [security](/domains/security.md) · subdomain `governance-risk`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

This concept is the management layer above Threat, vulnerability and risk: those terms define risk, this one is what an organisation does about it. It connects to the MBA's architecture work: a BIA uses the same business processes and capabilities that business architecture maps, and risk appetite is a governance decision. NIST SP 800-30 and ISO/IEC 27005 are the standard methods.

## Example

A BIA shows that order processing loses R$200k per hour of downtime while the intranet loses almost nothing. Order processing gets an RTO of 1 hour and a replicated database (mitigate); the intranet risk is accepted; card-data breach risk is partly transferred with cyber insurance.

## Broader concepts

- [Threat, vulnerability and risk](/concepts/security/threat-vulnerability-risk.md)

## Narrower concepts

- [Security controls](/concepts/security/security-controls.md)

## Related concepts

- [Threat modeling](/concepts/security/threat-modeling.md)

## Sources

1. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 14 IT Security Management and Risk Assessment, pp. 485-514 (security risk assessment p. 492; detailed risk analysis p. 495); ch. 15 Monitoring Risks, p. 526. Full management cycle, risk assessment approaches, risk register and the four risk responses.
2. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 8 Understanding Risk Management, pp. 344-356; ch. 9 Business Impact Analysis Concepts, pp. 406-412. Risk response types and BIA terms (RTO, RPO, MTBF) at exam level (SY0-501, 2017).
3. Course handout (n.d.). *Risk Management, Security Compliance and Audit Controls, chapter 26 (author not stated)*. — pp. 1-23 (whole handout). Audit perspective: risk analysis stages, risk treatment matrix, gap analysis and PDCA.
4. NIST (2012). *SP 800-30 Rev*. 1 - Guide for Conducting Risk Assessments. <https://csrc.nist.gov/pubs/sp/800/30/r1/final> — NIST risk assessment method.
5. NIST (2010). *SP 800-34 Rev*. 1 - Contingency Planning Guide for Federal Information Systems. <https://csrc.nist.gov/pubs/sp/800/34/r1/upd1/final> — Contingency planning and the BIA process.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
