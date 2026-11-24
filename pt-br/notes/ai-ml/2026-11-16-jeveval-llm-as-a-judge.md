---
type: Post Summary
title: O LLM-as-a-Judge tem um problema de pontuação (JevEval)
description: O JevEval troca as notas únicas de juízes LLM por perguntas explícitas respondidas como probabilidades calibradas, com código fixo calculando uma pontuação inspecionável.
resource: https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU
tags:
- ai-ml
- mlops
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU
  title: LLM-as-a-Judge has a scoring problem (JevEval)
  author: Sarthak Rastogi
status: stable
source_url: https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU
source: LinkedIn (forwarded)
source_type: linkedin
published: '2026-11-09'
domain: ai-ml
subdomain: mlops
concepts:
- llm-as-a-judge
selected_by: forwarded
content: handoff
notion_url: https://app.notion.com/p/41abaf74172d8100b77ec67af665737b
lang: pt-br
translation_of: /notes/ai-ml/2026-11-16-jeveval-llm-as-a-judge.md
---

# O LLM-as-a-Judge tem um problema de pontuação (JevEval)

**Fonte:** https://www.linkedin.com/posts/sarthakrastogi_ai-llms-aiagents-activity-7525705034559451137-njbU (post no LinkedIn)
**Autor:** Sarthak Rastogi
**Data:** 2026-11-09
**Tags:** #llm-evaluation #agents #deepeval #format-linkedin-post

---

## Sobre o que trata
O post argumenta que a maioria das métricas personalizadas de [LLM-as-a-judge](/pt-br/concepts/ai-ml/llm-as-a-judge.md) pede a uma única chamada de modelo que faça tudo de uma vez: interpretar critérios vagos, emitir um julgamento, gerar uma nota e explicar o raciocínio. É por isso que a mesma resposta pode receber 0,71 em uma execução e 0,66 em outra, sem um motivo claro. O JevEval (o post chama a abordagem de "Jev-as-a-Judge") é a correção proposta. Você escreve a avaliação como perguntas explícitas, o modelo juiz responde a elas com probabilidades calibradas, e código fixo converte essas probabilidades em uma nota.

## Como funciona
O post descreve três tipos de pergunta:

- **Noul:** uma proposição é verdadeira? (escrito "Noul" no post)
- **Score:** onde a resposta se situa em uma escala ordenada?
- **Choice:** qual comportamento ocorreu?

As perguntas são combinadas em métricas personalizadas, e cada pergunta tem o seu próprio peso. O exemplo é uma métrica de fidelidade a ferramentas, que verifica se as afirmações de um agente vieram das saídas das ferramentas, se os valores retornados foram relatados com precisão e se informações sem suporte foram apresentadas como fato. Você pode inspecionar as distribuições de probabilidade e o detalhamento da nota por trás de cada resultado. O modo estrito transforma os critérios em testes de aceitação rígidos. Avaliações conversacionais de múltiplos turnos também são suportadas, para verificar se os agentes mantêm restrições, verificam identidade, seguem fluxos de trabalho e permanecem nos seus papéis designados.

> A ideia central: as avaliações se tornam especificações explícitas, em vez de prompts que pedem a outro LLM uma opinião. O juiz toma as decisões, o código determinístico calcula a nota, e não há veredictos ou explicações gerados que possam se afastar do resultado.

## Padrões e técnicas identificados
- **Rubrica decomposta:** um critério vago é dividido em perguntas pequenas e respondíveis separadamente, cada uma com seu próprio peso.
- **Probabilidades na entrada, matemática fixa na saída:** o modelo fornece probabilidades calibradas e a pontuação é uma aritmética inspecionável, então julgamento e cálculo ficam separados.
- **Portões rígidos:** o modo estrito trata um critério como um teste de aceitação com aprovação/reprovação, e não como uma contribuição flexível para uma nota.

## Principais conclusões
- Uma nota vinda de uma única chamada de juiz em texto livre é difícil de depurar. Quebrar os critérios em perguntas explícitas torna a falha visível.
- Manter o julgamento (o modelo) separado do cálculo da nota (código) significa que a explicação não pode se afastar do número.
- Trate as avaliações como especificações. Uma métrica que você consegue ler pergunta por pergunta é mais fácil de confiar e de alterar do que um prompt de julgamento.

## Leitura de acompanhamento
- 🔗 Sarthak Rastogi, newsletter *AI Agent Engineering* — https://lnkd.in/ehxEYRJY → Os tutoriais do autor sobre como construir e melhorar apps e agentes de IA, linkados a partir do post.
- 📚 Zheng et al., 2023, *Judging LLM-as-a-Judge with MT-Bench and Chatbot Arena* (NeurIPS 2023) → O estudo de referência sobre juízes LLM e seus vieses, útil como base para o problema de confiabilidade que este post aborda.

---
*Resumo gerado: 2026-11-16*
