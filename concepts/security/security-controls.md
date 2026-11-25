---
type: Concept
title: 'Security controls'
description: 'Measures that reduce risk, classified by how they are implemented (management/administrative, operational, technical, physical) and by what they do (preventive, detective, corrective, deterrent, compensating).'
tags:
- security
- security
domain: security
subdomain: security
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: 'ch. 15 IT Security Controls, Plans, and Procedures, §15.2 Security Controls or Safeguards, pp. 516-524'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 1 Understanding Control Types, pp. 69-75 (deterrent, compensating, combining types and goals)'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 9 Policy and Compliance; ch. 10 Defense-in-Depth Security Architectures'
- title: 'NIST (2020). SP 800-53 Rev. 5 - Security and Privacy Controls for Information Systems and Organizations'
  resource: 'https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final'
  kind: standard
---

# Security controls

**Definition.** Stallings and Brown define a control as an action, device, procedure or other measure that reduces risk by eliminating or preventing a security violation, by minimising the harm it can cause, or by discovering and reporting it so it can be corrected (p. 517). They classify controls as management (policies, planning, standards), operational (procedures carried out mainly by people) and technical (hardware and software capabilities), each of which can be supportive, preventive, or detection-and-recovery. Exam guides use a parallel scheme: technical, administrative and physical by implementation, and preventive, detective, corrective, deterrent and compensating by goal; a compensating control is an alternative used when the primary control is not feasible (Gibson, ch. 1). NIST SP 800-53 is the large catalogue of concrete controls grouped into families.

Domain: [security](/domains/security.md) · subdomain `security`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Controls are the "treat" in risk treatment: a risk assessment says what needs reducing, and controls are selected, implemented and then monitored (Stallings fig. 15.1). Layering different types is defence in depth: a firewall rule (technical, preventive), log review (operational, detective), a backup restore (corrective) and a visible camera (physical, deterrent) cover each other's gaps. Audits check that controls exist and actually work.

## Example

A legacy system cannot support MFA, so the team adds a compensating control: it is reachable only through a jump host that does enforce MFA, and every session is logged.

## Broader concepts

- [Security risk management and business impact analysis](/concepts/security/security-risk-management.md)

## Narrower concepts

- [Least privilege and defense in depth](/concepts/security/least-privilege.md)
- [Network security controls (firewalls, segmentation, IDS/IPS)](/concepts/security/network-security.md)
- [Incident response](/concepts/security/incident-response.md)

## Related concepts

- [CIA triad (confidentiality, integrity, availability)](/concepts/security/cia-triad.md)

## Sources

1. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 15 IT Security Controls, Plans, and Procedures, §15.2 Security Controls or Safeguards, pp. 516-524. Definition of a control, the management/operational/technical classes and supportive/preventive/detection-and-recovery types.
2. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 1 Understanding Control Types, pp. 69-75 (deterrent, compensating, combining types and goals). The exam-standard taxonomy of control types and goals (SY0-501, 2017).
3. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 9 Policy and Compliance; ch. 10 Defense-in-Depth Security Architectures. Controls in policy frameworks and layered architectures.
4. NIST (2020). *SP 800-53 Rev*. 5 - Security and Privacy Controls for Information Systems and Organizations. <https://csrc.nist.gov/pubs/sp/800/53/r5/upd1/final> — NIST catalogue of security and privacy controls.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
