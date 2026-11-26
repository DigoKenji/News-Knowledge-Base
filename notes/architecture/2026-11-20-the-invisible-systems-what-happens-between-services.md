---
type: Article Summary
title: 'The Invisible Systems: What Happens Between Services'
description: Service calls hide real costs, so healthy services can still produce failing requests.
resource: https://blog.bytebytego.com/p/the-invisible-systems-what-happens
tags:
- architecture
- software-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://blog.bytebytego.com/p/the-invisible-systems-what-happens
  title: 'The Invisible Systems: What Happens Between Services'
  author: ByteByteGo
status: stable
source_url: https://blog.bytebytego.com/p/the-invisible-systems-what-happens
source: ByteByteGo
source_type: blog
published: '2026-11-19'
domain: architecture
subdomain: software-architecture
concepts: []
selected_by: discovery
content: open
complete: 'no'
notion_url: https://app.notion.com/p/41ebaf74172d8192bd94d043a800edf1
---

## 🎯 Core thesis
Every arrow between two services carries hidden work, resources and assumptions, so a request can fail even when both services are running correctly.

## 🧭 Why the author wrote it
Architecture diagrams show services as boxes and calls as arrows, and attention goes to the boxes (code, databases, CPU, deployments). The author argues the arrows deserve the same scrutiny, because that is where failures that look mysterious tend to come from.

## The Nature of a Service-to-Service Call
The available text of the article stops at this heading. The paywall begins immediately after it, so the section body is not available to expand here.

What the preview does state about this section is the framing that runs through the whole piece: a call between services is not free. It consumes work, resources and assumptions that a box-and-arrow diagram does not show.

> 🔎 Added context: this note covers only the free preview. The sections listed below are announced in the preview but their content is not included, so they are not expanded.

### Topics announced but not available in the preview
- Why moving data is costly
- Why healthy services can have problems
- How downstream calls spend time
- How retries can cause issues
- How slow dependencies impact requests
- How DNS changes impact requests

## 🔑 Key takeaways
- A request can fail while both services are running correctly, so a green health check on each service does not prove the call between them works.
- The author names three example failure modes: a service waiting for a connection, a call sent to an outdated address, and a request that arrives after the caller has already given up.
- Each arrow carries work, resources and assumptions that a standard architecture diagram leaves out.
- The article's full argument (costs of data movement, retries, slow dependencies, DNS changes) is behind the paywall and is not covered by this note.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Technology (network connections, addresses and DNS are implied); the preview does not cover Business, Data or Application domains |
| Scope | Solution: a single call between two services. Scope is stated only in the preview, so this is a light classification. |
| Stakeholders and concerns | Not stated in the preview. Implied concern: reliability of calls between services. |
| Frameworks and notations | None named |

### Decisions and trade-offs
The article describes no explicit decision in the available text.

### Foundations it rests on
No foundation entries match yet.

## 🏛️ For Kenji
This is your interpretation, not the author's. For an architect, the useful point is that the boundary between services is where reliability problems often sit, so reliability reviews should cover calls and their timing, not only each service in isolation. Question it raises: which of your current service-to-service calls have timeouts, retry rules and address resolution that someone has actually reviewed?

## 📚 Reading plan
### From the article
- [The Invisible Systems: What Happens Between Services](https://blog.bytebytego.com/p/the-invisible-systems-what-happens) — the full article is behind the paywall; read it in full for the sections listed above.
### To go further
- Retry storms (ontology concept `retry-storm`) — relevant to the article's planned section on how retries cause problems.
