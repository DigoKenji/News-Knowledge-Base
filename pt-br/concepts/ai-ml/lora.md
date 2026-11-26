---
type: Concept
title: Adaptação de baixo posto (LoRA)
description: Low-Rank Adaptation - um método de fine-tuning eficiente em parâmetros que congela os pesos do modelo e treina pequenas matrizes de posto baixo adicionadas a algumas camadas, reduzindo os parâmetros treináveis em ordens de grandeza com pouca perda de qualidade.
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
  at: '2026-11-24T07:00:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Hu, E. J. et al. (2022). LoRA - Low-Rank Adaptation of Large Language Models. ICLR 2022, arXiv:2106.09685'
  resource: 'https://arxiv.org/abs/2106.09685'
  kind: paper
- title: 'Aghajanyan, A. et al. (2021). Intrinsic Dimensionality Explains the Effectiveness of Language Model Fine-Tuning. ACL 2021, arXiv:2012.13255'
  resource: 'https://arxiv.org/abs/2012.13255'
  kind: paper
- title: 'Dettmers, T. et al. (2023). QLoRA - Efficient Finetuning of Quantized LLMs. NeurIPS 2023, arXiv:2305.14314'
  resource: 'https://arxiv.org/abs/2305.14314'
  kind: paper
- title: Hugging Face. PEFT - LoRA conceptual guide
  resource: 'https://huggingface.co/docs/peft/main/en/conceptual_guides/lora'
  kind: docs
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 7 Parameter-Efficient Finetuning, pp. 332-347'
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 5 LoRA p. 213 and QLoRA p. 215'
lang: pt-br
translation_of: /concepts/ai-ml/lora.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
- by: claude-opus (interactive, project thread; concept taxonomy)
  at: '2026-11-26T21:30:00-03:00'
  change: 'taxonomia: subdomínio definido pela taxonomia do domínio'
---

# Adaptação de baixo posto (LoRA)

**Definição.** LoRA adapta um grande modelo pré-treinado a uma nova tarefa aprendendo apenas uma atualização de posto baixo em matrizes de pesos selecionadas. Em vez de mudar uma matriz de pesos W de d×d, aprende duas matrizes finas B (d×r) e A (r×d) com posto r pequeno (por exemplo 8), e usa W + BA na inferência. Os pesos originais ficam congelados e compartilhados.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md). Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

A ideia se apoia na descoberta de que as mudanças necessárias para adaptar um modelo têm baixa "dimensão intrínseca". Hu et al. aplicaram LoRA às projeções de atenção do GPT-3 175B e reduziram os parâmetros treináveis cerca de 10.000 vezes e a memória de GPU cerca de 3 vezes, igualando a qualidade do fine-tuning completo. O adaptador pode ser fundido em W após o treino, sem latência extra na inferência, ou mantido separado para que um modelo base sirva muitos adaptadores intercambiáveis. O QLoRA acrescenta quantização de 4 bits da base congelada, o que tornou possível o fine-tuning de um modelo de 65B numa única GPU de 48 GB.

## Por que importa e trade-offs

LoRA torna o fine-tuning acessível e os adaptadores minúsculos (megabytes), por isso é a forma padrão de customizar modelos abertos. Pode render menos que o fine-tuning completo quando a nova tarefa está longe do pré-treino (por exemplo um idioma novo ou pré-treino continuado pesado), e o posto e as camadas-alvo são hiperparâmetros a ajustar.

## Exemplo

Treinar um adaptador LoRA de 20 MB que ensina a um modelo aberto de 8B o formato de notas de deep dive de Kenji, e carregá-lo sobre a base compartilhada só na hora de resumir.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 7 Parameter-Efficient Finetuning, pp. 332-347: LoRA entre os métodos PEFT, com cálculo de memória.
- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 5 LoRA p. 213 and QLoRA p. 215: Configurações práticas de LoRA/QLoRA para fine-tuning de um modelo de 8B.

## Fontes

1. Hu, E. J. et al. (2022). *LoRA - Low-Rank Adaptation of Large Language Models*. ICLR 2022, arXiv:2106.09685. <https://arxiv.org/abs/2106.09685> — Método, economia de parâmetros e memória, sem latência extra de inferência.
2. Aghajanyan, A. et al. (2021). *Intrinsic Dimensionality Explains the Effectiveness of Language Model Fine-Tuning*. ACL 2021, arXiv:2012.13255. <https://arxiv.org/abs/2012.13255> — Por que atualizações de posto baixo são suficientes.
3. Dettmers, T. et al. (2023). *QLoRA - Efficient Finetuning of Quantized LLMs*. NeurIPS 2023, arXiv:2305.14314. <https://arxiv.org/abs/2305.14314> — LoRA sobre um modelo base quantizado em 4 bits.
4. Hugging Face. *PEFT - LoRA conceptual guide*. <https://huggingface.co/docs/peft/main/en/conceptual_guides/lora> — Parâmetros práticos (posto, alpha, módulos-alvo) e fusão.
5. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 7 Parameter-Efficient Finetuning, pp. 332-347. LoRA entre os métodos PEFT, com cálculo de memória.
6. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 5 LoRA p. 213 and QLoRA p. 215. Configurações práticas de LoRA/QLoRA para fine-tuning de um modelo de 8B.

## Relações
- (nenhuma ainda)

## Notas
- [LiveLoRA: Migração exata de KV Cache entre atualizações LoRA para agentes LLM](/pt-br/notes/ai-ml/2026-11-24-livelora-exact-kv-cache-migration-across-lora-updates-for-ll.md)
