---
type: Article Summary
title: Como a Country Road Group construiu uma solução de integração orientada a padrões na AWS
description: Substituir middleware da era BizTalk por serviços nativos da nuvem que implementam padrões de integração diretamente reduziu em 80 por cento o custo de integração de um varejista.
resource: https://aws.amazon.com/blogs/architecture/how-country-road-group-built-a-pattern-driven-integration-solution-on-aws/
tags:
- architecture
- integration-and-software-architecture
- idempotency
- saga-pattern
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/how-country-road-group-built-a-pattern-driven-integration-solution-on-aws/
  title: How Country Road Group built a pattern-driven integration solution on AWS
  author: AWS Architecture Blog
status: stable
source_url: https://aws.amazon.com/blogs/architecture/how-country-road-group-built-a-pattern-driven-integration-solution-on-aws/
source: AWS Architecture
source_type: blog
published: '2026-11-19'
domain: architecture
subdomain: integration-and-software-architecture
concepts:
- idempotency
- saga-pattern
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81ceb69de35a42f4d863
lang: pt-br
translation_of: /notes/architecture/2026-11-20-how-country-road-group-built-a-pattern-driven-integration-so.md
---

## 🎯 Tese central
Middleware de integração legado pode ser substituído por serviços nativos da nuvem que implementam os mesmos padrões de integração diretamente, provisionados de forma declarativa a partir de código, sem nenhum runtime central de middleware entre os sistemas que estão sendo conectados.

## 🧭 Por que o autor escreveu isto
O post parte de um problema comum: as organizações pagam somas altas por licenças de middleware, hospedagem e manutenção, e as habilidades para operá-lo estão desaparecendo do mercado. Ele mostra como um varejista, a Country Road Group, saiu do BizTalk com uma plataforma orientada a padrões na AWS. Se a tese se sustentar, o custo do middleware e o risco de talentos desaparecem, porque os próprios padrões de interface nunca mudaram e os serviços da nuvem podem fornecê-los diretamente.

## Introdução
A introdução enquadra o problema. Produtos de integração empresarial como BizTalk, MuleSoft, TIBCO e Boomi resolveram o problema certo quando surgiram, mas acoplaram fortemente as aplicações ao middleware por meio de adaptadores proprietários, orquestrações e formatos de mensagem. O autor diz que muitas organizações hoje pagam centenas de milhares por ano em licenciamento, hospedagem e manutenção de middleware que não entrega nenhuma capacidade nova.

### Middleware de integração empresarial
Middleware, aqui, significa um produto central de integração pelo qual todo sistema conectado precisa passar. Seus adaptadores, orquestrações e formatos de mensagem são específicos desse produto, então as aplicações dependem dele e não conseguem sair facilmente. O acoplamento se torna a principal fonte de custo e risco ao longo do tempo.

### Padrões de interface
O autor nomeia quatro padrões de interface que não mudaram: publish-subscribe, roteamento baseado em conteúdo, request-reply e scatter-gather. Um sistema que envia eventos para muitos assinantes usa publish-subscribe, enquanto um que escolhe um destino a partir do conteúdo da mensagem usa roteamento baseado em conteúdo. Serviços nativos da nuvem agora implementam esses padrões diretamente, então nenhum runtime de middleware é necessário no meio.

### Contexto da Country Road Group
A CRG é uma varejista australiana de moda com cinco marcas (Country Road, Mimco, Witchery, Politix e Trenery) e mais de 600 lojas. Suas 21 interfaces de integração rodavam no Microsoft BizTalk Server hospedado em VMware e conectavam sistemas de ponto de venda, bancos de dados Oracle RMS e plataformas de e-commerce. Três pressões impulsionaram a mudança: o BizTalk se aproximava do fim do suporte, os custos de VMware subiam após a aquisição pela Broadcom, e a equipe já não conseguia contratar desenvolvedores de BizTalk.

### Abordagem declarativa e orientada a padrões
Em vez de escrever código de orquestração para cada interface, as equipes da CRG declaram qual padrão uma interface usa, e módulos Terraform pré-construídos provisionam a infraestrutura por trás dele. O post também cobre as quatro lanes de processamento que substituíram o BizTalk e o Error Management System personalizado que trata falhas em todas elas. A abordagem não depende de nenhuma ferramenta específica de infraestrutura como código.

> 🔎 Contexto adicional: Terraform é uma ferramenta de infraestrutura como código que descreve recursos da nuvem em arquivos de configuração e os cria por meio de um ciclo de plano e aplicação. Seu arquivo de estado registra quais recursos reais a configuração gerencia.

### Kit inicial
Os autores publicam um kit inicial de código aberto, a Enterprise Integration Patterns AWS Cloud Development Kit (AWS CDK) Library, com 29 constructs. Equipes que usam Terraform podem usá-lo como implementação de referência. O kit também inclui uma skill de agente que ajuda assistentes de programação com IA, como Kiro e Claude Code, a selecionar, configurar e compor o padrão certo.

## Por que a CRG construiu o SIP
O parque de integração da CRG sofria de proliferação de ferramentas e de middleware no fim de vida (BizTalk 2013), o que levou a instabilidade em produção e sobrecarga operacional. A Strategic Integration Platform (SIP) foi construída para corrigir isso. A seção nomeia quatro vetores de valor.

### Dívida legada
O objetivo é descomissionar o BizTalk 2013 e sua infraestrutura VMware e Broadcom sem suporte. Dívida legada, aqui, significa software fora de suporte e preso a hardware ou licenciamento que a empresa não quer mais pagar.

### Proliferação de ferramentas
A SIP consolida BizTalk, Workato, Apollo e Kafka em uma única solução de integração com um só conjunto de habilidades de engenharia. Menos ferramentas significam menos runtimes para aplicar patches, menos contratos com fornecedores e menos tipos de especialização para contratar.

### Baixa velocidade
A plataforma adota práticas de API-first e de integração contínua e entrega contínua (CI/CD), para que novas integrações com parceiros sejam entregues mais rápido. A velocidade é tratada como uma preocupação de arquitetura, e não apenas de processo de entrega.

### Risco de segurança e resiliência
A seção estabelece duas regras. Autenticação e autorização são padronizadas em todas as interfaces. As dead-letter queues (DLQs) são tratadas como armazenamentos de dados, porque uma mensagem com falha carrega uma cópia completa do payload de um cliente. Por isso, as DLQs precisam dos mesmos controles de criptografia, retenção e acesso do canal principal para permanecer dentro dos requisitos de residência e retenção de dados.

> ❌ **Anti-padrão — dead-letter queues descartáveis:** tratar uma DLQ como uma área temporária de retenção com controles mais frouxos que os do canal principal. **Por que as pessoas fazem isso:** mensagens com falha parecem detritos transitórios, e a fila raramente é revisada. **Consequência:** um caminho de falha vaza dados de clientes para fora dos controles que protegem o caminho principal. **Correção:** dê às DLQs as mesmas regras de criptografia, retenção e acesso do canal principal, como o artigo prescreve.

## Visão geral da arquitetura da SIP
A SIP segue a estratégia "Compose on Cloud-Native". Ela combina computação serverless da AWS (Spring Boot no AWS Lambda, AWS Step Functions e Amazon EventBridge) com Apache Camel no Amazon Elastic Container Service (Amazon ECS) com AWS Fargate para fluxos de longa duração e com estado. A arquitetura tem três camadas, mostradas na Figura 1, e o tráfego passa por um barramento de serviço de quatro lanes orientado a padrões, mostrado na Figura 2.

### Compose on Cloud-Native
A estratégia escolhe o runtime de acordo com a carga de trabalho. Computação serverless cuida da maior parte do processamento de eventos, e um runtime de contêineres com Apache Camel cuida dos fluxos que rodam por mais tempo ou mantêm estado. Cada lane roteia para o runtime adequado com base nas características do payload e na duração do processamento.

### Camada de padrões
Cada uma das 21 interfaces é expressa como uma composição de padrões padrão de integração, como publish-subscribe e roteamento baseado em conteúdo. Esta camada contém a lógica específica do negócio: handlers do Lambda que validam, transformam e roteiam payloads, orquestrados pelo Step Functions em fluxos de várias etapas e disparados por regras do Amazon EventBridge ou pelo Automic. Os padrões funcionam como um vocabulário compartilhado para a equipe de engenharia. Implantar uma interface começa pela escolha de um padrão declarativo ou de código Terraform, e não pela escrita de infraestrutura do zero.

### Camada de serviços da AWS
A solução provisiona sua superfície de integração a partir de serviços gerenciados da AWS. O Amazon API Gateway é o ponto de entrada, com AWS WAF protegendo e limitando a taxa à frente. O AWS Lambda cuida do processamento de eventos sem estado, e o AWS Step Functions orquestra fluxos de várias etapas com lógica padronizada de retry e catch. O Amazon EventBridge é o barramento de eventos canônico, e o Amazon Simple Queue Service (Amazon SQS) e o Amazon Simple Notification Service (Amazon SNS) fornecem buffer assíncrono e fan-out. O Amazon Simple Storage Service (Amazon S3) persiste mensagens, com S3 Intelligent-Tiering e arquivamento nas classes de armazenamento S3 Glacier. O AWS Transfer Family fornece endpoints SFTP e FTP gerenciados. O Amazon ECS com AWS Fargate executa as rotas do Apache Camel, e o Amazon DynamoDB guarda os metadados e o índice de erros. O Terraform é o mecanismo de provisionamento autoritativo para todos os recursos da AWS.

### Camada de conectividade de origem e destino
No lado da origem, a SIP conecta-se a sistemas on-premises, à Oracle Cloud Infrastructure (OCI), a aplicações core legadas (Oracle RMS, Manhattan WMS e Magenta Retail), a plataformas de nuvem e digitais (Salesforce Service Cloud e eStar Online) e a portais de parceiros B2B. No lado do destino, os fluxos entregam para bancos de dados Oracle, para o eStar (e-commerce), para sistemas de arquivos e para endpoints de parceiros.

### Consumo baseado em pull
A SIP puxa os dados, e as origens não os empurram. A SIP busca dados nos sistemas de origem segundo a sua própria programação, em vez de aceitar envios sem limite, então o consumidor controla a taxa. Quando o pull não se aplica, como nos webhooks de SaaS que só conseguem enviar, a SIP aceita o envio em uma fila e o processa no seu próprio ritmo, de modo que o mesmo princípio se mantém.

> 🔎 Contexto adicional: um webhook é um callback HTTP que um produto SaaS envia quando um evento acontece. Como quem envia decide quando enviar, é preciso uma fila para absorver os picos.

### Figuras 1 e 2
A Figura 1 mostra os principais blocos de construção em três camadas. A Figura 2 mostra o barramento de serviço multi-lane com quatro caminhos de processamento (batch, API, JDBC e evento), roteamento de transformação e gestão de erros. O texto extraído contém apenas as legendas das figuras, então os diagramas não são descritos aqui em mais detalhes.

## A SIP roteia o tráfego por quatro lanes orientadas a padrões
Cada uma das quatro lanes trata um tipo de tráfego, e cada uma roteia para o runtime que se encaixa no seu payload e na sua duração.

### Lane 1: MFT/Batch
Esta lane trata a ingestão baseada em arquivos. O Automic ou o Amazon EventBridge dispara o AWS Transfer Family para receber tráfego SFTP, arquiva os arquivos no S3 e passa o controle para um Lambda parser, que faz a validação e a transformação.

### Lane 2: API/Real-Time
Esta lane processa requisições REST síncronas por meio do Amazon API Gateway, em um orquestrador do Step Functions. Um motor de transformação roteia cada requisição para o Lambda, para jobs de até cinco minutos, ou para o Apache Camel no ECS Fargate, para fluxos longos e com estado.

### Lane 3: JDBC/Data Aggregator
Esta lane trata a conectividade direta com bancos de dados para extrações em lote. Ela roteia pelo Lambda e pelo Amazon AppFlow para agregação leve, ou pelo ECS Fargate para cargas mais pesadas.

### Lane 4: Event/Message
Esta lane gerencia fluxos assíncronos. Eventos externos entram pelo API Gateway e eventos internos são roteados pelo Amazon EventBridge, com o Amazon SNS para broadcast em fan-out e o Amazon SQS para consumo com buffer.

### Error Management System (EMS)
Em todas as quatro lanes, o EMS oferece uma única camada de tratamento de erros. Transações com falha são roteadas para uma dead-letter queue no Amazon SQS. O EMS guarda os metadados do erro no DynamoDB e o payload no S3, e cada falha carrega um correlation ID e um contrato de erro JSON padrão. O EMS é construído sobre Spring Boot no Lambda, com um front end em `Next.js` no AWS Amplify.

Os operadores L1 e L2 usam o dashboard do EMS para inspecionar, modificar e reenviar transações com falha de forma idempotente, sem envolvimento de desenvolvedores. Novas interfaces entram no EMS automaticamente por meio de um ID de interface único. O autor diz que isso levou a CRG da recuperação liderada por desenvolvedores (L4) para a resolução de primeira linha liderada por operadores (L1).

### Reenvio idempotente
Reenvio idempotente significa que reproduzir a mesma mensagem com falha produz o mesmo resultado que enviá-la uma única vez, de modo que um operador pode tentar de novo com segurança. O artigo depende dessa propriedade para que operadores corrijam e reenviem falhas sem alterar dados duas vezes.

> 🔎 Contexto adicional: idempotência é uma propriedade de uma operação. Repeti-la tem o mesmo efeito que executá-la uma vez. Muitos sistemas de mensagens entregam pelo menos uma vez, então os consumidores precisam tolerar duplicatas.

## Decisões de design principais
A seção lista quatro escolhas de design.

### Lambda primeiro, ECS como alternativa
O Lambda trata o processamento de eventos de curta duração. O Apache Camel no ECS Fargate trata fluxos longos, com estado e arquivos grandes. A troca é que as equipes passam a manter dois runtimes e precisam saber a qual deles cada fluxo pertence.

### Infraestrutura reproduzível
Todo recurso da AWS é definido em Terraform, de modo que qualquer ambiente pode ser reconstruído exatamente a partir do código. Isso corrige o desvio de configuração, uma mudança ruim ou um recurso apagado por acidente, e os mesmos templates podem subir um novo ambiente ou uma nova Região. O estado do Terraform fica em um bucket S3 versionado e com controle de acesso. A infraestrutura como código reconstrói a plataforma, mas não os seus dados, então os dados são protegidos à parte. A recuperação point-in-time do DynamoDB restaura tabelas, o versionamento do S3 restaura objetos, e as DLQs guardam mensagens com falha para redrive. Lambda, SQS, DynamoDB e Step Functions rodam em múltiplas Availability Zones por padrão, então a queda de uma zona não exige novo deploy.

### Amazon EventBridge como barramento de eventos canônico
Toda comunicação entre padrões passa pelo EventBridge. O barramento oferece roteamento baseado em conteúdo, replay de eventos e integração nativa com mais de 200 serviços da AWS. Colocar todos os padrões em um único barramento centraliza o roteamento, mas também torna o barramento uma dependência compartilhada de toda a plataforma.

### Segurança em cinco camadas
As cinco camadas são:
- Controle de acesso baseado em funções (RBAC) com privilégio mínimo pelo AWS Identity and Access Management (IAM).
- AWS WAF com isolamento por virtual private cloud (VPC).
- Classificação de dados, de Public a Restricted.
- TLS 1.2 ou superior em trânsito, com criptografia em repouso pelo AWS Key Management Service (AWS KMS).
- Log de auditoria estruturado com redação automatizada de informações pessoalmente identificáveis (PII).

## Código aberto: a biblioteca CDK de Enterprise Integration Patterns
A biblioteca de constructs do AWS CDK de código aberto codifica 29 padrões canônicos de integração como constructs L3 reutilizáveis. Cada construct provisiona a infraestrutura de mensageria, as permissões do IAM, as dead-letter queues e a lógica de retry a partir de uma única declaração. Os alarmes do Amazon CloudWatch são deliberadamente deixados para o consumidor, porque limites e destinos de notificação são específicos de cada ambiente. A biblioteca é escrita em TypeScript e AWS CDK. Equipes que usam Terraform ou outra ferramenta de infraestrutura como código podem ler os padrões e traduzi-los para o seu próprio framework de provisionamento.

## Pré-requisitos
Você precisa de uma conta AWS com o AWS CDK inicializado (bootstrapped) na Região de destino, Node.js 20 ou superior e a AWS CDK CLI v2. Familiaridade com TypeScript ajuda, mas os exemplos são curtos. O Caminho 2 também exige Kiro ou Claude Code.

## Primeiros passos
Há duas formas de ir de um requisito a um padrão implantado. O Caminho 1 é prático: você escreve a declaração do construct e executa os comandos do CDK por conta própria. O Caminho 2 é assistido por agentes: os agentes do repositório selecionam e configuram o construct, e você executa o deploy final. Os dois produzem o mesmo resultado, um pequeno `app.ts` que instancia um ou mais dos 29 constructs, de modo que a postura de segurança é idêntica nos dois casos. O autor sugere o Caminho 1 se você já sabe qual padrão precisa, e o Caminho 2 se estiver mapeando o vocabulário de uma plataforma legada. Os dois caminhos começam com a mesma configuração:

```bash
git clone https://github.com/aws-samples/sample-enterprise-integration-patterns-cdk.git
cd sample-enterprise-integration-patterns-cdk
npm ci
npm run build
npm test
```

O repositório é uma biblioteca de constructs, não uma aplicação implantável. O `app.ts` vem como um stub vazio, então um `cdk synth` em um clone novo não produz stacks, por design.

### Caminho 1: declare um padrão você mesmo
Abra o `app.ts` e declare o construct de que você precisa. O Guia de Implementação do README tem exemplos trabalhados de request-reply, publish-subscribe, scatter-gather e saga. Depois, sintetize e implante:

```bash
npx cdk synth
npx cdk deploy
```

**O que isto faz:**
- `npx cdk synth` gera os templates do CloudFormation a partir da declaração em `app.ts` sem criar nada na AWS.
- `npx cdk deploy` cria a stack na conta AWS e na Região inicializadas anteriormente.
- Executar `npx cdk deploy` é uma etapa separada da síntese, então o deploy é uma ação deliberada.
- O `app.ts` stub não produz nenhuma stack até que um construct seja declarado, por isso um clone novo não sintetiza nada.

### Caminho 2: deixe os agentes fazerem a seleção e a configuração
Abra o repositório no Kiro ou no Claude Code e selecione um agente no campo de chat. O Kiro, a IDE e CLI de programação agêntica da AWS, usa desenvolvimento orientado a especificações para transformar requisitos em linguagem natural em composições de padrões. Descreva o requisito de integração com as suas próprias palavras. O agente seleciona o construct, escreve a declaração no `app.ts` com as propriedades corretas e a valida com `npm run build`, `npm test` e `cdk synth`. O deploy continua sob sua responsabilidade: o agente `eip-architect` distribuído tem `cdk deploy` e `cdk destroy` negados por design. Quando o agente informa que a declaração está pronta, você executa os mesmos dois comandos do Caminho 1.

O repositório traz duas skills de agente e dois agentes personalizados:
- `integration-advisor` faz a descoberta. Ele faz perguntas, mapeia cada interface existente para um construct e escreve um arquivo de avaliação, sem tocar no código.
- `eip-architect` pega essa avaliação, escreve as declarações do CDK com a configuração correta para cada padrão e as verifica.

Use-os na ordem, ou vá direto ao `eip-architect` se já souber o que está construindo. A Figura 3 mostra os dois agentes rodando no Kiro CLI.

## Limpeza
Remova tudo o que você implantou com `npx cdk destroy`. Os constructs usam `RemovalPolicy.DESTROY` por padrão, então filas, tabelas e buckets criados por um padrão são removidos junto com a stack.

> 🔎 Contexto adicional: uma removal policy diz ao CloudFormation o que fazer com um recurso quando sua stack é excluída. Destruir é conveniente para uma demo, mas, em dados de produção, significa que excluir a stack pode apagar os dados também.

## Resultados
O custo anual dos serviços de integração caiu de cerca de US$ 300.000 para US$ 60.000, uma economia de 80 por cento. Quatro ferramentas de middleware (BizTalk, Workato, Apollo e Kafka) foram consolidadas em uma só. O objetivo principal, o descomissionamento completo do Microsoft BizTalk Server e de sua infraestrutura VMware e Broadcom, foi alcançado. A CRG agora implanta novas interfaces por meio de padrões padrão em horas, e não em semanas, usando templates Terraform padronizados e pipelines automatizados de CI/CD.

O Error Management System transferiu a resolução de falhas da engenharia para os operadores L1, o que muda o modelo de custo de suporte. Telemetria distribuída pelo Amazon CloudWatch, AWS X-Ray e Dynatrace rastreia cada transação de ponta a ponta com correlation IDs em cada salto de integração. A CRG migrou as interfaces em ondas agrupadas por função de negócio, e o rollback por grupo de serviços permite que qualquer onda com falha seja revertida sozinha. Como a camada de padrões é desacoplada da camada de serviços, novos serviços da AWS podem ser adicionados como etapas de uma composição de padrão existente sem re-arquitetar a plataforma.

### Migração em ondas com rollback por grupo de serviços
As interfaces se movem em ondas agrupadas por função de negócio, e não todas de uma vez. Cada onda pode ser revertida de forma independente se falhar, então um grupo com falha não força a reversão de toda a migração.

## Conclusão
Para organizações que estão aposentando middleware legado ou desenhando integrações do zero, o objetivo é uma arquitetura desacoplada em que os sistemas trocam dados por meio de padrões bem conhecidos, e não por um runtime central do qual todos dependem. O autor chama a abordagem da CRG de um playbook replicável. O middleware carrega custos que se acumulam em licenciamento, hospedagem, talentos escassos e entrega lenta, e uma migração orientada a padrões para serviços nativos da nuvem resolve os quatro ao mesmo tempo. O autor afirma que os padrões de integração são os mesmos quer a equipe use BizTalk, MuleSoft, TIBCO ou Boomi, e que só a camada de execução muda.

## Sobre os autores
A assinatura cita Aman Sharma, Andrew Hooker, Tim O'Sullivan e Vijay Krishnaswamy. As biografias dos autores não estavam presentes no texto extraído.

## 🔑 Principais conclusões
- Declarar o padrão de integração de uma interface e provisioná-lo a partir de módulos Terraform substituiu o código de orquestração por interface na CRG.
- Quatro lanes orientadas a padrões (batch/MFT, API, JDBC, evento) roteiam cada uma para o Lambda ou para o Apache Camel no ECS Fargate, conforme o payload e a duração.
- Um único Error Management System com dead-letter queues, correlation IDs e reenvio idempotente levou a recuperação de primeira linha para os operadores L1.
- Consolidar BizTalk, Workato, Apollo e Kafka em uma única plataforma reduziu o custo anual de integração de cerca de US$ 300.000 para US$ 60.000.
- Dead-letter queues guardam payloads completos de clientes, então precisam dos mesmos controles de criptografia, retenção e acesso do canal principal.
- Puxar dados das origens, em vez de aceitar envios sem limite, deixa o consumidor controlar a taxa; origens que só enviam passam por uma fila.
- Padrões padrão são a parte estável da integração; os serviços da nuvem mudam apenas a camada de execução.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios do TOGAF | Aplicação (camada de padrões, quatro lanes, EMS, composição de interfaces); Tecnologia (serviços da AWS, Terraform, resiliência multi-AZ, CI/CD); Dados (retenção de payloads, tratamento de DLQ, versionamento do S3, recuperação point-in-time do DynamoDB, residência de dados). Negócio aparece apenas como motivação (custo, talentos, velocidade). |
| Escopo | Segmento ou capacidade: uma plataforma de integração que atende às 21 interfaces da CRG em seus sistemas de varejo, e-commerce e parceiros. Não é uma arquitetura corporativa como um todo. |
| Stakeholders e preocupações | Arquitetos de integração e líderes de plataforma (custo e proliferação de ferramentas); gestores de engenharia (velocidade de entrega e habilidades); operadores (resolução de erros de primeira linha); equipes de segurança e compliance (criptografia, redação de PII, residência de dados); finanças (custo de VMware e licenciamento). |
| Frameworks e notações | Nenhum citado para TOGAF, ArchiMate, BPMN, BIZBOK ou C4. Enterprise Integration Patterns é citado como o catálogo de padrões por trás da biblioteca CDK. Terraform, AWS CDK, Kiro e Claude Code são citados como ferramentas. |

### Decisões e trade-offs
- **Substituir o BizTalk por uma plataforma nativa da nuvem orientada a padrões:** o artigo não lista as alternativas que considerou. Cita o fim do suporte, o aumento de custos de VMware e Broadcom, a dificuldade de contratação e a proliferação de ferramentas como motivos para mudar. O custo é que a CRG agora é dona da plataforma e de seus padrões, que exigem habilidades de engenharia próprias.
- **Lambda primeiro, ECS Fargate como alternativa:** o Lambda é usado para processamento de eventos curtos e o Apache Camel no ECS para fluxos longos ou com estado. O custo são dois runtimes entre os quais as equipes precisam escolher.
- **Pull em vez de push:** a SIP puxa das origens e enfileira as origens que só enviam. O custo é a lógica extra de fila e polling, em troca de uma taxa controlada pelo consumidor.
- **Terraform como autoridade de provisionamento:** todo recurso da AWS é definido em código, então os ambientes podem ser reconstruídos. O custo é que o estado precisa ser protegido e mantido consistente, e os dados precisam ser protegidos separadamente da infraestrutura.
- **EventBridge como barramento único entre padrões:** oferece roteamento baseado em conteúdo, replay e ampla integração com a AWS. O custo é uma dependência compartilhada por todos os padrões.
- **Tratamento de erros liderado por operadores, via EMS:** operadores reenviam transações com falha sem desenvolvedores. O custo é construir e manter o dashboard e o contrato de erro.
- **Biblioteca CDK de código aberto com alarmes do CloudWatch deixados para os consumidores:** as equipes ganham constructs reutilizáveis, mas limites e notificações continuam específicos de cada ambiente e precisam ser definidos por cada equipe. A removal policy padrão também apaga recursos junto com a stack.
- **Seleção assistida por agentes com deploy humano:** os agentes escrevem e validam o código CDK, mas o `eip-architect` tem `cdk deploy` e `cdk destroy` negados. O custo é que uma pessoa ainda executa o deploy final.
- **Migração em ondas com rollback por grupo de serviços:** as interfaces se movem em grupos por função de negócio, cada um capaz de reverter sozinho. O custo é manter caminhos antigos e novos rodando lado a lado durante a migração.

### Fundamentos em que se apoia
- [Arquitetura de Aplicação](/pt-br/foundations/application-architecture.md) — A SIP é uma arquitetura de aplicação: uma estrutura de capacidades (lanes, padrões, EMS) que interagem para mover dados entre sistemas.
- [Arquitetura de Tecnologia](/pt-br/foundations/technology-architecture.md) — Os serviços da AWS, os módulos Terraform e os pipelines de CI/CD formam os serviços e componentes de tecnologia de que a plataforma depende.
- [Domínio de arquitetura](/pt-br/foundations/architecture-domain.md) — O artigo cobre Aplicação e Tecnologia mais diretamente, e Dados pela retenção e recuperação de payloads, o que corresponde à divisão de domínios do TOGAF.

## 🏛️ Para o Kenji
Este é um caso útil para um arquiteto que decide se mantém, substitui ou re-plataforma um parque de middleware. Ele separa os padrões de interface estáveis do produto que os implementa, o que é um bom enquadramento para uma decisão de governança de arquitetura. Para a lente do MBA, o caso é uma história clara de custo e risco: custos de licença e hospedagem, escassez de talentos e risco de migração gerenciado em ondas. A pergunta que ele levanta: como você mediria o custo operacional de manter uma biblioteca de padrões em Terraform e CDK ao longo dos anos, comparado às taxas de licenciamento? Esta é a sua interpretação, não a do autor.

## 📚 Plano de leitura
### Do artigo
- [Repositório no GitHub: sample-enterprise-integration-patterns-cdk](https://github.com/aws-samples/sample-enterprise-integration-patterns-cdk/tree/main) — a origem dos 29 constructs, os passos de configuração e as skills de agente.
- [Guia de implementação](https://github.com/aws-samples/sample-enterprise-integration-patterns-cdk/blob/main/docs/implementation-guide.md) — exemplos trabalhados de request-reply, publish-subscribe, scatter-gather e saga.
- [bin/app.ts](https://github.com/aws-samples/sample-enterprise-integration-patterns-cdk/blob/main/bin/app.ts) — o arquivo de entrada onde você declara os constructs.
- [Kiro](https://kiro.dev/) — a IDE e CLI agêntica usada para a seleção assistida de padrões no Caminho 2.
- [Claude Code](https://claude.com/product/claude-code) — o outro assistente de programação suportado no Caminho 2.

### Para aprofundar
- Enterprise Integration Patterns (o catálogo de padrões de mensageria que a biblioteca implementa) — a referência original para publish-subscribe, roteamento baseado em conteúdo, request-reply, scatter-gather e saga.
- Gerenciamento de estado do Terraform e backends remotos — como proteger e compartilhar o arquivo de estado que o artigo mantém em um bucket S3 versionado.
- Design de dead-letter queues e redrive — como manter mensagens com falha em segurança e recuperá-las sem duplicar efeitos colaterais.
- Design de consumidor idempotente — a propriedade que permite que operadores reenviem transações com falha com segurança.
