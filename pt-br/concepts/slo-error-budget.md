---
type: Concept
title: Orçamento de erros de SLO
description: Um SLO é uma meta de confiabilidade de um serviço medida por um indicador (por exemplo 99,9% das requisições com sucesso em 30 dias); o orçamento de erros é a falha que essa meta ainda permite (0,1%), que os times gastam com releases e riscos e protegem quando está acabando.
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
lang: pt-br
translation_of: /concepts/slo-error-budget.md
---

# Orçamento de erros de SLO

**Definição.** Um indicador de nível de serviço (SLI) mede algo que importa aos usuários, como a parcela de requisições atendidas com sucesso em menos de 300 ms. Um objetivo de nível de serviço (SLO) define uma meta para ele numa janela de tempo. O orçamento de erros é 1 menos o SLO: a quantidade de falta de confiabilidade permitida. Burn rate é a velocidade com que o orçamento está sendo gasto, comparada com a velocidade que o gastaria exatamente até o fim da janela.

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md). Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

O SRE do Google introduziu a ideia de que 100% de confiabilidade é a meta errada: usuários não distinguem 99,99% de 100%, e persegui-la para toda mudança. O orçamento transforma confiabilidade num recurso compartilhado e mensurável entre os times de produto e de SRE. Uma política de orçamento de erros diz o que acontece quando ele acaba, por exemplo congelar funcionalidades até a confiabilidade se recuperar. Os alertas usam burn rate em múltiplas janelas: um burn rate de 14,4 em 1 hora significa 2% do orçamento de 30 dias gasto em uma hora, motivo para acionar alguém; um consumo lento ao longo de dias vira um ticket. Um SLO mensal de 99,9% permite cerca de 43 minutos de indisponibilidade total.

## Por que importa e trade-offs

Substitui discussões do tipo "está confiável o bastante?" por dados, e dá uma regra para equilibrar velocidade e estabilidade. Depende de escolher SLIs que reflitam a experiência real do usuário; SLIs ruins dão falso conforto, e a política só funciona se a liderança de fato a aplicar.

## Exemplo

Com um SLO de 99,9% e 10 milhões de requisições por mês, o orçamento é de 10.000 requisições com falha. Um deploy ruim que falha 6.000 num dia queima 60% dele, então o time pausa lançamentos arriscados até a janela se recuperar.

## Notas de revisão para Kenji

O rascunho cobria só o orçamento e o burn rate; a entrada agora define SLI e SLO também. Três das quatro fontes são livros de SRE do Google; o OpenSLO é a independente.

## Fontes

1. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 3 Embracing Risk. O'Reilly / Google. <https://sre.google/sre-book/embracing-risk/> — Orçamentos de erros e por que 100% é a meta errada.
2. Beyer, B. et al. (eds.) (2016). *Site Reliability Engineering*, ch. 4 Service Level Objectives. O'Reilly / Google. <https://sre.google/sre-book/service-level-objectives/> — Definições de SLI, SLO e SLA.
3. Beyer, B. et al. (eds.) (2018). *The Site Reliability Workbook*, ch. 5 Alerting on SLOs. O'Reilly / Google. <https://sre.google/workbook/alerting-on-slos/> — Burn rate e alertas em múltiplas janelas.
4. OpenSLO. *OpenSLO specification*. <https://github.com/OpenSLO/OpenSLO> — Especificação neutra de fornecedor para SLOs, SLIs e orçamentos de erros, independente do Google.

## Relações
- (nenhuma ainda)

## Notas
- [Sondando DNS na Cloudflare](/pt-br/notes/cloud-infra/2026-11-24-probing-dns-at-cloudflare.md)
