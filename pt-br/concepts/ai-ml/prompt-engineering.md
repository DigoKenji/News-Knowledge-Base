---
type: Concept
title: 'Engenharia de prompt'
description: 'Projetar as instruções, exemplos e contexto dados a um modelo para que ele produza a saída desejada sem alterar seus pesos - incluindo versionar e avaliar prompts, e defendê-los contra injeção e extração.'
tags:
- ai-ml
- llm-engineering
domain: ai-ml
subdomain: llm-engineering
status: draft
generated:
  by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
sources:
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 5 Prompt Engineering, pp. 211-251 (best practices p. 220; organize and version prompts p. 233; defensive prompt engineering p. 235)'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 9 In-Context and Few-Shot Learning with Prompts, p. 288'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 2 Opik: prompt monitoring, p. 46; ch. 11 Prompt monitoring, p. 413'
- title: 'Wei, J. et al. (2022). Chain-of-Thought Prompting Elicits Reasoning in Large Language Models. NeurIPS 2022'
  resource: 'https://arxiv.org/abs/2201.11903'
  kind: paper
- title: 'Brown, T. et al. (2020). Language Models are Few-Shot Learners. NeurIPS 2020'
  resource: 'https://arxiv.org/abs/2005.14165'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/prompt-engineering.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Engenharia de prompt

**Definição.** Um prompt é tudo o que vai para o modelo: um prompt de sistema (papel e regras definidos pela aplicação), o prompt do usuário e quaisquer exemplos ou contexto recuperado. As boas práticas de Huyen são escrever instruções claras e explícitas, dar contexto suficiente, quebrar tarefas complexas em subtarefas, dar tempo para o modelo pensar (chain-of-thought) e iterar. Prompts zero-shot e few-shot dependem do aprendizado em contexto. Como prompts são código, ela recomenda organizá-los e versioná-los separados da lógica da aplicação e avaliar cada mudança. A engenharia de prompt defensiva trata de jailbreak, injeção de prompt e extração de informação, e das defesas em camadas contra eles.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

Prompting é a alavanca mais barata e vem primeiro na escada de adaptação (prompt, depois RAG, depois fine-tuning). Ele se sobrepõe à engenharia de contexto, que amplia o foco da redação para tudo o que entra na janela de contexto. Neste projeto, os prompts em News-Automation/prompts/ são versionados e aprovados pelo Kenji, que é exatamente a prática que Huyen descreve.

## Exemplo

Em vez de "Resuma isto", o prompt define o público, as seções da saída, dois exemplos curtos de resumo e "se o artigo não tiver conteúdo de arquitetura, diga isso" - e fica guardado como arquivo versionado com um conjunto de avaliação.

## Conceitos mais amplos

- [Grande modelo de linguagem (LLM) e modelo de fundação](/pt-br/concepts/ai-ml/large-language-model.md)

## Conceitos mais específicos

- [Aprendizado em contexto](/pt-br/concepts/ai-ml/in-context-learning.md)

## Conceitos relacionados

- [Engenharia de contexto](/pt-br/concepts/ai-ml/context-engineering.md)
- [Prompt injection indireta](/pt-br/concepts/security/indirect-prompt-injection.md)

## Fontes

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 5 Prompt Engineering, pp. 211-251 (best practices p. 220; organize and version prompts p. 233; defensive prompt engineering p. 235). O tratamento mais completo: anatomia de um prompt, boas práticas, versionamento e ataques/defesas.
2. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 9 In-Context and Few-Shot Learning with Prompts, p. 288. Prompting como alternativa a dados rotulados.
3. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 2 Opik: prompt monitoring, p. 46; ch. 11 Prompt monitoring, p. 413. Rastreamento e monitoramento de prompts em produção.
4. Wei, J. et al. (2022). *Chain-of-Thought Prompting Elicits Reasoning in Large Language Models*. NeurIPS 2022. <https://arxiv.org/abs/2201.11903> — Prompting com cadeia de raciocínio.
5. Brown, T. et al. (2020). *Language Models are Few-Shot Learners*. NeurIPS 2020. <https://arxiv.org/abs/2005.14165> — Prompting few-shot em escala.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
