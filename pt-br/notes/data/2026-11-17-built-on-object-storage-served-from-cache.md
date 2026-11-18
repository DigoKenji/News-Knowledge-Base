---
type: Article Summary
title: Construído sobre object storage, servido a partir de cache
description: O Lakebase Postgres atende 99.99999% dos acessos a páginas a partir de caches locais, então o object storage fica fora do caminho das consultas.
resource: https://www.databricks.com/blog/built-object-storage-served-cache
tags:
- data
- databases
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://www.databricks.com/blog/built-object-storage-served-cache
  title: Built on object storage, served from cache
  author: Databricks
status: stable
source_url: https://www.databricks.com/blog/built-object-storage-served-cache
source: Databricks
source_type: blog
published: '2026-11-17'
domain: data
subdomain: databases
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d81bba857cafc95625f28
lang: pt-br
translation_of: /notes/data/2026-11-17-built-on-object-storage-served-from-cache.md
---

## 🎯 Tese central
Um banco de dados construído sobre object storage não precisa ler do object storage para responder consultas. No Lakebase Postgres, 99.99999% dos acessos a páginas são atendidos a partir de caches locais no nó de computação ou nos pageservers.

## 🧭 Por que o autor escreveu
A objeção comum é que o Postgres faz muitas leituras pequenas e sensíveis à latência, o object storage leva centenas de milissegundos por leitura, e, portanto, um Postgres construído sobre object storage tem que ser lento. O artigo aceita a premissa e rejeita a conclusão, que só se sustenta se toda leitura for ao object storage. Se a tese se confirmar, o object storage pode oferecer durabilidade, armazenamento praticamente ilimitado, branches e restaurações instantâneas e uma única cópia dos dados compartilhada com a análise, sem onerar a latência das consultas.

## A objeção e onde ela falha
### O argumento comum
O Postgres processa muitas leituras pequenas sensíveis à latência. O object storage é feito para requisições maiores com latência mais alta, e uma leitura dele pode levar centenas de milissegundos. A objeção conclui que o design precisa ser lento. A resposta do artigo é que isso pressupõe que toda consulta precisa tocar o object storage, e o Lakebase Postgres quase nunca faz isso.

## Medindo onde as leituras realmente caem
Os autores amostraram estatísticas de leitura de páginas em uma fatia da sua frota de produção. As leituras são divididas em camadas, e cada leitura de página é atendida pela primeira camada que contém a página.

### Camada 1: o cache local do nó de computação
Essa camada atendeu cerca de 1.1 bilhão de acessos a páginas por segundo, aproximadamente 99.46% do total. Essas leituras nunca saem do nó de computação, então custam memória ou latência de microssegundos, o mesmo que um cache aquecido em um Postgres tradicional.

### Camada 2: NVMe local do pageserver
Em uma falha no cache local, a leitura atravessa a rede até um pageserver. Essa camada atendeu cerca de 6 milhões de leituras por segundo, aproximadamente 0.54% do total. O pageserver responde diretamente do seu próprio disco NVMe.

### Camada 3: object storage
Apenas cerca de 39 requisições por segundo chegaram ao object storage. Isso equivale a aproximadamente 1 em 28 milhões de acessos a páginas, então mais de 99.99999% nunca precisam dele. O artigo diz que essas leituras raras são casos especiais, não o caminho normal.

## O que construir sobre object storage traz
### Durabilidade
O object storage, como o S3, é projetado para 99.999999999% de durabilidade. Assim, o estado durável não depende de nenhuma máquina específica.

### Armazenamento sem provisionamento
O armazenamento cresce junto com os dados. Não há discos para dimensionar nem risco de ficar sem espaço.

### Histórico imutável torna branches e restaurações baratos
O histórico é mantido em arquivos de camadas imutáveis, então criar branches e restaurações instantâneas são operações de metadados que não exigem cópia de dados. Um branch é um ponteiro para um LSN. A restauração pontual e a restauração a partir de snapshot criam um novo branch em um LSN, em vez de reescrever o atual.

### Computação stateless
O estado durável fica fora do nó de computação, então a computação pode escalar para zero e iniciar de novo sem mover dados.

### Uma cópia para transações e análise
Quando os dados operacionais estão no lake, os motores analíticos podem ler a mesma cópia por meio do LTAP.

## A restrição de design: manter o object storage fora do caminho da consulta
O artigo afirma que a velocidade é a parte com a qual você precisa se preocupar ao construir sobre object storage. A camada de armazenamento é organizada para que o object storage nunca fique na frente de uma consulta. Os dados quentes ficam em caches locais no nó de computação e nos pageservers, e o object storage apenas dá suporte a essas camadas.

> ❌ **Anti-padrão — object storage no caminho da consulta:** cada leitura de página vai direto ao object storage, então cada consulta espera por uma requisição remota. **Por que as pessoas fazem isso:** o artigo não diz; uma única camada é o design mais simples. **Consequência:** cada leitura leva centenas de milissegundos, que é a premissa do artigo para explicar por que um banco desse tipo seria lento. **Correção:** coloque caches no nó de computação e nos pageservers, para que o object storage só seja acessado em uma falha rara.

## Como a camada de armazenamento é dividida
O artigo observa que essa arquitetura é diferente o bastante do Postgres convencional para formar uma categoria própria, o lakebase. O design desacopla computação e armazenamento.

### Camada de computação
A camada de computação roda o Postgres padrão e mantém as páginas quentes em um cache local. Ela não tem estado durável, então pode iniciar, parar, escalar ou falhar sem colocar dados em risco.

### Safekeepers
Os safekeepers replicam o WAL. Uma transação é confirmada assim que um quórum de safekeepers reconhece o seu registro. Um protocolo baseado em Paxos cuida da eleição de líder e do failover.

> 🔎 Contexto adicionado: Paxos é um protocolo de consenso que permite a um grupo de nós concordar com um único valor mesmo quando alguns nós falham. WAL é o write-ahead log, o registro ordenado de alterações que o Postgres grava antes de aplicá-las.

### Pageservers
Os pageservers transformam o WAL em páginas. Eles constroem uma LSM tree em NVMe local e persistem o histórico materializado no object storage de forma assíncrona.

> 🔎 Contexto adicionado: uma LSM tree é uma estrutura otimizada para escrita que armazena as gravações em memória e as mescla em arquivos ordenados no disco. Histórico materializado, aqui, são versões de páginas já calculadas a partir do WAL.

### Object storage
O object storage guarda o histórico imutável de longo prazo e dá suporte aos pageservers. O Postgres nunca lê dele diretamente.

## O caminho de leitura, passo a passo
### GetPage@LSN
Quando o Lakebase Postgres precisa de uma página, ele verifica primeiro o cache local do nó de computação. Em uma falha, envia um GetPage@LSN a um pageserver. A requisição informa uma página e um LSN, e o pageserver devolve a página como ela existia naquele LSN.

> 🔎 Contexto adicionado: um LSN (log sequence number) é uma posição no WAL. Pedir uma página em um LSN significa pedir a versão daquela página até aquele ponto do log.

Em quase todos os casos, o pageserver já tem as camadas de que precisa no próprio NVMe. Só se não tiver é que ele busca camadas no object storage.

### Cache em todas as camadas
Cada camada guarda em cache o que busca na camada abaixo. Uma página devolvida por um pageserver vai para o cache do nó de computação. Camadas buscadas no object storage vão para o NVMe do pageserver. O custo de uma camada mais profunda é pago uma vez, e a próxima leitura dos mesmos dados fica local.

## Camadas de imagem e de delta mantêm o replay curto
O pageserver organiza os dados em dois tipos de arquivo de camada.

### Camadas de imagem
Uma camada de imagem guarda um snapshot de todas as páginas de um intervalo de chaves em um LSN.

### Camadas de delta
Uma camada de delta guarda os registros do WAL de um intervalo de chaves ao longo de um intervalo de LSNs.

### Reproduzindo deltas na leitura
Para responder a um GetPage@LSN, o pageserver começa na página e no LSN pedidos e desce pelas camadas, coletando os registros do WAL daquela página. Ele para na primeira imagem da página e então reproduz os deltas coletados por cima dessa imagem. O número de deltas é a medida de quanto trabalho uma leitura exige. Com zero deltas, a página vem direto de uma imagem. Com muitos, o pageserver reproduz uma longa cadeia do WAL antes de poder responder.

### Números da frota para replay
Na frota us-east-1, o p90 ficou abaixo de 1 delta por GetPage, então a maioria das requisições não precisa de replay. O p99 ficou em torno de 6. Ele caiu para esse nível depois de uma mudança que cria páginas-base (imagens) com mais frequência. Com imagens mais próximas dos LSNs que as leituras pedem, menos leituras precisam percorrer uma cadeia longa para reproduzir.

## Por que algumas leituras chegam ao object storage
Só uma pequena fração das leituras do pageserver vai além. As cerca de 39 requisições por segundo que chegam ao object storage têm uma característica comum: pedem dados que ninguém leu recentemente. Na prática, isso acontece em um de dois casos.

### Branches antigos e restaurações
Um branch é um ponteiro para um LSN. Quando um ponto de restauração está muito no passado, esse histórico pode não estar no NVMe do pageserver, então as primeiras leituras o buscam no object storage.

### Bancos inativos que acordam
Um banco que escalou para zero por semanas não tem mais os dados em cache. Suas primeiras leituras depois de acordar vão ao object storage.

### Só as primeiras leituras pagam
Nos dois casos, apenas as primeiras leituras pagam o custo. Depois disso, os dados ficam em cache nas camadas mais rápidas e seguem o mesmo caminho local de qualquer carga ativa. Durante a execução ativa de consultas, o object storage não está no caminho crítico.

## 🧠 Modelos mentais
### Cada camada guarda em cache a camada de baixo
**O modelo:** o custo de uma camada mais profunda é pago uma vez, porque cada camada mantém o que busca da que está abaixo dela.
**Como aplicar:** ao dimensionar um armazenamento em camadas, pergunte se uma falha preenche o cache da camada acima. Se preenche, avalie a cauda apenas pelo custo do primeiro acesso.

### Avalie um sistema em camadas pela taxa de queda
**O modelo:** a métrica útil é a parcela das requisições que cai em cada camada mais profunda, e não a latência de pior caso da camada mais lenta.
**Como aplicar:** amostre contagens de atendimento por camada em produção, como fizeram os autores, e verifique se a parcela da camada lenta é mínima.

### O custo do replay é o tamanho da cadeia de deltas
**O modelo:** em um armazenamento de páginas baseado em log, o custo de uma leitura é o número de deltas do WAL reproduzidos sobre a imagem mais próxima.
**Como aplicar:** acompanhe percentis de deltas por leitura, como p90 e p99. Se a cauda crescer, crie imagens-base com mais frequência.

## 🔑 Principais conclusões
- A cache local do nó de computação responde a cerca de 99.46% dos acessos a páginas. O NVMe do pageserver responde a cerca de 0.54%. Cerca de 1 em 28 milhões chega ao object storage.
- O Lakebase Postgres é projetado para que o object storage nunca fique na frente de uma consulta. Ele apenas dá suporte aos pageservers.
- A computação não guarda estado durável, então pode iniciar, parar, escalar ou falhar sem risco para os dados.
- O reconhecimento por quórum de safekeepers é o que confirma uma transação.
- Como o histórico é mantido em arquivos de camadas imutáveis, branches e restaurações instantâneas são operações de metadados, sem cópia de dados.
- Camadas de imagem mantêm o replay curto. O p90 fica abaixo de um delta por GetPage, e o p99 fica em torno de seis.
- As leituras no object storage vêm sobretudo de branches de pontos antigos no tempo e de bancos que acordam depois de escalar para zero, e só as primeiras leituras pagam esse custo.

## 🏛️ Para Kenji
Esta é a sua interpretação, não a do autor. Para um arquiteto que decide se uma camada de dados baseada em object storage serve a uma carga de trabalho, a verificação útil é medir separadamente a taxa de queda e o caminho de partida a frio. A latência em regime estável segue as camadas superiores, enquanto restaurações e despertares seguem a cauda rara. A pergunta que isso levanta: em qual taxa de queda os inícios a frio ficariam visíveis para os seus usuários, e como você mediria isso?

## 📚 Plano de leitura
### Do artigo
- [arquitetura do lakebase](https://docs.databricks.com/aws/en/oltp/projects/architecture#how-a-read-works) — as camadas de leitura, com a sequência exata que uma leitura de página segue.
- [arquitetura do lakebase](https://docs.databricks.com/aws/en/oltp/projects/architecture) — os componentes desacoplados de computação e armazenamento, na íntegra.
- [branches](https://docs.databricks.com/aws/en/oltp/projects/branches) — como um branch é mapeado para um LSN.
- [restauração pontual (point-in-time restore)](https://docs.databricks.com/aws/en/oltp/projects/point-in-time-restore) — o mecanismo de restauração descrito no artigo.
- [escalar a zero (scale to zero)](https://docs.databricks.com/aws/en/oltp/projects/scale-to-zero) — o caminho de despertar que envia as primeiras leituras ao object storage.
- [Building on object storage](https://www.databricks.com/product/lakebase) — o contexto de produto do design.
- [From monolith to Lakebase to LTAP](https://www.databricks.com/blog/lakebase-ltap-rethinking-database-storage) — o argumento de cópia compartilhada para análise.
- [WAL + S3: Lakebase storage for the era of agents](https://neon.com/blog/wal-s3-lakebase-storage-for-the-era-of-agents) — um texto complementar sobre WAL e object storage.
- [A New Era of Databases: Lakebase](https://www.databricks.com/blog/what-is-a-lakebase) — a definição da categoria lakebase.

### Para se aprofundar
- Write-ahead logging no PostgreSQL — o pano de fundo para registros do WAL, LSNs e replay.
- Log-structured merge trees — o caminho de escrita por trás do armazenamento dos pageservers.
- Paxos e commit por quórum — o consenso por trás do failover dos safekeepers.
