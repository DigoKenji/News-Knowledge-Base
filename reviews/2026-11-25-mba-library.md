---
type: Review Checklist
title: MBA library digest - certification checklist
description: 16 new draft concepts and 38 enriched concepts from the AI/ML and security books Kenji added to FIAP-MBA/Materiais/Referencias on 2026-11-24.
status: open
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
---

# MBA library digest — checklist

Kenji added 31 PDFs to `FIAP-MBA/Materiais/Referencias` on 2026-11-24 (29 distinct; all AI/ML or security). This PR turns them into knowledge-base entries. Nothing is quoted beyond short phrases; every claim points to a chapter, section or page.

**How to answer:** tick a box to certify, or write what to change after `Fix:`. Same rules as [the 2026-11-24 checklist](/reviews/2026-11-24-certification.md).

## Part 1 — New draft concepts (16)

### AI/ML (8)

- [ ] **M01 · [Gradient descent and backpropagation](/concepts/ai-ml/gradient-descent-backpropagation.md)** — How neural networks learn - gradient descent repeatedly nudges the weights against the gradient of the loss, and backpropagation computes that gradient efficiently with the chain rule, layer by layer from the output back.
  - Sources: 4 (3 from the MBA library, 1 paper/standard)
  - Fix: 
- [ ] **M02 · [Regularization and the bias-variance trade-off](/concepts/ai-ml/regularization.md)** — Techniques that reduce overfitting by limiting how closely a model can fit its training data (penalties, dropout, early stopping, augmentation), framed by the trade-off between bias (too simple) and variance (too sensitive to the sample).
  - Sources: 4 (3 from the MBA library, 1 paper/standard)
  - Fix: 
- [ ] **M03 · [Tokenization](/concepts/ai-ml/tokenization.md)** — Splitting text into the units a language model reads and writes - usually subword pieces learned from data (BPE, WordPiece, SentencePiece) - which sets the model's vocabulary, its context limits and its cost per request.
  - Sources: 4 (2 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M04 · [Fine-tuning](/concepts/ai-ml/fine-tuning.md)** — Further training a pre-trained model on a smaller, task-specific dataset - supervised fine-tuning on examples, then optional preference alignment (RLHF, DPO) - to change its behaviour or format rather than to add fresh facts.
  - Sources: 5 (3 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M05 · [Prompt engineering](/concepts/ai-ml/prompt-engineering.md)** — Designing the instructions, examples and context given to a model so it produces the wanted output without changing its weights - including versioning and evaluating prompts, and defending them against injection and extraction.
  - Sources: 5 (3 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M06 · [Generative models (VAE, GAN, diffusion, autoregressive)](/concepts/ai-ml/generative-models.md)** — Models trained to produce new data that resembles their training data by learning its probability distribution - the main families are variational autoencoders, GANs, autoregressive models (including LLMs), normalizing flows, energy-based and diffusion models.
  - Sources: 6 (3 from the MBA library, 3 paper/standard)
  - Fix: 
- [ ] **M07 · [Quantization](/concepts/ai-ml/quantization.md)** — Storing and computing a model's weights (and sometimes activations) in lower-precision numbers - e.g. 8-bit or 4-bit integers instead of 16/32-bit floats - to cut memory and speed up inference at a small cost in accuracy.
  - Sources: 5 (3 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M08 · [MLOps and LLMOps](/concepts/ai-ml/mlops-llmops.md)** — The engineering practices that take ML and LLM systems from notebook to reliable production - versioned data and models, automated pipelines (feature, training, inference), deployment, monitoring and feedback - with LLMOps adding prompts, guardrails and LLM-specific evaluation.
  - Sources: 3 (2 from the MBA library, 1 paper/standard)
  - Fix: 

### Security (8)

- [ ] **M09 · [Penetration testing and vulnerability assessment](/concepts/security/penetration-testing.md)** — Authorised, scoped attempts to find and safely exploit weaknesses in a system the way an attacker would, to prove real risk and drive fixes - distinct from an automated vulnerability scan, which only lists known weaknesses.
  - Sources: 6 (4 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M10 · [Incident response](/concepts/security/incident-response.md)** — The prepared, repeatable process for handling a security incident - preparation, detection and analysis, containment, eradication and recovery, and post-incident lessons - run by a defined team with runbooks, evidence handling and communication plans.
  - Sources: 6 (4 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M11 · [Security controls](/concepts/security/security-controls.md)** — Measures that reduce risk, classified by how they are implemented (management/administrative, operational, technical, physical) and by what they do (preventive, detective, corrective, deterrent, compensating).
  - Sources: 4 (3 from the MBA library, 1 paper/standard)
  - Fix: 
- [ ] **M12 · [Security risk management and business impact analysis](/concepts/security/security-risk-management.md)** — The continuous cycle of identifying assets, threats and vulnerabilities, assessing likelihood and impact (including a business impact analysis of critical processes), choosing a treatment - mitigate, transfer, avoid or accept - and monitoring the result.
  - Sources: 5 (3 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M13 · [Shared responsibility model](/concepts/security/shared-responsibility-model.md)** — The split of security duties between a cloud provider and its customer - the provider secures "of the cloud" (facilities, hardware, virtualisation), the customer secures "in the cloud" (data, identities, configuration) - with the line moving up as services go from IaaS to SaaS.
  - Sources: 5 (3 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M14 · [Network security controls (firewalls, segmentation, IDS/IPS)](/concepts/security/network-security.md)** — Controls that restrict and watch traffic between networks and hosts - firewalls that enforce an access policy, segmentation and DMZs that limit what each zone can reach, VPNs that protect traffic in transit, and intrusion detection or prevention systems that spot attacks.
  - Sources: 5 (4 from the MBA library, 1 paper/standard)
  - Fix: 
- [ ] **M15 · [Vulnerability management](/concepts/security/vulnerability-management.md)** — The continuous program of finding vulnerabilities (asset inventory, scanning, advisories), prioritising them by severity and exposure (e.g. CVSS plus context), remediating or mitigating them, and verifying and reporting the result.
  - Sources: 5 (3 from the MBA library, 2 paper/standard)
  - Fix: 
- [ ] **M16 · [Social engineering and phishing](/concepts/security/social-engineering.md)** — Attacks that manipulate people rather than systems - phishing, spear phishing, pretexting, tailgating - by exploiting trust, authority, urgency or helpfulness, countered by awareness training, verification procedures and technical controls such as MFA.
  - Sources: 4 (3 from the MBA library, 1 paper/standard)
  - Fix: 

## Part 2 — Enriched existing concepts (38)

Each got a **From the MBA library** section (book, chapter/pages, one line on what it adds) and the books added to its sources. Certified definitions were **not** changed, so these stay `stable`; skim and tell me if any citation looks wrong.

⚠️ Four old seed stubs had no definition at all, so I added a short one. These are new text inside a stable concept, please read them: [rag](/concepts/ai-ml/rag.md), [kv-cache](/concepts/ai-ml/kv-cache.md), [inference-tradeoffs](/concepts/ai-ml/inference-tradeoffs.md), [threat-modeling](/concepts/security/threat-modeling.md).

- **ai-ml** (23): [machine-learning](/concepts/ai-ml/machine-learning.md), [supervised-learning](/concepts/ai-ml/supervised-learning.md), [unsupervised-learning](/concepts/ai-ml/unsupervised-learning.md), [reinforcement-learning](/concepts/ai-ml/reinforcement-learning.md), [deep-learning](/concepts/ai-ml/deep-learning.md), [training-vs-inference](/concepts/ai-ml/training-vs-inference.md), [overfitting](/concepts/ai-ml/overfitting.md), [model-evaluation](/concepts/ai-ml/model-evaluation.md), [transformer](/concepts/ai-ml/transformer.md), [large-language-model](/concepts/ai-ml/large-language-model.md), [embedding](/concepts/ai-ml/embedding.md), [rag](/concepts/ai-ml/rag.md), [ai-agent](/concepts/ai-ml/ai-agent.md), [agent-memory](/concepts/ai-ml/agent-memory.md), [llm-as-a-judge](/concepts/ai-ml/llm-as-a-judge.md), [lora](/concepts/ai-ml/lora.md), [kv-cache](/concepts/ai-ml/kv-cache.md), [speculative-decoding](/concepts/ai-ml/speculative-decoding.md), [distillation](/concepts/ai-ml/distillation.md), [in-context-learning](/concepts/ai-ml/in-context-learning.md), [inference-tradeoffs](/concepts/ai-ml/inference-tradeoffs.md), [context-engineering](/concepts/ai-ml/context-engineering.md), [ai-gateway](/concepts/ai-ml/ai-gateway.md)
- **security** (12): [cia-triad](/concepts/security/cia-triad.md), [threat-vulnerability-risk](/concepts/security/threat-vulnerability-risk.md), [authentication-authorization](/concepts/security/authentication-authorization.md), [least-privilege](/concepts/security/least-privilege.md), [encryption](/concepts/security/encryption.md), [identity-access-management](/concepts/security/identity-access-management.md), [attack-surface](/concepts/security/attack-surface.md), [secure-software-development](/concepts/security/secure-software-development.md), [common-vulnerability-classes](/concepts/security/common-vulnerability-classes.md), [ai-security](/concepts/security/ai-security.md), [indirect-prompt-injection](/concepts/security/indirect-prompt-injection.md), [threat-modeling](/concepts/security/threat-modeling.md)
- **cloud-infra** (3): [cloud-computing](/concepts/cloud-infra/cloud-computing.md), [cloud-service-models](/concepts/cloud-infra/cloud-service-models.md), [virtualization-containers](/concepts/cloud-infra/virtualization-containers.md)

## Part 3 — The books

| Book | Used for | Note |
|---|---|---|
| Huyen, *AI Engineering* (2025) | 24 AI/ML entries | Main source for LLM apps: eval, prompting, RAG, agents, fine-tuning, inference |
| Goodfellow et al., *Deep Learning* (2016) | fundamentals, regularization, generative models | Already a source; page locators added |
| Deisenroth et al., *Mathematics for ML* (2020) | ML, gradients, regularization | Free official PDF |
| Raschka et al., *ML with PyTorch and Scikit-Learn* (2022) | classical ML, evaluation, transformers, RL, GANs | File was named "Hands-On Machine Learning with Pytorch" |
| Tunstall et al., *NLP with Transformers* (2022) | tokenization, transformer, distillation, quantization | |
| Iusztin & Labonne, *LLM Engineer's Handbook* (2024) | RAG, fine-tuning, inference, MLOps/LLMOps | |
| Foster, *Generative Deep Learning*, 2nd ed. (2023) | generative models | |
| Stallings & Brown, *Computer Security*, 3rd ed. (2015) | 18 security entries | Main security textbook |
| Chapple & Seidl, *CSA+ Study Guide* CS0-001 (2017) | IR, vulnerability management, IAM, SSDLC | The CS0-002 file is the *Practice Tests* book (questions only), not cited |
| Dotson, *Practical Cloud Security* (2019) | cloud security, IR, vulnerability management | |
| AWS, *Security Incident Response Guide* (2019) | IR, shared responsibility | Vendor whitepaper |
| Stuttard & Pinto, *Web Application Hacker's Handbook* (2008, 1st ed.) | vulnerability classes, auth, SSDLC | Cited for defensive lessons only |
| Easttom, *Penetration Testing Fundamentals* (2018) | pen testing, vulnerability scanning | |
| Davies, *Networking Fundamentals* (2019) | network security vocabulary | |
| Gibson, *Security+ SY0-501* (2017) | control types, risk/BIA, social engineering | Secondary only: exam is retired (current is SY0-701) |
| Beaver, *Hacking For Dummies*, 6th ed. (2018) | pen testing, social engineering | |
| Allen et al., *Kali Linux*, 2nd ed. (2014) | pen-test methodology and reporting | Tool chapters not used |
| *Risk Management, Security Compliance and Audit Controls* (handout, 23 pp.) | risk, controls, threat modelling | Author not stated; low evidence tier |

**Not used:** *Hacking: Ultimate Hacking for Beginners* (27 pp., low quality), `noname.pdf` (9-page vendor audit checklist), *Secure the Infosec Bag* and *Tribe of Hackers* (career advice), Messer's SY0-501 notes and the Syngress Security+ guide (2003) (both repeat Security+ material), *Advanced Penetration Testing* and *Becoming the Hacker* (attack playbooks, nothing defensive to add yet). Duplicates: *AI Engineering* and the risk handout were each uploaded twice.

## Foundations (updated with the two books added 2026-11-25)

Kenji added two architecture books on 2026-11-25: the **BIZBOK Guide v9.0** (Business Architecture Guild, 2020) and **Whelan & Meaden, *Business Architecture: A Practical Guide*** (Gower, 2012). The BIZBOK glossary quotes the FEAPO-adopted definitions (2017) of business, application, data, technical and enterprise architecture, so the Guild is a second issuing body next to The Open Group for 7 of the 10 foundations:

| Foundation | Second body quote | Evidence now |
|---|---|---|
| [Business Architecture](/foundations/business-architecture.md) | BIZBOK glossary (FEAPO) + Whelan & Meaden (tier C) | partial → **yes** |
| [Business capability](/foundations/business-capability.md) | BIZBOK glossary *Capability* + Whelan & Meaden (tier C) | partial → **yes** |
| [Application Architecture](/foundations/application-architecture.md) | BIZBOK glossary (FEAPO) | partial → **yes** |
| [Data Architecture](/foundations/data-architecture.md) | BIZBOK glossary (FEAPO) | partial → **yes** |
| [Technology Architecture](/foundations/technology-architecture.md) | BIZBOK glossary *Technical Architecture* (FEAPO) | partial → **yes** |
| [Enterprise Architecture](/foundations/enterprise-architecture.md) | BIZBOK glossary (FEAPO); TOGAF still has no formal entry | no → partial |
| [Solution Architecture](/foundations/solution-architecture.md) | BIZBOK glossary (quotes Critchley 2006, an individual) | stays partial |

Still `partial`: Architecture, Enterprise, Architecture domain. Neither book defines them independently (Whelan & Meaden quote IEEE 1471 and TOGAF).

All foundations stay `draft` until you certify them. Two caveats:
- v9.0 is an older BIZBOK. The Guild is on v15+, so a wording check against the current glossary would be ideal.
- The BIZBOK terms of use allow quoting for education with credit. The KB quotes only single glossary definitions, credited as "Business Architecture Guild®, BIZBOK® Guide v9.0".

I also removed the lecturer's name from three foundation pages (EN + pt-br) to follow the "never cite lecturer names" rule.

- [ ] Certify the 5 foundations that now meet the two-body rule (business architecture, business capability, application, data, technology architecture)
  - Fix: 
