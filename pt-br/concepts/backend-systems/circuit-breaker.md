---
type: Concept
title: Circuit breaker
description: Padrão de resiliência que interrompe chamadas a uma operação com probabilidade de falha, em vez de repeti-las, para que a dependência possa se recuperar e o chamador fique protegido.
tags:
- backend-systems
- reliability
domain: backend-systems
subdomain: reliability
status: draft
relations:
- type: mitigates
  target: /concepts/backend-systems/cascading-failure.md
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- title: 'EP234: Docker vs Kubernetes'
  resource: https://blog.bytebytego.com/p/ep234-docker-vs-kubernetes
  kind: practitioner
lang: pt-br
translation_of: /concepts/backend-systems/circuit-breaker.md
---

# Circuit breaker

**Definição.** Padrão de resiliência que interrompe chamadas a uma operação com probabilidade de falha, em vez de repeti-las, para que a dependência possa se recuperar e o chamador fique protegido.

Domínio: [backend-systems](/pt-br/domains/backend-systems.md). Do ciclo diário de 2026-11-27; rascunho, aguardando o Kenji.

## Relações
- **mitiga** [cascading-failure](/pt-br/concepts/backend-systems/cascading-failure.md)

## Notas
- [EP234: Docker vs Kubernetes](/pt-br/notes/cloud-infra/2026-11-27-ep234-docker-vs-kubernetes.md)
