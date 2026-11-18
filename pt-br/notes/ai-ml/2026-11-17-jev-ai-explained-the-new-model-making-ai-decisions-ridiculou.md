---
type: Article Summary
title: 'Jev AI explicado: o novo modelo que torna decisões de IA absurdamente baratas'
description: Jev é um modelo de decisão estruturada cuja baixa latência e preço tornam baratas muitas pequenas decisões de IA.
resource: https://medium.com/@anulagarwal12/jev-ai-explained-the-new-model-making-ai-decisions-ridiculously-cheap-24de2a6b7d37
tags:
- ai-ml
- llm-engineering
- structured-decision-model
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://medium.com/@anulagarwal12/jev-ai-explained-the-new-model-making-ai-decisions-ridiculously-cheap-24de2a6b7d37
  title: 'Jev AI Explained: The New Model Making AI Decisions Ridiculously Cheap'
  author: Anul Agarwal
status: stable
source_url: https://medium.com/@anulagarwal12/jev-ai-explained-the-new-model-making-ai-decisions-ridiculously-cheap-24de2a6b7d37
source: Medium
source_type: medium
published: '2026-09-20'
domain: ai-ml
subdomain: llm-engineering
concepts:
- structured-decision-model
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81879d26f4d4e6e9d194
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-jev-ai-explained-the-new-model-making-ai-decisions-ridiculou.md
---

# Jev AI explicado: o novo modelo que torna decisões de IA absurdamente baratas

**Fonte:** https://medium.com/@anulagarwal12/jev-ai-explained-the-new-model-making-ai-decisions-ridiculously-cheap-24de2a6b7d37
**Autor:** Anul Agarwal
**Data:** 2026-09-20
**Conceitos:** [Modelo de decisão estruturada](/pt-br/concepts/structured-decision-model.md)

---

## 🎯 Tese central
Jev é um modelo de decisão estruturada que devolve respostas que o seu software pode usar diretamente, e não texto gerado. Sua velocidade e seu preço baixo tornam viável executar muitas pequenas decisões de IA que um modelo de linguagem de propósito geral tornaria caras ou lentas.

## 🧭 Por que o autor escreveu
Desenvolvedores que testam o Jev relatam custos muito baixos para grandes lotes de decisões, como milhares de requisições por alguns dólares. O autor pergunta o que muda para um app ou produto se decisões recorrentes em segundo plano (qual mensagem é urgente, para onde vai uma tarefa, o que um agente faz em seguida) ficam quase gratuitas e rápidas, e conclui que a pergunta útil é quando evitar um modelo generativo caro, não se o Jev supera todas as IAs.

## Introdução (trecho inicial, sem título no original)
O artigo abre com a pergunta "O que acontece quando milhares de decisões de IA custam apenas alguns dólares?". Ele cita dois relatos de desenvolvedores: um rodou cerca de 5.000 requisições por US$ 2, e outro classificou 500 e-mails em segundos por 3,5 centavos. O autor enquadra isso como uma possível oportunidade de negócio: construir algo útil, cobrar pelo valor e ficar com mais de cada venda, em vez de pagá-la como conta de IA. Em seguida, ele faz a distinção-chave. O Jev não é um chatbot que compete pela qualidade da resposta. Ele é feito para as decisões tomadas nos bastidores, ou seja, qual mensagem precisa de atenção, para onde deve ir uma tarefa e o que um agente deve fazer em seguida.

## Primeiro, o que é exatamente o Jev AI?
O Jev é um modelo de IA da TypeSafe AI projetado para tomar decisões estruturadas em vez de gerar texto. A TypeSafe o chama de modelo System One. Você envia informações sobre uma situação junto com perguntas, e ele devolve respostas que o seu software pode usar diretamente.

O artigo ilustra com uma mensagem de cliente: "Os pagamentos estão falhando há 3 dias. Os clientes não conseguem comprar nada. Por favor, ajudem." Antes de responder, a sua aplicação precisa saber quão urgente isso é e quem deve tratar o caso. É esse o trabalho que o Jev visa. No exemplo semelhante de ticket de suporte da TypeSafe, a resposta de urgência é 0,999, o que significa uma probabilidade estimada de 99,9% de que a mensagem expresse urgência. A sua aplicação pode usar esse número para priorizar o ticket, enquanto outro modelo escreve a resposta.

O Jev aceita três tipos de pergunta: Choice, Score e Noul. O artigo diz que a tabela de tipos de pergunta é uma imagem e não define Noul além de afirmar que ele devolve uma probabilidade entre 0 e 1. Choice e Score também devolvem distribuições de probabilidade e valores de confiança.

O autor insiste que você não está pedindo ao Jev que invente uma resposta do zero. Você está definindo a decisão para a qual a sua aplicação precisa de ajuda.

> 🔎 Contexto adicional: "distribuição de probabilidade", aqui, é um conjunto de probabilidades sobre as opções permitidas, como 0,7 para uma categoria e 0,3 para outra. O valor de confiança é derivado dessa distribuição, como o artigo explica mais adiante.

## Por que não usar simplesmente um modelo de IA comum?
Você pode. O autor reconhece que modelos de linguagem gerais já lidam com classificação e respostas estruturadas. A proposta não é que essas tarefas fossem impossíveis antes. É que um modelo criado especificamente para elas consegue lidar com certas decisões em menos tempo e com menos custo do que um modelo de linguagem de propósito geral. O artigo observa que a LangChain descreve o Jev como um complemento aos modelos generativos, não como substituto.

O contraste oferecido é entre decidir se um cliente precisa de reembolso e escrever uma resposta cuidadosa que explique a política de reembolso. Ambas as tarefas exigem entender a mensagem, mas uma saída é uma decisão que a aplicação executa, e a outra é linguagem. A distinção importa mais quando o software tem milhares de pequenas decisões para tomar.

## Por que os desenvolvedores estão prestando atenção
### 1. As decisões podem acontecer em uma fração de segundo
A TypeSafe informa tempos de resposta de cerca de 70 a 500 milissegundos, ou 0,07 a 0,5 segundo. O modelo produz saídas estruturadas em paralelo, em vez de gerar a resposta token por token. Para uma pergunta, um segundo economizado pode não parecer significativo. O exemplo hipotético do autor deixa o ponto claro: um assistente que precisa de 10 decisões sequenciais de 2 segundos cada passa 20 segundos esperando, enquanto com 0,2 segundo cada o total é de 2 segundos. As decisões individuais são pequenas, mas o atraso acumulado não é.

As próprias avaliações de fluxo de trabalho da TypeSafe relatam um desempenho 193,6 vezes mais rápido e um custo 444,6 vezes menor. São números informados pela empresa para comparações específicas. A própria TypeSafe diz que eles provavelmente estão no extremo superior dos ganhos do mundo real. Eles não significam que toda aplicação fique 200 vezes mais rápida. O autor ainda considera que vale explorar a passagem de decisões repetidas de segundos para frações de segundo.

### 2. O custo torna experimentos maiores mais fáceis de justificar
A partir de 19 de setembro de 2026, o Jev 1.13 custa US$ 0,042 por milhão de tokens de entrada, sem cobrança por tokens de saída. Tokens são as pequenas unidades usadas para medir o texto enviado ao modelo. O artigo assume que cada requisição contém 1.000 tokens de entrada cobrados, cobrindo as informações e as perguntas. A tabela de custos também é uma imagem no original.

> 🔎 Contexto adicional: os próprios números do artigo dão 100.000 requisições × 1.000 tokens = 100 milhões de tokens de entrada, e 100 milhões de tokens × US$ 0,042 por milhão = US$ 4,20, que é o número usado pelo artigo.

O artigo é explícito ao dizer que esses são cálculos a partir do preço listado, não contas medidas de aplicações reais. Entradas mais longas custam mais, e hospedagem, outros modelos e ferramentas externas são despesas adicionais. Ainda assim, o autor sugere que você pode avaliar milhares de mensagens ou documentos sem que o modelo responda por quase toda a conta do experimento.

### 3. Várias perguntas podem compartilhar a mesma requisição
Considere uma mensagem de suporte que precisa de cinco verificações: categoria, urgência, gravidade, se um reembolso é solicitado e se o cliente incluiu os passos para reproduzir um bug. O Jev pode responder às cinco em uma única requisição. A TypeSafe diz que as perguntas rodam em paralelo, então acrescentar perguntas normalmente adiciona pouca ou nenhuma latência, embora o texto extra das perguntas ainda aumente o custo de entrada.

O artigo define um limite importante. Cada pergunta avalia apenas as informações fornecidas. As perguntas não podem ler as respostas umas das outras. Uma pergunta que exige um novo resultado de busca precisa esperar até que essa busca tenha acontecido. O benefício está em evitar esperas desnecessárias entre decisões independentes, não em pular o trabalho necessário para respondê-las.

## O que as pessoas estão de fato construindo?
O artigo apresenta as primeiras demonstrações como resultados relatados por desenvolvedores, não como números garantidos para toda implementação.

### Assistentes de navegador que escolhem a próxima ação
A Browser Use demonstrou um agente encontrando resultados de voos em cerca de 7 segundos, com um custo de modelo relatado de US$ 0,0039. Esse valor exclui os custos do navegador, e o cronômetro não inclui a observação inicial da página nem uma verificação final após a execução. O agente encontrou resultados de voos, mas não concluiu uma reserva. O design interessante é que o Jev escolhe uma ação entre os controles disponíveis na página, enquanto um pequeno modelo de linguagem cuida da geração de texto quando é preciso digitar.

### Organizando mais de 1.000 artigos de pesquisa
Um desenvolvedor, Hassan, relatou a classificação de 1.018 artigos de pesquisa em IA por US$ 0,08, com latência mediana de classificação de ponta a ponta de 256 milissegundos por artigo. O fluxo usou outro modelo para resumir cada artigo primeiro. Depois, o Jev recebeu cada título, o resumo e 24 tópicos possíveis, e escolheu entre eles. Os US$ 0,08 cobrem apenas a classificação, não o pipeline de pesquisa inteiro. A lição que o autor tira é separar as funções: um modelo prepara o material, e outro o organiza.

### Escolhendo qual modelo de IA deve tratar uma tarefa
A integração da LangChain usa o Jev para rotear requisições entre modelos segundo critérios definidos pelo desenvolvedor. Uma tarefa simples pode ir para um modelo mais barato, e uma tarefa exigente para um modelo mais capaz. O autor considera isso uma das aplicações mais práticas, porque você não está substituindo o modelo poderoso. Você está sendo mais seletivo sobre quando usá-lo.

## Onde está a oportunidade de ganhar dinheiro?
Os exemplos mostram custos de execução baixos, mas não estabelecem quanto alguém já ganhou. A conclusão do autor é que custos menores de decisão podem tornar certos pequenos produtos mais viáveis. O exemplo é uma ferramenta de caixa de entrada para pequenas empresas que organiza mensagens, sinaliza problemas urgentes de clientes e envia os casos incertos para revisão. Outro é um serviço de organização de documentos construído sobre uma ideia parecida, ou o roteamento dentro de um app de IA já existente, para que nem toda requisição vá para o modelo mais caro. O autor é explícito: são produtos potenciais, não negócios garantidos.

O cliente ainda precisa de um motivo para pagar. "Usa o Jev" é fraco. "Me ajuda a encontrar os problemas de clientes que preciso resolver hoje" é um motivo claro. O ângulo que o autor exploraria é um problema estreito e útil, com uma decisão repetida no centro. Custos menores poderiam permitir cobrar menos, oferecer mais uso ou ficar com mais receita depois das despesas, mas a oportunidade depende de o produto funcionar de forma confiável e resolver algo que as pessoas valorizam.

## O Jev torna a IA melhor do que todas as outras?
Não, e o autor diz que essa não é a comparação mais útil. O Jev não escreve o artigo, não gera a aplicação nem substitui o raciocínio aberto. O papel dele é tratar decisões estruturadas ao lado dos modelos que fazem essas outras tarefas.

O Jev também pode errar. Uma resposta pode seguir o formato exigido e ainda ser um julgamento errado, já que escolher uma categoria permitida não é o mesmo que escolher a certa. Os valores de confiança ajudam os desenvolvedores a decidir quando pedir revisão, mas precisam ser lidos corretamente. Para Choice e Score, a confiança é calculada a partir da distribuição de probabilidade, e uma confiança de 0,85 não significa simplesmente uma chance de 85% de estar certo. O limiar adequado depende da tarefa e do custo dos erros. O autor se sentiria confortável experimentando com categorias sugeridas de e-mail muito antes de deixar um modelo aprovar reembolsos ou apagar arquivos automaticamente.

## Como eu começaria
O autor começaria com uma decisão e 100 exemplos, não com um negócio autônomo. O Playground da TypeSafe permite colar texto, definir perguntas e inspecionar os resultados antes de construir uma integração. O quickstart oferece opções de configuração por API, SDK e agentes de codificação.

Para um experimento inicial, pegue mensagens de suporte revisadas manualmente e peça ao Jev que as categorize. Compare as respostas, inspecione os erros e meça o tempo e o custo do fluxo completo. O objetivo não é reproduzir a manchete de "200 vezes mais rápido". É verificar se essa decisão específica fica mais barata e mais rápida sem introduzir erros que anulem o benefício.

A conclusão do autor é que o valor não está em um novo modelo substituindo tudo. Está em uma parte útil de uma aplicação que talvez não precise mais de um modelo caro e de propósito geral a cada execução. Às vezes você precisa da IA para escrever a resposta. Às vezes você só precisa que ela decida o que acontece em seguida.

## 🧠 Modelos mentais

### Decidir versus escrever
**O modelo:** separe a tarefa que precisa de uma decisão que a sua aplicação pode executar da tarefa que precisa de linguagem. A primeira pode ir para um modelo estruturado, a segunda para um modelo generativo.
**Como aplicar:** para cada chamada de IA em um pipeline, pergunte se a saída é um rótulo, uma pontuação ou uma probabilidade, ou um texto em prosa. Direcione rótulos e pontuações para o modelo especializado mais barato.

### Separar a preparação da organização
**O modelo:** um modelo prepara o material (por exemplo, resumindo) e outro toma a decisão estruturada sobre ele.
**Como aplicar:** insira uma etapa de resumo ou extração antes de um classificador quando a entrada bruta for longa ou ruidosa demais para um modelo de decisão rápida.

### Problema estreito, decisão repetida no centro
**O modelo:** um produto funciona quando é construído em torno de uma decisão repetida que os usuários valorizam, e não em torno da tecnologia.
**Como aplicar:** antes de construir, nomeie a decisão que o usuário precisa tomar muitas vezes por dia e quanto uma resposta correta vale para ele.

### Comece com uma decisão e 100 exemplos
**O modelo:** valide uma única decisão em um pequeno conjunto de exemplos revisados antes de escalar.
**Como aplicar:** rotule 100 casos reais à mão, rode o modelo sobre eles, inspecione cada erro e meça o tempo total e o custo de todo o fluxo, não apenas a chamada ao modelo.

## 🔑 Principais conclusões
- O Jev visa decisões estruturadas (escolha, pontuação, probabilidade) em vez de texto gerado, e devolve resultados que o seu software pode usar diretamente.
- A TypeSafe informa tempos de resposta de 70 a 500 ms, com saídas produzidas em paralelo, e o exemplo do próprio artigo mostra como pequenos atrasos por decisão se acumulam em etapas sequenciais.
- Ao preço listado do Jev 1.13, de US$ 0,042 por milhão de tokens de entrada, as requisições de 1.000 tokens assumidas pelo artigo custam US$ 4,20 por 100.000 decisões.
- Várias perguntas independentes sobre a mesma entrada podem compartilhar uma requisição, mas as perguntas não podem ler as respostas umas das outras, e perguntas dependentes precisam esperar suas entradas.
- Os valores de confiança são derivados de uma distribuição de probabilidade e não são o mesmo que a chance de estar certo, então os limiares de revisão precisam ser definidos para cada tarefa.
- As comparações informadas pela empresa, de 193,6 vezes em velocidade e 444,6 vezes em custo, valem para avaliações específicas e não representam um ganho de velocidade geral.

## 🏛️ Para o Kenji
Para um arquiteto, o Jev sugere uma divisão no pipeline: um modelo estruturado rápido para roteamento, triagem e classificação, e um modelo generativo apenas onde se produz prosa, com um roteador escolhendo entre modelos mais baratos e mais capazes (o padrão da LangChain citado no artigo). A pergunta que ele levanta é como você mediria se uma decisão mais barata continua correta diante do custo dos seus erros, usando os seus próprios exemplos rotulados. Esta é a sua interpretação, não a do autor.

## 📚 Plano de leitura
### A partir do artigo
O artigo não cita links, então não há URLs para seguir a partir dele. Ele menciona a TypeSafe AI, a LangChain, a Browser Use e o TypeSafe Playground sem endereços.

### Para ir além
- O Playground e o quickstart da TypeSafe, como citados no artigo, para testar uma decisão com o seu próprio texto antes de qualquer integração: útil para o experimento de 100 exemplos que o autor propõe.
- A integração de roteamento de modelos da LangChain, como descrita no artigo, para o padrão de roteamento entre modelos mais baratos e mais capazes.
- Calibração de confiança para classificadores (como as pontuações de probabilidade e de confiança se mapeiam para acurácia real), necessária para definir limiares de revisão nas saídas de Choice e Score.
- Contabilidade de custos para fluxos de LLM com várias etapas (latência por etapa e cobrança de tokens por requisição), necessária para medir o fluxo completo que o artigo recomenda.
