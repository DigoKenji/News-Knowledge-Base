---
type: Concept
title: SLO error budget
description: An SLO is a target for a service's reliability measured by an indicator (for example 99.9% of requests succeed over 30 days); the error budget is the failure that target still allows (0.1%), which teams spend on releases and risk and protect when it runs low.
tags:
- cloud-infra
domain: cloud-infra
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 3 Embracing Risk. O'Reilly / Google
  resource: 'https://sre.google/sre-book/embracing-risk/'
  kind: book
- title: Beyer, B. et al. (eds.) (2016). Site Reliability Engineering, ch. 4 Service Level Objectives. O'Reilly / Google
  resource: 'https://sre.google/sre-book/service-level-objectives/'
  kind: book
- title: Beyer, B. et al. (eds.) (2018). The Site Reliability Workbook, ch. 5 Alerting on SLOs. O'Reilly / Google
  resource: 'https://sre.google/workbook/alerting-on-slos/'
  kind: book
- title: OpenSLO. OpenSLO specification
  resource: 'https://github.com/OpenSLO/OpenSLO'
  kind: standard
---

# SLO error budget

**Definition.** A service level indicator (SLI) measures something users care about, such as the share of requests served successfully in under 300 ms. A service level objective (SLO) sets a target for it over a window. The error budget is 1 minus the SLO: the amount of unreliability allowed. Burn rate is how fast that budget is being used compared with the rate that would use it exactly by the end of the window.

Domain: [cloud-infra](/domains/cloud-infra.md). Expanded on 2026-11-24 with independent sources; still a draft waiting for Kenji to certify.

## How it works

Google SRE introduced the idea that 100% reliability is the wrong target: users cannot tell 99.99% from 100%, and chasing it stops all change. The budget turns reliability into a shared, measurable resource between product and SRE teams. An error budget policy says what happens when it is exhausted, for example a feature freeze until reliability recovers. Alerting uses burn rate over multiple windows: a burn rate of 14.4 over 1 hour means 2% of a 30-day budget gone in an hour, worth paging; a slow burn over days becomes a ticket. A 99.9% monthly SLO allows about 43 minutes of full downtime.

## Why it matters and trade-offs

It replaces arguments about "is it reliable enough?" with data, and gives a rule for balancing speed and stability. It depends on choosing SLIs that reflect real user experience; bad SLIs give false comfort, and the policy only works if leadership actually enforces it.

## Example

With a 99.9% SLO and 10 million requests a month, the budget is 10,000 failed requests. A bad deploy that fails 6,000 in a day burns 60% of it, so the team pauses risky launches until the window recovers.

## Review notes for Kenji

The draft covered only the budget and burn rate; the entry now defines SLI and SLO too. Three of four sources are Google's SRE books; OpenSLO is the independent one.

## Sources

1. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 3 Embracing Risk. O'Reilly / Google. <https://sre.google/sre-book/embracing-risk/> — Error budgets and why 100% is the wrong target.
2. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 4 Service Level Objectives. O'Reilly / Google. <https://sre.google/sre-book/service-level-objectives/> — SLI, SLO and SLA definitions.
3. Beyer, B. et al. (eds.) (2018). *The Site Reliability Workbook*, ch. 5 Alerting on SLOs. O'Reilly / Google. <https://sre.google/workbook/alerting-on-slos/> — Burn rate and multi-window alerting.
4. OpenSLO. *OpenSLO specification*. <https://github.com/OpenSLO/OpenSLO> — Vendor-neutral specification of SLOs, SLIs and error budgets, independent of Google.

## Relations
- (none yet)

## Notes
- [Probing DNS at Cloudflare](/notes/cloud-infra/2026-11-24-probing-dns-at-cloudflare.md)
