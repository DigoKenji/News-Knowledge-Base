---
type: Article Summary
title: Como a Instacart usa LLMs para fazer as pessoas comprarem mais
description: Combinar dados históricos, interpretação por LLM e guardrails, e depois destilar tudo em um modelo pequeno com cache, torna a compreensão de consultas de compras precisa e rápida.
resource: https://blog.bytebytego.com/p/how-instacart-uses-llms-to-make-people
tags:
- ai-ml
- llm-engineering
- rag
- inference-tradeoffs
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://blog.bytebytego.com/p/how-instacart-uses-llms-to-make-people
  title: How Instacart Uses LLMs to Make People Buy More
  author: ByteByteGo
status: stable
source_url: https://blog.bytebytego.com/p/how-instacart-uses-llms-to-make-people
source: ByteByteGo
source_type: blog
published: '2026-11-16'
domain: ai-ml
subdomain: llm-engineering
concepts:
- rag
- inference-tradeoffs
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d810d88d9cad76f39ce7c
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-how-instacart-uses-llms-to-make-people-buy-more.md
---

# Como a Instacart usa LLMs para fazer as pessoas comprarem mais

**Fonte:** https://blog.bytebytego.com/p/how-instacart-uses-llms-to-make-people
**Autor:** ByteByteGo
**Data:** 2026-11-16
**Conceitos:** [Geração aumentada por recuperação (RAG)](/pt-br/concepts/rag.md) · [Vazão, latência e custo de inferência](/pt-br/concepts/inference-tradeoffs.md)

---

## 🎯 Tese central
A Instacart melhorou a busca de produtos combinando três etapas: evidências históricas de compra para propor candidatos, interpretação por LLM ancorada no contexto do catálogo e guardrails baseados em embeddings para filtrar o resultado. Depois, destilou o pipeline lento de modelos grandes em um modelo pequeno, com cache e ajuste fino, para que a busca ao vivo continuasse rápida.

## 🧭 Por que o autor escreveu
Os compradores digitam frases vagas, como "pão sem glúten", que raramente coincidem com a redação do catálogo, e o sistema de busca precisa transformá-las em categorias, consultas alternativas e tags estruturadas antes que a recuperação e o ranqueamento possam funcionar. A configuração anterior da Instacart usava vários modelos separados, treinados com dados de comportamento ruidosos, custava caro para manter e ainda interpretava a mesma consulta de forma inconsistente. Se a tese se confirmar, uma única camada de interpretação baseada em LLM pode atender todas essas tarefas de busca, desde que consiga cumprir os limites de latência em tempo real.

## O problema da abordagem inicial da Instacart

O artigo divide a busca de produtos em três atividades: compreensão da consulta (QU), recuperação e ranqueamento. A QU lê o que o usuário digitou, a recuperação encontra um conjunto de produtos relevantes, e o ranqueamento decide a ordem em que esses produtos são exibidos. O ponto central do artigo é que a saída da QU dirige as outras duas etapas, então uma frase como "pão sem glúten" deve ser lida como uma única intenção de compra, com o atributo sem glúten, e não como três palavras sem relação.

### Compreensão da consulta (QU)
A QU é a etapa que infere a intenção de compra por trás das palavras na caixa de busca. Na Instacart, ela pode produzir três tipos de informação: categorias de produtos, que mostram quais partes do catálogo importam; consultas alternativas, que podem encontrar produtos adequados; e tags estruturadas para o produto, a marca ou atributos específicos. A recuperação usa essas saídas para reunir candidatos, e o ranqueamento as usa para ordená-los. A equipe da Instacart já tinha sistemas de machine learning para isso, mas a dificuldade era fazê-los se comportar de forma consistente na enorme variedade de coisas que as pessoas buscam.

### Rótulos supervisionados a partir do comportamento
Um modelo supervisionado aprende com exemplos pareados com a resposta correta, então classificar uma consulta exigiria um rótulo que diga a qual categoria ela pertence. A Instacart consegue observar buscas, cliques e compras, mas nenhum desses sinais mostra diretamente se o sistema entendeu a consulta corretamente. Um comprador que busca "pão" e compra bananas não torna "pão" uma consulta de frutas, então a ligação entre consulta e compra é ruidosa. Avaliadores humanos fornecem rótulos mais limpos, mas custam tempo e dinheiro.

> ❌ **Anti-padrão — aprender rótulos diretamente do comportamento:** tratar cliques e compras como a resposta correta para uma consulta. **Por que as pessoas fazem isso:** são dados que já estão registrados e podem ser usados sem custo. **Consequência:** o modelo aprende o ruído do comportamento de compra, como pão levando a bananas, como se fosse intenção. **Correção:** use o comportamento apenas como evidência para candidatos e depois tenha uma etapa separada de interpretação e validação, como o artigo faz mais adiante.

### A cauda longa
Consultas raras e muito específicas, como "leite com chocolate ultrapasteurizado de 2% de gordura reduzida", têm pouco ou nenhum histórico de compras, então um sistema que aprende principalmente com cliques e compras passados tem exemplos insuficientes para lidar com elas. Cada uma dessas consultas pode representar muito pouco tráfego, mas, somadas, a cauda longa pode representar um grande conjunto de necessidades dos clientes. O artigo volta a esse grupo mais adiante, ao medir os resultados.

### Consultas amplas e ambíguas
Uma consulta ampla como "comida saudável" pode apontar para muitas categorias do catálogo ao mesmo tempo. O ponto do artigo é que o sistema deve lidar com essa ambiguidade, em vez de forçar a consulta em uma interpretação estreita. Esse é um dos motivos pelos quais uma abordagem simples, com um único rótulo, fica aquém.

> ❌ **Anti-padrão — forçar uma consulta ampla em uma única interpretação:** escolher uma categoria para uma consulta cujo significado é amplo. **Por que as pessoas fazem isso:** um único rótulo é mais simples de treinar, avaliar e exibir. **Consequência:** produtos relevantes das outras interpretações nunca chegam à recuperação. **Correção:** devolva várias categorias candidatas e deixe as etapas seguintes ranquearem ou filtrarem.

### Custo de manutenção de modelos separados
A Instacart rodava sistemas diferentes para tarefas diferentes. Um classificador de categorias usava um modelo FastText, e o sistema de reescrita de consultas extraía padrões das sessões dos usuários. Cada sistema tinha seu próprio processamento de dados, treinamento e implantação, o que aumentava o trabalho de manutenção. Mesmo assim, os sistemas davam interpretações inconsistentes para a mesma consulta.

> ❌ **Anti-padrão — um modelo por tarefa de busca:** construir um modelo separado, um pipeline de dados e uma implantação para cada parte da compreensão de consultas. **Por que as pessoas fazem isso:** cada tarefa parece estreita, então um modelo especializado parece eficiente. **Consequência:** a manutenção cresce a cada nova tarefa, e as saídas podem se contradizer para a mesma consulta. **Correção:** mover as tarefas para uma base compartilhada, que é a direção que a equipe da Instacart escolheu.

## Como um LLM genérico se torna útil para a Instacart

Um grande modelo de linguagem começa com um amplo conhecimento de linguagem e do mundo em geral. Por isso ele consegue dar sentido a consultas que nunca viu nos dados históricos. O exemplo do artigo é que um LLM pode já saber que "salsinha italiana" e "salsinha lisa" são a mesma coisa, e que "salsinha crespa" pode substituí-las, sem precisar de exemplos para aprender isso.

Conhecimento geral não é o mesmo que conhecer o catálogo e os clientes de uma loja específica, então a Instacart usou três técnicas para fechar essa lacuna.

### Conhecimento geral pré-treinado
Esse é o ponto de partida descrito no artigo. O treinamento do modelo lhe dá uma noção geral de como os produtos se relacionam, o que ajuda com formulações desconhecidas. O artigo é claro ao dizer que esse conhecimento não informa nada ao modelo sobre o estoque de um varejista nem sobre o comportamento dos compradores, então a empresa precisou acrescentar contexto de negócio por cima.

### Engenharia de contexto e RAG
A primeira técnica coloca as informações de negócio relevantes no prompt antes de pedir uma previsão ao modelo. O artigo chama isso de engenharia de contexto, e o RAG (geração aumentada por recuperação) é o método principal. O sistema puxa registros do catálogo e padrões históricos e os envia ao modelo junto com a consulta. Nas palavras do artigo, o RAG aqui significa dar ao modelo todas as informações úteis de negócio antes de pedir a previsão.

### Validação pós-processamento
A segunda técnica verifica a saída do modelo. A Instacart acrescentou guardrails que testam quão adequada é uma saída e quão bem ela se encaixa no catálogo de produtos. Um modelo pode produzir uma resposta plausível que ainda não corresponde a nenhum produto real, por isso a validação é mantida como uma parte separada do sistema.

> ❌ **Anti-padrão — confiar em saídas plausíveis:** aceitar qualquer categoria ou tag que o modelo produza porque parece correta. **Por que as pessoas fazem isso:** modelos de linguagem são fluentes, então suas respostas parecem confiáveis. **Consequência:** saídas que não correspondem a produtos reais do catálogo chegam à recuperação ou ao ranqueamento. **Correção:** adicione uma camada de validação que confira cada saída com o catálogo e com uma pontuação de relevância, como o artigo descreve.

### Ajuste fino de modelos menores
A terceira técnica é o ajuste fino (fine-tuning), ou seja, um treinamento extra que altera os parâmetros do modelo. Em vez de repetir toda a orientação de domínio no prompt a cada chamada, a equipe ensina um modelo a executar uma tarefa específica a partir de exemplos de alta qualidade. O artigo apresenta isso como o caminho para um modelo menor e mais rápido que ainda carrega o conhecimento de domínio da Instacart. As seções seguintes descrevem como isso foi feito.

> ❌ **Anti-padrão — repetir toda a orientação de domínio em cada prompt:** colocar todas as regras e exemplos do catálogo em cada requisição. **Por que as pessoas fazem isso:** é a forma mais rápida de acrescentar conhecimento sem nenhum treinamento. **Consequência:** cada requisição carrega mais contexto, o que adiciona custo e latência a cada chamada. **Correção:** ensine o comportamento de domínio a um modelo menor com exemplos de alta qualidade, o que o artigo atribui ao ajuste fino. *(O artigo não apresenta isso como anti-padrão; é o contraste implícito em sua redação.)*

## Como a Instacart sabe do que trata a consulta

O catálogo da Instacart é organizado como uma hierarquia de categorias. O caminho de exemplo do artigo é Carnes → Bovina → Costela bovina → Costela curta. Cada etapa do caminho torna a categoria mais específica, então, se o sistema de busca conhece o caminho inteiro, consegue identificar com precisão a parte certa do catálogo.

### Caminhos hierárquicos de categoria
Um caminho de categoria codifica a relação entre um grupo amplo e seus membros mais específicos, algo que uma lista plana de rótulos não faz. O caminho é a unidade que o sistema usa para decidir a qual parte do catálogo uma consulta se refere. Na nova abordagem, o caminho é tratado como uma única previsão e não é dividido em rótulos separados.

> ❌ **Anti-padrão — rótulos de categoria planos:** prever cada categoria como um rótulo independente, com sua própria pontuação de confiança. **Por que as pessoas fazem isso:** classificadores multirrótulo independentes são simples de construir e avaliar. **Consequência:** as relações entre as categorias se perdem. Para "buttermilk", o classificador anterior poderia prever "Laticínios" e "Leite" separadamente, o que é útil, mas incompleto. **Correção:** preveja o caminho completo da categoria para que a hierarquia faça parte da resposta.

### Proposta de candidatos e reranqueamento por LLM
A nova abordagem funciona em duas partes. Primeiro, o sistema encontra as principais categorias que historicamente seguiram a consulta em compras. Elas se tornam o conjunto de candidatos, o que liga o modelo aos próprios dados da Instacart. Como o comportamento de compra pode ser errático, o artigo então pede ao LLM que avalie esses candidatos. O LLM reranqueia os candidatos usando a consulta e o contexto da Instacart que recebeu. Por exemplo, "buttermilk" pode ser mapeado para Laticínios, depois Leite, depois Buttermilk, enquanto "vegan roast" pode ser mapeado para Carnes e depois Substitutos de Carne. Isso mostra como a compreensão da linguagem ajuda quando a formulação da consulta não coincide com nenhum nome de categoria.

### Guardrail de relevância com embeddings
Depois que o LLM faz sua previsão, um guardrail compara o significado da consulta original com o caminho de categoria previsto. Ambos são convertidos em embeddings, que são representações numéricas de texto, e o sistema mede a similaridade entre eles. Se a pontuação ficar abaixo de um limiar definido, o par é descartado. O guardrail é o que remove previsões fluentes, mas sem relação com a consulta.

### Divisão de responsabilidades
O artigo resume as três etapas como tendo funções diferentes. Os dados históricos propõem candidatos, o LLM os interpreta, e o guardrail filtra os resultados irrelevantes no final. Cada parte pode então ser avaliada pela função que cumpre, o que torna o sistema mais fácil de analisar do que um único modelo opaco.

## Como a Instacart torna as consultas mais eficazes

Mesmo uma consulta bem compreendida pode recuperar poucos produtos úteis se as palavras originais forem usadas sozinhas. A reescrita de consultas resolve isso gerando frases alternativas que ajudam a busca a encontrar mais candidatos.

### Reescrita de consultas
O sistema antigo de reescrita usava padrões das sessões dos usuários, mas cobria apenas cerca de 50% do tráfego de busca. A equipe tentou primeiro pedir a um LLM reescritas com um único prompt geral, e isso não funcionou como esperado. A razão, segundo o artigo, é que uma instrução geral de reescrever não diz que tipo de alternativa seria útil. Reescrever "1% milk" como "one percent milk" mantém o significado, mas não oferece nada novo para o comprador encontrar.

> ❌ **Anti-padrão — um único prompt genérico de reescrita:** pedir ao modelo que reescreva a consulta sem dizer que tipo de reescrita se quer. **Por que as pessoas fazem isso:** um único prompt é o design mais simples. **Consequência:** as reescritas costumam ser formulações equivalentes e agregam pouco valor à recuperação. **Correção:** divida a tarefa pelo tipo de mudança desejada, com um prompt dedicado para cada tipo.

### Reescritas de sinônimo, mais amplas e de substituto
A Instacart separou a reescrita em três tarefas, e o artigo explica por que a diferença importa:
- **Reescritas de sinônimo** expressam o mesmo pedido com outras palavras. "One percent milk" é o exemplo, e o significado se mantém.
- **Reescritas mais amplas** removem parte da especificidade, então "milk" é uma reescrita mais ampla de "1% milk". Isso relaxa as restrições.
- **Reescritas de substituto** sugerem um produto diferente que poderia funcionar como alternativa. "2% milk" é o exemplo, e isso muda o produto pedido.

Cada tipo tem seu próprio prompt, com instruções específicas, exemplos e orientação de raciocínio. Depois da geração, a equipe aplica verificações de pós-processamento e filtros semânticos. O resultado foi uma cobertura de reescrita acima de 95%, com precisão acima de 90% nos três tipos.

> 🔎 Contexto adicionado: precisão aqui significa a parcela das reescritas geradas que é considerada correta. Cobertura significa a parcela das consultas para as quais é produzida pelo menos uma reescrita útil.

## Extraindo tags de busca da consulta

A terceira tarefa é a rotulagem de papéis semânticos (SRL), que extrai conceitos estruturados, como produto, marca e outras características, de uma consulta de compras. Para "2% chocolate milk", o produto é leite, e o teor de gordura e o sabor são as outras características. Quando esses conceitos ficam explícitos, o processo de busca consegue usá-los mais facilmente.

A Instacart usa essas tags para recuperação, ranqueamento, publicidade e filtros. O principal desafio é obter tags precisas sem tornar cada busca lenta e cara.

### Consultas principais e cache
O tráfego de busca é distribuído de forma desigual. Um pequeno conjunto de consultas frequentes, chamadas de consultas principais (head queries), responde por uma grande parte do tráfego, enquanto um conjunto muito maior de consultas incomuns ou inéditas forma o restante. Para consultas comuns, a Instacart pode gerar tags antecipadamente e armazená-las em cache, para que buscas posteriores possam usá-las imediatamente.

### Pipeline offline de professor
Pré-computar não basta, porque o número de frases possíveis é praticamente ilimitado e novas combinações não param de aparecer. Por isso, o artigo usa um segundo componente: um pipeline offline de professor e um modelo menor que roda durante a busca ao vivo. "Offline" significa que o trabalho acontece fora da requisição em tempo real que o usuário está esperando, então o pipeline pode usar um modelo poderoso e um contexto mais rico. Para uma consulta comum, ele encontra informações relevantes em sistemas internos, fornece isso ao LLM, gera tags e as valida contra o catálogo.

O exemplo do artigo é "verdant machine". Sem contexto de negócio, um LLM poderia ler isso como maquinário. As evidências internas da Instacart ligam a expressão a uma marca de smoothies chamada MuchPure e às categorias de smoothies e sucos. A saída validada tem duas finalidades: preenche o cache de consultas comuns e se torna dado de treinamento para o modelo menor.

> ❌ **Anti-padrão — pré-computar tags para cada frase possível:** tentar armazenar em cache a saída de todas as consultas com antecedência. **Por que as pessoas fazem isso:** um cache completo tornaria a inferência ao vivo desnecessária. **Consequência:** o cache não acompanha novas combinações de palavras, então consultas não cacheadas ainda precisam de um fallback. **Correção:** armazene em cache as consultas principais e mantenha um modelo rápido para o restante. *(O artigo indica a limitação; o rótulo "anti-padrão" é enquadramento deste resumo.)*

### Modelo aluno online
Quando chega uma consulta ao vivo, o sistema verifica primeiro o cache e devolve as tags armazenadas, se existirem. Caso contrário, a consulta vai para o modelo menor com ajuste fino. O artigo descreve esse modelo como um aluno que aprendeu com exemplos gerados pelo professor, então ele captou como as consultas se mapeiam para tags estruturadas, incluindo padrões do domínio da Instacart.

## Ensinando o modelo menor

Para o seu sistema de SRL em tempo real, a Instacart fez ajuste fino do Llama-3 8B, um modelo com cerca de 8 bilhões de parâmetros. Parâmetros são os valores numéricos que um modelo aprende durante o treinamento. Embora 8 bilhões seja muito, o modelo é bem menor que o modelo de fronteira usado pelo pipeline professor.

### LoRA (Low-Rank Adaptation)
A Instacart treinou o modelo com LoRA. No LoRA, os pesos originais do modelo permanecem fixos, e o modelo aprende um conjunto relativamente pequeno de parâmetros adicionais. Isso reduz o trabalho de treinamento necessário para adaptar o modelo à tarefa. Todos os exemplos de treinamento vêm do pipeline offline.

### Destilação de conhecimento
O artigo descreve isso como uma forma de destilação de conhecimento: um modelo menor aprende com as saídas produzidas por um modelo professor mais capaz. O resultado foi um aluno que produziu menos tags incorretas, mas deixou de fora um pouco mais de tags corretas. O artigo apresenta isso como o trade-off da abordagem, que a equipe aceitou para obter um modelo capaz de atender o tráfego ao vivo.

## Como a Instacart tornou o modelo rápido para a busca ao vivo

A busca ao vivo precisa responder rapidamente, porque atrasos longos são visíveis para os compradores em um site de alto tráfego. A equipe mirou uma latência de servir o modelo na casa das centenas de milissegundos. A primeira configuração medida no artigo levou quase 700 milissegundos em uma GPU A100. Mais adiante, o artigo informa uma meta de 300 milissegundos, que a equipe alcançou.

> 🔎 Contexto adicionado: A100 e H100 são GPUs da NVIDIA para data centers, e a H100 é a geração mais nova. O artigo não dá tempos separados para cada mudança, então apenas o resultado combinado é relatado aqui.

### Fundindo o adaptador LoRA
Depois do treinamento, os ajustes do LoRA podem ser combinados aos pesos do modelo base. Isso elimina a necessidade de manter o adaptador como um cálculo separado durante a inferência. Segundo a equipe de engenharia da Instacart, essa fusão reduziu a latência em cerca de 30%. Combinada com a atualização para GPUs H100, ela atingiu a meta de 300 milissegundos. O ganho total veio tanto da forma como o modelo foi servido quanto do hardware mais rápido.

> ❌ **Anti-padrão — servir o adaptador sem fusão (implícito):** manter o adaptador LoRA como uma etapa separada no momento da inferência. **Por que as pessoas fazem isso:** é a saída padrão do treinamento LoRA e facilita manter o adaptador substituível. **Consequência:** cada requisição faz cálculo extra, o que aumenta a latência. **Correção:** funda o adaptador nos pesos base antes da implantação. *(O artigo relata o efeito da fusão; o rótulo de anti-padrão é uma inferência a partir desse resultado.)*

### Quantização FP8
A quantização usa números de menor precisão para os cálculos do modelo. A equipe testou a quantização FP8, que reduziu a latência em mais 10%, mas diminuiu um pouco o recall. A Instacart implantou o modelo não quantizado porque priorizava resultados de maior qualidade. Embora um modelo mais rápido estivesse disponível, a equipe escolheu a qualidade, e o artigo apresenta isso como um trade-off de engenharia deliberado.

### Inferência em tempo real seletiva
A equipe também reduziu a frequência com que a inferência ao vivo é necessária. O cache teve um grande efeito sobre o custo total e, segundo a estimativa da equipe, apenas 2% das consultas exigiram inferência em tempo real. O modelo menor, portanto, atendeu uma parcela limitada do tráfego, enquanto as tags em cache cobriram os casos comuns. O autoscaling de GPUs então rodou menos GPUs em períodos mais calmos.

## Conclusão

A Instacart avaliou o sistema de SRL em tempo real com um teste A/B. Para as consultas de cauda longa discutidas antes, relatou uma redução de 6% na profundidade média de rolagem. O artigo interpreta isso como os usuários encontrando os itens certos mais rapidamente.

O sistema lidava com milhões de consultas de cold start a cada semana. São consultas sem histórico específico e sem resposta pré-computada. Para as consultas de cauda longa, as reclamações dos usuários sobre resultados ruins caíram 50%, com apenas um aumento marginal de latência. O artigo apresenta esses resultados como a conexão entre os problemas e as soluções: melhor classificação, reescritas e tags ajudam os compradores a encontrar produtos relevantes com mais facilidade.

O artigo termina com o próximo objetivo. A Instacart quer entender a jornada mais ampla do comprador, e o artigo diz que o design atual oferece uma base sólida para isso.

> 🔎 Contexto adicionado: profundidade de rolagem é o quanto, em média, o comprador desce nos resultados. O artigo considera um valor menor como sinal de que os itens certos apareceram mais cedo.

## 🧠 Modelos mentais
### Divisão de trabalho: o histórico propõe, o LLM interpreta, o guardrail filtra
**O modelo:** cada etapa do pipeline de consulta tem uma única função. Os dados históricos fornecem candidatos, o LLM os ranqueia e explica no contexto, e uma verificação por embeddings remove resultados irrelevantes.
**Como aplicar:** ao projetar uma funcionalidade com LLM, decida qual componente propõe opções, qual as julga e qual impõe a restrição rígida.

### Professor offline, aluno online
**O modelo:** um modelo grande com contexto rico produz saídas de alta qualidade fora da requisição em tempo real. Um modelo pequeno treinado com essas saídas responde às requisições ao vivo.
**Como aplicar:** coloque o raciocínio caro em jobs em lote cuja saída é validada e armazenada, e reserve o caminho ao vivo para um modelo que atenda o orçamento de latência.

### Cacheie a cabeça, modele a cauda
**O modelo:** consultas frequentes cobrem a maior parte do tráfego, então seus resultados podem ser pré-computados e armazenados em cache. A cauda longa rara precisa de um modelo que funcione sem nenhum resultado prévio.
**Como aplicar:** meça primeiro a distribuição do tráfego e depois decida qual parte da carga vale a pena cachear e qual precisa de inferência ao vivo.

### O tipo de reescrita deve corresponder à mudança desejada
**O modelo:** um sinônimo mantém a intenção, uma reescrita mais ampla relaxa as restrições, e um substituto muda o produto. Cada um afeta a recuperação de forma diferente.
**Como aplicar:** escreva um prompt separado e uma verificação separada para cada tipo de mudança, em vez de uma única instrução geral de reescrita.

### Troque qualidade por latência apenas de propósito
**O modelo:** uma variante mais rápida, como a quantização FP8, pode reduzir a latência, mas custa um pouco de recall, e esse custo só é aceitável se for uma decisão consciente.
**Como aplicar:** meça latência e qualidade para cada otimização e registre qual perda de qualidade a equipe aceitou.

## 🔑 Principais conclusões
- Logs de comportamento, como cliques e compras, são evidências ruidosas, então a Instacart os usa para propor categorias candidatas, e não como rótulo final.
- Prever o caminho completo da categoria, e não rótulos planos separados, preserva as relações entre as categorias, como Laticínios, depois Leite, depois Buttermilk.
- Dividir a reescrita em tipos de sinônimo, mais ampla e de substituto elevou a cobertura de reescrita acima de 95%, com precisão acima de 90% nos três.
- Um pipeline professor com saídas validadas no catálogo preenche o cache e treina um aluno Llama-3 8B com ajuste fino, que atende as consultas não cacheadas.
- A fusão do adaptador LoRA reduziu a latência em cerca de 30%, e, com GPUs H100, o sistema atingiu a meta de 300 milissegundos.
- Apenas cerca de 2% das consultas precisaram de inferência em tempo real, então o cache levou a maior parte da economia de custo.
- O teste A/B nas consultas de cauda longa mostrou uma redução de 6% na profundidade média de rolagem e de 50% nas reclamações sobre resultados ruins.

## 🏛️ Para Kenji
Esta é a sua interpretação, não a do autor. A arquitetura se encaixa em um design orientado à governança: um LLM propõe e explica, validação determinística e um guardrail por embeddings impõem restrições, e o caminho online é um modelo menor destilado a partir de saídas validadas. Para um Staff Architect, a pergunta útil é que evidências você precisaria para mostrar que a camada de validação continua compatível com o catálogo depois que ele mudar, e como detectaria a deriva do comportamento do modelo aluno em relação às saídas do professor.

## 📚 Plano de leitura
### Do artigo
- [Building the Intent Engine](https://tech.instacart.com/building-the-intent-engine-how-instacart-is-revamping-query-understanding-with-llms-3ac8051ae7ac) — o post original do Instacart Tech Blog, que o artigo cita como fonte dos detalhes do sistema.
### Para se aprofundar
- LoRA: Low-Rank Adaptation of Large Language Models — o método por trás do ajuste fino e da fusão de adaptadores descritos no artigo.
- Distilling the Knowledge in a Neural Network — a origem da ideia de professor e aluno que o artigo aplica à geração de tags.
- Geração aumentada por recuperação — pano de fundo sobre como trazer contexto de negócio para os prompts, o método principal do artigo para ancorar o domínio.
