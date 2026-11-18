---
type: Article Summary
title: Entendendo Embeddings Posicionais em LLMs Modernos
description: Raschka percorre a codificação de posição, desde tabelas aprendidas e senoides até RoPE, e depois como os modelos abertos de 2026 combinam RoPE parcial, camadas NoPE e viés aprendido.
resource: https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
tags:
- ai-ml
- llm-engineering
- rope
- kv-cache
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-16T06:55:00-03:00'
sources:
- resource: https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
  title: Understanding Positional Embeddings in Modern LLMs
  author: Sebastian Raschka
status: stable
source_url: https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
source: Ahead of AI
source_type: blog
published: '2026-11-15'
domain: ai-ml
subdomain: llm-engineering
concepts:
- rope
- kv-cache
selected_by: discovery
content: open
notion_url: https://app.notion.com/p/41abaf74172d814881fcc33b2038e53a
lang: pt-br
translation_of: /notes/ai-ml/2026-11-16-understanding-positional-embeddings.md
---

# Entendendo Embeddings Posicionais em LLMs Modernos

**Fonte:** https://magazine.sebastianraschka.com/p/understanding-positional-embeddings
**Autor:** Sebastian Raschka, PhD
**Data:** 2026-11-15
**Tags:** #llm #architecture #positional-embeddings #rope #attention

---

## 💬 Sobre o que é
Raschka explica como LLMs codificam a posição dos tokens, desde os embeddings absolutos aprendidos do GPT-2 até o [RoPE](/pt-br/concepts/rope.md) e as escolhas que os modelos abertos mais recentes fazem. É um fundamento deliberadamente técnico para um futuro artigo sobre tendências de arquitetura, com exemplos visuais, alguma matemática e uma pequena implementação em PyTorch.

## 1. Onde a informação de posição entra em um LLM
A informação de posição é adicionada em pontos diferentes. O GPT-2 soma um vetor de posição logo após o embedding do token. O MiniMax-M3 a injeta dentro da atenção em cada bloco, usando RoPE. A auto-atenção simples não consegue distinguir "The red fox jumps over the gray fox" da versão com as palavras trocadas, já que os tokens e os embeddings são idênticos. Uma máscara causal fornece alguma ordem implícita, mas não consegue distinguir uma distância de uma posição de uma distância de dez, então sinais explícitos ainda ajudam.

## 2. Somando um vetor de posição a cada embedding de token

### 2.1 Embeddings de posição absolutos aprendidos no GPT-2
O GPT-2 aprende um vetor por posição (1,024 deles) e o soma ao embedding do token. O tamanho do contexto é fixado antes do treinamento, e estendê-lo é caro. Os pesos posicionais são 0.30% do total do GPT-2 Medium com 1,024 posições, mas passariam de 22% com um contexto de 100k tokens.

### 2.2 Codificações de posição senoidais fixas no transformer original
O transformer de 2017 soma vetores fixos de seno e cosseno. Eles podem ser calculados além do comprimento de treinamento, embora isso não garanta boa qualidade ali. O artigo levantou a hipótese de que essa estrutura facilita o aprendizado de deslocamentos relativos.

## 3. Codificando a posição relativa com RoPE
[RoPE](/pt-br/concepts/rope.md) (Su et al., 2021) mantém a ideia senoidal, mas a move para dentro da atenção. Queries e keys são rotacionadas por um ângulo definido por sua posição, enquanto os values não passam pela rotação. O produto escalar então depende apenas do deslocamento entre as posições. No exemplo trabalhado, deslocar os dois tokens em três posições mantém o score em 13, enquanto aproximar apenas a key em uma posição o muda para cerca de 19.92.

## 4. Calculando as frequências de rotação
Cada par de features recebe uma frequência θᵢ = b^(−2i/d), com base b = 10,000 no artigo RoFormer. Várias frequências permitem que alguns pares mudem rapidamente com a posição e outros lentamente. Para uma dimensão de cabeça igual a 8, isso resulta em [1, 0.1, 0.01, 0.001] radianos por posição.

## 5. Implementando RoPE em PyTorch
As tabelas de cosseno e seno são calculadas uma vez por modelo e broadcastadas entre batch e cabeças. Apenas queries e keys passam pelo RoPE. O código confirma que deslocar todas as posições em 3 mantém os scores de atenção inalterados. O artigo original do RoPE e o LLaMA emparelham as features em layout intercalado, enquanto a maioria dos códigos usa split-half, que é equivalente após reordenar e é mais fácil de vetorizar.

## 6. Escolhas posicionais nas arquiteturas modernas
O RoPE completo em cada bloco é um item básico dos modelos abertos desde o LLaMA, mas muitos designs mais novos o variam.

### 6.1 RoPE parcial
O RoPE parcial rotaciona apenas parte de cada cabeça. O MiniMax-M3 rotaciona 64 de 128 dimensões, reduzindo pela metade as tabelas de cosseno e seno. O [KV cache](/pt-br/concepts/kv-cache.md) ainda armazena keys e values completos, então a economia é modesta. Raschka chama o RoPE parcial de "hardly worth it" na opinião dele, citando um artigo em que cerca de 10% das features frequentemente igualaram o RoPE completo.

### 6.2 Camadas NoPE
O NoPE não usa codificação posicional explícita e depende da máscara causal. Em um exemplo simplificado, o peso do primeiro token diminui conforme o prefixo cresce, o que uma camada posterior pode interpretar como posição. Um modelo NoPE de 107M ficou em primeiro lugar em generalização de comprimento em dez tarefas, mas um modelo de código de 1.3B degradou nos comprimentos mais longos. O SmolLM3 (3B) aplica RoPE em três de cada quatro blocos, e seus autores relataram melhor desempenho em contexto longo.

### 6.3 Viés de posição relativa aprendido
O Inkling, da Thinky, adiciona um número aprendido a cada score query-key escalonado antes do softmax, dependendo do estado oculto da query e da distância até a key. O T5 usou uma versão mais simples em 2019.

### 6.4 Como essas escolhas aparecem nos modelos recentes
Na galeria do autor, o RoPE completo em toda camada softmax continua sendo a escolha mais comum, mas sua fatia caiu de 2025 para 2026. Os designs mistos de RoPE/NoPE estão crescendo, e o RoPE parcial passou de uma minoria dos modelos com RoPE em 2025 para a maioria em 2026. Os grupos são pequenos, então essas tendências são provisórias.

## Padrões e técnicas identificados
- **Tabela de embeddings absolutos aprendida:** um vetor por índice, simples, mas preso a um tamanho de contexto fixo.
- **Rotação dentro da atenção (RoPE):** queries e keys rotacionadas pela posição, de modo que os scores dependem do deslocamento relativo.
- **Tabelas pré-calculadas de cos/sen:** calculadas uma vez e reutilizadas entre batch, cabeças e queries.
- **Camadas híbridas RoPE/NoPE:** RoPE omitido em alguns blocos, apoiando-se na máscara causal nesses blocos.
- **Viés relativo aprendido:** um termo dependente da distância e condicionado pela query, somado antes do softmax.

## Principais conclusões
- A máscara causal fornece alguma informação de ordem, mas sinais explícitos de posição ainda ajudam a atenção a estimar distâncias.
- A propriedade-chave do RoPE é que os scores de atenção dependem apenas do deslocamento relativo, então um deslocamento comum do prefixo não altera nada.
- Tabelas absolutas aprendidas são caras de estender; métodos baseados em rotação evitam esse custo.
- O RoPE continua sendo a escolha mais comum, mas o RoPE parcial e as misturas RoPE/NoPE estão crescendo.

## Leitura complementar
- 📄 RoFormer (Su et al., 2021), sem URL no texto da fonte → o artigo original do RoPE, no qual o texto se baseia.
- 📄 Attention Is All You Need (2017), sem URL no texto da fonte → a origem das codificações senoidais e da hipótese do deslocamento relativo.
- 📚 The Impact of Positional Encoding on Length Generalization in Transformers, sem URL no texto da fonte → o artigo sobre NoPE e seus experimentos de generalização de comprimento.
- 🔗 Fractional Rotation, Full Potential?, sem URL no texto da fonte → o estudo do RoPE parcial na escala de 1B a 8B.
- 📚 Understanding and Coding the KV Cache in LLMs from Scratch (Raschka), sem URL no texto da fonte → contexto útil sobre como o cache afeta a memória de contexto longo.

---
*Resumo gerado: 2026-11-16*
