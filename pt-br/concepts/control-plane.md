---
type: Concept
title: Plano de controle
description: A parte de um sistema que decide e configura o que deve acontecer - alocação, regras de roteamento, estado desejado, escalonamento - em oposição ao plano de dados, que faz o trabalho de fato em cada requisição ou pacote.
tags:
- backend-systems
- system-design
domain: backend-systems
subdomain: system-design
status: stable
verified:
- by: Kenji
  at: '2026-11-24'
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: Yang, L. et al. (2004). RFC 3746 - Forwarding and Control Element Separation (ForCES) Framework. IETF
  resource: 'https://www.rfc-editor.org/rfc/rfc3746'
  kind: standard
- title: Kubernetes. Kubernetes Components
  resource: 'https://kubernetes.io/docs/concepts/overview/components/'
  kind: docs
- title: AWS (2022). AWS Fault Isolation Boundaries - Control planes and data planes. Whitepaper
  resource: 'https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/control-planes-and-data-planes.html'
  kind: practitioner
lang: pt-br
translation_of: /concepts/control-plane.md
---

# Plano de controle

**Definição.** A divisão plano de controle / plano de dados vem de redes: o plano de controle calcula rotas, o plano de dados encaminha pacotes. Em sistemas de nuvem e distribuídos, o plano de controle é a camada de gestão (APIs para criar, alterar e excluir recursos, escalonadores, controladores que reconciliam estado desejado e real), e o plano de dados é o que atende o tráfego (VMs, contêineres, balanceadores, leituras e escritas em armazenamento).

Domínio: [backend-systems](/pt-br/domains/backend-systems.md) · subdomínio `system-design`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O Kubernetes é o exemplo clássico: API server, etcd, scheduler e controller manager formam o plano de controle; kubelets e pods nos nós de trabalho são o plano de dados. Numa service mesh, o istiod do Istio é o plano de controle e os sidecars Envoy o plano de dados. A AWS projeta para estabilidade estática: o plano de dados continua rodando com a última configuração conhecida se o plano de controle falhar, porque planos de controle são mais complexos, mudam com mais frequência e falham mais. Planos de controle normalmente lidam com muito menos tráfego mas precisam de consistência forte; planos de dados precisam de alto throughput e disponibilidade.

## Por que importa e trade-offs

Separá-los permite que cada um escale e falhe de forma independente, e evita que uma queda da gestão vire uma queda do atendimento. O risco é o acoplamento voltar escondido: um plano de dados que chama o plano de controle a cada requisição, ou passos de recuperação que dependem do plano de controle justamente quando ele está fora.

## Exemplo

Durante um incidente no plano de controle do EC2 da AWS você pode não conseguir criar novas instâncias, enquanto as instâncias já em execução continuam atendendo o tráfego normalmente.

## Notas de revisão para Kenji

O rascunho restringia isto a orquestração de jobs ("decide qual trabalho roda e quando"); agora é a divisão geral plano de controle/plano de dados, da qual o escalonamento de jobs é um caso.

## Fontes

1. Yang, L. et al. (2004). *RFC 3746 - Forwarding and Control Element Separation (ForCES) Framework*. IETF. <https://www.rfc-editor.org/rfc/rfc3746> — A origem em redes da separação entre controle e encaminhamento.
2. Kubernetes. *Kubernetes Components*. <https://kubernetes.io/docs/concepts/overview/components/> — Componentes do plano de controle vs componentes dos nós.
3. AWS (2022). *AWS Fault Isolation Boundaries - Control planes and data planes*. Whitepaper. <https://docs.aws.amazon.com/whitepapers/latest/aws-fault-isolation-boundaries/control-planes-and-data-planes.html> — Definição em nuvem, estabilidade estática, por que planos de controle falham mais.

## Relações
- (nenhuma ainda)

## Notas
- [A OpenAI acaba de abrir o código do Symphony. Toda startup de IA deveria prestar atenção](/pt-br/notes/ai-ml/2026-11-20-openai-just-open-sourced-symphony-every-ai-startup-should-pa.md)
