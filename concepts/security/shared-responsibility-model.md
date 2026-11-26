---
type: Concept
title: 'Shared responsibility model'
description: 'The split of security duties between a cloud provider and its customer - the provider secures "of the cloud" (facilities, hardware, virtualisation), the customer secures "in the cloud" (data, identities, configuration) - with the line moving up as services go from IaaS to SaaS.'
tags:
- security
- cloud-security
domain: security
subdomain: cloud-security
also_in:
- cloud-infra/cloud-platforms
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Amazon Web Services (2019). AWS Security Incident Response Guide (whitepaper, June 2019)'
  resource: 'https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf'
  kind: docs
  locator: 'Shared Responsibility, pp. 3-4 (figure 1)'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 1 The Cloud Shared Responsibility Model, pp. 6-9'
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§5.9 Cloud Security Risks and Countermeasures, p. 187; §5.10 Data Protection in the Cloud, p. 189'
- title: 'NIST (2011). SP 800-144 - Guidelines on Security and Privacy in Public Cloud Computing'
  resource: 'https://csrc.nist.gov/pubs/sp/800/144/final'
  kind: standard
- title: 'Cloud Security Alliance. Security Guidance for Critical Areas of Focus in Cloud Computing'
  resource: 'https://cloudsecurityalliance.org/research/guidance'
  kind: standard
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Shared responsibility model

**Definition.** AWS states it as: AWS manages security of the cloud, the customer is responsible for security in the cloud. For infrastructure services such as virtual machines, the provider runs everything from the physical facilities up to the virtualisation layer, and the customer manages the guest operating system and patches, applications, and the configuration of provider controls such as security groups, network ACLs and IAM. For abstracted services (object storage, managed databases) the provider also runs the OS and platform, and the customer still manages its data, encryption choices, classification and permissions (AWS guide, pp. 3-4). Dotson notes that the split depends on the service model and that other parties, such as internal teams or managed-service partners, can hold parts of it.

Domain: [security](/domains/security.md) · subdomain `cloud-security`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Most cloud breaches happen on the customer side of the line: public storage buckets, leaked keys, over-broad IAM roles. The model therefore tells you where to spend effort: identity and access management, configuration and data protection. It also shapes incident response, since logs and forensics below the line depend on what the provider exposes. It is the security face of the cloud service models.

## Example

A company runs its database on a managed service. The provider patches the database engine; the company still has to keep it off the public internet, enforce encryption and limit which roles can read the customer table.

## Broader concepts

- [Service models (IaaS, PaaS, SaaS)](/concepts/cloud-infra/cloud-service-models.md)

## Related concepts

- [Identity and access management (IAM)](/concepts/security/identity-access-management.md)
- [Incident response](/concepts/security/incident-response.md)

## Sources

1. Amazon Web Services (2019). *AWS Security Incident Response Guide (whitepaper, June 2019)*. <https://d1.awsstatic.com/whitepapers/aws_security_incident_response.pdf> — Shared Responsibility, pp. 3-4 (figure 1). Provider's own statement of the model and how it changes for infrastructure versus abstracted services.
2. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 1 The Cloud Shared Responsibility Model, pp. 6-9. Vendor-neutral explanation across IaaS, PaaS and SaaS.
3. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §5.9 Cloud Security Risks and Countermeasures, p. 187; §5.10 Data Protection in the Cloud, p. 189. Cloud risks and the customer's data-protection duties.
4. NIST (2011). *SP 800-144 - Guidelines on Security and Privacy in Public Cloud Computing*. <https://csrc.nist.gov/pubs/sp/800/144/final> — NIST guidelines on public cloud security and privacy.
5. Cloud Security Alliance. Security Guidance for Critical Areas of Focus in Cloud Computing. <https://cloudsecurityalliance.org/research/guidance> — CSA guidance, a second independent body.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
