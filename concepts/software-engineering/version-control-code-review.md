---
type: Concept
title: Version control and code review
description: Version control records every change to code (who, what, when, why) so teams can work in parallel and roll back; code review has another engineer examine each change before it is merged, to catch defects and share knowledge.
tags:
- software-engineering
- delivery
- core
domain: software-engineering
subdomain: delivery
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Chacon, S. and Straub, B. (2014). Pro Git, 2nd ed. Apress
  resource: 'https://git-scm.com/book/en/v2'
  kind: book
- title: Bacchelli, A. and Bird, C. (2013). Expectations, Outcomes, and Challenges of Modern Code Review. ICSE 2013
  resource: 'https://doi.org/10.1109/ICSE.2013.6606617'
  kind: paper
- title: Google. Google Engineering Practices - Code Review
  resource: 'https://google.github.io/eng-practices/review/'
  kind: practitioner
- title: 'Hohpe, G. (2020). The Software Architect Elevator: Redefining the Architect''s Role in the Digital Enterprise. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781492077541'
  kind: book
  locator: 'ch. 25 Software Is Collaboration'
updated:
- by: claude-opus (interactive, project thread; MBA library digest, Architect Elevator)
  at: '2026-11-26T20:30:00-03:00'
  change: 'added MBA library citations; definition unchanged'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Version control and code review

**Definition.** A version control system (Git is the de facto standard) stores the history of a codebase as commits, supports branches for parallel work and merges them back. Modern code review happens on pull requests: a reviewer reads the diff, comments, and approves before merge. Bacchelli and Bird found at Microsoft that finding defects is the stated goal, but knowledge transfer and team awareness are major real outcomes.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `delivery`. Core vocabulary entry (approved seed list, 2026-11-24); draft waiting for Kenji to certify.

## How it fits

Version control is the foundation for CI/CD, auditability and collaboration. Code review is a quality gate, and the bottleneck when AI generates code faster than humans can read it (verification tax). Small pull requests get better reviews.

## Example

Every change in this project goes through a branch and a pull request on GitHub that Kenji reviews before merge.

## Narrower concepts

- [Verification tax](/concepts/software-engineering/verification-tax.md)
- [Continuous integration and continuous delivery (CI/CD)](/concepts/software-engineering/ci-cd.md)

## From the MBA library

Books added to FIAP-MBA/Materiais/Referencias (Architect Elevator added 2026-11-26). Summaries with locators; nothing copied beyond short phrases.

- **Hohpe, G. (2020)**, *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*, ch. 25 Software Is Collaboration: Version control and continuous integration as collaboration tools beyond code, for documents and plans as well.

## Sources

1. Chacon, S. and Straub, B. (2014). *Pro Git*, 2nd ed. Apress. <https://git-scm.com/book/en/v2> — Version control concepts, branching and merging.
2. Bacchelli, A. and Bird, C. (2013). *Expectations, Outcomes, and Challenges of Modern Code Review*. ICSE 2013. <https://doi.org/10.1109/ICSE.2013.6606617> — Empirical study of why teams review code and what it achieves.
3. Google. *Google Engineering Practices - Code Review*. <https://google.github.io/eng-practices/review/> — Code review standards used at scale.
4. Hohpe, G. (2020). *The Software Architect Elevator: Redefining the Architect's Role in the Digital Enterprise*. O'Reilly. <https://books.google.com/books?vid=ISBN9781492077541> — ch. 25 Software Is Collaboration. Version control and continuous integration as collaboration tools beyond code, for documents and plans as well.

## Relations
- (none yet)

## Notes
Notes tagged with this concept are listed in the concept index after each run.
