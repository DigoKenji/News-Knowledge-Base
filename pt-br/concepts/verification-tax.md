---
type: Concept
title: Imposto de verificação
description: A parcela crescente do tempo de engenharia gasta revisando, testando e corrigindo código gerado por IA, que pode anular a velocidade ganha ao gerá-lo - um termo recente e informal, não estabelecido.
tags:
- software-engineering
- ai-assisted-development
domain: software-engineering
subdomain: ai-assisted-development
status: draft
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
lang: pt-br
translation_of: /concepts/verification-tax.md
---

# Imposto de verificação

**Definição.** O imposto de verificação é o custo de checar um trabalho que você não escreveu. Com assistentes de código com IA, escrever código fica mais barato, mas cada linha ainda precisa ser entendida, revisada e testada por alguém responsável por ela. À medida que o volume gerado cresce, a revisão vira o gargalo e o ganho líquido de produtividade pode diminuir ou ficar negativo.

Domínio: [software-engineering](/pt-br/domains/software-engineering.md) · subdomínio `ai-assisted-development`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

Evidências do efeito por trás: o ensaio randomizado do METR em 2025 mostrou que desenvolvedores experientes de open source ficaram 19% mais lentos com ferramentas de IA nos próprios repositórios, embora acreditassem estar mais rápidos, com tempo indo para revisar e corrigir a saída. O relatório DORA 2024 do Google mostrou que maior adoção de IA se correlacionava com menor throughput e estabilidade de entrega, provavelmente por causa de mudanças maiores. A análise da GitClear sobre mudanças de código relatou mais churn e copiar-colar desde que os assistentes de IA se espalharam. Pesquisas (Stack Overflow 2025) mostram que muitos desenvolvedores desconfiam da precisão da saída da IA. Formas de reduzir o imposto: mudanças menores, testes e especificações escritos antes, checagens automáticas e revisão assistida por IA com humanos focando nas partes arriscadas.

## Por que importa e trade-offs

Explica por que ganhos de código com IA medidos em linhas ou tarefas iniciadas muitas vezes não aparecem nas métricas de entrega. As evidências são iniciais e mistas (outros estudos mostram ganhos em projetos novos ou tarefas de iniciantes), então trate como uma hipótese a medir no seu próprio time.

## Exemplo

Um agente produz um pull request de 1.200 linhas em dez minutos; revisá-lo direito leva três horas de um engenheiro sênior, mais do que levaria escrever uma mudança focada de 200 linhas.

## Notas de revisão para Kenji

"Imposto de verificação" não é um termo estabelecido; nenhuma norma ou fonte revisada por pares o define. As fontes sustentam o efeito por trás. Considere renomear para algo mais padrão (por exemplo "carga de revisão de código de IA") ou mantê-lo com esta ressalva.

## Fontes

1. Becker, J. et al. (2025). *Measuring the Impact of Early-2025 AI on Experienced Open-Source Developer Productivity*. METR, arXiv:2507.09089. <https://arxiv.org/abs/2507.09089> — Ensaio randomizado - desenvolvedores 19% mais lentos com IA em bases de código conhecidas.
2. DORA / Google Cloud (2024). *Accelerate State of DevOps Report 2024*. <https://dora.dev/research/2024/dora-report/> — Adoção de IA associada a menor throughput e estabilidade de entrega.
3. Stack Overflow (2025). *2025 Developer Survey - AI*. <https://survey.stackoverflow.co/2025/ai> — Confiança dos desenvolvedores na saída da IA e frustrações com ela.

## Relações
- (nenhuma ainda)

## Notas
- [O que desenvolvedores e QA precisam uns dos outros quando a IA escreve o código](/pt-br/notes/software-engineering/2026-11-21-what-developers-and-qa-need-from-each-other-when-ai-writes-t.md)
