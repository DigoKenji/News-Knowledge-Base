---
type: Concept
title: 'Incident response'
description: 'The prepared, repeatable process for handling a security incident - preparation, detection and analysis, containment, eradication and recovery, and post-incident lessons - run by a defined team with runbooks, evidence handling and communication plans.'
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
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 5 Building an Incident Response Program (phases of incident response); ch. 6 Analyzing Symptoms; ch. 7 Performing Forensic Analysis; ch. 8 Recovery and Post-Incident Response'
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§17.4 Computer Security Incident Response Teams, pp. 567-574 (triage function p. 570)'
- title: 'Amazon Web Services (2019). AWS Security Incident Response Guide (whitepaper, June 2019)'
  resource: 'https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf'
  kind: docs
  locator: 'Foundation of Incident Response (educate, prepare, simulate, iterate), pp. 2-5; Prepare - People and Technology, pp. 10-25; Runbooks and Automation, pp. 29-32'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 7 Detecting, Responding to, and Recovering from Security Incidents, pp. 139-170'
- title: 'NIST (2012). SP 800-61 Rev. 2 - Computer Security Incident Handling Guide'
  resource: 'https://csrc.nist.gov/pubs/sp/800/61/r2/final'
  kind: standard
- title: 'NIST (2025). SP 800-61 Rev. 3 - Incident Response Recommendations and Considerations for Cybersecurity Risk Management'
  resource: 'https://csrc.nist.gov/pubs/sp/800/61/r3/final'
  kind: standard
---

# Incident response

**Definition.** A security incident is an event that actually or potentially jeopardises the confidentiality, integrity or availability of a system or its information. The classic lifecycle in NIST SP 800-61 Rev. 2, used by Chapple and Seidl, has four phases: preparation; detection and analysis; containment, eradication and recovery; and post-incident activity. Stallings and Brown describe the computer security incident response team (CSIRT) and its functions: detecting incidents, triage through a single point of contact, responding, and documenting. NIST SP 800-61 Rev. 3 (2025) now maps incident response onto the Cybersecurity Framework 2.0 functions rather than a separate lifecycle. In the cloud, the AWS guide organises the work as educate, prepare (people and technology), simulate and iterate, and stresses that responsibilities follow the shared responsibility model.

Domain: [security](/domains/security.md) · subdomain `security`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Incident response is where the other controls are tested for real: logging and observability decide whether you can detect and reconstruct what happened, least privilege limits the blast radius, and backups decide how fast you recover. The lessons-learned step feeds risk management and threat modelling. Rehearsal matters as much as the plan: both the AWS guide and Dotson (ch. 7) push simulations and automated runbooks, because cloud incidents unfold faster than manual processes.

## Example

An alert shows an access key used from an unknown country. The team follows the runbook: disables the key (containment), finds and removes the resources the attacker created (eradication), rotates related credentials and restores service (recovery), then adds a rule that blocks long-lived keys (lessons learned).

## Broader concepts

- [Security controls](/concepts/security/security-controls.md)

## Related concepts

- [Observability (logs, metrics, traces)](/concepts/cloud-infra/observability.md)
- [Shared responsibility model](/concepts/security/shared-responsibility-model.md)

## Sources

1. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 5 Building an Incident Response Program (phases of incident response); ch. 6 Analyzing Symptoms; ch. 7 Performing Forensic Analysis; ch. 8 Recovery and Post-Incident Response. Four full chapters on building and running the IR program, following NIST SP 800-61.
2. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §17.4 Computer Security Incident Response Teams, pp. 567-574 (triage function p. 570). CSIRT functions and the steps for handling an incident.
3. Amazon Web Services (2019). *AWS Security Incident Response Guide (whitepaper, June 2019)*. <https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf> — Foundation of Incident Response (educate, prepare, simulate, iterate), pp. 2-5; Prepare - People and Technology, pp. 10-25; Runbooks and Automation, pp. 29-32. Cloud-specific incident response: preparing access, runbooks and automation in AWS.
4. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 7 Detecting, Responding to, and Recovering from Security Incidents, pp. 139-170. What changes in detection and response in cloud environments.
5. NIST (2012). *SP 800-61 Rev*. 2 - Computer Security Incident Handling Guide. <https://csrc.nist.gov/pubs/sp/800/61/r2/final> — Classic four-phase incident handling lifecycle.
6. NIST (2025). *SP 800-61 Rev*. 3 - Incident Response Recommendations and Considerations for Cybersecurity Risk Management. <https://csrc.nist.gov/pubs/sp/800/61/r3/final> — Current NIST guidance (2025), aligned to CSF 2.0.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
