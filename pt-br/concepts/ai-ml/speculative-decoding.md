---
type: Concept
title: Decodificação especulativa
description: Uma forma sem perdas de acelerar a geração de LLMs - um modelo rascunho rápido adivinha vários próximos tokens e o modelo grande verifica todos numa única passagem, mantendo os que confirma.
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
  by: claude (Project_News daily run)
  at: '2026-11-16T06:50:00-03:00'
  expanded:
    by: claude-opus (interactive, project thread)
    at: '2026-11-24T20:30:00-03:00'
sources:
- title: 'Leviathan, Y., Kalman, M. and Matias, Y. (2023). Fast Inference from Transformers via Speculative Decoding. ICML 2023, arXiv:2211.17192'
  resource: 'https://arxiv.org/abs/2211.17192'
  kind: paper
- title: 'Chen, C. et al. (2023). Accelerating Large Language Model Decoding with Speculative Sampling. arXiv:2302.01318'
  resource: 'https://arxiv.org/abs/2302.01318'
  kind: paper
- title: 'Cai, T. et al. (2024). Medusa - Simple LLM Inference Acceleration Framework with Multiple Decoding Heads. ICML 2024, arXiv:2401.10774'
  resource: 'https://arxiv.org/abs/2401.10774'
  kind: paper
- title: 'Iusztin, P. and Labonne, M. (2024). LLM Engineer''s Handbook. Packt'
  resource: 'https://books.google.com/books?vid=ISBN9781836200079'
  kind: book
  locator: 'ch. 8 Speculative decoding, pp. 295-303'
- title: 'Huyen, C. (2025). AI Engineering: Building Applications with Foundation Models. O''Reilly'
  resource: 'https://books.google.com/books?vid=ISBN9781098166304'
  kind: book
  locator: 'ch. 9 Model Optimization (overcoming the autoregressive bottleneck), pp. 426-440'
relations:
- type: part-of
  target: /concepts/ai-ml/inference-tradeoffs.md
lang: pt-br
translation_of: /concepts/ai-ml/speculative-decoding.md
updated:
- by: claude-opus (interactive, project thread; MBA library digest)
  at: '2026-11-25T00:45:00-03:00'
  change: 'adicionadas citações da biblioteca do MBA; definição inalterada'
---

# Decodificação especulativa

**Definição.** Speculative decoding (também speculative sampling) usa um pequeno modelo rascunho para propor k tokens e depois roda o modelo-alvo uma vez sobre os k em paralelo. Os tokens são aceitos até a primeira discordância, e ali é amostrado um token de correção. Uma regra de amostragem por rejeição garante que a saída tem exatamente a mesma distribuição do modelo-alvo sozinho, então a qualidade não muda.

Domínio: [ai-ml](/pt-br/domains/ai-ml.md) · subdomínio `llm-engineering`. Expandido em 2026-11-24 com fontes independentes; certificado por Kenji em 2026-11-24.

## Como funciona

O decode é limitado por memória: gerar um token ou verificar cinco custa quase o mesmo tempo, porque o gargalo é ler os pesos. Se o rascunho acerta com frequência (alta taxa de aceitação), saem vários tokens por passo caro, tipicamente um ganho de 2-3× em latência. Rascunhos podem ser um modelo pequeno separado, cabeças extras de previsão no modelo-alvo (Medusa), um rascunho no nível das features (EAGLE) ou busca de n-gramas no próprio prompt. Leviathan et al. (Google) e Chen et al. (DeepMind) publicaram o método de forma independente em 2022-2023.

## Por que importa e trade-offs

Respostas mais rápidas com a mesma qualidade, por isso a maioria dos motores de serviço suporta. Os ganhos diminuem com lotes grandes (a GPU já está ocupada), o rascunho consome memória e cálculo, e a taxa de aceitação cai em saídas criativas ou com temperatura alta.

## Exemplo

O rascunho propõe "o gato sentou no"; o alvo aceita "o gato sentou", rejeita "no" e amostra "perto" no lugar, então três tokens mais uma correção saem de uma única passagem do alvo.

## Da biblioteca do MBA

Livros adicionados em FIAP-MBA/Materiais/Referencias (2026-11-24). Resumos com localizadores; nada foi copiado além de frases curtas.

- **Iusztin, P. and Labonne, M. (2024)**, *LLM Engineer's Handbook*, ch. 8 Speculative decoding, pp. 295-303: Decodificação com rascunho e verificação, com código.
- **Huyen, C. (2025)**, *AI Engineering: Building Applications with Foundation Models*, ch. 9 Model Optimization (overcoming the autoregressive bottleneck), pp. 426-440: Decodificação especulativa ao lado de outras acelerações de decodificação.

## Fontes

1. Leviathan, Y., Kalman, M. and Matias, Y. (2023). *Fast Inference from Transformers via Speculative Decoding*. ICML 2023, arXiv:2211.17192. <https://arxiv.org/abs/2211.17192> — Método e prova de que as saídas seguem a distribuição do alvo.
2. Chen, C. et al. (2023). *Accelerating Large Language Model Decoding with Speculative Sampling*. arXiv:2302.01318. <https://arxiv.org/abs/2302.01318> — Derivação independente da DeepMind, aceleração de 2-2,5× no Chinchilla 70B.
3. Cai, T. et al. (2024). *Medusa - Simple LLM Inference Acceleration Framework with Multiple Decoding Heads*. ICML 2024, arXiv:2401.10774. <https://arxiv.org/abs/2401.10774> — Variante sem modelo rascunho usando cabeças extras no modelo-alvo.
4. Iusztin, P. and Labonne, M. (2024). *LLM Engineer's Handbook*. Packt. <https://books.google.com/books?vid=ISBN9781836200079> — ch. 8 Speculative decoding, pp. 295-303. Decodificação com rascunho e verificação, com código.
5. Huyen, C. (2025). *AI Engineering: Building Applications with Foundation Models*. O'Reilly. <https://books.google.com/books?vid=ISBN9781098166304> — ch. 9 Model Optimization (overcoming the autoregressive bottleneck), pp. 426-440. Decodificação especulativa ao lado de outras acelerações de decodificação.

## Relações
- **parte de** [Throughput/latência/custo de inferência](/pt-br/concepts/ai-ml/inference-tradeoffs.md)

## Notas
As notas marcadas com este conceito são listadas no índice de conceitos após cada execução.
