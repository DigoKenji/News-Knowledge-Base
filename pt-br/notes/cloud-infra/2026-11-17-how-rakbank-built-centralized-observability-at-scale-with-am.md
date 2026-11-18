---
type: Article Summary
title: Como a Rakbank construiu observabilidade centralizada em escala com o Amazon OpenSearch Service
description: A Rakbank distribui logs somente de anexação entre armazenamento quente, UltraWarm e frio no S3 para manter retenção de sete anos com cerca de 55% menos custo do que um cluster todo quente.
resource: https://aws.amazon.com/blogs/architecture/how-rakbank-built-centralized-observability-at-scale-with-amazon-opensearch-service/
tags:
- cloud-infra
- observability
- model-context-protocol
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/how-rakbank-built-centralized-observability-at-scale-with-amazon-opensearch-service/
  title: How Rakbank built centralized observability at scale with Amazon OpenSearch Service
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/how-rakbank-built-centralized-observability-at-scale-with-amazon-opensearch-service/
source: AWS Architecture
source_type: blog
published: '2026-11-17'
domain: cloud-infra
subdomain: observability
concepts:
- model-context-protocol
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d811386fef0fb4afbfe3f
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-17-how-rakbank-built-centralized-observability-at-scale-with-am.md
---

## 🎯 Tese central
A Rakbank opera uma plataforma de logs com Amazon OpenSearch Service entre contas e em Multi-AZ, que ingere cerca de 150 GB de logs por dia e mantém sete anos de retenção ao distribuir os índices entre nós quentes com EBS, UltraWarm e armazenamento frio com S3. Segundo os autores, isso reduz o custo em cerca de 55% em comparação com um cluster todo quente.

## 🧭 Por que o autor escreveu o artigo
O volume de logs da plataforma cresceu de cerca de 5 GB/dia para cerca de 150 GB/dia, com 25 dashboards e cerca de 25 usuários simultâneos, e a conformidade exige sete anos de retenção. O artigo argumenta que um único cluster quente seria caro nessa escala, enquanto um desenho quente/morno/frio com um modelo de dados somente de anexação oferece custo previsível baseado em capacidade, busca rápida nos dados recentes e armazenamento barato de longo prazo. Se a tese se sustentar, o custo de observabilidade deixa de crescer linearmente com o tempo de retenção dos dados.

## Contexto e perfil da carga de trabalho
A carga de trabalho roda como microsserviços em contêineres no Amazon Elastic Kubernetes Service (Amazon EKS), e os requisitos de observabilidade vêm de alguns números concretos. Os logs hoje têm em média cerca de 150 GB por dia, ante cerca de 5 GB/dia antes da migração. Isso resulta em cerca de 9 TB retidos no OpenSearch Service em uma janela de 60 dias (150 GB/dia × 60 dias), com os 30 dias mais recentes pesquisáveis diretamente. A retenção é de sete anos, alinhada aos requisitos do Banco Central dos Emirados Árabes Unidos (CBUAE) e à política interna de retenção de dados da Rakbank. Com 150 GB por dia, os autores estimam cerca de 54 TB de logs arquivados por ano, ou cerca de 383 TB em sete anos. Do lado do consumo, 25 dashboards atendem cerca de 25 usuários simultâneos.

### Logs somente de anexação
A propriedade de dados mais importante do artigo é que os logs são somente de anexação. Depois que um documento de log é gravado, ele nunca é atualizado nem excluído, então a plataforma não tem o custo de atualizações ou de exclusões por consulta. O artigo observa que, quando a regulação exige imutabilidade do tipo WORM (write once, read many), o bucket de arquivo no S3 pode impô-la com o Amazon S3 Object Lock.

> 🔎 Contexto adicionado: armazenamento WORM significa que os dados podem ser gravados uma vez e lidos muitas vezes, mas não modificados nem excluídos durante um período definido. O Amazon S3 Object Lock é o recurso da AWS que impõe isso no nível do bucket ou do objeto.

Os autores tratam o caráter somente de anexação como arquiteturalmente significativo por dois motivos. Primeiro, os índices deixam de receber gravações depois que são rotacionados, então podem ser submetidos a force merge para um único segmento por shard antes de passar para o nível morno, e isso é seguro. Segundo, não há sobrecarga de atualização conforme os índices descem pelos níveis.

## Pipeline de telemetria: Fluent Bit
A equipe segue um princípio que o artigo resume como "instrumente uma vez, padronize em todo lugar". Ela usa um único padrão de Fluent Bit em todos os domínios bancários (varejo, empresas e atacado), de modo que todos os domínios produzem telemetria da mesma forma.

### Fluent Bit como DaemonSet
O Fluent Bit roda como DaemonSet no cluster EKS. Um DaemonSet coloca um pod em cada nó, então o coletor de cada nó lê o stdout e o stderr dos contêineres. Antes do roteamento, ele enriquece cada registro com metadados do Kubernetes: pod, namespace e labels. Esse enriquecimento é o que permite que os usuários filtrem os logs por serviço ou equipe depois.

> 🔎 Contexto adicionado: o Fluent Bit é um processador e encaminhador de logs leve e de código aberto. No Kubernetes, um DaemonSet é a forma padrão de executar um agente por nó.

### Arquivo paralelo com dupla escrita
O Fluent Bit faz uma dupla escrita. Um caminho envia os registros para o endpoint de ingestão do OpenSearch Service, que é o nível quente. Um caminho paralelo grava os objetos de log brutos diretamente no Amazon S3 como um backup durável que não depende da camada de busca. Se o cluster de busca falhar ou for reconstruído, o arquivo bruto continua existindo.

## Topologia entre contas e conectividade privada
O desenho separa a carga de trabalho do seu plano de observabilidade em duas contas da AWS.

- **Conta de aplicação:** o cluster Amazon EKS que executa os pods da aplicação na VPC (Virtual Private Cloud) da aplicação, o DaemonSet do Fluent Bit para coleta e roteamento de logs, e um caminho paralelo de arquivo para o Amazon S3 (o bucket Log Archive and Backup).
- **Conta de observabilidade:** um domínio dedicado de logs centralizados do OpenSearch Service na VPC do OpenSearch Service, distribuído em três Availability Zones, além do OpenSearch Service Dashboards como camada de visualização, roteado para os nós de dados quentes.

### Transporte privado pelo AWS Transit Gateway
O fluxo de logs entre contas passa pelo AWS Transit Gateway, de modo que toda a comunicação permanece privada dentro da AWS e nenhum tráfego sai para a internet pública. Na prática, as tabelas de rotas do Transit Gateway fornecem alcançabilidade entre a VPC da aplicação e a VPC do OpenSearch Service.

> 🔎 Contexto adicionado: o AWS Transit Gateway é um hub que conecta muitas VPCs e contas por meio de uma única camada de roteamento. O tráfego passa pela rede da AWS, e não pela internet pública.

### Acesso de rede restrito
O acesso é limitado na fronteira da VPC por security groups e network ACLs. Eles permitem apenas o tráfego de ingestão necessário, HTTPS na porta 443, para o domínio. O artigo descreve isso como o controle da camada de rede, que fica abaixo dos controles de identidade da próxima seção.

## Fluxo de identidade e autorização entre contas
O artigo descreve o acesso como governado por três camadas do modelo de segurança do OpenSearch Service: isolamento de rede (VPC e conectividade privada pelo Transit Gateway), a política de acesso baseada em recurso do domínio e o controle de acesso refinado (FGAC) no domínio em VPC.

### Single sign-on para pessoas
Os usuários finais acessam o OpenSearch Service Dashboards por SAML 2.0 com single sign-on federado ao Active Directory corporativo da Rakbank. O acesso segue as identidades e os grupos corporativos existentes, então as pessoas não precisam de credenciais separadas do OpenSearch Service.

> 🔎 Contexto adicionado: SAML 2.0 é um padrão de federação baseado em XML que permite que um provedor de identidade, aqui o Active Directory, garanta a identidade dos usuários para um provedor de serviço, aqui o domínio do OpenSearch, sem compartilhar senhas.

### Papéis separados para escrita e leitura
A ingestão de logs da conta de aplicação é autorizada separadamente do acesso dos usuários. O caminho de escrita e o caminho de leitura usam papéis distintos, com privilégio mínimo. O resumo do artigo é que toda a telemetria flui de forma privada dentro da AWS, enquanto o acesso continua governado centralmente no Active Directory.

## Topologia do cluster (papéis de nó por AZ)
O domínio é uma implantação em 3 AZs com nós dedicados de cluster-manager separados dos nós de dados.

| Papel | Instância por AZ | Quantidade | Finalidade |
|---|---|---|---|
| Cluster manager | m8g.large.search | 3 (1/AZ) | Estado do cluster, alocação de shards, sem dados |
| Nó de dados quente | m8g.2xlarge.search | 3 (1/AZ) | Indexação ativa + consulta em tempo real |
| Nó UltraWarm | ultrawarm1.medium.search | 3 (1/AZ) | Somente leitura, segmentos mornos no S3 |
| Nível frio | Sem nós | — | Armazenamento desanexado no S3, gerenciado pelo OpenSearch Service |

### Nós dedicados de cluster-manager
Os nós cluster-manager cuidam do estado do cluster e da alocação de shards, mas não guardam dados. A razão do artigo para separá-los é isolar o gerenciamento do estado do cluster da carga de indexação e de consulta. Os autores dizem que isso importa quando o nível quente recebe cerca de 150 GB/dia.

### Instâncias Graviton
Os nós de dados quentes e de cluster-manager usam instâncias Graviton (m8g), que o artigo escolhe por melhor relação preço/desempenho.

> 🔎 Contexto adicionado: Graviton é a família de processadores baseados em ARM da AWS. O nome "m8g" indica uma família de instâncias Graviton de uso geral.

## Escalonamento de armazenamento: dado certo · armazenamento certo · custo certo
O mecanismo central é o armazenamento multinível do OpenSearch Service. Os índices envelhecem entre os níveis conforme políticas do Index State Management (ISM) baseadas na idade do índice.

| Nível | Base | Retenção | Padrão de acesso | Instância/armazenamento |
|---|---|---|---|---|
| Quente | Volumes Amazon Elastic Block Store (Amazon EBS) (gp3) nos nós de dados | 0–15 dias | Dashboards em tempo real, troubleshooting ativo, alertas | m8g.2xlarge.search |
| Morno (UltraWarm) | Amazon S3 + cache local | 15–30 dias | Análise histórica somente leitura. Otimizado em custo, ainda totalmente pesquisável | ultrawarm1.medium.search |
| Frio | Amazon S3 (desanexado, sem nós) | 30–60 dias | Logs antigos para investigação sob demanda. Anexar ao UltraWarm quando necessário | Baseado em S3, 0 de computação em repouso |

### Nível quente
O nível quente roda em volumes gp3 do EBS anexados aos nós de dados. Ele atende dashboards em tempo real, troubleshooting ativo e alertas, por isso é o único nível em que a plataforma paga por armazenamento local em velocidade total.

### Transição de quente para UltraWarm
O ISM move os índices para o UltraWarm por volta de 15 dias. O UltraWarm mantém os dados dos shards primários no S3 e armazena em cache localmente os blocos quentes. As consultas continuam funcionando, mas com maior latência e a um custo por GB bem menor. Como os índices rotacionados não recebem mais gravações, eles passam por force merge antes da migração, o que melhora o desempenho de leitura no nível morno.

### Transição de UltraWarm para frio
Por volta de 30 dias, os índices são desanexados para o nível frio. O armazenamento frio é S3 puro, sem computação dedicada. Índices frios não são pesquisáveis diretamente. Para consultar um deles, você o anexa de volta aos nós UltraWarm sob demanda. O artigo observa que consultar o arquivo bruto no S3 sem reanexar é um caminho separado, que usa o Amazon Athena.

### Arquivo de conformidade com S3 e Athena
Tudo também é gravado em um bucket separado do Amazon S3, que guarda a janela completa de sete anos. Ele é consultado com o Amazon Athena pelo menor custo por GB. O ponto do artigo é que isso desacopla por quanto tempo os dados precisam ser mantidos de quanto computação quente se paga.

> 🔎 Contexto adicionado: o Amazon Athena é um serviço de consulta SQL sem servidor que lê dados diretamente do S3, então você paga por consulta de dados varridos, e não por computação ociosa permanente.

### Durabilidade sem réplicas nos níveis mornos e frios
O artigo diz que a durabilidade do S3 permite que o desenho use zero réplicas nos níveis morno e frio e ainda assim seja durável, porque o S3 cuida da redundância. Isso reduz ainda mais o custo. O UltraWarm é somente leitura como nível morno, e os Graviton (m8g) alimentam os nós quentes e de cluster-manager.

## Alertas
Os dados do nível quente alimentam os alertas do OpenSearch Service, com e-mail e Microsoft Teams como destinos para alarmes operacionais em tempo real. Os alertas se limitam ao nível quente, o que corresponde ao papel desse nível nas operações em tempo real.

## Dimensionamento de produção e modelo de custo
A meta de dimensionamento é de 150 GB/dia em média com retenção de 60 dias, dos quais 30 dias são diretamente pesquisáveis. O cluster é provisionado com folga para um pico de cerca de 200 GB/dia, e o dimensionamento dos nós decorre desse pico. A Rakbank avaliou duas topologias.

**Opção 1: Quente + UltraWarm (recomendada)**
- 3 × nós de dados quentes m8g.2xlarge.
- 3 × nós ultrawarm1.medium.search mais o nível frio.
- 3 × nós dedicados de cluster-manager m8g.large.
- Cerca de 55% de economia em comparação com o todo quente.

**Opção 2: Todo quente**
- 3 × r8g.4xlarge, cerca de 8.320 GB cada.
- 3 × nós administrativos m8g.large, sem nível morno.
- Custo mais alto, com todos os dados residentes nos nós quentes.

O preço é On-Demand, e as Reserved Instances reduzem ainda mais. A conclusão do artigo é que, em uma janela de 9 TB e 60 dias, a maioria dos dados quase não é consultada depois das primeiras duas semanas. Mover os índices mais antigos para os níveis mornos e frios com base no S3 elimina a necessidade de manter todo o conjunto de trabalho em nós r8g quentes. Os autores atribuem a diferença de cerca de 55% a essa mudança.

## Escalando a arquitetura de observabilidade com o OpenSearch Service
À medida que a plataforma cresceu, a arquitetura precisou atender a três requisitos: custo previsível em alto volume de ingestão, consultas repetidas de dashboards sem acoplamento de custo por consulta e retenção multinível, da consulta em tempo real ao arquivamento de sete anos. O artigo diz que o OpenSearch Service atende a isso com um modelo baseado em instâncias e com o armazenamento quente/morno/frio. A ingestão e as consultas rodam em capacidade provisionada, e os índices mais antigos envelhecem para níveis baseados em S3 com baixo custo por GB. O resultado é um modelo de custo baseado em capacidade que escala de forma previsível conforme crescem o volume de dados e o uso dos dashboards.

## Resiliência: Multi-AZ e o trade-off do Standby
O artigo compara duas opções de implantação.

| Opção de implantação | SLA | Failover | Nós |
|---|---|---|---|
| Multi-AZ (sem Standby) | 99,9% por padrão | automático | 3 AZs · 3 nós de dados + 3 nós dedicados de cluster-manager |
| Multi-AZ com Standby | 99,99% | < 1 min automático em falha de AZ | 3 AZs · 2 de dados ativos + 1 standby |

### Multi-AZ sem Standby
A Rakbank roda seu domínio de produção sem Standby. Os nós de dados ficam distribuídos pelas três Availability Zones, e as réplicas de shards oferecem redundância. Todos os nós de dados continuam atendendo buscas.

### Multi-AZ com Standby
Com o Standby habilitado, apenas dois dos três nós de dados atendem às buscas, e o terceiro fica reservado para o failover. O artigo descreve isso como uma redução de 33% na capacidade ativa de busca em troca de um SLA mais alto. O Standby também exige pelo menos duas réplicas por índice, o que aumenta o armazenamento. Ele é suportado apenas em famílias de instâncias específicas, então o artigo recomenda verificar a lista atual antes de planejar. Se um SLA de 99,99% não for um requisito obrigatório, rodar sem Standby mantém todos os nós de dados atendendo buscas.

## Recursos de observabilidade com IA
O artigo diz que a Rakbank está avaliando recursos mais novos de machine learning e de agentes que podem ser colocados sobre o armazenamento em níveis.

- **AI Assistant:** geração de consultas a partir de linguagem natural, por exemplo "Mostre picos de erro nas últimas 24h", com base no Amazon Bedrock.
- **Servidor MCP e investigador de agentes:** agentes de DevOps executam consultas PPL de forma autônoma para análise de causa raiz. Um Memory Store mantém o contexto entre as conversas dos agentes.
- **Direct Query:** consulta dados do Amazon S3 sem ingeri-los no OpenSearch Service. É uma alternativa ao Amazon Athena para o arquivo no S3.
- **Trace Correlation:** disponível conforme a coleta de traces é adicionada ao pipeline. Unifica logs, traces e métricas em uma única visão, com correlação automática entre traces e logs e mapas de serviço.
- **Superfície de consulta:** PPL, DSL, SQL e PromQL, com integração ao Amazon Managed Service for Prometheus e ao Amazon OpenSearch Ingestion para ingestão de alto desempenho.

> 🔎 Contexto adicionado: PPL (Piped Processing Language) é uma linguagem de consulta em estilo de pipes para o OpenSearch, e DSL se refere ao DSL JSON de consultas do OpenSearch. O artigo apresenta esses recursos como opções, e não como algo que a equipe já adotou.

## Resultados
Rodando sobre o padrão Fluent Bit, a plataforma atende três públicos a partir de uma única visão. As operações de suporte ganham triagem mais rápida e troubleshooting guiado. As equipes de engenharia a usam para análise de causa raiz e depuração ao longo do caminho da requisição. O negócio obtém tendências de jornada e insights acionáveis. Os resultados líquidos do artigo são um menor tempo médio de resolução (MTTR), maior confiabilidade, insights baseados em dados e melhor eficiência operacional. A Rakbank mediu um desempenho mais rápido depois de atualizar entre versões do OpenSearch Service (da 1.3 à atual), com latências de um dígito em milissegundos para consultas lexicais e vetoriais em seu próprio ambiente.

## Lições de engenharia
O artigo termina com quatro lições.

- **Multinível desde o primeiro dia.** Desenhe os níveis quente, morno e frio desde o início. O artigo diz que adaptar a distribuição em níveis a um cluster ativo de 150 GB/dia é bem mais difícil do que começar com as políticas ISM já em vigor.
- **Dimensione para o pico, não para hoje.** A ingestão cresce conforme mais microsserviços entram em operação, então provisione folga no nível quente e na quantidade de shards.
- **Modele explicitamente o trade-off do Standby.** Os números do artigo: 99,99% custa 33% da capacidade ativa de busca e exige duas réplicas por índice. Decida, por carga de trabalho, se esse SLA vale a pena.
- **Explore a vantagem do modelo somente de anexação.** Logs somente de anexação são o candidato recomendado para armazenamento multinível, sem sobrecarga de atualização, com force merge limpo antes da migração para o morno e rotação previsível.

## 🧠 Modelos mentais
### Dado certo, armazenamento certo, custo certo
**O modelo:** cada nível armazena os dados no custo e no desempenho que o padrão de acesso desses dados justifica. Os dados recentes ficam em armazenamento local rápido, e os dados antigos ficam no S3, sem computação.
**Como aplicar:** para cada classe de dados, pergunte por quanto tempo ele é consultado em tempo real, por quanto tempo precisa ser mantido e quem o consulta. Associe cada resposta a um nível e separe retenção de computação quente.

### Dados somente de anexação tornam a distribuição em níveis barata
**O modelo:** quando os dados nunca são atualizados, os índices rotacionados são imutáveis. Isso torna o force merge seguro, evita sobrecarga de atualização conforme os dados descem pelos níveis e mantém a rotação previsível.
**Como aplicar:** antes de escolher um desenho de distribuição em níveis, verifique se os dados são gravados uma única vez. Se forem, inclua o force merge e as transições de nível na política do ciclo de vida.

### Precifique o SLA em capacidade
**O modelo:** um SLA de disponibilidade mais alto pode custar capacidade ativa. Na configuração do artigo, o Standby troca 33% da capacidade de busca por um SLA de 99,99%.
**Como aplicar:** registre o SLA que a carga de trabalho precisa e a capacidade que ela abre mão por isso, e então decida por carga de trabalho, e não por padrão.

## 🔑 Principais conclusões
- Distribuir por idade do índice entre quente, UltraWarm e armazenamento frio baseado em S3 reduziu o custo da Rakbank em cerca de 55% em comparação com um cluster todo quente, segundo o modelo de dimensionamento dos autores.
- Logs somente de anexação tornam seguro o force merge antes da migração para o morno e eliminam a sobrecarga de atualização conforme os índices descem pelos níveis.
- A telemetria entre contas trafega de forma privada pelo AWS Transit Gateway, com security groups e network ACLs permitindo apenas HTTPS na porta 443 para o domínio.
- O acesso é governado separadamente da ingestão. Os usuários entram por federação SAML 2.0 com o Active Directory, e os caminhos de escrita e de leitura usam papéis distintos com privilégio mínimo.
- Multi-AZ com Standby oferece um SLA de 99,99% ao custo de 33% da capacidade ativa de busca e de duas réplicas por índice.
- Desenhar a distribuição em níveis desde o início custa bem menos do que adaptá-la a um cluster em produção.

## 🏛️ Para o Kenji
Esta é uma referência útil para um desenho de logs multiconta com retenção regulada na AWS, especialmente quando os dados são somente de anexação e precisam ser mantidos por anos. Para um arquiteto, a parte reaproveitável é a estrutura de decisão: separar retenção de computação quente, definir explicitamente o trade-off do SLA e manter os caminhos de escrita e de leitura em papéis separados. A pergunta que ela levanta: quais das suas cargas de trabalho têm dados que nunca são atualizados e poderiam ir para níveis baseados em S3, e qual padrão de acesso justificaria manter algum deles quente? _Esta é a sua interpretação, não a do autor._

## 📚 Plano de leitura
### Do artigo
O texto do artigo não cita links, então não há nada a seguir a partir do próprio artigo.

### Para aprofundar
- Documentação do Index State Management (ISM) do Amazon OpenSearch Service — para ver como as políticas de transição entre níveis deste desenho são escritas.
- Documentação de UltraWarm e armazenamento frio do Amazon OpenSearch Service — para conferir a lista atual de instâncias e o comportamento de anexar índices frios sob demanda.
- Documentação de Multi-AZ com Standby do Amazon OpenSearch Service — para verificar as famílias de instâncias suportadas e os requisitos de réplicas antes de planejar uma implantação com Standby.
- Documentação do Amazon S3 Object Lock — para ver como a retenção WORM é imposta no arquivo de conformidade.
