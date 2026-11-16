# News-Knowledge-Base (OKF)

Kenji's study knowledge base and the source of truth for Project_News. The agents in [`News-Automation`](https://github.com/DigoKenji/News-Automation) fill it; Kenji reviews it. It holds no code.

| Path | What |
|---|---|
| `notes/<domain>/` | One summary per article or paper (`article-summarizer` format + frontmatter) |
| `ontology/` | Domains, concepts and relations. New concepts arrive as `status: proposed`; Kenji certifies them. |
| `digests/{daily,weekly,monthly}/` | Every digest sent to Telegram |
| `inbox/` | Full-text handoff from Hermes (Medium saves, requested Medium articles, LinkedIn posts), waiting for the next run |
| `requests/` | Medium articles the daily run selected, for Hermes to hand over |
| `.state/` | Run state: last run, source health |

Notion gets a reading copy of every note, organized by source type (Medium, arXiv, Blog, LinkedIn). Sync is one-way.
