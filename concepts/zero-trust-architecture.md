---
type: Concept
title: Zero-trust architecture
description: A security model that grants no implicit trust based on network location - every request to a resource is authenticated, authorised and evaluated against policy (identity, device health, context) at the time it is made, with least-privilege access.
tags:
- security
- appsec
domain: security
subdomain: appsec
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Rose, S. et al. (2020). NIST SP 800-207 - Zero Trust Architecture
  resource: 'https://csrc.nist.gov/pubs/sp/800/207/final'
  kind: standard
- title: Ward, R. and Beyer, B. (2014). BeyondCorp - A New Approach to Enterprise Security. USENIX login 39(6)
  resource: 'https://research.google/pubs/beyondcorp-a-new-approach-to-enterprise-security/'
  kind: paper
- title: CISA (2023). Zero Trust Maturity Model v2.0
  resource: 'https://www.cisa.gov/zero-trust-maturity-model'
  kind: standard
---

# Zero-trust architecture

**Definition.** NIST SP 800-207 defines zero trust as a set of principles that "assumes there is no implicit trust granted to assets or user accounts based solely on their physical or network location". A zero trust architecture (ZTA) is an enterprise's plan that applies those principles: protect resources rather than network segments, and decide access per session.

Domain: [security](/domains/security.md) · subdomain `appsec`. Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

NIST's logical components: a policy engine decides, a policy administrator sets up or tears down the session, and policy enforcement points sit in front of every resource. Decisions use identity (strong, often phishing-resistant MFA), device posture, request context and behaviour signals, and are re-evaluated continuously. Google's BeyondCorp, started after the 2009 Aurora attacks, moved employees off the VPN: every internal app is reached through an access proxy that checks user and device. CISA's maturity model organises adoption into five pillars: identity, devices, networks, applications and workloads, data.

## Why it matters and trade-offs

The old perimeter model fails once an attacker is inside (phishing, stolen VPN credentials) and does not fit cloud and remote work. Zero trust limits lateral movement and blast radius. It is a multi-year programme, not a product; it needs good asset and identity inventories, adds latency and friction, and vendors often sell partial tools as "zero trust".

## Example

An engineer opens the internal admin panel from a café without a VPN; the access proxy checks their identity, MFA and that their laptop is managed and patched, then allows only that app.

## Sources

1. Rose, S. et al. (2020). *NIST SP 800-207 - Zero Trust Architecture*. <https://csrc.nist.gov/pubs/sp/800/207/final> — Definition, tenets, logical components, deployment models.
2. Ward, R. and Beyer, B. (2014). *BeyondCorp - A New Approach to Enterprise Security*. USENIX login 39(6). <https://research.google/pubs/beyondcorp-a-new-approach-to-enterprise-security/> — Large-scale production implementation without a privileged network.
3. CISA (2023). *Zero Trust Maturity Model v2.0*. <https://www.cisa.gov/zero-trust-maturity-model> — Five pillars and maturity stages.

## Relations
- (none yet)

## Notes
- [Agent Trust Infrastructure (ATI): Federated Identity and Attestation for Cross-Organization AI Agents](/notes/security/2026-11-18-agent-trust-infrastructure-ati-federated-identity-and-attest.md)
