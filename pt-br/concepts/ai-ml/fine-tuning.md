---
type: Concept
title: 'Fine-tuning (ajuste fino)'
description: 'Continuar o treino de um modelo pré-treinado em um conjunto menor e específico da tarefa - fine-tuning supervisionado com exemplos e, opcionalmente, alinhamento por preferências (RLHF, DPO) - para mudar seu comportamento ou formato, e não para acrescentar fatos novos.'
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
  locator: 'ch. 2 Post-Training (SFT p. 80, preference finetuning p. 83); ch. 7 Finetuning, pp. 307-361 (Finetuning and RAG p. 316; PEFT p. 332); ch. 8 Dataset Engineering, p. 363'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 5 Supervised Fine-Tuning, pp. 177-228 (full fine-tuning p. 211, LoRA p. 213, QLoRA p. 215); ch. 6 Preference Alignment and DPO, pp. 229-260'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 2 Fine-Tuning Transformers, p. 45; ch. 9 Fine-Tuning a Language Model / a Classifier, pp. 289-297'
- title: 'Ouyang, L. et al. (2022). Training language models to follow instructions with human feedback. NeurIPS 2022'
  resource: 'https://arxiv.org/abs/2203.02155'
  kind: paper
- title: 'Rafailov, R. et al. (2023). Direct Preference Optimization: Your Language Model is Secretly a Reward Model. NeurIPS 2023'
  resource: 'https://arxiv.org/abs/2305.18290'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/fine-tuning.md
updated:
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Fine-tuning (ajuste fino)

**Definição.** Fine-tuning é aprendizado por transferência: parte-se de um modelo que já aprendeu padrões gerais e ajustam-se os pesos para um objetivo mais estreito. Para LLMs, Huyen separa o pós-treino em fine-tuning supervisionado (SFT, treinar com pares prompt-resposta para o modelo seguir instruções) e fine-tuning por preferências (ensinar qual de duas respostas as pessoas preferem, via RLHF ou Direct Preference Optimization). O fine-tuning completo atualiza todos os pesos e exige muita memória; o fine-tuning eficiente em parâmetros (PEFT), como LoRA ou QLoRA, treina poucos parâmetros extras e chega a resultados próximos com custo muito menor. A regra prática dela: "fine-tuning é para forma, RAG é para fatos" (Huyen, p. 317).

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

Os dois livros tratam o fine-tuning como um passo posterior, não o primeiro: tente prompting, depois RAG, e faça fine-tuning quando precisar de estilo ou formato de saída consistente, de um modelo menor e mais barato que iguale um maior em uma tarefa, ou de um comportamento de domínio que prompts não alcançam (Huyen, cap. 7 "When to Finetune"; Iusztin e Labonne, cap. 5). O custo real é o conjunto de dados: qualidade e cobertura dos exemplos importam mais que quantidade (Huyen, cap. 8). Modelos com fine-tuning também precisam ser reavaliados para regressões e segurança.

## Exemplo

O Project_News poderia fazer fine-tuning de um modelo aberto pequeno com algumas centenas de notas aprovadas, para ele escrever no formato da casa, enquanto os fatos atuais dos artigos continuam chegando por recuperação (RAG).

## Conceitos mais amplos

- [Grande modelo de linguagem (LLM) e modelo de fundação](/pt-br/concepts/ai-ml/large-language-model.md)

## Conceitos mais específicos

- [Adaptação de baixo posto (LoRA)](/pt-br/concepts/ai-ml/lora.md)
- [Destilação](/pt-br/concepts/ai-ml/distillation.md)

## Conceitos relacionados

- [Geração aumentada por recuperação (RAG)](/pt-br/concepts/ai-ml/rag.md)
- [Quantização](/pt-br/concepts/ai-ml/quantization.md)

## Fontes

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 2 Post-Training (SFT p. 80, preference finetuning p. 83); ch. 7 Finetuning, pp. 307-361 (Finetuning and RAG p. 316; PEFT p. 332); ch. 8 Dataset Engineering, p. 363. Quando fazer ou não fine-tuning, cálculo de memória, PEFT, fusão de modelos e curadoria de dados.
2. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 5 Supervised Fine-Tuning, pp. 177-228 (full fine-tuning p. 211, LoRA p. 213, QLoRA p. 215); ch. 6 Preference Alignment and DPO, pp. 229-260. SFT e DPO de ponta a ponta em um modelo de 8B, incluindo criação de datasets de instruções e de preferências.
3. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 2 Fine-Tuning Transformers, p. 45; ch. 9 Fine-Tuning a Language Model / a Classifier, pp. 289-297. Fine-tuning clássico de encoders para classificação e com poucos rótulos.
4. Ouyang, L. et al. (2022). *Training language models to follow instructions with human feedback*. NeurIPS 2022. <https://arxiv.org/abs/2203.02155> — Receita SFT + RLHF por trás dos modelos que seguem instruções.
5. Rafailov, R. et al. (2023). *Direct Preference Optimization: Your Language Model is Secretly a Reward Model*. NeurIPS 2023. <https://arxiv.org/abs/2305.18290> — Direct Preference Optimization.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
