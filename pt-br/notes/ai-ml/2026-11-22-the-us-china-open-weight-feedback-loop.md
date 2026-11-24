---
type: Article Summary
title: O ciclo de realimentação dos modelos open-weight EUA-China
description: Modelos open-weight dos EUA e da China se aprimoram mutuamente, e os modelos proprietários também se beneficiam, de modo que a concorrência ajuda a todos.
resource: https://sebastianraschka.com/blog/2026/us-china-open-weight-feedback-loop.html
tags:
- ai-ml
- rope
- kv-cache
- mixture-of-experts
- multi-head-latent-attention
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://sebastianraschka.com/blog/2026/us-china-open-weight-feedback-loop.html
  title: The US-China Open-Weight Feedback Loop
  author: Sebastian Raschka
status: stable
source_url: https://sebastianraschka.com/blog/2026/us-china-open-weight-feedback-loop.html
source: Sebastian Raschka
source_type: blog
published: '2026-11-21'
domain: ai-ml
concepts:
- rope
- kv-cache
- mixture-of-experts
- multi-head-latent-attention
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d81adafc7d665b3bc0c4f
lang: pt-br
translation_of: /notes/ai-ml/2026-11-22-the-us-china-open-weight-feedback-loop.md
---

## 🎯 Tese central
Modelos open-weight dos EUA e da China dependem das ideias uns dos outros, e os modelos proprietários são ao mesmo tempo melhores e mais baratos porque os modelos open-weight existem, de modo que a concorrência entre eles é boa.

## 🧭 Por que o autor escreveu o texto
O post parte de uma afirmação sobre como as ideias de arquitetura se espalham entre laboratórios e países, e a tabela de referências é a evidência para ela. 🔎 Contexto adicional: o artigo diz que o texto foi publicado originalmente como uma Substack Note, então é um post curto e guiado por opinião, e não um estudo completo. Se a tese se sustentar, os projetos dos lançamentos open-weight funcionam como um bem comum compartilhado, do qual o progresso dos modelos proprietários também se beneficia.

## A afirmação inicial
### Modelos open-weight
O autor afirma que os modelos open-weight dos EUA "não seriam tão bons sem os modelos open-weight chineses, e vice-versa". A afirmação trata de dependência mútua, não de um lado copiar o outro. 🔎 Contexto adicional: open-weight significa que os parâmetros treinados são publicados para que outros possam baixar, executar e construir sobre eles. O artigo não define o termo.

### Modelos proprietários
O autor afirma que os modelos proprietários "não seriam tão bons (e baratos) sem os modelos open-weight". O artigo não apresenta mecanismo nem números para essa ligação, então deve ser lido como a visão do autor. 🔎 Contexto adicional: modelos proprietários são oferecidos por meio de uma API ou de um produto, e seus pesos não são publicados.

### Concorrência
O post encerra sua abertura com uma regra de uma linha: "Concorrência é uma coisa boa."

## Referências
A tabela lista, para cada modelo, a ideia anterior que ele adotou. O autor observa que a figura à qual se refere não é abrangente, apenas uma seleção.

### Embeddings de posição rotacionais (RoPE)
LLaMA (2023) usa os embeddings de posição rotacionais do RoFormer para implementar o popular método RoPE, que codifica as posições dos tokens rotacionando queries e keys em cada camada de atenção. O autor aponta para seu artigo completo sobre embeddings posicionais para mais detalhes, mas esse artigo não está no texto extraído.
> 🔎 Contexto adicional: rotacionar queries e keys por ângulos que dependem da posição faz com que o produto escalar entre elas dependa da distância entre os tokens. É por isso que a atenção consegue raciocinar sobre o quão distantes as coisas estão.

### Atenção de consultas agrupadas (grouped-query attention)
O modelo citado aqui é o DeepSeek LLM 67B (2023). O artigo diz que a grouped-query attention "agora é uma técnica padrão em que as cabeças de consulta compartilham conjuntos de keys e values".
> 🔎 Contexto adicional: na atenção multi-cabeça padrão, cada cabeça de consulta tem suas próprias keys e values. A grouped-query attention permite que várias cabeças de consulta compartilhem um único conjunto, o que reduz o cache KV. O artigo não dá nenhum valor de tamanho, então nenhum é informado aqui.

### Especialistas refinados (mistura de especialistas)
O DeepSeekMoE popularizou o design de especialistas refinados, e o OLMoE (2024) e muitos outros o adaptaram. O artigo credita essa linhagem e não informa a quantidade de especialistas.
> 🔎 Contexto adicional: uma camada de mistura de especialistas contém muitas sub-redes especialistas, e um roteador envia cada token para apenas alguns deles. Os designs refinados usam mais especialistas, e menores.

### Predição de múltiplos tokens
O DeepSeek-V3 (2024) adaptou o objetivo de predição de múltiplos tokens da Meta. Durante o treinamento, um módulo extra prevê um token além do alvo usual de próximo token.
> 🔎 Contexto adicional: o objetivo padrão treina o modelo para prever apenas o próximo token, e isso adiciona um sinal de treinamento para um token mais à frente. O artigo descreve o módulo extra apenas como um componente de treinamento e não diz se ele é usado na inferência.

### Atenção latente multi-cabeça (MLA)
O DeepSeek-V2 popularizou a MLA como alternativa à combinação de GQA com abordagens de janela deslizante para reduzir o cache KV. No Kimi K2 (2025), keys e values compartilham uma representação comprimida no cache. O autor linka sua página sobre MLA para detalhes, que não está no texto.
> 🔎 Contexto adicional: a MLA armazena em cache uma única representação comprimida, a partir da qual keys e values podem ser recuperados, em vez de armazenar keys e values completos por cabeça.

### Atenção híbrida com a regra delta com porta (gated delta rule)
O Qwen3-Next (2025) foi uma das primeiras arquiteturas a usar um design de atenção híbrida com a regra delta com porta do Gated DeltaNet. O autor linka uma página sobre atenção híbrida, que não está no texto.
> 🔎 Contexto adicional: um design híbrido mistura camadas de atenção com outras camadas de mistura de sequências. A regra delta com porta é uma regra de atualização para um estado de memória recorrente. O artigo não informa as proporções entre camadas, então nenhuma é informada aqui.

### Portões aprendidos sobre a saída da atenção
O Trinity Large da Arcee (2026) usa um portão sigmoide aprendido, tomado do trabalho de atenção com portões do Qwen, para escalar a saída da atenção antes da projeção de saída. O Instella-MoE da AMD (2026) combina a MLA no estilo DeepSeek com portões sobre a saída da atenção, em que a entrada determina quanto da saída da MLA passa adiante.
> 🔎 Contexto adicional: uma sigmoide comprime qualquer valor para o intervalo entre 0 e 1, de modo que o portão pode escalar cada saída da atenção de zero até força total. No Instella, o valor do portão depende da entrada, então a quantidade que passa varia por token.

### Balanceamento por quantis para o viés do roteador
O Marin Mochi (2026) usa o método de balanceamento por quantis de Jianlin Su, conforme a tabela atribui a Open Athena. O método agrupa em faixas (bins) os scores brutos do roteador para estimar como ajustar o viés de roteamento de cada especialista.
> 🔎 Contexto adicional: um viés de roteamento desloca quais especialistas um roteador prefere, então ajustá-lo pode distribuir a carga entre os especialistas. O artigo não diz como as faixas são formadas nem qual é o tamanho dos ajustes.

## 🧠 Modelos mentais
### Ciclo de realimentação entre modelos abertos e proprietários
**O modelo:** Ideias circulam entre modelos open-weight e proprietários, e entre países. Cada lançamento se torna um bloco de construção para os outros, então a qualidade sobe dos dois lados ao mesmo tempo.
**Como aplicar:** Ao avaliar uma família de modelos, identifique quais técnicas ela tomou emprestadas e quais ela liberou para outros reutilizarem.

### Concorrência como motor
**O modelo:** A concorrência entre laboratórios é o que mantém o ciclo funcionando; nas palavras do autor, "uma coisa boa".
**Como aplicar:** Considere um cenário com várias fontes de modelos mais saudável do que um dominado por um único fornecedor, e verifique se a sua arquitetura não depende de uma única fonte.

## 🔑 Principais conclusões
- O autor argumenta que os modelos open-weight dos EUA e da China não seriam tão bons sem um ao outro.
- O autor diz que os modelos proprietários são ao mesmo tempo melhores e mais baratos porque os modelos open-weight existem, sem apresentar números.
- As técnicas viajam ao longo de linhagens nomeadas: o RoPE do RoFormer para o LLaMA (2023), a grouped-query attention no DeepSeek LLM 67B (2023) e os especialistas refinados do DeepSeekMoE para o OLMoE (2024).
- Modelos posteriores continuam tomando emprestado: a MLA do DeepSeek-V2 para o Kimi K2 (2025), a regra delta com porta do Gated DeltaNet no Qwen3-Next (2025) e o portão do Qwen no Arcee Trinity (2026).
- A tabela é uma seleção de empréstimos, não um mapa completo.
- A conclusão do autor é que a concorrência é uma coisa boa.

## 🏛️ Para o Kenji
Interpretação, não do autor: a tabela funciona como um mapa prático de quais técnicas de atenção, cache e roteamento hoje se repetem entre famílias de modelos abertos e fechados, o que importa ao avaliar fornecedores de modelos para uma arquitetura. Pergunta que isso levanta: se os lançamentos open-weight de uma região desacelerassem, quais das técnicas de que seus sistemas dependem ainda seriam mantidas, e como mudariam os seus critérios de seleção de modelos?

## 📚 Plano de leitura
### Do artigo
- Nenhum link no texto do artigo traz URL, então nenhum pode ser listado. O autor cita suas próprias páginas sobre embeddings posicionais, GQA, MLA e atenção híbrida, mas o texto extraído não tem endereços para elas.

### Para se aprofundar
- Understanding Positional Embeddings in Modern LLMs (o artigo completo do autor) — base mais profunda sobre o RoPE e métodos relacionados.
- Grouped-query attention (a página do autor sobre GQA) — como as cabeças de consulta compartilham keys e values.
- Multi-head latent attention (a página do autor sobre MLA) — detalhes do cache KV comprimido.
- Hybrid Attention (a página do autor) — designs híbridos como o Gated DeltaNet.
- Open October LLM Architecture Roundup (item de "Leia a seguir") — dez lançamentos de arquiteturas open-weight de outubro de 2026.
- Implementing LLM Architectures From Scratch (item de "Leia a seguir") — compara implementações open-weight com código de referência.

## Conceitos
- [Embedding de posição rotacional (RoPE)](/pt-br/concepts/ai-ml/rope.md)
- [KV cache](/pt-br/concepts/ai-ml/kv-cache.md)
- [Mistura de especialistas](/pt-br/concepts/ai-ml/mixture-of-experts.md)
- [Atenção latente multi-cabeça](/pt-br/concepts/ai-ml/multi-head-latent-attention.md)
