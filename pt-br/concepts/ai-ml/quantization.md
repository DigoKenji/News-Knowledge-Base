---
type: Concept
title: 'Quantização'
description: 'Armazenar e calcular os pesos de um modelo (e às vezes as ativações) com números de menor precisão - por exemplo, inteiros de 8 ou 4 bits em vez de floats de 16/32 bits - para reduzir memória e acelerar a inferência com pequena perda de precisão.'
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
  locator: 'ch. 7 Numerical Representations p. 325 and Quantization p. 328; ch. 9 Model Optimization, p. 426'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 8 Model quantization, pp. 303-316 (GGUF and llama.cpp p. 309; GPTQ and EXL2 p. 311)'
- title: 'Tunstall, L., von Werra, L. and Wolf, T. (2022). Natural Language Processing with Transformers, revised edition. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098136796'
  kind: book
  locator: 'ch. 8 Making Models Faster with Quantization, pp. 230-243'
- title: 'Jacob, B. et al. (2018). Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference. CVPR 2018'
  resource: 'https://arxiv.org/abs/1712.05877'
  kind: paper
- title: 'Dettmers, T. et al. (2023). QLoRA: Efficient Finetuning of Quantized LLMs. NeurIPS 2023'
  resource: 'https://arxiv.org/abs/2305.14314'
  kind: paper
lang: pt-br
translation_of: /concepts/ai-ml/quantization.md
---

# Quantização

**Definição.** Cada parâmetro é um número, e seu formato define a memória necessária: FP32 usa 4 bytes, FP16/BF16 2 bytes, INT8 1 byte, formatos de 4 bits meio byte. A quantização mapeia os valores originais para o formato menor com uma escala (e às vezes um ponto zero). A quantização pós-treino (PTQ) converte um modelo treinado diretamente e é a escolha comum para LLMs (formatos e ferramentas como GGUF/llama.cpp, GPTQ, EXL2, AWQ); o treino ciente de quantização simula baixa precisão durante o treino para preservar a acurácia. Huyen a coloca entre as otimizações de modelo para inferência, e no fine-tuning ela viabiliza o QLoRA, que treina adaptadores LoRA sobre um modelo base de 4 bits.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Novo conceito da biblioteca do MBA (livros adicionados em FIAP-MBA/Materiais/Referencias, 2026-11-24); rascunho aguardando certificação do Kenji.

## Como se encaixa

A quantização é o motivo de um modelo de 7-8B rodar em um notebook: cerca de 16 GB de pesos em FP16 viram cerca de 4-5 GB em 4 bits. Ela troca acurácia por memória e velocidade, então o efeito deve ser medido no seu próprio conjunto de avaliação, não presumido; larguras de bit muito baixas degradam primeiro o raciocínio e a qualidade em contexto longo. Ela se combina com as outras alavancas de inferência (KV cache, batching, decodificação especulativa).

## Exemplo

Rodar localmente um modelo de 8B da família Llama em um arquivo GGUF de 4 bits em vez do checkpoint original em BF16: cerca de um quarto da memória, tokens mais rápidos e uma pequena queda nos benchmarks.

## Conceitos mais amplos

- [Throughput/latência/custo de inferência](/pt-br/concepts/ai-ml/inference-tradeoffs.md)

## Conceitos relacionados

- [Adaptação de baixo posto (LoRA)](/pt-br/concepts/ai-ml/lora.md)
- [Destilação](/pt-br/concepts/ai-ml/distillation.md)

## Fontes

1. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 7 Numerical Representations p. 325 and Quantization p. 328; ch. 9 Model Optimization, p. 426. Formatos numéricos, cálculo de memória e quantização tanto para fine-tuning quanto para inferência.
2. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 8 Model quantization, pp. 303-316 (GGUF and llama.cpp p. 309; GPTQ and EXL2 p. 311). Comparação prática de formatos e ferramentas de quantização de LLMs.
3. Tunstall, L., von Werra, L. and Wolf, T. (2022). *Natural Language Processing with Transformers, revised edition*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098136796> — ch. 8 Making Models Faster with Quantization, pp. 230-243. Quantiza um transformer e compara velocidade e acurácia.
4. Jacob, B. et al. (2018). *Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference*. CVPR 2018. <https://arxiv.org/abs/1712.05877> — Quantização só com inteiros e treino ciente de quantização.
5. Dettmers, T. et al. (2023). *QLoRA: Efficient Finetuning of Quantized LLMs*. NeurIPS 2023. <https://arxiv.org/abs/2305.14314> — QLoRA: fine-tuning sobre um modelo base de 4 bits.

## Relações
- (nenhuma ainda)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
