---
type: Paper Summary
title: 'Agent Trust Infrastructure (ATI): Federated Identity and Attestation for Cross-Organization AI Agents'
description: ATI adds deterministic identity, trust, attestation and policy checks outside the agent's model, raising attack
  containment from 43% to 84%.
resource: https://arxiv.org/abs/2611.23202
tags:
- security
- appsec
- multi-agent-orchestration
- model-context-protocol
- oauth2
- oidc
- threat-modeling
- indirect-prompt-injection
- zero-trust-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://arxiv.org/abs/2611.23202
  title: 'Agent Trust Infrastructure (ATI): Federated Identity and Attestation for Cross-Organization AI Agents'
  author: Viet K. Nguyen, Mohammad I. Husain
status: stable
source_url: https://arxiv.org/abs/2611.23202
source: arXiv
source_type: arxiv
published: '2026-11-17'
domain: security
subdomain: appsec
concepts:
- multi-agent-orchestration
- model-context-protocol
- oauth2
- oidc
- threat-modeling
- indirect-prompt-injection
- zero-trust-architecture
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d815495c5cd37068a21c5
---

## 🎯 In one sentence
ATI is a framework-independent security layer that checks cross-organization AI agents outside their language model, raising attack containment from 43% to 84% across 14 attack scenarios at about 174 µs of overhead per request.

## ❓ The problem
AI agents now call tools and hand work to agents run by other organizations. When such a request arrives, the receiving side usually cannot answer three questions: is the caller the agent it claims to be, is it running the approved model, prompt and tools, and is this particular action allowed. Agent frameworks such as CrewAI, AutoGen, LangGraph and the OpenAI Agents SDK coordinate agents, but none gives an agent an identity another framework or organization can verify. The existing identity tools were built for other principals: X.509 certificates bind names to keys for hosts and people, OAuth 2.0 authorizes applications acting for a user, and SPIFFE issues short-lived identities to workloads. The hard part is that an agent's behavior depends on which model and system prompt is running at that moment, and a swapped model or edited prompt leaves its key and certificate unchanged. Two terms: **attestation** means checking measured facts about the running system against approved values; **zero trust** means checking every request where it arrives, not trusting it because of its network location.

## 💡 The idea
The key insight is to move every security decision out of the model and into deterministic code. ATI places one reference monitor in front of each organization's agents, and every request and every tool call must pass its checks. The model cannot argue its way past a check, because the check does not read the model's output. The authors adapt the measure-then-attest pattern of TPMs, treating the model, the approved tools and the system prompt as measured items. This lets a receiving organization verify the calling agent's configuration, which the governance tools the authors examined did not allow.

## ⚙️ How it works
### Identity plane
Each agent holds an Ed25519 key pair, and its Agent Identifier (AID) is the SHA-256 hash of its public key, so the AID cannot be chosen freely. Each request must be signed with that key, and the plane checks the signature against the public key. A request with no key is refused at the front door before any agent runs. The simulation shortens the hash to 64 bits, and the authors note that a deployment would use all of it. Identity proves possession of a key, not membership in an organization; the Trust plane supplies that.

### Trust plane: AgentCA and federation
Each organization runs its own certificate authority, an AgentCA, which issues X.509-style certificates that tie an AID to an organization, a role and the CA's signature. The eight organization CAs form a flat federation with no common root, and each CA vouches only for its own organization, enforced by a name constraint that requires the issuer to match the certificate's organization. A request passes only if its certificate is signed by a federation CA, the signature verifies, the AID matches the key that signed the request, the certificate is within its validity window, and neither the AID nor the partner organization is revoked. The authors carry only the fields a check reads: AID, organization, role, issuer, validity period and signature.

### Attestation plane
Identity and Trust establish who is calling, but not what is running behind that identity. Just before the model is called, the Attestation plane compares three things with approved values: the running model, the pinned tool web_search (checked by module and function name), and a SHA-256 hash of the system prompt. Any mismatch is refused before the model is invoked. In the simulation the check runs once when a request enters the federation, not on a timer, and it uses plain comparisons rather than signed reports.

### Policy plane
Policy checks every tool call, such as each db_read and db_write, which the earlier planes never see. It enforces three deterministic rules. First, a write may go only to the organization where the request started, so other organizations can read but not write. Second, a transaction already marked as processed cannot be processed again. Third, the financial-aid office refuses any read that would return more than one record, as a backstop against bulk export. A rule fires only on the call it matches: the cross-organization rule fires only if the agent calls db_write, which is why the replay and delegation numbers stop short of 100%. Prompt injection is not handled here.

### Accountability plane
This plane blocks nothing. For each allowed tool call it appends a log entry containing the AID, the model, a timestamp, the tool and a SHA-256 hash of the arguments, so the log shows what happened without exposing content. Each entry includes the hash of the entry before it and is signed with the organization's CA key, so editing or deleting an entry in the middle breaks the chain and a verify step detects it. The plane also flags any single request that reads private data from two or more offices, the collusion pattern (A11), for later review instead of refusing it, because a legitimate multi-office task looks the same.

### Across two organizations
The two organizations share only their CA public keys. When Org 1's liaison agent calls Org 2, it signs the request and attaches its certificate. Org 2's monitor checks the signature against the AID, the certificate against Org 1's CA and revocation list, attests its own answering agent, and applies its Policy rules; the request may read Org 2's records but not write them. If any check fails, Org 1 gets a refusal and nothing is written. Each organization keeps its own log. The authors release the checks as an Apache-2.0 package, but state that the results come from the testbed, not from that package.

## 🧪 Evidence
| Setting | Baseline | This paper | Metric |
|---|---|---|---|
| Overall attack containment, 14 attack cases, all four blocking planes | 43% | 84% | share of attack trials stopped |
| Same, 11 attack cases evaluated consistently across all configurations | 40% | 80% | share of attack trials stopped |
| External attacker (A1) | 20% | 100% | share stopped |
| Model replacement (A7) | 0% | 100% | share stopped |
| System-prompt tampering (A14) | 27% | 100% | share stopped |
| Replay attack (A5) | 15% | 75% | share stopped |
| Unauthorized delegation (A9) | 16% | 71% | share stopped |
| Benign task success, 6 cases | 91% | 92% | share of correct trials |
| Check time per request, one tool call (Table III) | not applicable | approximately 174 µs (mean 173.8 µs, median 167.7 µs) | time per request |

The authors ran the attacks in a single-process simulation, with 75 trials per attack case, and a panel of five judge models scored each trial. The incremental ablation shows that each plane lifts the attacks it targets: Identity, Trust, Attestation and Policy add 6, 12, 15 and 8 points to overall containment in the table's cumulative columns, while overall containment reached 76% after Attestation. Results are model-dependent without ATI: A6 prompt injection was stopped 0% of the time on two models and 93% on another, so single-model testing can mislead. The authors also report cross-model results: the same attack was refused by some models and executed by others, while the deterministic entry checks prevented nearly all targeted attacks regardless of model. The cross-framework test ran one benign task on each of the six frameworks, and all six returned the correct result, but the 14 attacks were run only on CrewAI and AutoGen. These results do not show performance under heavy load or with thousands of agents, nor behavior with organizations that administer their own infrastructure.

## ⚠️ Limitations
- Prompt injection (A6) stays low with all four blocking planes on, because no plane targets untrusted data in what the agent reads, and the authors treat it as a separate problem.
- Collusion (A11) cannot be blocked without also blocking legitimate multi-office work, so the plane only flags it.
- Attestation relies on measurements reported by the calling agent's host, so it cannot detect falsified reports from a compromised host, and the monitor runs in the same process as the agent.
- Certificate validity metadata sits outside the signed payload, and the log does not detect deletion of entries from the end of the chain.
- Four of the five judge models are also among the models being tested, and the authors say they are not fully independent.
- Certificate transparency, distributed revocation and hardware roots of trust are not built, and encryption of traffic and availability under load are out of scope.
- Some Table I values for A12 to A14 are filled in from how the checks work rather than measured.
- 🔎 Added context: The 84% headline is an average over five models and five wordings, and the authors show that attack success varies strongly by model, so a deployment on one specific model could sit well away from that average.
- 🔎 Added context: The 174 µs figure covers only the ATI checks, timed on one laptop; a real deployment would also carry signatures and certificates across the network at every hop, which the simulation skips.

## 🏗️ What it means for building systems
_Interpretation, not the authors' claim:_ ATI would sit as a security proxy in front of each agent service, with the agent itself unchanged, much as an API gateway or service mesh sits in front of ordinary services. The cost would be per-request signature checks, which the authors measure at well under a millisecond, plus per-organization certificate issuance, a revocation process, and a signing key protected for the log. To trust it you would need host attestation you can rely on, since the authors' measurements come from the host, and a privacy review of logs and certificates, which the authors say exposes organizational relationships.

## 🔑 Key takeaways
- Identity checks at the front door stop only outsiders with no credentials (A1 rose from 20% to 100%); they do not prove that the model or prompt is the approved one.
- Attestation of the model, tools and system prompt stops model replacement (A7, 0% to 100%) and system-prompt tampering (A14, 27% to 100%), which identity checks let through.
- With all four blocking planes on, overall containment rose from 43% to 84% across 14 attack cases, while benign task success moved from 91% to 92%.
- Policy rules enforced in code only partly stop replay and cross-organization writes (A5 rose from 15% to 75%, A9 from 16% to 71%) because each rule fires only on the call it matches.
- Model-level safety varies widely: the same attack can be refused by one model and carried out by another, so single-model evaluations can overstate security.
- The full check stack adds about 174 µs per request in the simulation, mostly spent verifying signatures in Identity and Trust.

## 📚 Reading plan
### From the paper
- [ATI repository](https://github.com/CalPolyPomonaCS/Agent-Trust-Infrastructure-ATI-Official) — the open-source ati package and its README with setup steps and token formats.
- [Microsoft Agent Governance Toolkit](https://github.com/microsoft/agent-governance-toolkit) — the closest governance tool the authors compare against, which reports prompt-injection detection rates.
- [Model Context Protocol](https://modelcontextprotocol.io) — the protocol the authors say ATI does not depend on, and the one whose MCP Inspector flaw they cite.
- [CVE-2025-49596](https://nvd.nist.gov/vuln/detail/CVE-2025-49596) — the remote code execution flaw in an MCP component that motivates the tool-level checks.
- [SPIFFE](https://spiffe.io) — the workload-identity model the Identity plane follows.

### To go further
- Zero Trust Architecture, NIST SP 800-207 — the never-trust-by-default rule that ATI applies to agents.
- Binding Agent ID (arXiv:2512.17538) — an accountability approach that proves which program an agent runs with zero-knowledge proofs.
- Authenticated Delegation and Authorized AI Agents (arXiv:2501.09674) — scoped, expiring delegation tokens that the authors chose not to simulate.
- The Entity Attestation Token (RFC 9711) and IETF WIMSE workload credentials — the token formats ATI uses for identity and attestation.
- Confidential computing attestation with TPM and TEE hardware — what the authors say would strengthen the host-trust gap.

## Concepts
[Multi-agent orchestration](/concepts/multi-agent-orchestration.md) · [Model Context Protocol (MCP)](/concepts/model-context-protocol.md) · [OAuth 2.0](/concepts/oauth2.md) · [OpenID Connect](/concepts/oidc.md) · [Threat modeling](/concepts/threat-modeling.md) · [Indirect prompt injection](/concepts/indirect-prompt-injection.md) · [Zero-trust architecture](/concepts/zero-trust-architecture.md)
