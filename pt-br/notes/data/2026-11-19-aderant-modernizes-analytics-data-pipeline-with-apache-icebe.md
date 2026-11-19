---
type: Article Summary
title: Aderant moderniza o pipeline de dados analíticos com Apache Iceberg e AWS Lambda
description: Dividir tabelas gold pela complexidade das consultas permite que um pipeline Iceberg orientado a eventos entregue análises quase em tempo real, com custo menor.
resource: https://aws.amazon.com/blogs/architecture/aderant-modernizes-analytics-data-pipeline-with-apache-iceberg-and-aws-lambda
tags:
- data
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/aderant-modernizes-analytics-data-pipeline-with-apache-iceberg-and-aws-lambda
  title: Aderant modernizes analytics data pipeline with Apache Iceberg and AWS Lambda
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/aderant-modernizes-analytics-data-pipeline-with-apache-iceberg-and-aws-lambda
source: AWS Architecture
source_type: blog
published: '2026-11-19'
domain: data
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d810b8b6bc06a421b2fb8
lang: pt-br
translation_of: /notes/data/2026-11-19-aderant-modernizes-analytics-data-pipeline-with-apache-icebe.md
---

## 🎯 Tese central
Dividir tabelas gold pela complexidade das consultas e executá-las em um pipeline medallion baseado em Iceberg e orientado a eventos entrega análises quase em tempo real para um produto SaaS multi-tenant, com consultas mais rápidas e custo de pipeline menor do que as alternativas que o autor mediu.

## 🧭 Por que o autor escreveu
O Stridyn, ferramenta de análise da Aderant, atende 700 ambientes de clientes, e seu pipeline em lote levava cerca de 15 minutos por ciclo, então os clientes viam as mudanças operacionais com atraso ao tomar decisões de faturamento e de gestão de casos jurídicos. O post, escrito em coautoria com a Aderant, descreve a reformulação e os benchmarks que a sustentam. Se a tese se confirmar, outras equipes de análise que rodam SaaS multi-tenant podem chegar a uma atualidade de minutos sem reescrever SQL complexo e sem adotar streaming como padrão.

## O desafio: processamento em lote limitando a atualidade da análise
O Stridyn atende 700 ambientes de clientes que executam o Expert Sierra, o produto de gestão de escritórios de advocacia em nuvem da Aderant. Ele lê de 266 tabelas de origem em bancos Microsoft SQL Server e entrega relatórios financeiros, análises de faturamento e dashboards operacionais para escritórios de advocacia em todo o mundo.

### Ciclo de processamento em lote
Um ciclo completo de processamento levava aproximadamente 15 minutos, e a sobrecarga dos jobs em lote somava latência antes de a transformação começar. A atualidade fica limitada pela duração do ciclo: uma mudança feita logo após o início de um ciclo espera o ciclo inteiro para chegar à análise.

> ❌ **Anti-padrão — Ciclos fixos em lote para dados sensíveis à atualidade:** Executar todo o pipeline em uma agenda fixa, com sobrecarga de lote antes de qualquer transformação começar. **Por que as pessoas fazem isso:** Não declarado no artigo. **Consequência:** Os clientes sentem atrasos entre as mudanças operacionais e a visibilidade na análise. **Correção:** Ingestão orientada a eventos, em que cada novo arquivo dispara o processamento das próprias mudanças.

### Disponibilidade em tempo real para os clientes
Os clientes sentiam atrasos entre as mudanças operacionais e a visibilidade na análise, o que afetava decisões de faturamento e de gestão de casos sensíveis ao tempo. O objetivo declarado é ter os dados acessíveis em minutos, da origem ao destino, sem mudar a experiência do usuário final.

### Consultas complexas lentas em tabelas externas
Relatórios que usam funções de janela, junções entre várias tabelas fato e expressões de tabela comum (CTEs) eram lentos quando consultavam tabelas externas. Esses formatos de consulta depois orientam o desenho das 34 tabelas gold complexas.

### Escala do volume de CDC
A arquitetura tinha dificuldade com dados personalizados, mudanças de esquema e um volume crescente de eventos de captura de dados de alteração (CDC), que já chegavam a 380 milhões por mês somando todos os tenants.

## A solução: arquitetura medallion orientada a eventos com Apache Iceberg
O novo pipeline tem três camadas, bronze, silver e gold, todas construídas sobre tabelas Apache Iceberg armazenadas no Amazon S3. O AWS Lambda cuida do processamento de CDC orientado a eventos, o AWS Glue cuida das transformações, e o Amazon Redshift serve as análises finais.

### Arquitetura medallion
Uma arquitetura medallion organiza os dados em camadas bronze, silver e gold, e cada camada é construída a partir da anterior. A bronze recebe os payloads de CDC, a silver guarda registros normalizados do estado atual e a gold guarda agregações moldadas para os relatórios.

> 🔎 Contexto adicionado: Os nomes bronze, silver e gold são rótulos convencionais; o artigo descreve o que cada camada guarda, e não define o termo.

### Tabelas Apache Iceberg no Amazon S3
> 🔎 Contexto adicionado: Apache Iceberg é um formato aberto de tabelas cujos metadados registram quais arquivos de dados compõem cada versão da tabela, o que permite commits e snapshots sobre arquivos em armazenamento de objetos.

O artigo usa Iceberg nas três camadas, de modo que um único formato governa ingestão, transformação e agregações gold. O Amazon S3 guarda os dados, e o AWS Glue Data Catalog registra os metadados das tabelas em todas as camadas.

### Papéis de Lambda, Glue e Redshift
O Lambda processa eventos individuais de CDC para a bronze, o Glue executa transformações incrementais baseadas em conjuntos entre as camadas, e o Redshift serve os dashboards. Escritas pequenas por evento vão para funções, enquanto as transformações vão para jobs Spark.

## Como funciona: o ciclo de vida de uma mudança de dados
O artigo acompanha uma mudança desde o banco de origem até o dashboard em cinco etapas.

### Captura e entrega do CDC
> 🔎 Contexto adicionado: A captura de dados de alteração lê o fluxo de mudanças de um banco, para que inserções, atualizações e exclusões sigam para jusante à medida que acontecem.

O AWS Database Migration Service (AWS DMS) captura as mudanças dos bancos Microsoft SQL Server e grava arquivos Parquet em uma zona de pouso no S3, organizada por tabela e por tenant. Essa organização por tabela e tenant é o que depois permite que o processamento seja feito por tenant sem pipelines separados.

### Ingestão orientada a eventos (raw para bronze)
Uma notificação de evento do S3 envia uma mensagem ao Amazon Simple Queue Service (Amazon SQS) quando um arquivo chega, e essa mensagem invoca uma função Lambda. A função processa o payload de CDC e grava nas tabelas Iceberg da bronze em poucos segundos por evento. O SQS absorve picos de tráfego e oferece filas de mensagens mortas (dead-letter queue) para eventos que falham.

### Transformação de bronze para silver
Jobs do AWS Glue leem apenas os dados incrementais das tabelas bronze, usando watermarks mantidos no Amazon DynamoDB. Um watermark registra até onde o processamento avançou, para que a próxima execução comece desse ponto. Os jobs aplicam normalização e deduplicação por meio de operações MERGE do Iceberg, produzindo tabelas silver do estado atual com aderência a ACID.

> 🔎 Contexto adicionado: Uma instrução MERGE atualiza as linhas que atendem a uma condição e insere as demais, em uma única operação.

### Agregação de silver para gold
A camada gold usa duas abordagens, escolhidas pela complexidade da tabela. Jobs incrementais do AWS Glue atendem 77 tabelas gold padrão, e visões materializadas do Iceberg com comparação de snapshots atendem 34 tabelas com padrões de consulta complexos, que, segundo o autor, não podem ser facilmente tornadas incrementais.

### Entrega da gold para o Amazon Redshift
Os changesets delta das snapshots das visões materializadas são gravados em tabelas nativas do Amazon Redshift, onde o armazenamento colunar, os zone maps e as chaves de ordenação entregam desempenho de consulta abaixo de 100 milissegundos para os dashboards dos usuários finais.

> 🔎 Contexto adicionado: Zone maps guardam os valores mínimo e máximo por bloco, para que o motor possa pular blocos, e as chaves de ordenação organizam os dados no disco para tornar esses pulos eficazes.

## Por que Apache Iceberg
A Aderant escolheu o Apache Iceberg como formato de tabela em todas as camadas por três motivos.

### Transações ACID para operações concorrentes
Com 700 tenants gerando eventos de CDC ao mesmo tempo, o isolamento por snapshot do Iceberg ajuda a evitar conflitos quando várias funções Lambda gravam nas mesmas tabelas.

> 🔎 Contexto adicionado: Isolamento por snapshot significa que cada leitor vê uma versão consistente da tabela, e que cada commit fica visível de uma só vez.

### Viagem no tempo e comparação de snapshots
Para as 34 tabelas gold complexas, os metadados de snapshot do Iceberg identificam os deltas sem controle manual de watermark. O sistema compara snapshots consecutivos para descobrir exatamente quais linhas mudaram e então grava apenas esses deltas no Amazon Redshift. O título fala em viagem no tempo, mas o uso descrito no corpo é a comparação de snapshots.

### Evolução de esquema sem interromper o pipeline
À medida que a Aderant integra novas fontes de dados e os clientes personalizam seus esquemas, o Iceberg lida com adição de colunas e mudanças de tipo sem reconstruir as tabelas inteiras. O artigo apresenta isso como uma forma de manter o pipeline funcionando durante mudanças de esquema, mas não dá números para isso.

## Por que AWS Lambda para o processamento de CDC
Depois de avaliar três abordagens arquiteturais, a equipe escolheu o Lambda por custo, latência e simplicidade operacional. O artigo não nomeia as outras duas abordagens.

### Computação serverless com cobrança por invocação
O modelo de cobrança por invocação combina com cargas de CDC, em que o volume de eventos oscila ao longo do dia. A capacidade chega a zero em períodos tranquilos e absorve picos sazonais de 10x sem capacidade pré-provisionada, de modo que o custo acompanha o volume real de dados, e não a infraestrutura reservada.

### Implantação por imagem de contêiner Docker
As funções Lambda são implantadas como imagens de contêiner Docker por meio do Amazon Elastic Container Registry (Amazon ECR). O modelo de contêiner acomoda as grandes dependências de bibliotecas das operações Iceberg, incluindo PyIceberg e pandas, que seriam difíceis de empacotar de outra forma.

## Arquitetura
A legenda da figura diz: "Figure 1: End-to-end data flow through the medallion architecture, from source databases to the Amazon Redshift serving layer." (Tradução livre: "Figura 1: Fluxo de dados de ponta a ponta pela arquitetura medallion, dos bancos de origem à camada de serviço do Amazon Redshift.")

> 🔎 Contexto adicionado: A imagem da Figura 1 não está no texto extraído, então o fluxo abaixo é descrito a partir dos parágrafos ao redor.

### Camada de origem e buffer SQS
O AWS DMS replica eventos de CDC do Microsoft SQL Server para uma zona de pouso no S3 como arquivos Parquet. Quando os arquivos chegam, as notificações de eventos do S3 publicam mensagens em uma fila SQS, que absorve picos e guarda eventos com falha em uma fila de mensagens mortas.

### Ingestão na bronze com contêineres Lambda
As funções Lambda, implantadas como contêineres Docker a partir do Amazon ECR, consomem mensagens do SQS e gravam tabelas Iceberg da bronze no S3. Elas usam a biblioteca PyIceberg para gravações alinhadas a ACID. O DynamoDB armazena o estado do watermark para acompanhar o progresso e ajudar a evitar gravações duplicadas.

### Orquestração agendada com EventBridge
O Amazon EventBridge dispara uma função Lambda de decisão a cada 5 minutos, que verifica se os jobs Glue de bronze para silver devem rodar, com base na disponibilidade dos dados. Os jobs do AWS Glue 5.0 (Apache Spark 3.5 com Python 3.11) então leem a bronze de forma incremental, aplicam operações MERGE e gravam a silver. O fluxo do artigo é orientado a eventos na ingestão, mas dirigido por agendamento da bronze em diante.

### Caminhos de silver para gold
O EventBridge detecta a conclusão dos jobs de bronze para silver e dispara uma segunda função Lambda de decisão para as 77 tabelas gold padrão. Essa função inicia jobs Glue incrementais que gravam nas tabelas nativas do Redshift por meio da Amazon Redshift Data API. Para as 34 tabelas complexas, as visões materializadas do Iceberg executam a transformação em SQL, e um job Glue separado compara os snapshots para gravar os changesets delta no Redshift.

### Camada de serviço do Redshift Serverless
O Amazon Redshift Serverless guarda tabelas nativas com armazenamento colunar, zone maps e chaves de ordenação, com latência de consulta abaixo de 100 milissegundos. Uma camada de API de dados GraphQL fica entre o Redshift e os dashboards, servindo 42 relatórios pré-construídos, incluindo demonstrações financeiras, análises de faturamento e dashboards de visão geral do escritório.

### Observabilidade, segurança e entrega
O Amazon CloudWatch oferece observabilidade de ponta a ponta, com métricas personalizadas de latência de processamento, taxas de erro e throughput por tenant. O AWS Identity and Access Management (IAM) aplica acesso de privilégio mínimo, e os dados do S3 são criptografados no lado do servidor pelo AWS Key Management Service (AWS KMS). A infraestrutura é definida em Terraform e implantada por meio de pipelines de integração contínua e entrega contínua (CI/CD) do GitHub Actions.

## Visões materializadas do Iceberg: resolvendo o problema da consulta complexa
As 34 tabelas gold complexas envolvem funções LAG e de janela, junções entre várias tabelas fato, UNIONs com agregações e CTEs. O processamento incremental tradicional exigiria redesenhar a lógica de cada consulta, então a equipe usa visões materializadas do Iceberg como etapa intermediária.

> ❌ **Anti-padrão — Reescrever SQL analítico complexo para torná-lo incremental:** Redesenhar a lógica de cada consulta complexa para que ela possa ser processada de forma incremental. **Por que as pessoas fazem isso:** Não declarado no artigo; o impulso é que o processamento incremental parece exigir isso. **Consequência:** Cada consulta complexa precisa de um redesenho próprio, que o artigo diz ser exigido pelo processamento incremental tradicional nessas 34 tabelas. **Correção:** Materializar a consulta como uma visão materializada do Iceberg e extrair os deltas a partir dos snapshots.

### Visões materializadas do Iceberg
> 🔎 Contexto adicionado: Uma visão materializada armazena o resultado de uma consulta, e atualizá-la renova esse resultado armazenado.

A equipe cria as visões materializadas com Spark SQL padrão (CREATE MATERIALIZED VIEW AS SELECT ...) sobre as tabelas Iceberg da silver. O artigo descreve isso como um processo de cinco etapas:

- Criar a visão materializada do Iceberg sobre as tabelas da camada silver.
- O Iceberg acompanha as mudanças nas fontes automaticamente, sem controle manual de watermark.
- Um job do AWS Glue atualiza a visão a cada 2–5 minutos, contornando o intervalo mínimo de 1 hora da atualização automática.
- A extração de deltas compara os snapshots do Iceberg entre as atualizações.
- Somente as linhas alteradas são gravadas no Amazon Redshift como atualizações de tabela nativa.

### Comparação de snapshots para deltas
A visão cuida da complexidade computacional, e a comparação de snapshots identifica o que mudou. Juntas, elas mantêm o processamento incremental sem reescrever a consulta analítica, que é o argumento central do artigo para essa abordagem.

## Resultados quantificáveis
O artigo relata ganhos em três dimensões: desempenho de consulta, latência do pipeline e custo de infraestrutura.

### Desempenho de consulta
A equipe avaliou 16 consultas analíticas, de agregações simples a pivôs complexos sobre 9 milhões de linhas, comparando tabelas nativas do Redshift com tabelas externas consultadas pelo Redshift Spectrum. As tabelas nativas foram 10–26x mais rápidas em todos os tipos de consulta, retornando resultados consistentemente em menos de 100 milissegundos, independentemente do tamanho do conjunto de dados. O maior ganho foi de 25.8x em conjuntos de dados menores e acessados com frequência, e consultas de pivô complexas com funções de janela ainda ganharam 13.7x.

Os benchmarks rodaram no Redshift Serverless com 16 RPU. O autor espera que clusters provisionados, com computação dedicada e cache persistente, tenham desempenho melhor, mas isso não foi medido.

#### Tabelas nativas do Redshift
As tabelas nativas guardam os dados em formato colunar, com zone maps e chaves de ordenação, e por isso superaram as tabelas externas neste benchmark. O artigo atribui o ganho a esses recursos de tabela e ao desenho da camada de serviço.

#### Amazon S3 Express One Zone
A equipe também validou o S3 Express One Zone para cargas que permanecem em tabelas Iceberg baseadas em S3, sem o Redshift. Ele reduziu a latência de consulta em 31–78% em comparação com o S3 Standard, com os maiores ganhos em consultas complexas que envolvem várias varreduras de tabela.

### Latência do pipeline
A maior melhoria operacional é o tempo de ponta a ponta, que caiu de aproximadamente 15 minutos por ciclo completo de processamento para poucos segundos, da origem até as tabelas Iceberg da bronze. As atualizações da camada gold são concluídas em menos de 5 minutos nas cargas iniciais e em menos de 1 minuto nas atualizações incrementais. O sistema sustenta throughput de mais de 186,000 eventos por segundo em todas as 266 tabelas e 700 tenants.

### Custo de infraestrutura
Escolher o processamento de eventos baseado em Lambda em vez de alternativas de streaming reduziu o custo de computação do pipeline em 77%. O modelo serverless, com cobrança por invocação, escala até zero em períodos tranquilos e absorve picos sazonais de 10x sem capacidade pré-provisionada.

> ❌ **Anti-padrão — Streaming como padrão para todos os tenants (implícito na comparação):** Enviar os eventos de CDC de todos os tenants por um pipeline de streaming. **Por que as pessoas fazem isso:** Não declarado no artigo. **Consequência:** A comparação do artigo implica um custo de computação do pipeline maior, medido pela diferença de 77%. **Correção:** Manter os tenants padrão no Lambda e mover apenas os tenants de maior volume para streaming, como o artigo planeja.

## Em produção: operações multi-tenant
A arquitetura roda em produção hoje, processando dados de 700 ambientes de clientes em 266 tabelas de origem. Três aspectos se destacam.

### Isolamento de tenants com infraestrutura compartilhada
Os eventos de CDC de cada tenant passam pelas mesmas funções Lambda e pelos mesmos jobs do AWS Glue. A estrutura de particionamento por tenant do Iceberg mantém o isolamento dos dados, e arquivos de configuração controlam o comportamento por tenant sem mudanças de código.

### Tratamento cuidadoso de relatórios complexos
O Expert Bill Report envolve várias tabelas de dimensão e agregações de fatos. Ele processa 900,000 registros em aproximadamente 5 minutos na carga inicial, e as execuções incrementais seguintes tratam apenas os registros alterados, em segundos.

### Gestão de watermarks autorrecuperável
Os watermarks do DynamoDB acompanham o último timestamp processado por tabela e por tenant. Se uma função Lambda falhar, o watermark permanece inalterado, e a próxima invocação reprocessa a partir do ponto correto. O autor diz que isso oferece semântica de exatamente uma vez (exactly-once) sem lógica complexa de transações distribuídas.

> 🔎 Contexto adicionado: Semântica exactly-once significa que cada mudança tem efeito uma única vez, mesmo quando o processamento é repetido.

## Perspectivas para o futuro
A Aderant planeja continuar expandindo a ferramenta. Estes são planos, não funcionalidades descritas como já construídas.

### Análise de streaming para tenants de alto volume
A arquitetura Lambda suporta um modelo híbrido. Tenants com os maiores volumes de eventos podem migrar para o Amazon Kinesis, com latência de sub-segundo, enquanto os tenants padrão permanecem no caminho Lambda, de custo mais baixo.

### Análises com IA
O plano integra o Amazon Bedrock para consultas em linguagem natural sobre a camada gold, de modo que os usuários dos escritórios de advocacia possam fazer perguntas sobre dados de faturamento e de casos de forma conversacional.

### Qualidade de dados automatizada
O framework de validação será ampliado para monitorar continuamente a consistência dos dados entre as camadas do pipeline, com alertas automáticos quando forem detectadas discrepâncias.

## Conclusão
A jornada saiu do processamento orientado a lotes para um pipeline orientado a eventos, baseado em Iceberg. Os eventos de CDC agora se movem em poucos segundos, em vez de ciclos em lote de 15 minutos. O desempenho de consulta é 10–26x melhor graças às tabelas nativas do Redshift, e os custos de infraestrutura são 77% menores do que os de alternativas de streaming.

A principal percepção é que nem toda tabela precisa da mesma estratégia de processamento. A Aderant usa jobs incrementais do AWS Glue para tabelas diretas e visões materializadas do Iceberg com comparação de snapshots para padrões complexos. Isso entrega atualidade quase em tempo real em todas as 111 tabelas gold (77 mais 34) sem reescrever a lógica de SQL complexo.

A conclusão também apresenta o alinhamento ACID, as capacidades de viagem no tempo e a evolução de esquema do Iceberg como uma base sólida para pipelines de produção em escala. A introdução promete um framework de decisão, mas o corpo não tem uma seção separada para ele; os equivalentes mais próximos são a divisão em dois caminhos na gold e os três motivos para o Iceberg.

A conclusão também aponta para a documentação do Glue com Iceberg, as páginas de serviço do Lambda e do Glue e uma arquitetura de referência do AWS Samples, todas listadas no plano de leitura abaixo.

## Sobre os autores
O texto extraído inclui este título sem conteúdo, então não há o que expandir.

## 🧠 Modelos mentais
### Dividir o pipeline pela complexidade da tabela
**O modelo:** Nem toda tabela precisa da mesma estratégia de processamento. Classifique cada tabela derivada pela dificuldade de tornar sua consulta incremental e então escolha o mecanismo mais barato que se encaixe.
**Como aplicar:** Antes de construir um caminho de atualização, separe as tabelas em grupos padrão e complexos, e reserve visões materializadas com diferenças de snapshot para o grupo complexo.

### O watermark como único registro de progresso
**O modelo:** Um timestamp por tabela e por tenant é o único registro do que já foi processado. Uma execução com falha o deixa inalterado, então a próxima execução retoma do ponto certo.
**Como aplicar:** Avance o watermark somente depois que a gravação tiver sucesso, e faça com que cada job leia apenas os dados posteriores a ele.

### Pagar por evento, não por capacidade reservada
**O modelo:** Para cargas de CDC com picos, o custo deve acompanhar o volume de eventos e cair para perto de zero em períodos tranquilos.
**Como aplicar:** Use computação com cobrança por invocação para os tenants padrão e reserve capacidade de streaming para os de maior volume, como o artigo planeja.

## 🔑 Principais conclusões
- Trocar o ciclo em lote de 15 minutos por ingestão orientada a eventos, baseada em CDC, trouxe as mudanças para as tabelas Iceberg da bronze em poucos segundos.
- A camada gold usa duas estratégias: jobs incrementais do Glue para 77 tabelas padrão e visões materializadas do Iceberg com comparação de snapshots para 34 tabelas complexas, cobrindo todas as 111 sem reescrever SQL complexo.
- A comparação de snapshots do Iceberg identifica linhas alteradas sem controle manual de watermark, de modo que apenas os deltas são gravados no Redshift.
- No benchmark do autor, com 16 consultas no Redshift Serverless com 16 RPU, as tabelas nativas foram 10–26x mais rápidas do que as tabelas externas do Spectrum.
- Ao processamento Lambda com cobrança por invocação foi atribuída uma redução de 77% no custo de computação do pipeline em relação a alternativas de streaming; o artigo não dá valores absolutos de custo.
- Watermarks por tabela e por tenant no DynamoDB permitem que uma execução Lambda com falha retome do ponto correto, sem lógica de transação distribuída.
- O isolamento multi-tenant vem do particionamento por tenant sobre caminhos de código compartilhados de Lambda e Glue, com arquivos de configuração controlando o comportamento por tenant.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor: a divisão em dois caminhos na gold é uma regra reutilizável para arquitetos que projetam plataformas de dados para produtos multi-tenant, em que cada tabela derivada é classificada pela complexidade da consulta antes de se escolher seu mecanismo de atualização. Os números de desempenho e custo (10–26x, 77%) são relativos, sem linhas de base absolutas, então qualquer reuso precisaria de medições próprias. A pergunta que isso levanta: como a alegação de exatamente uma vez se sustenta quando uma gravação tem sucesso mas a atualização do watermark falha, e o que você testaria para confirmar?

## 📚 Plano de leitura
### Do artigo
- [Aderant modernizes analytics data pipeline with Apache Iceberg and AWS Lambda](https://aws.amazon.com/blogs/architecture/aderant-modernizes-analytics-data-pipeline-with-apache-iceberg-and-aws-lambda) — o post original, com o diagrama de arquitetura da Figura 1 que esta nota só pode descrever em texto.

> 🔎 Contexto adicionado: Os links citados no artigo não têm URLs no texto extraído, então eles estão listados abaixo pelo título.

### Para aprofundar
- Using Apache Iceberg tables in AWS Glue — o ponto de partida recomendado pelo autor para Iceberg na AWS.
- AWS Lambda and AWS Glue service documentation — a documentação dos dois serviços dos quais a arquitetura mais depende.
- AWS Samples repository on GitHub — a arquitetura de referência do autor, com código de exemplo.
