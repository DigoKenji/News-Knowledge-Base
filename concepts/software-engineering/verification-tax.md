---
type: Concept
title: Verification tax
description: The growing share of engineering time spent reviewing, testing and correcting AI-generated code, which can cancel out the speed gained by generating it - a recent, informal term rather than an established one.
tags:
- software-engineering
- devex
domain: software-engineering
subdomain: devex
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Becker, J. et al. (2025). Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity. METR, arXiv:2507.09089'
  resource: 'https://arxiv.org/abs/2507.09089'
  kind: paper
- title: DORA / Google Cloud (2024). Accelerate State of DevOps Report 2024
  resource: 'https://dora.dev/research/2024/dora-report/'
  kind: practitioner
- title: Stack Overflow (2025). 2025 Developer Survey - AI
  resource: 'https://survey.stackoverflow.co/2025/ai'
  kind: practitioner
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomy: subdomain set from domain taxonomy'
---

# Verification tax

**Definition.** The verification tax is the cost of checking work you did not write yourself. With AI coding assistants, writing code gets cheaper but every line still has to be understood, reviewed and tested by someone accountable for it. As generated volume rises, review becomes the bottleneck and the net productivity gain can shrink or turn negative.

Domain: [software-engineering](/domains/software-engineering.md) · subdomain `devex`. Expanded on 2026-11-24 with independent sources; certified by Kenji on 2026-11-24.

## How it works

Evidence for the underlying effect: METR's 2025 randomised trial found experienced open-source developers were 19% slower with AI tools on their own repositories, although they believed they were faster, with time going to reviewing and fixing output. Google's DORA 2024 report found that higher AI adoption correlated with lower delivery throughput and stability, likely because of larger change sets. GitClear's analysis of code changes reported more churn and copy-paste since AI assistants spread. Surveys (Stack Overflow 2025) show many developers distrust the accuracy of AI output. Ways to lower the tax: smaller changes, tests and specs written first, automated checks, and AI-assisted review with humans focusing on risky parts.

## Why it matters and trade-offs

It explains why AI coding gains measured in lines or tasks started often do not show up in delivery metrics. The evidence is early and mixed (other studies show gains on greenfield or junior tasks), so treat it as a hypothesis to measure in your own team.

## Example

An agent produces a 1,200-line pull request in ten minutes; reviewing it properly takes a senior engineer three hours, longer than writing a focused 200-line change would have.

## Review notes for Kenji

"Verification tax" is not an established term; no standard or peer-reviewed source defines it. The sources support the underlying effect. Consider renaming to something more standard (for example "AI code review burden") or keeping it with this caveat.

## Sources

1. Becker, J. et al. (2025). *Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity*. METR, arXiv:2507.09089. <https://arxiv.org/abs/2507.09089> — Randomised trial - developers 19% slower with AI on familiar codebases.
2. DORA / Google Cloud (2024). *Accelerate State of DevOps Report 2024*. <https://dora.dev/research/2024/dora-report/> — AI adoption associated with lower delivery throughput and stability.
3. Stack Overflow (2025). *2025 Developer Survey - AI*. <https://survey.stackoverflow.co/2025/ai> — Developer trust in and frustrations with AI output.

## Relations
- (none yet)

## Notes
- [What developers and QA need from each other when AI writes the code](/notes/software-engineering/2026-11-21-what-developers-and-qa-need-from-each-other-when-ai-writes-t.md)
