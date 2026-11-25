---
type: Concept
title: 'Network security controls (firewalls, segmentation, IDS/IPS)'
description: 'Controls that restrict and watch traffic between networks and hosts - firewalls that enforce an access policy, segmentation and DMZs that limit what each zone can reach, VPNs that protect traffic in transit, and intrusion detection or prevention systems that spot attacks.'
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
  locator: 'ch. 8 Intrusion Detection, pp. 267-303; ch. 9 Firewalls and Intrusion Prevention Systems, pp. 304-335 (types of firewalls p. 308; DMZ and configurations p. 317; IPS p. 322)'
- title: 'Dotson, C. (2019). Practical Cloud Security: A Guide for Secure Design and Deployment. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492037514'
  kind: book
  locator: 'ch. 6 Network Security, pp. 109-137 (firewalls and network segmentation p. 121)'
- title: 'Davies, G. (2019). Networking Fundamentals (Microsoft MTA 98-366). Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781838643508'
  kind: book
  locator: 'ch. 1 Firewalls p. 12, DMZ p. 26, VPNs p. 27; ch. 9 OSI Model p. 250; ch. 10 TCP/IP p. 268; ch. 14 Packet-filtering firewalls p. 407'
- title: 'Chapple, M. and Seidl, D. (2017). CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001. Sybex'
  resource: 'https://books.google.com/books?vid=ISBN9781119348979'
  kind: book
  locator: 'ch. 10 Defense-in-Depth Security Architectures'
- title: 'NIST (2009). SP 800-41 Rev. 1 - Guidelines on Firewalls and Firewall Policy'
  resource: 'https://csrc.nist.gov/pubs/sp/800/41/r1/final'
  kind: standard
---

# Network security controls (firewalls, segmentation, IDS/IPS)

**Definition.** A firewall sits between networks and enforces an access policy on traffic that crosses it. Stallings and Brown describe the main types: packet-filtering (rules on addresses, ports and protocol), stateful inspection (tracks connections), application-level and circuit-level gateways (proxies), plus host-based and personal firewalls; and where to place them, including a DMZ for public-facing servers between an external and an internal firewall. Segmentation splits a network into zones (VLANs, subnets, cloud security groups) so a compromise in one zone does not reach everything. An intrusion detection system (IDS) watches host or network activity using signature (misuse) or anomaly detection and raises alerts; an intrusion prevention system (IPS) sits inline and can block. Davies's networking basics (OSI and TCP/IP layers, VLANs, VPNs, DMZ) are the vocabulary these controls are written in.

Domain: [security](/domains/security.md) · subdomain `security`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Network controls are one layer of defence in depth, not the whole defence: the perimeter model (trusted inside, untrusted outside) breaks down with cloud, remote work and lateral movement, which is why zero-trust architecture moves the decision to identity and each request. In the cloud the same ideas reappear as security groups, network ACLs and private subnets (Dotson, ch. 6). IDS alerts feed security monitoring and incident response.

## Example

A web shop puts its web servers in a DMZ that accepts only HTTPS from the internet, lets only the web tier reach the database subnet on the database port, and runs a network IDS that alerts on SQL-injection signatures.

## Broader concepts

- [Security controls](/concepts/security/security-controls.md)

## Related concepts

- [Zero-trust architecture](/concepts/security/zero-trust-architecture.md)
- [Least privilege and defense in depth](/concepts/security/least-privilege.md)

## Sources

1. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — ch. 8 Intrusion Detection, pp. 267-303; ch. 9 Firewalls and Intrusion Prevention Systems, pp. 304-335 (types of firewalls p. 308; DMZ and configurations p. 317; IPS p. 322). Firewall types and placement, IDS analysis approaches, honeypots and IPS.
2. Dotson, C. (2019). *Practical Cloud Security: A Guide for Secure Design and Deployment*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492037514> — ch. 6 Network Security, pp. 109-137 (firewalls and network segmentation p. 121). How the same controls look in cloud networks.
3. Davies, G. (2019). *Networking Fundamentals (Microsoft MTA 98-366)*. Packt. <https://books.google.com/books?vid=ISBN9781838643508> — ch. 1 Firewalls p. 12, DMZ p. 26, VPNs p. 27; ch. 9 OSI Model p. 250; ch. 10 TCP/IP p. 268; ch. 14 Packet-filtering firewalls p. 407. Networking vocabulary underneath the controls (MTA 98-366 level).
4. Chapple, M. and Seidl, D. (2017). *CompTIA Cybersecurity Analyst (CSA+) Study Guide, Exam CS0-001*. Sybex. <https://books.google.com/books?vid=ISBN9781119348979> — ch. 10 Defense-in-Depth Security Architectures. Segmentation and layered network design for analysts.
5. NIST (2009). *SP 800-41 Rev*. 1 - Guidelines on Firewalls and Firewall Policy. <https://csrc.nist.gov/pubs/sp/800/41/r1/final> — NIST guidelines on firewalls and firewall policy.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
