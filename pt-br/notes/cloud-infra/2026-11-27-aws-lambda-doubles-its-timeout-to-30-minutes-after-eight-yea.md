---
type: Article Summary
title: 'AWS Lambda dobra o timeout para 30 minutos após oito anos em 15'
description: A AWS dobrou o timeout do Lambda para 30 minutos, mas filas, streams e limites de concorrência precisam ser verificados antes de o novo valor ser seguro.
resource: https://www.infoq.com/news/2026/11/aws-lambda-30-minute-timeout/
tags:
- cloud-infra
- serverless
- message-queue
- dead-letter-queue
- idempotency
- batch-stream-processing
- scalability
- visibility-timeout
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/aws-lambda-30-minute-timeout/
  title: AWS Lambda Doubles Its Timeout to 30 Minutes after Eight Years at 15
  author: Steef-Jan Wiggers
status: stable
source_url: https://www.infoq.com/news/2026/11/aws-lambda-30-minute-timeout/
source: InfoQ
source_type: blog
published: '2026-11-25'
domain: cloud-infra
concepts:
- serverless
- message-queue
- dead-letter-queue
- idempotency
- batch-stream-processing
- scalability
- visibility-timeout
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/425baf74172d81e88335ee0094f88d93
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-27-aws-lambda-doubles-its-timeout-to-30-minutes-after-eight-yea.md
---

## 🎯 Tese central
A AWS dobrou o timeout máximo das funções de evento on-demand do Lambda, de 15 para 30 minutos, mas as filas, streams, limites de concorrência e de conexões ao redor de cada função determinam se uma execução mais longa é segura.

## 🧭 Por que o autor escreveu
O limite de 15 minutos vigorava desde 2018, e a AWS cita como cargas de trabalho que ultrapassaram esse teto as requisições de IA agêntica que encadeiam chamadas de modelo e de ferramentas, a transcodificação de mídia, as simulações de Monte Carlo e a geração de relatórios. A mudança é uma configuração por função, mas os autores argumentam que tudo o que está conectado à função determina se uma execução mais longa é segura.

## A mudança e onde ela se aplica
O novo limite vale para invocações síncronas, assíncronas e por event source mapping em todas as regiões comerciais, com preço padrão e sem necessidade de adesão (opt-in). O teto de 15 minutos seguiu um limite de 60 segundos no lançamento, em 2014, e depois cinco minutos a partir de 2015.

O Lambda rejeita qualquer valor acima de 1,800 segundos com um erro de validação. O artigo mostra a mudança em uma única chamada de CLI:

```bash
aws lambda update-function-configuration \
    --function-name my-demo-function \
    --timeout 1800
```

**O que isso faz:**
- `aws lambda update-function-configuration` altera as configurações de uma função existente.
- `--function-name my-demo-function` seleciona a função; o nome é o marcador de posição do artigo.
- `--timeout 1800` define o limite em segundos, ou seja, 30 minutos, que é o máximo aceito pelo Lambda.
- A mudança é feita por função, então cada função precisa de sua própria atualização.

A mudança cobre apenas uma das três formas do Lambda. Web Functions mantêm 15 minutos, os Lambda MicroVMs permitem oito horas, e nas Lambda Managed Instances as invocações síncronas passam a ter 30 minutos, enquanto as assíncronas mantêm os 90 minutos introduzidos em setembro.

### Funções serverless
O Lambda é um serviço de funções serverless: você envia o código, a AWS o executa quando um gatilho dispara, e você não provisiona nem gerencia servidores. O provedor escala a capacidade conforme a demanda, então as configurações que você controla são coisas como memória e timeout. O timeout é o tempo máximo que uma invocação pode rodar antes de o Lambda interrompê-la.

### Event source mapping
Um event source mapping é o recurso do Lambda que lê de uma origem, como uma fila ou um stream, e invoca a função com lotes de registros. O novo limite se aplica a esses mapeamentos também. É aqui que o timeout encontra o restante do sistema, que é o tema da terceira seção.

## A capacidade vem primeiro
Os autores apontam a capacidade como a primeira coisa a verificar:

> Concorrência é igual a requisições por segundo multiplicadas pela duração média em segundos. Na mesma taxa de requisições, uma função que roda por 30 minutos em vez de 15 precisa do dobro da concorrência. Por exemplo, 1 requisição por segundo com 30 minutos cada precisa de 1,800 execuções simultâneas.

### Concorrência
Concorrência é o número de execuções rodando ao mesmo tempo. A fórmula do artigo é requisições por segundo vezes a duração média em segundos, então o mesmo tráfego mantido por duas vezes mais tempo exige o dobro de execuções simultâneas. O exemplo dele, 1 requisição por segundo com 30 minutos cada, exige 1,800 execuções simultâneas.

> 🔎 Contexto adicionado: esta fórmula é uma forma da lei de Little, que relaciona a concorrência média, a taxa de chegada e o tempo gasto no sistema.

## O que fica ao redor da função
O restante do post cobre os sistemas ao redor da função:
- Um visibility timeout do SQS precisa ser de pelo menos seis vezes o timeout da função, o que significa três horas com 30 minutos.
- Kinesis e DynamoDB Streams leem um lote por shard por vez, então uma função lenta desacelera o shard.
- Mapeamentos de Amazon MQ, DocumentDB e Kafka criados antes de 19 de novembro permanecem em 14 minutos.
- Um NAT gateway ainda derruba conexões ociosas após 350 segundos.
- A fase Init mantém seu limite de 10 segundos.

### Fila de mensagens
Uma fila de mensagens permite que remetentes coloquem mensagens nela e que consumidores as retirem depois, de modo que os dois lados não precisam estar disponíveis ao mesmo tempo. No Lambda, a fila alimenta a função por meio de um event source mapping, e as configurações da fila precisam combinar com o timeout da função. A próxima seção trata do visibility timeout.

### Processamento batch versus stream
Streams são sequências ordenadas e ilimitadas de registros, e Kinesis e DynamoDB Streams são lidos shard por shard. O ponto do artigo é que a função processa um lote por shard por vez, então uma função que leva mais tempo por lote faz o shard inteiro ficar para trás.

### Gateway NAT
Um NAT gateway permite que recursos privados acessem a internet. O artigo diz que ele derruba conexões ociosas após 350 segundos, então uma função que espera numa conexão parada por mais tempo pode descobrir que ela já não existe.

### Fase Init
A fase Init é o trabalho de inicialização que o Lambda faz antes de o handler rodar. O artigo diz que ela mantém o limite de 10 segundos mesmo com o timeout da invocação dobrado, então trabalho pesado de inicialização continua limitado.

## A regra do SQS, testada
Wibo van der Sluis, um praticante independente de AWS, testou a regra do SQS na eu-west-1. Ele elevou uma função para 1,800 segundos atrás de uma fila com visibility timeout de 90 minutos e depois reduziu a fila para 60 segundos. As duas mudanças foram aceitas com o mapeamento ainda habilitado, enquanto um novo mapeamento a partir da mesma fila foi recusado.

> Então a verificação existe, mas só na criação. Outra coisa que o teste mostrou: na criação, o Lambda comparou a fila com o próprio timeout da função, e não com seis vezes esse valor. Não testei onde exatamente fica esse limite, então trate as seis vezes como uma orientação que você mesmo precisa aplicar.

### Visibility timeout
O visibility timeout é o tempo em que uma mensagem recebida do SQS fica oculta de outros consumidores enquanto um consumidor a processa. Se o processamento passar dessa janela, a mensagem pode voltar a ficar visível e ser entregue uma segunda vez. O artigo pede pelo menos seis vezes o timeout da função, e não explica de onde vem esse fator.

### Dead-letter queue
Uma dead-letter queue recebe mensagens que não puderam ser processadas após um número definido de tentativas, para que deixem de bloquear a fila principal. As novas tentativas multiplicam a espera: van der Sluis observa que uma função assíncrona que atinge o timeout três vezes terá rodado por 90 minutos antes de o evento chegar a uma dead-letter queue.

> ❌ **Anti-padrão — aumentar o timeout para código lento atrás de uma API:** elevar o timeout da função para 30 minutos para que o código lento termine. **Por que as pessoas fazem isso:** é um único valor de configuração por função, como o artigo diz. **Consequência:** van der Sluis conclui que 30 minutos é a correção errada para código lento atrás de uma API. **Correção:** corrigir o código lento; o artigo não oferece outra correção aqui.

> ❌ **Anti-padrão — confiar na verificação da criação:** presumir que o Lambda vai recusar uma combinação insegura de fila e timeout sempre que um dos dois mudar. **Por que as pessoas fazem isso:** existe um erro de validação, o que sugere que a regra é sempre aplicada. **Consequência:** no teste, as duas mudanças foram aceitas enquanto o mapeamento continuou ativo. **Correção:** aplicar a regra dos seis vezes nas suas próprias verificações de implantação.

## Reações de praticantes
Luc van Donkersgoed, principal engineer na NS e AWS Serverless Hero, escreve:

> O inferno congelou. O AWS Lambda agora suporta timeouts de 30 minutos. Não sei bem o que sinto a respeito.

Magnus Eriksson, arquiteto e ex-funcionário da AWS, observa que o Lambda "é de qualquer forma bastante caro por segundo de CPU em comparação com EC2" e ainda o reservaria para cargas curtas e com picos. Matthew Bonig, AWS DevTools Hero e coautor de The CDK Book, olhou mais adiante: "Um dia vai ser de 8 horas, e aí ninguém vai conseguir entender quando usar isso em vez dos MicroVMs."

No Reddit, a resposta mais votada recebeu bem a mudança porque evita Fargate e ECS para jobs em lote de uma hora. A resposta abaixo dela foi que o Fargate é lindamente simples. Um comentarista explica onde o limite maior se encaixa:

> Uma execução de 30 minutos ainda é cobrada pela duração total na memória configurada, então verifique o custo por invocação antes de mover trabalho no estilo batch para cá, e mantenha o handler idempotente com uma dead-letter queue, porque uma falha ou timeout no meio de uma execução longa pode deixar trabalho parcial para trás. Para trabalho em várias etapas, execuções longas ou qualquer coisa que precise de estado claro e retries, Step Functions com Fargate ou ECS ainda costuma ser mais limpo.

### Custo por invocação
O Lambda cobra pelo tempo de uma execução na memória que você configura, então uma execução de 30 minutos custa muito mais por invocação do que uma curta. A observação de Magnus sobre segundos de CPU aponta na mesma direção. O conselho do comentarista do Reddit é verificar o custo por invocação antes de mover trabalho no estilo batch para cá.

### Idempotência
Idempotência significa que repetir uma operação tem o mesmo efeito de executá-la uma única vez. O comentarista do Reddit aconselha manter o handler idempotente, porque uma falha ou timeout no meio de uma execução longa pode deixar trabalho parcial para trás. Esse é um conselho do comentarista, não do artigo.

## Contexto competitivo e questões em aberto
Joseph Emison, CTO da Branch Insurance, lê a mudança como o time do Lambda reconhecendo que o Cloud Run é seu concorrente mais forte. Serviços do Cloud Run permitem 60 minutos por requisição, e o Azure Functions nos planos Flex Consumption e Premium não impõe máximo, com um período de graça de 60 minutos durante o scale-in. A AWS não disse se o limite do Lambda vai mudar de novo.

### Limites do Cloud Run e do Azure Functions
O Cloud Run executa contêineres sob demanda, e a comparação do artigo usa seu limite de 60 minutos por requisição. Os planos Flex Consumption e Premium do Azure Functions não têm um timeout máximo declarado.

> 🔎 Contexto adicionado: scale-in é a remoção de instâncias conforme a carga cai, e o período de graça dá tempo para que o trabalho em execução termine.

## 🧠 Modelos mentais
### A cadeia de timeouts
**O modelo:** O timeout de uma função é um elo em uma cadeia de timeouts, e os elos ao redor precisam ser dimensionados a partir dele. O exemplo do artigo é o visibility timeout do SQS, definido em seis vezes o timeout da função.
**Como aplicar:** Liste cada fila, stream, conexão e limite de inicialização que toca a função, e confira cada um contra o novo valor antes de implantar.

### Concorrência é taxa vezes duração
**O modelo:** A concorrência de execuções é igual à taxa de requisições vezes a duração média em segundos, então uma execução mais longa precisa de capacidade proporcionalmente maior.
**Como aplicar:** Antes de mudar um timeout, multiplique a taxa de requisições esperada pela nova duração e compare o resultado com a concorrência que você realmente consegue executar.

### Retentativas multiplicam a espera
**O modelo:** Retentativas e timeouts se multiplicam. Uma função assíncrona que atinge o timeout três vezes já rodou por 90 minutos antes de seu evento chegar a uma dead-letter queue.
**Como aplicar:** Calcule o tempo até a falha no pior caso, que é o timeout vezes o número de tentativas, e decida se esse atraso é aceitável.

## 🔑 Principais conclusões
- O limite para funções de evento on-demand dobrou para 30 minutos nas invocações síncronas, assíncronas e por event source mapping, sem necessidade de adesão.
- Concorrência é igual a requisições por segundo vezes a duração média, então 1 requisição por segundo com 30 minutos cada precisa de 1,800 execuções simultâneas.
- Os visibility timeouts do SQS devem ser de pelo menos seis vezes o timeout da função, o que significa três horas com 30 minutos.
- Kinesis e DynamoDB Streams leem um lote por shard por vez, então uma função lenta desacelera o shard inteiro.
- O limite dobrado vale para uma das três formas do Lambda: Web Functions mantêm 15 minutos e os MicroVMs permitem oito horas.
- O teste de van der Sluis mostra que a regra da fila é verificada apenas na criação de um mapeamento, então você precisa aplicá-la por conta própria.

## 🏛️ Para o Kenji
*Esta é a sua interpretação, não a do autor.* Na sua trajetória rumo a Staff Architect, o hábito útil aqui é tratar a mudança de timeout como uma decisão de plataforma e mapear cada fila, stream e conexão que a função toca antes de aprová-la. O artigo cita cadeias agênticas de IA, com chamadas de modelo e de ferramentas, como um dos motivos, e esse é exatamente o tipo de fluxo em que um teto mais alto muda o desenho de todo o caminho. Qual timeout nos seus próprios fluxos orientados a eventos ou de IA quebraria primeiro se algum elo fosse elevado?

## 📚 Plano de leitura
### Do artigo
- [AWS Lambda Doubles Its Timeout to 30 Minutes after Eight Years at 15](https://www.infoq.com/news/2026/11/aws-lambda-30-minute-timeout/) — o artigo original; o texto extraído não contém outros links, então leia-o para ver as reações e o teste do SQS.

### Para aprofundar
- Visibility timeout do Amazon SQS e sua interação com event source mappings do Lambda — para ver de onde vem a orientação dos seis vezes.
- Lei de Little — a identidade de filas por trás da fórmula de concorrência.
- Step Functions com Fargate ou ECS — a alternativa que um comentarista do Reddit recomenda para trabalho de várias etapas e longa duração.
- Desenho de handler idempotente para funções orientadas a filas — como tornar seguro repetir trabalho parcial.

## Conceitos
- [serverless](/pt-br/concepts/cloud-infra/serverless.md)
- [message-queue](/pt-br/concepts/backend-systems/message-queue.md)
- [dead-letter-queue](/pt-br/concepts/backend-systems/dead-letter-queue.md)
- [idempotency](/pt-br/concepts/backend-systems/idempotency.md)
- [batch-stream-processing](/pt-br/concepts/data/batch-stream-processing.md)
- [scalability](/pt-br/concepts/backend-systems/scalability.md)
- [visibility-timeout](/pt-br/concepts/backend-systems/visibility-timeout.md)
