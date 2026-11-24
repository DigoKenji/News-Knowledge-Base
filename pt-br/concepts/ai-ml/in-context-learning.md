---
type: Concept
title: Aprendizado em contexto
description: A capacidade de um modelo de linguagem de aprender uma tarefa a partir de instruções ou exemplos colocados no prompt e executá-la em novas entradas, sem nenhuma mudança nos pesos.
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
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
- title: 'Brown, T. et al. (2020). Language Models are Few-Shot Learners. NeurIPS 2020, arXiv:2005.14165'
  resource: 'https://arxiv.org/abs/2005.14165'
  kind: paper
- title: 'Dong, Q. et al. (2024). A Survey on In-context Learning. EMNLP 2024, arXiv:2301.00234'
  resource: 'https://arxiv.org/abs/2301.00234'
  kind: paper
- title: 'Min, S. et al. (2022). Rethinking the Role of Demonstrations - What Makes In-Context Learning Work? EMNLP 2022, arXiv:2202.12837'
  resource: 'https://arxiv.org/abs/2202.12837'
  kind: paper
- title: 'Olsson, C. et al. (2022). In-context Learning and Induction Heads. Transformer Circuits Thread, arXiv:2209.11895'
  resource: 'https://arxiv.org/abs/2209.11895'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/in-context-learning.md
---

# Aprendizado em contexto

**Definição.** Aprendizado em contexto (ICL) é aprender no momento da inferência apenas a partir do contexto. Com alguns exemplos de entrada e saída (few-shot) ou só uma descrição (zero-shot), o modelo infere o padrão e o aplica à próxima entrada. Nada é treinado; tire os exemplos e a habilidade desaparece.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O GPT-3 (Brown et al., 2020) popularizou o ICL ao mostrar que o desempenho few-shot cresce com a escala do modelo. Por que funciona ainda é estudado: Olsson et al. ligaram o fenômeno a "induction heads", circuitos de atenção que copiam padrões vistos antes no contexto; outros trabalhos o descrevem como inferência bayesiana implícita ou como o modelo executando um algoritmo de aprendizado na própria passagem direta. Min et al. encontraram um resultado surpreendente: os rótulos nos exemplos importam menos que o esperado, enquanto o formato, a distribuição das entradas e o espaço de rótulos importam muito.

## Por que importa e trade-offs

ICL permite adaptar um modelo em minutos sem pipeline de treino, por isso dar exemplos no prompt é a primeira ferramenta a tentar. Ele é sensível à escolha e à ordem dos exemplos, consome tokens de contexto em toda chamada (custo e latência) e, para tarefas grandes e estáveis, fine-tuning ou LoRA geralmente o superam.

## Exemplo

Prompt "ótimo filme → positivo; enredo chato → negativo; amei o elenco →" e o modelo responde "positivo".

## Fontes

1. Brown, T. et al. (2020). *Language Models are Few-Shot Learners*. NeurIPS 2020, arXiv:2005.14165. <https://arxiv.org/abs/2005.14165> — Aprendizado em contexto zero, one e few-shot e seu crescimento com a escala.
2. Dong, Q. et al. (2024). *A Survey on In-context Learning*. EMNLP 2024, arXiv:2301.00234. <https://arxiv.org/abs/2301.00234> — Definição, desenho de exemplos e explicações do ICL.
3. Min, S. et al. (2022). *Rethinking the Role of Demonstrations - What Makes In-Context Learning Work?* EMNLP 2022, arXiv:2202.12837. <https://arxiv.org/abs/2202.12837> — Formato e distribuição das entradas importam mais que rótulos corretos.
4. Olsson, C. et al. (2022). *In-context Learning and Induction Heads*. Transformer Circuits Thread, arXiv:2209.11895. <https://arxiv.org/abs/2209.11895> — Uma explicação mecanicista do ICL.

## Relações
- (nenhuma ainda)

## Notas
- [IA em planilhas do Excel: o Kumo, da NVIDIA, roda 23 vezes mais rápido que o TabFM, do Google](/pt-br/notes/data/2026-11-20-ai-on-excel-sheet-nvidias-kumo-runs-23x-faster-than-googles.md)
