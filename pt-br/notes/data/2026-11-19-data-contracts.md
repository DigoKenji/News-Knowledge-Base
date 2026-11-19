---
type: Article Summary
title: Contratos de dados
description: Contratos de dados aplicam a disciplina de design de APIs aos conjuntos de dados, oferecendo a pessoas e agentes de IA metadados verificáveis e versionados, em que podem confiar.
resource: https://martinfowler.com/articles/data-contracts.html
tags:
- data
- api-versioning
- model-context-protocol
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://martinfowler.com/articles/data-contracts.html
  title: Data Contracts
  author: Martin Fowler
status: stable
source_url: https://martinfowler.com/articles/data-contracts.html
source: Martin Fowler
source_type: blog
published: '2026-11-18'
domain: data
concepts:
- api-versioning
- model-context-protocol
selected_by: discovery
content: open
complete: 'no'
notion_url: https://app.notion.com/p/41dbaf74172d81969fbae4bba366e65a
lang: pt-br
translation_of: /notes/data/2026-11-19-data-contracts.md
---

## 🎯 Tese central
Contratos de dados aplicam a disciplina de design de APIs aos dados: cada conjunto de dados recebe uma descrição versionada e legível por máquina, que cobre seu schema, qualidade, níveis de serviço, responsabilidade e termos de uso. Pessoas e agentes de IA podem confiar nessa descrição, e as ferramentas podem verificá-la.

## 🧭 Por que o autor escreveu o texto
A confiança se torna o gargalo quando dados de muitas equipes são combinados, e agentes de IA agravam o problema porque tratam cada valor como verdade. Os autores, Simon Harrer (CEO da Entropy Data, no comitê diretor dos padrões Bitol ODCS e ODPS) e Arif Wider (professor na HTW Berlin, ex-Head of Data and AI na Thoughtworks Alemanha), defendem que contratos explícitos e verificados resolvem isso e se tornarão, para dados, o que o OpenAPI é para APIs REST.

## Por que precisamos de contratos de dados
O artigo começa com Alex, um engenheiro de dados de uma seguradora que está construindo um modelo para prever quais clientes vão comprar mais produtos. Alex precisa saber se o conjunto de dados tem a informação necessária e o significado certo, a que ele se conecta, quão atualizado e completo ele é, se a estrutura vai continuar estável e se a GDPR e as regras de dados pessoais permitem o processamento e o recompartilhamento dos resultados. A confiança abrange a fonte (qualidade, confiabilidade, situação legal) e os destinatários (se eles tratam os dados de forma responsável). Os contratos respondem a essas perguntas continuamente, e não apenas uma vez.

## Um exemplo simples de contrato de dados
Em sua forma mais simples, um contrato de dados descreve a forma dos dados que um provedor oferece, como um schema. Os exemplos usam o Open Data Contract Standard (ODCS), que o artigo chama de padrão de fato da indústria, mantido pelo projeto Bitol na Linux Foundation e classificado como Adopt no Thoughtworks Technology Radar.

```yaml
apiVersion: "v3.2.0"
kind: "DataContract"
id: "customers"
name: "Customers"
version: "1.0.0"
status: "active"

schema:
  - name: "customers"
    physicalType: "TABLE"
    description: "All customers holding a life insurance policy since 2020-01-01"
    properties:
      - name: "customer_id"
        logicalType: "string"
        primaryKey: true
      - name: "customer_name"
        businessName: "Customer Name"
        logicalType: "string"
        physicalType: "VARCHAR(100)"
        examples:
          - "Alex Doe"
        classification: "restricted"
```

**O que isso faz:**
- `apiVersion` e `kind` declaram um contrato de dados ODCS 3.2.
- `id`, `name`, `version` e `status` dão ao contrato uma identidade e um ciclo de vida; `active` e `1.0.0` sinalizam um conjunto de dados mantido e estável.
- `physicalType` informa o tipo técnico e `businessName` o rótulo de negócio de uma coluna.
- `classification: "restricted"` é um atributo de governança; o artigo observa que cada coluna carrega detalhes técnicos, de negócio e de governança.

O artigo observa que um contrato assim é mais provavelmente exibido como uma página em um marketplace de dados do que lido como YAML bruto.

```yaml
schema:
  - name: "customers"
    properties:
      - name: "customer_name"
        quality:
          - type: "sql"
            description: "Expect less than 1% empty name fields"
            query: |
              SELECT AVG(
                CASE
                  WHEN customer_name IS NULL
                    OR TRIM(customer_name) = ''
                  THEN 1
                  ELSE 0
                END
              )
              FROM customers
            mustBeLessThan: 0.01
slaProperties:
  - property: "freshness"
    value: 24
    unit: "hours"
    element: "customers.last_updated_at"
    description: "Within 24 hours of the last update"
```

**O que isso faz:**
- A regra de qualidade em SQL calcula a proporção de nomes vazios; `mustBeLessThan: 0.01` a transforma em uma verificação de aprovação ou reprovação.
- `slaProperties` acrescenta uma promessa de atualidade (freshness), medida em `customers.last_updated_at`.
- O ponto do artigo: status e versão mostram estabilidade, mas "a confiança real surge quando essas garantias são verificadas automaticamente".

## O que isso significa para agentes de IA?
Agentes enfrentam as perguntas de Alex em toda tarefa, sem ninguém para perguntar. Um humano desconfiado hesita ou pergunta ao dono; um agente, como citam Sadalage e Chandrasekaran, "trata cada valor como verdade", então produz resultados confiantemente errados e os humanos deixam de confiar nele. Um contrato responde às perguntas numa forma que um agente consegue usar: descoberta em um marketplace que agentes podem pesquisar, schema e semântica para joins corretos, regras de qualidade e SLAs que mostram o quanto confiar em um resultado, e termos de uso que mostram o que é permitido.

## O que há em um contrato de dados
O artigo lista seis propriedades de um contrato de dados. Ele é legível por máquina (YAML estruturado, e não prosa em uma wiki), verificável (promessas que as ferramentas conseguem aprovar ou reprovar), versionado (um esquema que separa mudanças inofensivas de mudanças quebráveis), orientado pelo provedor (publicado de forma unilateral pelo produtor), orientado ao consumidor (escrito para a decisão do consumidor) e fonte da verdade para os metadados.

### Contrato de dados
Um contrato de dados é uma descrição publicada da forma de um conjunto de dados, de sua semântica, de suas garantias de qualidade, de seus níveis de serviço e de seus termos de uso. O provedor o escreve e responde pelas suas promessas, mas ele é escrito para o consumidor, que o usa para decidir se os dados servem e para confiar neles ao longo do tempo. Catálogos, marketplaces, documentação e agentes leem esse mesmo artefato, em vez de manter cópias divergentes. Suas promessas precisam ser concretas o bastante para que as ferramentas possam verificá-las.

## Open Data Contract Standard
O ODCS define um formato YAML padronizado para contratos. É um projeto graduado da Linux Foundation sob o Bitol, sua especificação pública no GitHub tem mais de 1000 estrelas, e ele traz um JSON Schema para validação. A Figura 1 mapeia suas seções; o artigo usa o ODCS 3.2, a versão mais recente na data da escrita. Um passo a passo interativo está em datacontract.com.

```yaml
apiVersion: "v3.2.0"
kind: "DataContract"
id: "customers"
name: "Customers"
version: "1.0.0"
status: "active"
```

**O que isso faz:**
- `apiVersion` fixa a versão do padrão que o documento segue.
- `id` é um identificador único; `name` é o rótulo legível.
- `version` e `status` controlam o versionamento e o ciclo de vida, tratados mais adiante no artigo.

## Schema e qualidade dos dados
O schema define estrutura e semântica. Aqui ele é uma tabela, mas a mesma estrutura descreve mensagens Kafka, arquivos em armazenamento de objetos ou o payload de uma API. Cada coluna carrega detalhes técnicos, semântica de negócio e atributos de governança. O exemplo do artigo é `annual_premium`, armazenado como inteiro em centavos, seguindo a convenção monetária da empresa. Relacionamentos, como chaves estrangeiras, também podem ser declarados.

```yaml
schema:
  - name: "customers"
    physicalType: "TABLE"
    description: "All customers holding a life insurance policy since 2020-01-01"
    properties:
      - name: "customer_id"
        logicalType: "string"
        primaryKey: true
      - name: "customer_name"
        businessName: "Customer Name"
        logicalType: "string"
        physicalType: "VARCHAR(100)"
        required: true
        examples:
          - "Alex Doe"
        classification: "restricted"
        tags:
          - "pii:true"
      - name: "annual_premium"
        logicalType: "integer"
        physicalType: "INTEGER"
        description: "Annual premium in cents"
        required: true
        logicalTypeOptions:
          minimum: 0
      - name: "customer_status"
        logicalType: "string"
        physicalType: "TEXT"
        examples:
          - "active"
      - name: "last_updated_at"
        logicalType: "date"
        physicalType: "TIMESTAMP"
        description: "Timestamp of the last update to the customer record"
```

**O que isso faz:**
- `primaryKey`, `required: true` e `minimum: 0` declaram a chave, os campos obrigatórios e um prêmio não negativo.
- A `description` de `annual_premium` registra a convenção de centavos, que o consumidor precisa conhecer para ler o número.
- `tags: "pii:true"` marca dados pessoais; o artigo usa depois essas tags para orientar o mascaramento.

### Regra de qualidade de dados
Uma regra de qualidade é uma asserção que as ferramentas conseguem avaliar e aprovar ou reprovar, e não uma aspiração. Os tipos do artigo são consultas SQL que comparam um valor calculado com um limiar, e métricas de biblioteca, como `invalidValues`. Os limiares podem ser absolutos, como um número mínimo de linhas, ou relativos, como no máximo 1% de nomes vazios. As regras podem estar no nível da tabela ou da coluna e são verificadas automaticamente com as ferramentas adequadas.

```yaml
schema:
  - name: "customers"
    quality:
      - type: "sql"
        description: "Expect more than 100K rows"
        query: "select count(*) from customers"
        mustBeGreaterThan: 100000
    properties:
      - name: "customer_name"
        quality:
          - type: "sql"
            description: "Expect less than 1% empty name fields"
            query: |
              SELECT AVG(
                CASE
                  WHEN customer_name IS NULL
                    OR TRIM(customer_name) = ''
                  THEN 1
                  ELSE 0
                END
              )
              FROM customers
            mustBeLessThan: 0.01
      - name: "customer_status"
        quality:
          - type: "library"
            description: "Only valid customer statuses"
            metric: "invalidValues"
            arguments:
              validValues:
                - "active"
                - "lapsed"
                - "cancelled"
            mustBe: 0
```

**O que isso faz:**
- A regra SQL no nível da tabela espera mais de 100.000 linhas.
- A regra no nível da coluna calcula a média de um indicador 1 ou 0 sobre nomes NULL ou em branco; o resultado precisa ficar abaixo de 0,01.
- A regra `library` conta os status inválidos em relação a uma lista permitida; `mustBe: 0` não admite nenhum.

## Responsabilidade e canais de suporte
A responsabilidade é central. A equipe define quem é dono e mantém o contrato, com membros e papéis, e os canais de suporte dizem aos consumidores como chegar a eles. O artigo chama essa responsabilidade acessível de crítica para construir confiança.

```yaml
team:
  name: "customer-data"
  members:
    - username: "carola@example.com"
      name: "Carola Doe"
      role: "Owner"

support:
  - channel: "#customer-data-support"
    tool: "slack"
    url: "https://example.slack.com/archives/C123456789"
```

**O que isso faz:**
- `team` nomeia o grupo responsável e um membro com o papel `Owner`.
- `support` informa o canal e a URL do Slack que os consumidores usam para fazer perguntas.
- O `username` é um endereço de contato, então o contrato carrega a identidade do dono.

## Termos de uso e SLAs
Os termos de uso dizem aos consumidores o que podem e o que não podem fazer com os dados. O padrão cobre finalidade, diretrizes de uso e limitações, e pode vincular políticas externas, como políticas de privacidade ou licenças. Os SLAs declaram garantias de forma mensurável, como retenção e atualidade. A promessa de atualidade é verificada contra `last_updated_at` em cada teste, e uma violação alerta a equipe responsável.

```yaml
description:
  purpose: "Provides customer master data for analytics and reporting"
  usage: "Used by analytics team for customer insights and cross-sell prediction"
  limitations: "Contains only customers with an active policy"
  authoritativeDefinitions:
    - url: "https://example.com/policies/gdpr-compliance"
      type: "businessDefinition"
      description: "GDPR compliance policy for handling customer data"
slaProperties:
  - property: "retention"
    value: 5
    unit: "year"
    description: "Data will be deleted after 5 years"
  - property: "freshness"
    value: 24
    unit: "hours"
    element: "customers.last_updated_at"
    description: "Within 24 hours of the last update"
```

**O que isso faz:**
- `purpose`, `usage` e `limitations` informam ao consumidor o uso pretendido e que apenas clientes com apólice ativa estão incluídos.
- `authoritativeDefinitions` vincula uma política externa de GDPR como definição de negócio.
- `slaProperties` expressa retenção (5 anos) e atualidade (24 horas em um elemento nomeado).

## Localização dos dados
Os servidores informam onde os dados estão, incluindo os detalhes de conexão, mas não credenciais. O padrão cobre plataformas como Snowflake, Databricks, BigQuery, Oracle e Postgres, streams de eventos como Kafka, Kinesis e Pub/Sub, arquivos no S3 e arquivos CSV em SFTP, com um tipo personalizado como alternativa. Os ambientes são codificados como entradas de servidor separadas.

```yaml
servers:
  - server: "production"
    type: "snowflake"
    environment: "prod"
    account: "insurance-corp"
    database: "prod_lifeinsurance"
    schema: "dp_customers_v1"
```

**O que isso faz:**
- `type` e `environment` identificam a plataforma e a etapa.
- `account`, `database` e `schema` localizam o schema no Snowflake sem nenhuma credencial.

```yaml
servers:
  - server: "production"
    type: "kafka"
    environment: "prod"
    host: "kafka.example.com:9092"
    format: "avro"

schema:
  - name: "customer_events"
    physicalType: "topic"
```

**O que isso faz:**
- No caso de um stream de eventos, o servidor nomeia o host do cluster e o formato da mensagem (`avro`).
- O schema descreve um `topic` em vez de uma tabela, então a mesma estrutura cobre streams.

## Papéis e segurança
Os papéis descrevem como os consumidores obtêm acesso. Cada papel nomeia um papel IAM na plataforma de dados e o tipo de acesso que concede. Os provedores costumam definir um papel com dados pessoais e outro sem eles. O contrato não lista quem tem um papel; isso é decidido por consumidor, quando uma solicitação de acesso é aprovada.

```yaml
roles:
  - role: "customers_reader"
    access: "read"
    description: "Read access to all customer data"
```

**O que isso faz:**
- `role` é o nome do papel na plataforma, e `access: "read"` é a permissão que ele concede.
- O contrato define o papel; conceder esse papel a uma pessoa acontece depois, fora do contrato.

## Contexto para IA
O contexto para IA é uma orientação escrita para agentes. `instructions` funcionam como um prompt de sistema para o conjunto de dados, `constraints` declaram o que um agente não deve fazer, e `verifiedStatements` associam perguntas comuns a uma consulta verificada, para que um agente reutilize uma resposta correta em vez de adivinhar. O artigo observa que o ODCS 3.2 acrescentou esse bloco.

```yaml
context:
  instructions: >
    Use this data for analytics on life insurance customers.
    Annual premiums are stored in cents; divide by 100 before
    reporting amounts.
  constraints:
    - constraint: >
        Do not use this data to make decisions about individual
        customers, such as pricing a policy or assessing a claim.
  verifiedStatements:
    - question: "How many active customers do we have?"
      answer: |
        SELECT COUNT(*)
        FROM customers
        WHERE customer_status = 'active'
```

**O que isso faz:**
- As `instructions` em bloco dobrado dizem ao agente que os prêmios estão em centavos e precisam ser divididos por 100.
- A `constraint` proíbe decisões sobre clientes individuais, como precificar uma apólice ou avaliar um sinistro.
- `verifiedStatements` associa uma pergunta de negócio a uma resposta em SQL que o provedor verificou.

## Precificando o uso dos dados
O preço expressa uma cobrança interna que reflete o valor dos dados. Na prática, ele costuma ficar vazio ou repassa o custo do acesso na plataforma. O exemplo do artigo é 10 USD por terabyte escaneado.

```yaml
price:
  priceAmount: 10
  priceCurrency: "USD"
  priceUnit: "per TB scanned"
```

**O que isso faz:**
- `priceAmount` e `priceCurrency` informam o valor e a moeda.
- `priceUnit` declara a base: por terabyte escaneado.

## Extensibilidade integrada
O padrão tem dois mecanismos de extensão. `customProperties`, em todos os níveis, carregam metadados que o padrão não conhece, e fornecedores usam o mesmo mecanismo para adicionar funções. `authoritativeDefinitions` vincula o contrato, ou qualquer elemento dele, a definições em outros lugares, inclusive padrões abertos. A recomendação é preferir as propriedades oficiais do padrão e manter as personalizadas no mínimo.

```yaml
customProperties:
  - property: "legalEntity"
    value: "Insurance Corp AG"
    description: "Legal entity that owns and provides this data"

authoritativeDefinitions:
  - url: "https://example.com/apis/customers/openapi.yaml"
    type: "openapi"
    description: "OpenAPI specification of the REST API serving this data"
```

**O que isso faz:**
- `customProperties` registra a entidade jurídica, para a qual o padrão não tem campo.
- `authoritativeDefinitions` aponta para a especificação OpenAPI da API REST que serve os mesmos dados.

> ❌ **Anti-padrão — proliferação de propriedades personalizadas:** acrescentar propriedades personalizadas para informações que o padrão já cobre. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** não declarada no artigo. **Correção:** use primeiro as propriedades oficiais do padrão e mantenha as personalizadas no mínimo.

## Contratos de dados no contexto
Os contratos ganham valor pelas conexões que fazem. O artigo cita três: vincular colunas a um modelo de domínio em vez de repetir o significado delas, colocar contratos nas portas dos produtos de dados e permitir que o contrato seja a página do produto a partir da qual os consumidores solicitam acesso.

## Vinculando contratos de dados a ontologias
Documentar cada conjunto de dados separadamente não basta quando agentes combinam fontes; a semântica precisa ser definida entre as fontes, em uma camada abrangente. Combinar ontologias com contratos resolve dois problemas. **Desduplicação de metadados:** um conceito como Customer ID é definido uma única vez, com seus exemplos, classificação e descrição, e os contratos se vinculam a ele. **Consistência semântica:** um consumidor consegue saber se duas colunas com nomes parecidos significam a mesma coisa, e se duas colunas com nomes diferentes significam, o que importa além de simplesmente casar chaves.

### Vínculo com ontologia
Um contrato se mapeia em uma ontologia apontando um elemento para um conceito da ontologia por meio de `authoritativeDefinitions`, identificado por um IRI ou URL. A ontologia pode ser expressa em RDF ou OWL, ou com o padrão de ontologia do projeto Apache Ossie, que estava em incubação na Apache Software Foundation. A Figura 2 mostra dois contratos vinculando propriedades a um mesmo conceito, para que o consumidor saiba como uni-los.

```yaml
schema:
  - name: "customer_360"
    properties:
      - name: "cid"
        authoritativeDefinitions:
          - type: "semantics"
            url: "https://example.com/ns/Customer.id"
```

**O que isso faz:**
- `type: "semantics"` liga a coluna `cid` a um elemento da ontologia, em vez de repetir o significado dela.
- A `url` nomeia o conceito pelo IRI, então cada contrato que o vincula compartilha a mesma definição.

## Produtos de dados se apoiam em contratos de dados
Contratos de dados se combinam com produtos de dados, que trazem pensamento de produto para os dados e são centrais no data mesh. Os contratos são a linguagem de descrição de interfaces que torna as portas de um produto explícitas e verificáveis. Desde 2025, o Open Data Product Standard (ODPS) descreve os metadados do produto em cima do ODCS. A Figura 3 mostra um produto com uma porta de entrada e três portas de saída.

### Produto de dados como quantum arquitetural
Um produto de dados é a unidade completa: o código e o pipeline, os dados e os metadados que os descrevem. O artigo o chama de quantum arquitetural, uma unidade autocontida cujas portas de entrada e saída são interfaces ODCS. Os contratos das portas de saída continuam centrais, porque guardam as promessas de que muitas partes dependem. Informações compartilhadas de dono e suporte podem migrar para a descrição do produto, reduzindo a duplicação.

```yaml
apiVersion: "v1.1.0"
  kind: "DataProduct"
  id: "customers"
  name: "Customers"
  status: "active"
  
  description:
    purpose: >
      Master data for all customers holding a life insurance
      policy. All customers since 2020-01-01.
    usage: "Used by analytics teams for customer insights and cross-sell prediction"
    limitations: "Contains only customers with an active policy"
  
  inputPorts:
    - name: "customer_events"
      version: "1.0.0"
      contractId: "kafka_customer_events"
  
  outputPorts:
    - name: "customers_pii_snowflake_v1"
      description: "All customer records, with PII."
      version: "1.0.0"
      contractId: "customers_pii_snowflake"
    - name: "customers_pii_snowflake_v2"
      description: "All customer records, with PII."
      version: "2.0.0"
      contractId: "customers_pii_snowflake"
    - name: "customers_npii_snowflake_v1"
      description: "All customer records, without PII data."
      version: "1.0.0"
      contractId: "customers_npii_snowflake"
```

**O que isso faz:**
- `inputPorts` referencia o contrato upstream `kafka_customer_events` na versão 1.0.0.
- `outputPorts` lista três portas; duas compartilham o contrato `customers_pii_snowflake` nas versões 1.0.0 e 2.0.0, então duas versões principais rodam lado a lado.
- `apiVersion` (v1.1.0) é a versão do ODPS, separada da versão do ODCS usada nos contratos.
- A indentação é reproduzida como está no trecho de origem: a primeira linha começa sem recuo, as demais são indentadas.

## Contratos de dados são a página de detalhes do produto em um marketplace de dados
Em um marketplace de dados, os dados se tornam compráveis, e o contrato é a página de detalhes do produto: dá ao consumidor o que ele precisa para decidir se os dados servem à sua finalidade. Um consumidor solicita acesso com uma finalidade de processamento que combina com os termos de uso. Essa solicitação é o clique de "comprar". A Figura 4 mostra o contrato como central para provedor, consumidor, governança e plataforma. A Figura 5 mostra um agente de IA solicitando acesso, sendo aprovado e recebendo acesso de curta duração.

### Acordo de acesso
A solicitação cria um acordo de acesso formado pelo contrato solicitado, pela finalidade declarada e pelo consumidor solicitante. Ele só passa a valer quando o provedor aprova, e só então a plataforma concede o acesso. O artigo contrasta o acordo com o contrato: o contrato é unilateral, não nomeia consumidor e não tem prazo; o acordo é bilateral, tem data de início e expiração opcional, e, para agentes, pode durar apenas alguns minutos. Ainda não existe padrão para isso; o RFC do projeto Bitol para um Open Access Agreement Standard (OAAS) propõe registrar quem pode acessar qual porta de saída, para qual finalidade e por quanto tempo.

> ❌ **Anti-padrão — confundir o contrato com o acordo de acesso:** tratar o contrato publicado como se concedesse acesso. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** não declarada no artigo. **Correção:** mantenha o contrato unilateral para as promessas e um acordo separado, aprovado e com prazo, para o acesso.

Resultados de testes publicados acrescentam sinais de confiança ao marketplace, por exemplo, atualidade atendida em 29 dos últimos 30 dias.

## Trabalhando com contratos de dados
O artigo pede o rigor de uma API: criar o contrato de forma deliberada, mantê-lo junto ao código, evoluí-lo sem quebrar consumidores e verificá-lo automaticamente.

## Data-first versus contract-first
No modelo data-first, os dados já são compartilhados, por exemplo como tabelas de data warehouse. Você representa a estrutura existente em um contrato e acrescenta, de forma iterativa, as promessas implícitas. A Data Contract CLI, de código aberto, consegue gerar um primeiro rascunho a partir de DDL SQL, JSON Schema ou Avro Schema. Depois de publicado, o contrato se torna a fonte da verdade e os dados precisam segui-lo: se uma coluna for renomeada, o teste de contrato falha e a renomeação é tratada como mudança quebrável. No modelo contract-first, a conversa vem antes de qualquer código, idealmente em um workshop facilitado entre provedor e consumidores. Os rascunhos podem existir em um quadro branco, em um editor de YAML, no Data Product Canvas, em um template Excel compatível com ODCS ou no Data Contract Editor, de código aberto. O estado final é um contrato compatível com o padrão, refinado até que todas as partes concordem, e que o pipeline é então construído para entregar.

> ❌ **Anti-padrão — regenerar o contrato após uma renomeação:** rodar a geração de novo para que o contrato acompanhe a tabela alterada. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** não declarada; o artigo diz que a renomeação deve ser tratada como mudança quebrável. **Correção:** deixe o teste de contrato falhar e trate a renomeação como mudança quebrável.

## Seu habitat natural é um repositório Git
Onde quer que um contrato seja rascunhado, seu habitat natural é um repositório Git, ao lado do YAML do produto de dados, do código que produz os dados e da configuração de CI/CD. Os contratos ficam em uma subpasta dedicada. Os autores recomendam não colocá-los longe do código do pipeline, porque o pipeline geralmente precisa mudar junto com o contrato.

```text
customers.odps.yaml
  |- datacontracts
     |- customers_pii_snowflake_v1.odcs.yaml
     |- customers_pii_snowflake_v2.odcs.yaml
     |- customers_npii_snowflake_v1.odcs.yaml
  |- code
     |- pipeline.sql
  .gitlab-ci.yml
```

**O que isso faz:**
- O YAML do produto fica na raiz; os contratos ficam em `datacontracts/` e o código do pipeline em `code/`.
- Os nomes dos arquivos combinam conjunto de dados (`customers_pii`), tecnologia (`snowflake`) e versão principal (`v1`), mas a versão principal não está no ID, então as versões podem coexistir.
- `.gitlab-ci.yml` na raiz mostra que a configuração do pipeline vive no mesmo repositório.

> ❌ **Anti-padrão — guardar contratos longe do código do pipeline:** manter os contratos separados do código que produz os dados. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** o pipeline geralmente precisa mudar junto com o contrato, então a separação favorece descasamentos. **Correção:** mantenha-os juntos e altere-os em um mesmo pull request.

## A história do versionamento e das mudanças quebráveis
Os contratos seguem o SemVer. Uma mudança maior, como remover ou renomear uma coluna, é quebrável; uma mudança menor, como adicionar uma coluna, não é quebrável; um patch, como corrigir um erro de digitação ou atualizar canais de suporte, é pequeno. Uma mudança quebrável é lançada como uma nova versão principal ao lado de um novo conjunto de dados, é introduzida gradualmente ao lado da antiga, e a antiga é descontinuada após um período de carência. O campo status sinaliza a etapa: `draft` durante o desenvolvimento, `active` quando as promessas passam a ser vinculantes, depois `deprecated` e `retired`. Com produtos de dados, o ciclo de vida costuma acompanhar o lançamento do produto, com cada versão principal oferecida como uma porta de saída própria.

### Versionamento semântico para contratos
O SemVer divide uma versão em números maior, menor e de patch, e o artigo dá a cada um um significado contratual. Os consumidores conseguem distinguir uma mudança inofensiva de uma quebrável lendo o número. A versão principal fica atrelada ao conjunto de dados que protege, então uma mudança quebrável significa um novo conjunto de dados, e não uma edição silenciosa. O status acompanha a versão para mostrar se ela está sendo introduzida, ativa ou descontinuada.

Para streams de eventos, isso se mapeia nos modos de compatibilidade do schema registry. Uma versão menor precisa ser compatível para frente (forward), para que consumidores existentes continuem lendo os novos eventos, e idealmente totalmente compatível, para que novos consumidores consigam reprocessar eventos antigos. Cuidado com o padrão do registry: a compatibilidade backward permite apagar um campo, exatamente o que quebra consumidores existentes. Tudo que o registry rejeitaria é uma versão principal, publicada como um novo tópico.

> ❌ **Anti-padrão — confiar na compatibilidade padrão do registry:** aceitar a remoção de um campo como mudança menor rotineira porque o padrão a permite. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** consumidores existentes quebram. **Correção:** exija compatibilidade forward para versões menores e publique como nova versão principal, em um novo tópico, tudo o que o registry rejeitar.

## Automatize tudo
Um contrato que ninguém verifica é um tigre de papel, que não vale mais do que uma página de wiki. A lista de automações do artigo:
- Gerar documentação HTML e disponibilizá-la na intranet.
- Publicar em repositórios de metadados, como marketplaces de produtos de dados ou catálogos.
- Testar o ambiente de teste contra o contrato no CI/CD a cada pull request, e bloquear o merge em caso de falha.
- Testar a produção todos os dias, e informar o dono e os consumidores downstream sobre promessas quebradas.
- Gerar DDL SQL para que as tabelas do provedor sejam compatíveis com o contrato desde o projeto.
- Derivar políticas de mascaramento e de acesso, como mascarar colunas marcadas com `pii:true` para papéis sem acesso a dados pessoais.
- Gerar consultas para consumidores que evitem `select *`.
- Comparar uma nova versão do contrato com a antiga em um pull request, e reprovar uma mudança quebrável em uma atualização menor ou de patch.
- Validar os dados novos antes que cheguem aos consumidores, e reter os dados que violam o contrato.

A Data Contract CLI, de código aberto, com mais de 1 milhão de downloads por mês, entende nativamente o ODCS e suporta essas tarefas. O Thoughtworks Technology Radar a classifica como Trial (novembro de 2025), especialmente útil para validação, detecção de mudanças quebráveis e governança em CI/CD.

### Padrão de quarentena
Valide os dados novos contra o contrato antes que os consumidores os vejam, e retenha os dados que violam o contrato. O artigo cita o padrão como parte da lista de automações e não dá mais detalhes.

> ❌ **Anti-padrão — contrato não verificado:** manter o contrato apenas como documentação, sem nenhum teste que o execute. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** na primeira vez que um consumidor encontrar uma promessa quebrada, a confiança se perde. **Correção:** transforme cada promessa em um teste automatizado.

## Contratos de dados pertencem ao seu pipeline de CI/CD
Cada commit é verificado antes de ser entregue. A CLI tem um comando por etapa: `datacontract lint` valida o contrato, `datacontract breaking` detecta mudanças quebráveis em relação à versão anterior, e `datacontract ci` testa os dados contra o contrato em cada ambiente. Só quando todas as etapas passam o pipeline implanta em produção e publica o contrato com `datacontract publish`. Um segundo pipeline, agendado, executa `datacontract test --server prod` em um cron e publica os resultados, informando provedores e consumidores afetados assim que uma promessa quebrar. A Figura 6 mostra o fluxo: verificado a cada commit, e testado contra a produção todas as noites.

## Os custos e benefícios dos contratos de dados
Os contratos se pagam em confiança e automação, mas não são gratuitos e não compensa escrevê-los para todo conjunto de dados. Agentes amplificam os dois lados desse equilíbrio.

### Os benefícios
Quando dados analíticos são descritos por contratos, eles podem ser testados automaticamente e monitorados continuamente, e ações corretivas podem ser disparadas quando a qualidade cai abaixo de um limiar prometido. Como as expectativas dos consumidores ficam escritas, mudanças quebráveis podem ser detectadas e podem interromper uma implantação, em vez de se propagar para serviços downstream. A responsabilidade passa a recair sobre os provedores, o que constrói confiança entre provedores e consumidores, negócio e TI, e humanos e os agentes que atuam em seu nome.

## Gere seu pipeline de dados
Contratos de dados possibilitam o desenvolvimento orientado por especificação. O contrato é o núcleo da especificação: entregue a um agente de programação junto com um plugin de skills que descrevem como os pipelines são construídos na empresa, ele consegue gerar quase o pipeline inteiro. Testar os dados contra o contrato durante a geração dá ao agente um feedback que o guia. A Figura 7 mostra esse fluxo, e o artigo aponta para um exemplo de execução em dataproduct.com.

### Desenvolvimento orientado por especificação
O contrato é tratado como a especificação que orienta a geração de código, em vez de documentação escrita depois do código. Testar o pipeline gerado contra o contrato fecha o ciclo, de modo que cada execução é corrigida pelas mesmas promessas nas quais os consumidores confiam.

## Converse com seus dados
A parte difícil de conversar com dados não é gerar SQL, mas gerar o SQL correto. Um agente diante de centenas de tabelas e milhares de colunas precisa adivinhar a semântica das colunas e a lógica dos joins, e também decidir se pode responder. Antes de tudo, ele precisa encontrar os dados. Contratos publicados em um marketplace que agentes conseguem pesquisar, por exemplo via MCP, tornam os dados descobríveis. Depois de encontrados, o contrato para de deixar margem para adivinhação: fornece o significado das colunas, os caminhos de join por meio de vínculos semânticos e os termos de uso.

### Model Context Protocol
> 🔎 Contexto adicionado: O Model Context Protocol é um padrão aberto que permite que aplicações de IA se conectem a ferramentas externas, arquivos, bancos de dados e APIs por meio de uma interface comum.

No artigo, o MCP é o caminho pelo qual agentes pesquisam um marketplace de dados e encontram contratos.

## O custo
Contratos exigem tempo e esforço para escrever e manter, e os donos precisam cumprir suas promessas. Agentes elevam as apostas, porque um agente age sobre um contrato desatualizado com a mesma confiança com que age sobre um correto. O artigo desaconselha contratos para todos os dados: concentre-se nas interfaces em que uma equipe compartilha dados com outra, idealmente por meio de produtos de dados. Não faça tudo de uma vez. Comece onde dói, como problemas de qualidade, pipelines quebrados ou falhas de comunicação, ou especifique as interfaces de um novo caso de uso desde o início. Introduzir contratos é também uma mudança cultural em direção a assumir a posse dos dados, e tornar as coisas explícitas pode expor responsabilidades ausentes que precisam ser resolvidas antes. Os provedores são medidos por funcionalidades, então ajude-os com templates de plataforma, workshops com consumidores sobre os níveis de serviço de que precisam, confiabilidade demonstrada no marketplace e responsabilidade clara pelo alerta quando uma promessa quebra.

> ❌ **Anti-padrão — contratar todos os dados de uma vez:** lançar contratos para todos os conjuntos de dados em um único movimento. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** parece burocracia quando o contrato não resolve um problema atual. **Correção:** comece onde um problema dói, ou especifique as interfaces de um novo caso de uso desde o início.

## Por que os contratos de dados vão estar em toda parte
Os autores esperam que contratos de dados se tornem, para dados, o que o OpenAPI é hoje para APIs REST, por três razões: agentes tornam compensador escrevê-los bem, consumidores vão começar a escrever os seus próprios, e um padrão aberto os mantém portáveis.

## Na era dos agentes, o incentivo aponta para o lado certo
O esforço vai para o que pode ser automatizado. Schema e regras de qualidade recebem cuidado, enquanto campos em prosa, como nomes de negócio, descrições e termos de uso, recebem menos, porque só os humanos os leem. Agentes levam esses campos ao pé da letra: uma descrição vaga leva ao pipeline errado, uma cláusula de termos de uso ausente leva ao uso indevido, e uma finalidade ambígua leva a uma resposta confiantemente errada. A resposta do artigo é um ciclo de feedback: faça uma pergunta, veja uma resposta errada, corrija a descrição ou os termos, e pergunte de novo. Ele observa que o ODCS 3.2 acrescentou o bloco de contexto para IA, escrito para agentes.

> ❌ **Anti-padrão — preencher só as partes automatizáveis:** escrever com cuidado schema e regras de qualidade enquanto os campos em prosa ficam vagos. **Por que as pessoas fazem isso:** o artigo diz que o esforço vai para o que pode ser automatizado. **Consequência:** agentes agem sobre descrições e termos vagos. **Correção:** escreva os campos em prosa com o mesmo rigor, e teste-os fazendo perguntas a um agente.

## Contratos orientados pelo consumidor acrescentam a perspectiva que faltava
A confiança também precisa ir do consumidor para o provedor, por exemplo ao compartilhar dados sensíveis, o que os termos de uso cobrem. Ao aplicar contratos orientados pelo consumidor aos dados, um consumidor publica o próprio contrato de promessas e expectativas na mesma estrutura ODCS. Só a direção muda: onde o schema de um provedor declara uma promessa, o de um consumidor declara uma expectativa. Isso possibilita o contract-first a partir do lado do consumidor, em que ele especifica os dados de que precisa sem saber quem os fornece, e um agente pode pesquisar correspondências. Contratos de consumidores também mostram quais dados são usados em comparação com o que foi concedido, o que ajuda os provedores a otimizar seus serviços e a automatizar a governança.

### Contrato orientado pelo consumidor
Um contrato orientado pelo consumidor registra o que um consumidor precisa, e não o que um provedor promete. Aplicado a dados, o consumidor escreve um documento ODCS de expectativas, reaproveitando a estrutura do provedor. O mesmo formato dos dois lados permite automatizar a correspondência.

## Padrões abertos possibilitam a soberania sobre os metadados
Metadados são pequenos em volume, mas estrategicamente centrais: mapeiam todos os dados valiosos e sustentam cada catálogo, verificação de qualidade, ferramenta de linhagem e agente. Grandes fornecedores dão de graça suas ferramentas de metadados porque, quando os metadados estão em um formato proprietário, tudo o que os lê fica preso a eles. Como metadados ocupam tão pouco espaço, não há motivo de armazenamento para mantê-los proprietários, então fazê-lo é "puramente uma jogada de controle". Um padrão aberto é a defesa real: mantém os metadados portáveis, permite combinar ferramentas de fornecedores diferentes e coloca o formato sob uma governança neutra quanto a fornecedores, que o ODCS obtém por meio da Linux Foundation. Um teste simples de aprisionamento: você consegue exportar todos os contratos como arquivos simples para o Git e reconstruir seu catálogo em outro lugar só a partir deles? O OpenAPI mostra o mesmo para sistemas operacionais.

> ❌ **Anti-padrão — formato de metadados proprietário:** manter contratos e metadados de catálogo apenas no formato próprio de um fornecedor. **Por que as pessoas fazem isso:** o artigo atribui isso à vontade dos fornecedores de controlar os dados. **Consequência:** tudo o que lê os metadados fica preso ao fornecedor. **Correção:** use um padrão aberto e mantenha os contratos como arquivos simples no Git.

> 🔎 Contexto adicionado: O índice do artigo lista cinco barras laterais, mas o texto delas não está no artigo extraído, então não são cobertas nesta nota. Os agradecimentos citam 19 colaboradores e divulgam o uso de IA para brainstorming, esboços e revisão de rascunhos.

## 🧠 Modelos mentais
### O contrato é a página de detalhes do produto
**O modelo:** em um marketplace de dados, o contrato faz o papel de página de produto: permite ao consumidor julgar se os dados servem antes de pedir acesso.
**Como aplicar:** antes de publicar, verifique se um consumidor conseguiria decidir se os dados servem apenas pelo contrato.

### Promessas não verificadas são tigres de papel
**O modelo:** uma promessa só vale quando as ferramentas a verificam automaticamente.
**Como aplicar:** transforme cada regra de qualidade e cada SLA em um teste que rode no CI e agendado contra a produção.

### Coisas que mudam juntas ficam juntas
**O modelo:** o contrato muda junto com o pipeline que produz os dados, então ambos pertencem a um mesmo repositório.
**Como aplicar:** mantenha os contratos em uma subpasta ao lado do código do pipeline, e revise-os no mesmo pull request.

### Versione pelo grau de quebra
**O modelo:** uma versão principal marca uma mudança quebrável, uma versão menor acrescenta uma mudança compatível, e um patch corrige algo pequeno.
**Como aplicar:** classifique cada mudança de schema pela pergunta de se a leitura ou a consulta de um consumidor existente quebra, e então incremente a versão de acordo.

## 🔑 Principais conclusões
- Combinar dados de muitas equipes torna a confiança o gargalo, e agentes amplificam o risco porque tratam cada valor como verdade.
- Um contrato de dados é uma descrição legível por máquina, verificável, versionada e orientada pelo provedor, e a única fonte da verdade sobre os metadados.
- Regras como "menos de 1% de campos de nome vazios" e SLAs como "dentro de 24 horas" só importam quando testes automatizados as verificam.
- Mudanças quebráveis, como remover ou renomear uma coluna, exigem uma nova versão principal e um novo conjunto de dados, e a versão antiga é descontinuada por meio de um ciclo de vida de status.
- O padrão de compatibilidade backward do registry permite apagar um campo, o que quebra consumidores existentes; versões menores precisam de compatibilidade forward.
- Contratos pertencem ao Git ao lado do código do pipeline, e o CI/CD executa lint, verificações de mudanças quebráveis e testes antes da publicação no marketplace.
- Um acordo de acesso é uma concessão separada, bilateral e com prazo, que só passa a valer com a aprovação do provedor, e ainda não existe padrão para ele.

## 🏛️ Para o Kenji
Interpretação própria, não do autor: as partes de contexto para IA, as restrições e os termos de uso são o que os agentes de fato leem em um conjunto de dados, então funcionam como uma superfície de governança, semelhante a um model card, e merecem revisão, e não apenas documentação. Os autores alertam que os campos em prosa recebem menos rigor porque só os humanos os leem, o que sugere que a revisão de governança deveria checar a prosa do contrato com o mesmo rigor que o schema. A pergunta que isso levanta: quais políticas voltadas para IA na sua organização poderiam ser escritas como termos de uso legíveis por máquina e verificadas antes que um agente toque nos dados?

## 📚 Plano de leitura
### A partir do artigo
- Making Your Data Ready for Agentic AI (Pramod Sadalage e Prem Chandrasekaran) — a fonte do argumento de que "agentes tratam cada valor como verdade". O trecho não traz link de destino, então nenhuma URL é indicada.
- RFC for an Open Access Agreement Standard (projeto Bitol) — o formato proposto para registrar quem pode acessar qual porta, para qual finalidade e por quanto tempo. Não há URL no trecho.
- Building an "Amazon.com" for Your Data Products — a fonte dos sinais de confiança do marketplace, como resultados de testes publicados. Não há URL no trecho.
- Data Product Canvas — o canvas de workshop em que o contrato é uma parte central. Não há URL no trecho.
### Para ir além
- Open Data Contract Standard (Bitol, Linux Foundation) — a referência completa de campos e o JSON Schema para validação.
- Data Contract CLI — a ferramenta de código aberto para lint, verificações de mudanças quebráveis e testes de CI.
- Especificação OpenAPI — o modelo ao qual os autores comparam os contratos.
- Modos de compatibilidade do schema registry (forward, backward, full) — necessários para raciocinar sobre o versionamento de streams de eventos.
- Apache Ossie ontology standard — a camada semântica para vincular contratos a ontologias.
- Consumer-driven contract testing — a origem da ideia, aplicada pelo artigo aos dados, de um contrato do lado do consumidor.

## 🔗 Conceitos
- [api-versioning](/pt-br/concepts/api-versioning.md)
- [model-context-protocol](/pt-br/concepts/model-context-protocol.md)
