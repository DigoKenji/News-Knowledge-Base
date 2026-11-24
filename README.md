---
type: Readme
title: News-Knowledge-Base
description: How this OKF bundle is organized.
---

# News-Knowledge-Base (OKF)

This repo is an **[Open Knowledge Format](https://github.com/GoogleCloudPlatform/knowledge-catalog/tree/main/okf) v0.2 bundle**. Start at [`index.md`](index.md); changes are in [`log.md`](log.md).

Kenji's study knowledge base and the source of truth for Project_News. The agents in [`News-Automation`](https://github.com/DigoKenji/News-Automation) fill it; Kenji reviews it. It holds no code.

| Path | What |
|---|---|
| `notes/<domain>/` | One summary per article or paper (`article-summarizer` format + frontmatter) |
| `domains/`, `concepts/<domain>/` | The ontology as OKF concept files, one folder per theme (same domains as `notes/`), core vocabulary first in each theme index. Relations are typed links. Agent-proposed concepts are `status: draft`; Kenji certifies by adding a `verified` entry. |
| `digests/{daily,weekly,monthly}/` | Every digest sent to Telegram |
| `reviews/` | Certification checklists: every draft in one list for Kenji to approve or fix, kept as the record afterwards |
| `inbox/` | Full-text handoff from Hermes (Medium saves, requested Medium articles, LinkedIn posts), waiting for the next run |
| `requests/` | Medium articles the daily run selected, for Hermes to hand over |
| `.state/` | Run state: last run, source health |

Notion gets a reading copy of every note, organized by source type (Medium, arXiv, Blog, LinkedIn). Sync is one-way.
