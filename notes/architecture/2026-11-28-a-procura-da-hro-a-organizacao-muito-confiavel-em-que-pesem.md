---
type: Article Summary
title: À procura da HRO, a organização muito confiável (em que pesem os paradoxos)
description: Governance meant to keep humans in AI-assisted threat detection can quietly erode the frontline judgment reliable
  detection depends on.
resource: https://mitsloanreview.com.br/a-procura-da-hro-a-organizacao-de-alta-confiabilidade/
tags:
- architecture
- business-architecture
- artificial-intelligence
- machine-learning
- concept-drift
- human-in-the-loop
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-28T06:47:00-03:00'
sources:
- resource: https://mitsloanreview.com.br/a-procura-da-hro-a-organizacao-de-alta-confiabilidade/
  title: À procura da HRO, a organização muito confiável (em que pesem os paradoxos)
  author: unknown
status: stable
source_url: https://mitsloanreview.com.br/a-procura-da-hro-a-organizacao-de-alta-confiabilidade/
source: MIT Sloan Management Review Brasil
source_type: blog
published: '2026-11-26'
domain: architecture
subdomain: business-architecture
concepts:
- artificial-intelligence
- machine-learning
- concept-drift
- human-in-the-loop
selected_by: architecture-lane (PC run)
content: pc-run
complete: 'yes'
notion_url: https://app.notion.com/p/426baf74172d815eb1e0cf2924a6562f
foundation_candidates:
- accountability signals
- naturalistic decision making
- cognitive anchoring
---

## 🎯 Core thesis
Governance designed to keep humans in control of AI-assisted detection can, through implicit accountability signals, erode the frontline judgment that reliable detection depends on, and it can be recalibrated to avoid that.

## 🧭 Why the author wrote it
High-reliability organizations (HROs), such as nuclear plants, aircraft carriers and air traffic control, have long shown that safety depends on how people exercise judgment under uncertainty, not only on technology. Organizations now adopt AI detection tools under governance frameworks that borrow HRO principles, but the author's research suggests these frameworks may carry a hidden side effect. If the thesis is right, governance has to be audited for the messages it sends to the people operating the system, not only for its technical controls.

## Opening: lessons from high-stakes operations
The article opens with the HRO research finding that reliable organizations protect frontline judgment, distribute decision authority to the people closest to a problem, and treat emerging anomalies as signals rather than noise. These lessons were often learned through disaster.

### High-reliability organization (HRO)
An HRO is an organization that operates under extreme risk and depends on keeping weak signals visible. The article frames HRO principles as three commitments: protect frontline judgment, push authority toward whoever is nearest the problem, and read anomalies as information. These principles are the benchmark against which the rest of the article tests AI governance.

### Alarm overload at Three Mile Island
At Three Mile Island (Pennsylvania, USA, 1979), operators faced more than 100 simultaneous alarms with no prioritization. The alarm flood produced cognitive paralysis instead of an adequate response, just as the reactor began to melt. The lesson is that more signal is not more safety: when every alert competes equally, people can no longer tell which one matters.

### Columbia and the cost of escalation
Before the Columbia's final mission in 2003, NASA engineers suspected that foam insulation on one of the shuttle's wings had been damaged during launch. They had channels to take their concerns to senior levels, but avoided them because of the organizational cost of challenging senior leaders. The investigation report, quoted in the article, concluded that management techniques created barriers, unnoticed by those who set them, that kept engineers' concerns and dissenting views away and contributed to blind spots that hid the danger from the foam impact. The lesson is that an escalation channel that exists on paper fails when using it is socially expensive.

## Opening: AI detection and the governance paradox
Modern AI detection systems are adaptive machine learning tools that flag fraudulent transactions, network intrusions, compliance breaches and early signs of patient deterioration. The article says they promise to recognize patterns better than humans and at scale, but they remain subject to bias, model drift and hallucinations.

### False positives and false negatives
A false positive floods the organization with unnecessary alerts, overloads it and wastes resources. A false negative, a missed threat, can have catastrophic consequences. The article cites the Salt Typhoon attacks on US telecommunications networks and the 2024 Change Healthcare data breach as examples of the stakes. Both error types matter, and the paradox starts here: governance frameworks built to protect against unwanted AI behavior may undermine the human vigilance they rely on.

## Governance policies send signals that shape engagement
The article's central claim is that governance structures are not neutral when applied to oversight. Through the author's research with colleagues, it argues they send implicit relational messages about who is trusted, which kinds of reasoning count, and when independent thinking is welcome. These messages shape how people exercise their judgment. Technology professionals, risk managers and other leaders who design AI governance increasingly aim to follow an HRO-like model, although that link is not always acknowledged.

### Accountability signals
Accountability signals are the implicit messages that governance mechanisms send to employees. They can produce two opposite responses. In some cases they yield automatic compliance: people choose decisions justified by procedure because those are easier to defend, not because they exercise independent judgment. In others they encourage reckless deviation, where people stop following compliance procedures to protect their own interests. The article notes there are no longitudinal studies yet that systematically document the effects of these design choices, but says the underlying mechanism, relational signaling, is well established in accountability research. Its conclusion for this section is that the most widespread principles of responsible governance may, paradoxically, create conditions for less safe environments.

## How accountability signals can erode reliability
The article reviewed 14 of the main AI governance frameworks, in ongoing research with law and public policy experts, covering finance, health, cybersecurity and cross-sector guidance. Recurring elements mostly aim to preserve human oversight. Four practices appear most often: a designated person for human oversight ("human in the loop"); escalation and override mechanisms so that human expertise prevails when needed; documentation and audits to stay sensitive to operating conditions; and oversight scaled to the level of risk. The author stresses that these practices are well-intentioned and were designed to correct problems associated with both "bad models" and "bad users", such as automation bias and complacency. Yet each carries an accountability signal that can undermine the reliability it is meant to protect.

### Human in the loop: "your role is to review, not decide"
This practice formalizes frontline staff as reviewers and approvers of AI alerts, with their responsibilities written into workflows and checklists. The article argues it changes how analysts see their role: instead of formulating risk assessments, they audit machine outputs. Auditing asks whether a conclusion already presented should be challenged. Judgment requires integrating context, uncertainty and tacit knowledge to reach an independent assessment. When the AI result comes first, the system's assessment works as a cognitive anchor. Analysts stop asking "what is happening here?" and start asking "is there a reason to contest what the system found?" When the system raises no alert, analysts may read the silence as evidence of no threat, which reduces proactive investigation of risks the system may have missed. Less experienced analysts are most affected, because without a developed intuition they treat AI output as definitive rather than provisional.

> ❌ **Anti-pattern — Auditing instead of judging:** the reviewer's role is defined as checking AI conclusions through formal workflows and checklists. **Why people do it:** it keeps a human in the loop and prevents responsibility from shifting onto the system. **Consequence:** AI output becomes an anchor, and a missing alert reads as a missing threat. **Fix:** require a provisional, independent reading before the AI result is shown (see "Form your own view, then compare" below).

### Escalation protocols: "judgment belongs to whoever is above you"
Governance emphasizes the ability to challenge or replace AI recommendations. Organizations operationalize this through formal hierarchical escalation, often requiring a superior's or a committee's approval before a decision can be contested. The intention is to avoid blind automation and provide procedural safeguards. But these mechanisms sit inside power hierarchies, so they signal that the independent judgment of frontline staff is not fully legitimate. Disagreeing with an AI recommendation becomes an exception that must be justified upward, not a routine part of professional autonomy.

The article gives concrete examples. Nurses using sepsis detection tools built on Epic can usually override an alert only with a supervising physician's authorization. Many report stopping overrides, not because they agree with the system, but because escalating is itself a burden that requires interrupting or challenging more senior people. Others ignore the alerts altogether and pass responsibility upward. In security operations, a 2023 survey of 2,000 security analysts found that SOC teams received an average of 4,484 alerts per day, and 67% were not investigated because of alert fatigue and false positives. The author's point is that escalation turns frontline discretion into a request for permission and moves decision power toward people further from the operation, even though HRO practice depends on valuing expertise closest to the problem.

> ❌ **Anti-pattern — Escalation as permission-seeking:** divergence from the AI must be approved by a supervisor or committee before it counts. **Why people do it:** it provides procedural safeguards against blind automation. **Consequence:** staff skip escalation or ignore alerts, and decisions drift toward people far from the operation. **Fix:** make disagreement trigger a conversation with a peer rather than an approval chain (see "Consult to decide" below).

### Documentation and audit: "decisions must be justifiable to others, not just appropriate to the situation"
The third principle is traceability: AI-supported decisions must be recorded, explained and reviewable, to ensure accountability, promote learning and provide transparency to internal and external oversight. The article notes that documentation requirements quietly shift judgment from interpreting events in real time to justifying them afterward. When decisions must be rationalized and reviewed by third parties, people begin to prioritize what will seem reasonable after the fact over what seems appropriate in the moment. The burden is concrete: in anti-money-laundering compliance, an estimated 90% to 95% of transaction monitoring alerts are false positives, yet each one must be investigated and documented individually to meet audit requirements.

The effect falls hardest on pattern-recognition judgment, which the article calls essential to expert performance under pressure. Research on naturalistic decision-making shows that practitioners often rely on rapid, experience-based pattern recognition. Such judgments are neither irrational nor merely intuitive; they result from extensive tacit learning. Because they are hard to explain after the fact and hard to fit into standard documentation formats, they tend to be dismissed in audit-driven environments. Over time, frontline staff avoid judgments they cannot easily document, which weakens their ability to notice changing operating conditions in real time.

> ❌ **Anti-pattern — Documenting only explicit reasoning:** records accept only rule-based, linear justifications. **Why people do it:** standard audit formats favor them and make decisions easy to trace. **Consequence:** pattern-based expertise is excluded from the record and gradually avoided in practice. **Fix:** add a field for the basis and confidence of each decision (see "State the basis" below).

### Proportional oversight: "know before acting which decisions will later be classed as high-risk"
Governance scales oversight with risk. High-impact decisions receive additional analysis, while routine decisions are streamlined to preserve efficiency. The intention is to allocate scarce human attention where it matters most. The author's concern is that these models require users to anticipate risk under uncertainty: they must classify a decision as routine or exceptional before its consequences are known, even though consequences often become clear only later. Routine situations can escalate quickly, and cases classed as exceptional can end without serious consequences.

In practice, professionals adapt to the classification and focus their attention where the system says risk lies. Low-risk situations are then resolved quickly, without the same follow-up. By framing uncertainty in predefined categories and thresholds, these models can reduce vigilance precisely in routine work, where early warning signs are most likely to appear. Users learn that careful judgment is expected only when a case crosses a predefined line.

> ❌ **Anti-pattern — Vigilance by threshold:** attention concentrates on cases above a predefined risk line, and routine cases are closed quickly. **Why people do it:** it allocates scarce human attention to the cases the model marks as most dangerous. **Consequence:** weak early signals in routine cases go unnoticed, because staff learn that careful judgment is expected only above the threshold. **Fix:** question accepted routine outcomes as rigorously as deviations (see "Question what is accepted, not only what deviates" below).

## How to recalibrate accountability signals for reliability
The author does not argue for abandoning oversight. Implementing AI without any governance is not a reasonable option, and the disasters that motivated HRO-inspired models were real. The task is to design operating principles that restore judgment within professional roles rather than replace it. Each of the four practices can be recalibrated to send a different signal.

### Form your own view, then compare
Human in the loop positions staff as auditors of AI conclusions. The remedy is to require users to articulate an initial assessment before consulting the outputs. This keeps sense-making an active process of interpretation and reduces false negatives by encouraging analysts to look for threats the system may have missed. The mechanism need not be complicated: a brief, structured assessment, even a single sentence recording the analyst's initial reading, changes the cognitive starting point, so the analyst begins from their own reading of the situation rather than from the machine's. The approach reflects a bias-reduction principle the article links to intelligence analysis: form independent judgments before encountering information that could act as an anchor.

### Consult to decide
Escalation protocols route disagreement up the hierarchy and turn frontline discretion into a request for permission. The alternative is lateral: consultation and peer review that lower the relational cost of raising concerns while still recognizing the expertise of those closest to the problem. A nurse could consult a colleague instead of escalating to a supervising physician. A security analyst could raise a concern with a peer for a second opinion before it becomes a formal challenge to the system's decision. The critical change is structural: disagreement with the AI should trigger a conversation, not a chain of approvals.

### State the basis
Documentation formats favor linear, rule-based justifications and so set aside recognition-based expertise. The article's point is that the problem is not documentation as such, but formats that treat only explicit reasoning as legitimate. The proposed remedy is to add a "confidence level and basis for the decision" field that distinguishes the grounds behind each decision. Example options include "followed the established rule," "recognized a pattern I had seen before," and "something looks off, but I cannot yet explain why." Tacit expertise is then recognized and recorded in audit processes rather than excluded. In anti-money-laundering work, analysts could classify some cases as "alerts dismissed on recognized patterns" without writing a fully detailed justification for each closed alert. Traceability is preserved, and the way expert judgment actually operates is respected.

### Question what is accepted, not only what deviates
Proportional oversight concentrates vigilance on cases classed as high-risk and reduces attention elsewhere. The counterweight is to examine conformity with the same rigor as deviation. In most governance models, accepting AI outputs is treated as natural, and only disagreement triggers review. But accepting a recommendation without questioning it can be as risky as contesting it. Periodically asking professionals to briefly reflect on why they agree with a routine classification turns alignment into active judgment rather than its absence. It also works as a calibration mechanism: if analysts cannot articulate why they agree, the organization receives an early signal that vigilance is slipping. For security operations centers, the article suggests random "concordance audits" of a small percentage of routine cases, which keeps low-risk classifications under periodic check without creating an unsustainable review burden.

## Closing: the design window is open now
The author notes that AI governance leaders tend to worry about "bad models" or "bad users," and calls these emphases incomplete. Design choices in accountability mechanisms can reshape how employees interpret the value of their own judgment and the limits of their authority, through implicit relational signals that may erode the very vigilance these structures should protect. The paradox deserves attention now, while governance architectures are still being designed and can be recalibrated. For leaders implementing AI in threat detection, the imperative is to audit accountability structures with the same rigor applied to the algorithms, because the signals governance sends to the people who operate the system may matter as much as the system the organization adopts.

## 🧠 Mental models
### Accountability signals
**The model:** Governance mechanisms send implicit messages about who is trusted, which reasoning counts and when independent thought is welcome. People respond to these messages as much as to the rules themselves.
**How to apply it:** Before adding a control, ask what it tells the person doing the work, and watch for two opposite responses: automatic compliance and prudent evasion.

### Audit versus judgment
**The model:** Checking whether a conclusion should be challenged starts from someone else's answer. Forming a judgment starts from the situation and integrates context, uncertainty and tacit knowledge.
**How to apply it:** When designing a review step, require a provisional, independent reading before the system's output is shown.

### Recognition-based expertise
**The model:** Experienced practitioners in high-pressure settings often recognize patterns quickly. This judgment is the product of extensive tacit learning, not of irrationality.
**How to apply it:** Give recognition-based decisions a place in records and reviews, so they are neither excluded nor forced into rule-based justifications.

## 🔑 Key takeaways
- The article presents protecting frontline judgment, distributing authority to those closest to a problem, and treating anomalies as signals as the core lessons of HRO research.
- Columbia's engineers had escalation channels but avoided them, because challenging senior leaders carried real organizational cost. A channel that exists on paper is not a working channel.
- Human in the loop, escalation, documentation and risk-scaled oversight are well-intentioned, yet each sends an implicit accountability signal that can weaken the reliability it aims to protect.
- When the AI result is the first thing an analyst sees, it becomes a cognitive anchor, and a missing alert can be read as a missing threat, especially by less experienced staff.
- Escalation chains turn frontline discretion into a request for permission. In the article's examples, nurses skipped overrides and analysts passed responsibility upward.
- Documentation formats that accept only explicit reasoning push pattern-based expertise out of the record. The article proposes a field for the basis and confidence of each decision.
- Recalibration keeps oversight but changes its signal: a short initial assessment, peer consultation, a basis field, and random audits of routine decisions.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Business: primary (governance structure, roles, decision rights, escalation and documentation processes). Application: context only, since AI detection tools and alert systems appear but their design is not discussed. Data and Technology: not covered. |
| Scope | Segment or capability: the threat-detection and supervision function inside organizations across finance, health and cybersecurity. Not enterprise strategy, and not one specific solution. |
| Stakeholders and concerns | Frontline analysts and nurses (professional judgment, workload, the cost of escalating). Supervisors and committees (accountability, legitimacy of decisions). Technology, risk and AI governance leaders (controls that work). Auditors and regulators (traceability). Organizations (missed threats from false negatives, wasted resources from false positives). |
| Frameworks and notations | None named. HRO research, naturalistic decision-making and "human in the loop" are named as concepts or research traditions, not architecture notations. |

### Decisions and trade-offs
- **Governance versus autonomy:** the article rules out operating AI without any governance, but recommends recalibrating how governance signals its expectations. Cost: more design effort, and no longitudinal evidence yet on the effects of either design.
- **Sequencing of AI output and analyst view:** AI result first (efficient, but anchoring) versus analyst's provisional view first (recommended). Cost: one short, structured step per case, which the article describes as brief.
- **Escalation hierarchy versus lateral consultation:** the author recommends peer consultation so that disagreement starts a conversation rather than an approval chain. Cost: peers must be available, and the hierarchy's role in these cases is reduced.
- **Documentation format:** standard rule-based audit records (traceable, but they exclude expertise) versus a basis-and-confidence field (recognizes tacit expertise). Cost: the field is less standardized and harder to aggregate, a trade-off the article does not quantify.
- **Proportional oversight versus concordance audits:** risk-scaled review concentrates attention but can relax routine vigilance. The author recommends random audits of routine decisions. Cost: additional review load, which the article says must remain sustainable.

### Foundations it rests on
- [Architecture](/foundations/architecture.md) — the article treats governance structures as designed arrangements that "can be recalibrated," close to the definition's focus on design and evolution, though it never uses the term.
- [Business Architecture](/foundations/business-architecture.md) — its focus on roles, decision rights and processes falls within the organization dimension of business architecture, though the article does not use the term.
- [Enterprise Architecture](/foundations/enterprise-architecture.md) — the governance frameworks it analyses span whole organizations and sectors, but the article does not describe an enterprise-wide architecture.

## 🏛️ For Kenji
As an architect, you already design review steps, escalation paths and audit trails. This article shows that each of these is an organizational design decision with behavioral effects, not only a control. It applies directly when you design human-in-the-loop workflows, decision rights and audit records, including the order in which an AI result and the analyst's view are presented, and what a record is able to capture. In MBA terms, it sits in organizational behavior and governance design. The question it raises for you: after a human-in-the-loop control goes live, how would you measure whether reviewers still exercise independent judgment? *This is your interpretation, not the author's.*

## 📚 Reading plan
### From the article
- No links are cited in the article text.
### To go further
- Columbia Accident Investigation Board report (2003) — the primary source for the organizational-barriers finding the article quotes.
- Karl Weick and Kathleen Sutcliffe, *Managing the Unexpected* — a standard book on the principles of high-reliability organizations.
- Gary Klein, *Sources of Power* — the naturalistic decision-making research behind the article's recognition-based judgment argument.
- Charles Perrow, *Normal Accidents* — a counterpoint to HRO research on how tightly coupled complex systems produce accidents.
- Later parts of the "Paradoxo da IA" series — the article is labeled part 1; the later parts are not in this text.

## Concepts
- [Artificial intelligence](/concepts/ai-ml/artificial-intelligence.md)
- [Machine learning](/concepts/ai-ml/machine-learning.md)
- [Concept drift](/concepts/ai-ml/concept-drift.md)
- [Human-in-the-loop](/concepts/ai-ml/human-in-the-loop.md)
