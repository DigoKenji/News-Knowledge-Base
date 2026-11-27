---
type: Concept
title: Visibility timeout
description: How long a received queue message stays hidden from other consumers, so it can be processed once or redelivered
  if it is not acknowledged in time.
tags:
- backend-systems
- messaging-and-workflows
domain: backend-systems
subdomain: messaging-and-workflows
status: draft
relations:
- type: part-of
  target: /concepts/backend-systems/message-queue.md
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- title: AWS Lambda Doubles Its Timeout to 30 Minutes after Eight Years at 15
  resource: https://www.infoq.com/news/2026/11/aws-lambda-30-minute-timeout/
  kind: practitioner
---

# Visibility timeout

**Definition.** How long a received queue message stays hidden from other consumers, so it can be processed once or redelivered if it is not acknowledged in time.

Domain: [backend-systems](/domains/backend-systems.md). From the daily run on 2026-11-27; draft, waiting for Kenji.

## Relations
- **part-of** [message-queue](/concepts/backend-systems/message-queue.md)

## Notes
- [AWS Lambda Doubles Its Timeout to 30 Minutes after Eight Years at 15](/notes/cloud-infra/2026-11-27-aws-lambda-doubles-its-timeout-to-30-minutes-after-eight-yea.md)
