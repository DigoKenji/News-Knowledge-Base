---
type: Concept
title: Serverless (sem servidor)
description: Um modelo de nuvem em que os desenvolvedores implantam código ou usam serviços sem provisionar ou gerenciar servidores; o provedor os executa, escala (inclusive a zero) e cobra por requisição ou tempo de execução. Funções como serviço (FaaS) é a forma mais conhecida.
tags:
- cloud-infra
- core
domain: cloud-infra
subdomain: core
status: draft
generated:
  by: claude-opus (interactive, project thread; core vocabulary)
  at: '2026-11-24T22:30:00-03:00'
sources:
- title: Jonas, E. et al. (2019). Cloud Programming Simplified - A Berkeley View on Serverless Computing. arXiv 1902.03383
  resource: 'https://arxiv.org/abs/1902.03383'
  kind: paper
- title: CNCF Serverless Working Group (2018). CNCF Serverless Whitepaper v1.0
  resource: 'https://github.com/cncf/wg-serverless/blob/master/whitepapers/serverless-overview/README.md'
  kind: standard
- title: Amazon Web Services. What is AWS Lambda? (Lambda Developer Guide)
  resource: 'https://docs.aws.amazon.com/lambda/latest/dg/welcome.html'
  kind: docs
lang: pt-br
translation_of: /concepts/cloud-infra/serverless.md
---

# Serverless (sem servidor)

**Definição.** Jonas et al. (Berkeley) definem computação serverless por três traços: computação e armazenamento desacoplados, execução de código sem gerenciar alocação de recursos e pagamento proporcional aos recursos usados em vez dos alocados. O Serverless Whitepaper da CNCF distingue FaaS (funções acionadas por eventos, como AWS Lambda) de backend como serviço (bancos, filas e autenticação gerenciados).

Domínio: [cloud-infra](/pt-br/domains/cloud-infra.md) · subdomínio `core`. Entrada do vocabulário central (lista aprovada, 2026-11-24); rascunho aguardando a certificação de Kenji.

## Como se encaixa

Serverless é ideal para cargas com picos ou orientadas a eventos e para equipes pequenas: nenhum custo ocioso, nenhum patch. Os trade-offs são latência de partida a frio, limites de tempo de execução, testes locais mais difíceis e custo que pode superar servidores sempre ligados sob carga alta constante. As tarefas agendadas na nuvem deste projeto seguem a mesma ideia de "pagar por execução".

## Exemplo

Quando uma foto é enviada ao armazenamento de objetos, um evento aciona uma função que cria três miniaturas e termina após 400 ms. Sem envios durante a noite, nada roda e nada é cobrado; num dia movimentado, o provedor roda milhares de cópias em paralelo.

## Conceitos mais específicos

- [FinOps e custo de nuvem](/pt-br/concepts/cloud-infra/finops.md)

## Fontes

1. Jonas, E. et al. (2019). *Cloud Programming Simplified - A Berkeley View on Serverless Computing*. arXiv 1902.03383. <https://arxiv.org/abs/1902.03383> — Definição de serverless, seus traços e desafios em aberto.
2. CNCF Serverless Working Group (2018). *CNCF Serverless Whitepaper v1.0*. <https://github.com/cncf/wg-serverless/blob/master/whitepapers/serverless-overview/README.md> — FaaS versus BaaS e casos de uso, pela fundação cloud native.
3. Amazon Web Services. *What is AWS Lambda?* (Lambda Developer Guide). <https://docs.aws.amazon.com/lambda/latest/dg/welcome.html> — Comportamento concreto de FaaS - gatilhos de eventos, escala, cobrança.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
