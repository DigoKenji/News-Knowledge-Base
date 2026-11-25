---
type: Concept
title: 'Social engineering and phishing'
description: 'Attacks that manipulate people rather than systems - phishing, spear phishing, pretexting, tailgating - by exploiting trust, authority, urgency or helpfulness, countered by awareness training, verification procedures and technical controls such as MFA.'
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
- title: 'Beaver, K. (2018). Hacking For Dummies, 6th edition. Wiley'
  resource: 'https://books.google.com/books?vid=ISBN9781119485476'
  kind: book
  locator: 'ch. 6 Social Engineering, pp. 67-84 (countermeasures p. 80)'
- title: 'Gibson, D. (2017). CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide. YCDA'
  resource: 'https://books.google.com/books?vid=ISBN9781939136053'
  kind: book
  locator: 'ch. 6 Social Engineering pp. 278-294 (phishing p. 281, spear phishing p. 284, why it works p. 292)'
- title: 'Stallings, W. and Brown, L. (2015). Computer Security: Principles and Practice, 3rd edition. Pearson'
  resource: 'https://books.google.com/books?vid=ISBN9780133773927'
  kind: book
  locator: '§6.5 Propagation - Social Engineering, p. 218; §6.8 Phishing, p. 224; §17.1 Security Awareness, Training, and Education, p. 557'
- title: 'MITRE ATT&CK. Initial Access tactic (TA0001), incl. Phishing'
  resource: 'https://attack.mitre.org/tactics/TA0001/'
  kind: docs
---

# Social engineering and phishing

**Definition.** Beaver calls social engineering "people hacking": maliciously exploiting the trusting nature of people to obtain information or access (p. 67). Phishing sends deceptive messages that lure people to reveal credentials, open malware or approve payments; spear phishing targets a specific person with researched details; whaling targets executives; pretexting invents a scenario (a fake IT helpdesk call); physical variants include tailgating into buildings. Gibson explains why it works through principles such as authority, intimidation, consensus, scarcity, urgency, familiarity and trust. Stallings and Brown treat it as a propagation route for malware (trojans, spam) and as an information-theft payload (phishing), and answer it with security awareness, training and education.

Domain: [security](/domains/security.md) · subdomain `security`. New concept from the MBA library (books added to FIAP-MBA/Materiais/Referencias, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Phishing is among the most common initial-access techniques (MITRE ATT&CK TA0001), so it is often where an incident starts. Defences combine people, process and technology: training and phishing simulations, call-back verification for payment or password changes, email filtering and authentication (SPF, DKIM, DMARC), and phishing-resistant MFA so stolen passwords alone are not enough. Prompt injection is a close cousin for AI agents: manipulating the model instead of the person.

## Example

Finance receives an urgent email that looks like it is from the CEO asking for a same-day transfer to a new supplier. The procedure requires calling the CEO on a known number; the call shows the email is fake.

## Broader concepts

- [Threat, vulnerability and risk](/concepts/security/threat-vulnerability-risk.md)

## Related concepts

- [Authentication vs authorization](/concepts/security/authentication-authorization.md)
- [Indirect prompt injection](/concepts/security/indirect-prompt-injection.md)

## Sources

1. Beaver, K. (2018). *Hacking For Dummies, 6th edition*. Wiley. <https://books.google.com/books?vid=ISBN9781119485476> — ch. 6 Social Engineering, pp. 67-84 (countermeasures p. 80). Why attackers use it, how tests are run, and countermeasures.
2. Gibson, D. (2017). *CompTIA Security+ Get Certified Get Ahead: SY0-501 Study Guide*. YCDA. <https://books.google.com/books?vid=ISBN9781939136053> — ch. 6 Social Engineering pp. 278-294 (phishing p. 281, spear phishing p. 284, why it works p. 292). Attack types and the persuasion principles behind them (SY0-501, 2017).
3. Stallings, W. and Brown, L. (2015). *Computer Security: Principles and Practice, 3rd edition*. Pearson. <https://books.google.com/books?vid=ISBN9780133773927> — §6.5 Propagation - Social Engineering, p. 218; §6.8 Phishing, p. 224; §17.1 Security Awareness, Training, and Education, p. 557. Social engineering as malware vector and the awareness program that counters it.
4. MITRE ATT&CK. Initial Access tactic (TA0001), incl. Phishing. <https://attack.mitre.org/tactics/TA0001/> — ATT&CK classifies phishing as an initial-access technique.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
