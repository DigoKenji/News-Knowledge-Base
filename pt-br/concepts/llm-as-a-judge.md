---
type: Concept
title: LLM como juiz
description: Usar um modelo de linguagem forte, com uma rubrica, para pontuar ou comparar as saídas de outro modelo - um substituto escalável da avaliação humana que precisa ele próprio ser validado contra julgamentos humanos.
tags:
- ai-ml
- mlops
domain: ai-ml
subdomain: mlops
status: draft
generated:
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Zheng, L. et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. NeurIPS 2023, arXiv:2306.05685'
  resource: 'https://arxiv.org/abs/2306.05685'
  kind: paper
- title: 'Liu, Y. et al. (2023). G-Eval - NLG Evaluation using GPT-4 with Better Human Alignment. EMNLP 2023, arXiv:2303.16634'
  resource: 'https://arxiv.org/abs/2303.16634'
  kind: paper
- title: 'Gu, J. et al. (2024). A Survey on LLM-as-a-Judge. arXiv:2411.15594'
  resource: 'https://arxiv.org/abs/2411.15594'
  kind: paper
lang: pt-br
translation_of: /concepts/llm-as-a-judge.md
---

# LLM como juiz

**Definição.** LLM-as-a-judge é um método de avaliação em que um modelo recebe uma tarefa, uma ou mais respostas candidatas e critérios de correção, e devolve uma nota, um ranking ou uma preferência. É usado onde métricas de correspondência exata falham: respostas abertas, resumos, utilidade, tom.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `mlops`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

Três formatos: nota de resposta única (nota de 1 a 10 contra uma rubrica), comparação em pares (qual de A ou B é melhor) e correção guiada por referência (comparar com uma resposta-padrão). Zheng et al. viram que o GPT-4 como juiz concordava com especialistas humanos com a mesma frequência com que humanos concordavam entre si (mais de 80%) no MT-Bench. Vieses conhecidos: viés de posição (prefere a primeira ou a segunda resposta), viés de verbosidade (prefere respostas mais longas), autopreferência (prefere o próprio estilo) e capacidade limitada em matemática ou código que não consegue verificar. Mitigações: trocar as posições e tirar a média, usar rubricas com chain-of-thought (G-Eval), vários juízes e calibrar contra uma amostra rotulada por humanos.

## Por que importa e trade-offs

Torna a avaliação de aplicações de LLM barata e contínua, permitindo testes de regressão de prompts e modelos. Mas um juiz é um modelo com seus próprios erros; trate suas notas como uma medição que precisa de validação, não como verdade absoluta, e nunca deixe o mesmo modelo ser juiz e competidor sem checagem.

## Exemplo

Antes de mudar o prompt do resumidor, rode 50 artigos pelos prompts antigo e novo e pergunte a um modelo juiz qual resumo segue melhor a rubrica de deep dive, trocando as posições.

## Fontes

1. Zheng, L. et al. (2023). *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena*. NeurIPS 2023, arXiv:2306.05685. <https://arxiv.org/abs/2306.05685> — O termo, concordância com humanos e vieses de posição, verbosidade e autopromoção.
2. Liu, Y. et al. (2023). *G-Eval - NLG Evaluation using GPT-4 with Better Human Alignment*. EMNLP 2023, arXiv:2303.16634. <https://arxiv.org/abs/2303.16634> — Julgamento com rubrica mais chain-of-thought e sua correlação com humanos.
3. Gu, J. et al. (2024). *A Survey on LLM-as-a-Judge*. arXiv:2411.15594. <https://arxiv.org/abs/2411.15594> — Taxonomia de formatos, vieses e práticas de confiabilidade.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
