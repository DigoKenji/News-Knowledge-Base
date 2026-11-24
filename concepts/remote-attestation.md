---
type: Concept
title: Remote attestation
description: A protocol in which a machine proves to a remote verifier, with evidence signed by hardware it cannot forge, exactly what software and configuration it is running, so the verifier can decide whether to trust it before sending secrets or work.
tags:
- security
- appsec
domain: security
subdomain: appsec
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Birkholz, H. et al. (2023). RFC 9334 - Remote ATtestation procedureS (RATS) Architecture. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc9334'
  kind: standard
- title: Coker, G. et al. (2011). Principles of Remote Attestation. International Journal of Information Security 10(2)
  resource: 'https://doi.org/10.1007/s10207-011-0124-7'
  kind: paper
- title: AWS. Cryptographic attestation (AWS Nitro Enclaves User Guide)
  resource: 'https://docs.aws.amazon.com/enclaves/latest/user/set-up-attestation.html'
  kind: docs
---

# Remote attestation

**Definition.** In remote attestation, an attester produces evidence about its state (measurements of firmware, OS, application code), signed by a hardware root of trust such as a TPM or a CPU security module. A verifier checks the signature and compares the measurements with known-good reference values, then a relying party acts on the result. The IETF RATS architecture (RFC 9334) standardises these roles.

Domain: [security](/domains/security.md) · subdomain `appsec`. Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

During boot, each stage measures (hashes) the next before running it and records the hash in protected registers, building a chain from hardware up. On request, the hardware signs a quote of those measurements together with a fresh nonce from the verifier, which prevents replay. Confidential computing extends this to enclaves and confidential VMs (Intel SGX/TDX, AMD SEV-SNP, AWS Nitro Enclaves): a key management service releases a decryption key only to an enclave whose attested code hash matches policy. Attestation proves what is running, not that the code is bug-free.

## Why it matters and trade-offs

It lets you trust remote machines you do not control, for example processing private data in the cloud where even the operator cannot see it (the zero-operator-access design in the 2026-11-21 note). Costs: hardware dependency, complex reference-value management for every software update, and trust in the hardware vendor's keys and implementation, which have had vulnerabilities.

## Example

Before a phone sends a user's voice recording to a cloud server for AI processing, it checks the server's attestation and sends data only if the server runs the exact audited build.

## Sources

1. Birkholz, H. et al. (2023). *RFC 9334 - Remote ATtestation procedureS (RATS) Architecture*. IETF. <https://www.rfc-editor.org/rfc/rfc9334> — Roles (attester, verifier, relying party), evidence, reference values.
2. Coker, G. et al. (2011). *Principles of Remote Attestation*. International Journal of Information Security 10(2). <https://doi.org/10.1007/s10207-011-0124-7> — Foundational principles - fresh, comprehensive, constrained disclosure, trustworthy mechanism.
3. AWS. *Cryptographic attestation* (AWS Nitro Enclaves User Guide). <https://docs.aws.amazon.com/enclaves/latest/user/set-up-attestation.html> — Attestation-gated key release in a production confidential-computing service.

## Relations
- (none yet)

## Notes
- [How Bee implemented Zero Operator Access for personal AI on AWS](/notes/security/2026-11-21-how-bee-implemented-zero-operator-access-for-personal-ai-on.md)
