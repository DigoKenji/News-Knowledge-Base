---
type: Concept
title: Viés de posição
description: Uma tendência sistemática de um modelo, especialmente quando atua como juiz ou ranqueador, de favorecer uma opção por causa de onde ela aparece (primeira, última ou numa certa posição) e não por causa do seu conteúdo.
tags:
- ai-ml
- mlops
domain: ai-ml
subdomain: mlops
status: draft
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Wang, P. et al. (2024). Large Language Models are not Fair Evaluators. ACL 2024, arXiv:2305.17926'
  resource: 'https://arxiv.org/abs/2305.17926'
  kind: paper
- title: 'Zheng, L. et al. (2023). Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena. NeurIPS 2023, arXiv:2306.05685'
  resource: 'https://arxiv.org/abs/2306.05685'
  kind: paper
- title: 'Shi, L. et al. (2024). Judging the Judges - A Systematic Study of Position Bias in LLM-as-a-Judge. arXiv:2406.07791'
  resource: 'https://arxiv.org/abs/2406.07791'
  kind: paper
lang: pt-br
translation_of: /concepts/position-bias.md
---

# Viés de posição

**Definição.** Há viés de posição quando trocar a ordem dos candidatos muda o veredito. Em comparações em pares de LLM-as-a-judge aparece como preferência pela resposta A ou B independentemente da qualidade; em múltipla escolha, como preferência por certas letras; em contextos longos, como uso melhor da informação do começo e do fim do que do meio.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `mlops`. Expandido em 2026-11-24 com fontes independentes; ainda um rascunho aguardando a certificação de Kenji.

## Como funciona

Mede-se rodando cada comparação duas vezes com as posições trocadas: a consistência é a parcela de pares com o mesmo veredito nos dois sentidos. Wang et al. mostraram que só trocar a ordem fez um modelo mais fraco "vencer" o ChatGPT na maior parte de um benchmark julgado pelo GPT-4. O viés é mais forte quando as duas respostas têm qualidade parecida. Mitigações: avaliar as duas ordens e só contar vitória se for consistente (senão é empate), embaralhar a ordem no conjunto de dados, pedir que o juiz raciocine antes de decidir e usar vários juízes.

## Por que importa e trade-offs

Viés de posição sem controle deixa avaliações e rankings automáticos errados de um jeito que parece preciso. Trocar as posições dobra o custo da avaliação, e a ordem é só um entre vários vieses de juiz (verbosidade, autopreferência).

## Exemplo

Pergunte a um juiz "o resumo A ou B é melhor?" e ele diz A; troque-os e ele ainda escolhe o primeiro. O veredito fala da posição, não dos resumos.

## Fontes

1. Wang, P. et al. (2024). *Large Language Models are not Fair Evaluators*. ACL 2024, arXiv:2305.17926. <https://arxiv.org/abs/2305.17926> — Trocar a ordem inverte vereditos; calibração balanceada de posição.
2. Zheng, L. et al. (2023). *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena*. NeurIPS 2023, arXiv:2306.05685. <https://arxiv.org/abs/2306.05685> — Viés de posição medido com taxas de consistência entre juízes.
3. Shi, L. et al. (2024). *Judging the Judges - A Systematic Study of Position Bias in LLM-as-a-Judge*. arXiv:2406.07791. <https://arxiv.org/abs/2406.07791> — Métricas sistemáticas (estabilidade de repetição, consistência de posição, justiça de preferência) em muitos juízes.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
