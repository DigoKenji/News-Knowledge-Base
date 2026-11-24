---
type: Article Summary
title: Data Contracts
description: Data contracts apply API-design discipline to datasets, giving people and AI agents verifiable, versioned metadata they can trust.
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
---

## 🎯 Core thesis
Data contracts apply API-design discipline to data: each dataset gets a versioned, machine-readable description of its schema, quality, service levels, ownership and terms of use, which people and AI agents can rely on and tooling can verify.

## 🧭 Why the author wrote it
Trust becomes the bottleneck when data from many teams is combined, and AI agents make the problem worse because they treat every value as truth. The authors, Simon Harrer (CEO of Entropy Data, on the steering committee for the Bitol standards ODCS and ODPS) and Arif Wider (professor at HTW Berlin, former Head of Data and AI at Thoughtworks Germany), argue that explicit, verified contracts fix this and will become for data what OpenAPI is for REST APIs.

## Why We Need Data Contracts
The article opens with Alex, a data engineer at an insurer building a model to predict which customers will buy more products. Alex must know whether the dataset has the needed information and meaning, what it connects to, how fresh and complete it is, whether its structure will stay stable, and whether GDPR and personal-data rules allow the processing and resharing of results. Trust covers the source (quality, reliability, legal status) and the recipients (whether they handle data responsibly). Contracts answer these questions continuously, not once.

## A Simple Example Data Contract
At its simplest, a data contract describes the shape of the data a provider offers, such as a schema. The examples use the Open Data Contract Standard (ODCS), which the article calls the de facto industry standard, governed by the Bitol project at the Linux Foundation and rated Adopt in the Thoughtworks Technology Radar.

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

**What this does:**
- `apiVersion` and `kind` declare an ODCS 3.2 data contract.
- `id`, `name`, `version` and `status` give the contract an identity and lifecycle; `active` and `1.0.0` signal a maintained, stable dataset.
- `physicalType` gives the technical type and `businessName` the business label of a column.
- `classification: "restricted"` is a governance attribute; the article notes that each column carries technical, business and governance details.

The article notes that a contract like this is more likely rendered as a page in a data marketplace than read as raw YAML.

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

**What this does:**
- The SQL quality rule computes the share of empty names; `mustBeLessThan: 0.01` turns it into a pass or fail check.
- `slaProperties` adds a freshness promise, measured on `customers.last_updated_at`.
- The article's point: the status and version show stability, but "actual trust comes when these guarantees are verified automatically."

## What Does This Mean for AI Agents?
Agents face Alex's questions on every task, with nobody to ask. A distrustful human hesitates or asks the owner; an agent, as Sadalage and Chandrasekaran are quoted, "treat[s] every value as truth", so it produces confidently wrong results and humans stop trusting it. A contract answers the questions in a form an agent can act on: discoverability in a marketplace agents can search, schema and semantics for correct joins, quality rules and SLAs that show how far to trust a result, and terms of use that show what is allowed.

## What's in a Data Contract
The article lists six properties of a data contract. It is machine-readable (structured YAML, not prose in a wiki), verifiable (promises that tooling can pass or fail), versioned (a scheme that separates harmless from breaking change), provider-driven (published one-sidedly by the producer), consumer-oriented (written for the consumer's decision), and the source of truth for metadata.

### Data contract
A data contract is a published description of a dataset's shape, semantics, quality guarantees, service levels and terms of use. The provider writes it and owns its promises, but it is written for the consumer, who uses it to decide whether the data fits and to rely on it over time. Catalogs, marketplaces, documentation and agents all read this one artifact rather than keeping divergent copies. Its promises must be concrete enough that tooling can check them.

## Open Data Contract Standard
ODCS defines a standardized YAML format for contracts. It is a Linux Foundation graduate project under Bitol, its public GitHub specification has over 1000 stars, and it ships a JSON Schema for validation. Figure 1 maps its sections; the article uses ODCS 3.2, the latest version at writing. An interactive walkthrough is on datacontract.com.

```yaml
apiVersion: "v3.2.0"
kind: "DataContract"
id: "customers"
name: "Customers"
version: "1.0.0"
status: "active"
```

**What this does:**
- `apiVersion` pins the standard version the document follows.
- `id` is a unique identifier; `name` is the readable label.
- `version` and `status` drive versioning and lifecycle, covered later in the article.

## Schema and Data Quality
The schema defines structure and semantics. Here it is a table, but the same structure describes Kafka messages, files in object storage or an API payload. Each column carries technical details, business semantics and governance attributes. The article's example is `annual_premium`, stored as an integer in cents to follow the company's money convention. Relationships such as foreign keys can also be declared.

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

**What this does:**
- `primaryKey`, `required: true` and `minimum: 0` declare the key, the mandatory fields and a non-negative premium.
- The `description` of `annual_premium` records the cents convention, which a consumer must know to read the number.
- `tags: "pii:true"` marks personal data; the article later uses such tags to drive masking.

### Data quality rule
A quality rule is an assertion that tooling can evaluate and pass or fail, not an aspiration. The article's types are SQL queries that compare a computed value to a threshold, and library metrics such as `invalidValues`. Thresholds can be absolute, like a minimum row count, or relative, like at most 1% empty names. Rules can sit at table or column level and are verified automatically with the right tooling.

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

**What this does:**
- The table-level SQL rule expects more than 100,000 rows.
- The column-level rule averages a 1-or-0 flag over NULL or blank names; the result must stay below 0.01.
- The `library` rule counts invalid statuses against an allowed list; `mustBe: 0` allows none.

## Ownership and Support Channels
Ownership is central. The team defines who owns and maintains the contract, with members and roles, and support channels tell consumers how to reach them. The article calls this reachable responsibility critical to building trust.

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

**What this does:**
- `team` names the owning group and an member with the `Owner` role.
- `support` gives the Slack channel and URL that consumers use to ask questions.
- The `username` is a contact address, so the contract carries the owner's identity.

## Terms of Use and SLAs
Terms of use tell consumers what they can and cannot do with the data. The standard covers purpose, usage guidelines and limitations, and can link external policies such as privacy policies or licences. SLAs state guarantees in measurable form, such as retention and freshness. The freshness promise is checked against `last_updated_at` on every test, and a breach alerts the owning team.

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

**What this does:**
- `purpose`, `usage` and `limitations` tell the consumer the intended use and that only active-policy customers are included.
- `authoritativeDefinitions` links an external GDPR policy as a business definition.
- `slaProperties` express retention (5 years) and freshness (24 hours on a named element).

## Location of the Data
Servers say where the data lives, including connection details but no credentials. The standard covers platforms such as Snowflake, Databricks, BigQuery, Oracle and Postgres, event streams such as Kafka, Kinesis and Pub/Sub, files on S3, and CSV files on SFTP, with a custom type as fallback. Environments are encoded as separate server entries.

```yaml
servers:
  - server: "production"
    type: "snowflake"
    environment: "prod"
    account: "insurance-corp"
    database: "prod_lifeinsurance"
    schema: "dp_customers_v1"
```

**What this does:**
- `type` and `environment` identify the platform and stage.
- `account`, `database` and `schema` locate the Snowflake schema without any credentials.

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

**What this does:**
- For an event stream, the server names the cluster host and the message format (`avro`).
- The schema describes a `topic` instead of a table, so the same structure covers streams.

## Roles & Security
Roles describe how consumers get access. Each role names an IAM role on the data platform and the kind of access it grants. Providers often define one role with personal data and one without. The contract does not list who holds a role; that is decided per consumer when an access request is approved.

```yaml
roles:
  - role: "customers_reader"
    access: "read"
    description: "Read access to all customer data"
```

**What this does:**
- `role` is the platform role name, and `access: "read"` is the permission it grants.
- The contract defines the role; granting it to a person happens later, outside the contract.

## AI Context
AI context is guidance written for agents. `instructions` act as a system prompt for the dataset, `constraints` state what an agent must not do, and `verifiedStatements` pair common questions with a checked query so an agent reuses a correct answer instead of guessing. The article notes that ODCS 3.2 added this block.

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

**What this does:**
- The folded `instructions` tell the agent that premiums are in cents and must be divided by 100.
- The `constraint` forbids individual decisions such as pricing a policy or assessing a claim.
- `verifiedStatements` pairs a business question with a SQL answer the provider has checked.

## Pricing the Use of Data
The price expresses an internal charge that reflects the data's value. In practice it is usually empty or passes on what access costs on the platform. The article's example is 10 USD per terabyte scanned.

```yaml
price:
  priceAmount: 10
  priceCurrency: "USD"
  priceUnit: "per TB scanned"
```

**What this does:**
- `priceAmount` and `priceCurrency` give the amount and currency.
- `priceUnit` states the basis: per terabyte scanned.

## Built-in Extensibility
The standard has two extension mechanisms. `customProperties` at every level carry metadata the standard does not know, and vendors use the same mechanism to add functions. `authoritativeDefinitions` link the contract, or any element in it, to definitions elsewhere, including open standards. The advice is to prefer official standard properties and keep custom ones to a minimum.

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

**What this does:**
- `customProperties` records the legal entity, which the standard has no field for.
- `authoritativeDefinitions` points to the OpenAPI specification of the REST API serving the same data.

> ❌ **Anti-pattern — custom-property sprawl:** adding custom properties for information the standard already covers. **Why people do it:** the article does not say. **Consequence:** not stated in the article. **Fix:** use official standard properties first, and keep custom ones to a minimum.

## Data Contracts in Context
Contracts gain value through their connections. The article names three: link columns to a domain model instead of restating their meaning, put contracts on the ports of data products, and let the contract be the product page from which consumers request access.

## Linking Data Contracts to Ontologies
Documenting each dataset separately is not enough when agents combine sources; semantics must be defined across sources in an overarching layer. Combining ontologies with contracts solves two problems. **Metadata de-duplication:** a concept such as Customer ID is defined once, with its examples, classification and description, and contracts link to it. **Semantic consistency:** a consumer can tell whether two similarly named columns mean the same thing, and whether two differently named ones do, which matters beyond matching keys.

### Ontology link
A contract maps into an ontology by pointing an element at an ontology concept through `authoritativeDefinitions`, identified by an IRI or URL. The ontology can be expressed in RDF or OWL, or with the ontology standard in the Apache Ossie project, which was incubating at the Apache Software Foundation. Figure 2 shows two contracts linking properties to one concept, so a consumer knows how to join them.

```yaml
schema:
  - name: "customer_360"
    properties:
      - name: "cid"
        authoritativeDefinitions:
          - type: "semantics"
            url: "https://example.com/ns/Customer.id"
```

**What this does:**
- `type: "semantics"` ties the column `cid` to an ontology element rather than restating its meaning.
- The `url` names the concept by IRI, so every contract linking to it shares the same definition.

## Data Products Build Upon Data Contracts
Data contracts pair with data products, which bring product thinking to data and are central to data mesh. Contracts are the interface description language that makes a product's ports explicit and verifiable. Since 2025, the Open Data Product Standard (ODPS) describes product metadata on top of ODCS. Figure 3 shows a product with one input port and three output ports.

### Data product as architectural quantum
A data product is the whole unit: the code and pipeline, the data, and the metadata that describes them. The article calls it an architectural quantum, a self-contained unit whose input and output ports are ODCS interfaces. The output-port contracts remain central because they hold the promises that many parties depend on. Shared ownership and support information can move into the product description, reducing duplication.

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

**What this does:**
- `inputPorts` references the upstream `kafka_customer_events` contract at version 1.0.0.
- `outputPorts` lists three ports; two share the `customers_pii_snowflake` contract at versions 1.0.0 and 2.0.0, so two major versions run side by side.
- `apiVersion` (v1.1.0) is the ODPS version, separate from the ODCS version used in the contracts.
- Indentation is reproduced as the source extract has it: the first line is flush left, the others are indented.

## Data Contracts are the Product Details Page in a Data Marketplace
In a data marketplace, data becomes shoppable, and the contract is the product details page: it gives consumers what they need to decide whether the data fits their purpose. A consumer requests access with a processing purpose that matches the terms of use. That request is the "buy" click. Figure 4 shows the contract as central for provider, consumer, governance and the platform. Figure 5 shows an AI agent requesting access, being approved and receiving short-lived access.

### Access agreement
The request creates an access agreement made of the requested contract, the stated purpose and the requesting consumer. It takes effect only when the provider approves, and only then does the platform grant access. The article contrasts it with the contract: a contract is one-sided, names no consumer and has no time; an agreement is bilateral, has a start date and optional expiry, and for agents can last only a few minutes. No standard exists yet; the Bitol project's RFC for an Open Access Agreement Standard (OAAS) proposes recording who may access which output port, for what purpose and for how long.

> ❌ **Anti-pattern — confusing the contract with the access agreement:** treating the published contract as if it granted access. **Why people do it:** the article does not say. **Consequence:** not stated in the article. **Fix:** keep the one-sided contract for promises and a separate, approved, time-bound agreement for access.

Published test results add trust signals to the marketplace, for example freshness met on 29 of the last 30 days.

## Working with Data Contracts
The article asks for the rigor of an API: create the contract deliberately, keep it next to the code, evolve it without breaking consumers, and check it automatically.

## Data-first vs. Contract-first
With data-first, data is already shared, for example as warehouse tables. You represent its existing structure in a contract and add the implicit promises iteratively. The open-source Data Contract CLI can turn SQL DDL, JSON Schema or Avro Schema into a first draft. Once published, the contract becomes the source of truth and the data must follow it: if a column is renamed, the contract test fails and the rename is handled as a breaking change. With contract-first, the conversation comes before any code, ideally a facilitated workshop between provider and consumers. Drafts can live on a whiteboard, in a YAML editor, in the Data Product Canvas, in an ODCS-compliant Excel template or in the open-source Data Contract Editor. The end state is a standard-compliant contract, refined until all parties agree, that the pipeline is then built to deliver.

> ❌ **Anti-pattern — regenerating the contract after a rename:** rerunning generation so the contract matches the changed table. **Why people do it:** the article does not say. **Consequence:** not stated; the article says the rename must be treated as a breaking change. **Fix:** let the contract test fail and handle the rename as a breaking change.

## Its Natural Habitat is a Git Repository
Wherever a contract is drafted, its natural habitat is a Git repository, beside the data product YAML, the code that produces the data and the CI/CD configuration. Contracts sit in a dedicated subfolder. The authors advise against placing them far from the pipeline code, because the pipeline usually has to change together with the contract.

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

**What this does:**
- The product YAML sits at the root; contracts live in `datacontracts/` and pipeline code in `code/`.
- File names combine dataset (`customers_pii`), technology (`snowflake`) and major version (`v1`), but the major version is not in the ID, so versions can live side by side.
- `.gitlab-ci.yml` at the root shows that the pipeline configuration lives in the same repository.

> ❌ **Anti-pattern — storing contracts far from pipeline code:** keeping contracts separate from the code that produces the data. **Why people do it:** the article does not say. **Consequence:** the pipeline usually has to change with the contract, so separation invites mismatch. **Fix:** keep them together and change them in one pull request.

## The Story of Versioning and Breaking Changes
Contracts follow SemVer. A major change, such as removing or renaming a column, is breaking; a minor change, such as adding a column, is non-breaking; a patch, such as fixing a typo or updating support channels, is small. A breaking change ships as a new major version next to a new dataset, is phased in beside the old one, and the old one is phased out after a grace period. The status field signals the stage: `draft` while in development, `active` once promises are binding, then `deprecated` and `retired`. With data products, the lifecycle often follows the product release, with each major version offered as its own output port.

### Semantic versioning for contracts
SemVer splits a version into major, minor and patch numbers, and the article gives each a contract meaning. Consumers can tell a harmless change from a breaking one by reading the number. The major version is tied to the dataset it protects, so a breaking change means a new dataset rather than a silent edit. The status runs alongside the version to show whether a version is being phased in, active, or phased out.

For event streams this maps onto schema registry compatibility modes. A minor version must be forward compatible, so existing consumers keep reading new events, and ideally fully compatible, so new consumers can replay old events. Beware the registry default: backward compatibility permits deleting a field, which is exactly what breaks existing consumers. Anything the registry would reject is a major version, published as a new topic.

> ❌ **Anti-pattern — relying on the registry's default compatibility:** accepting a field deletion as a routine minor change because the default allows it. **Why people do it:** the article does not say. **Consequence:** existing consumers break. **Fix:** require forward compatibility for minor versions, and publish anything the registry rejects as a new major version on a new topic.

## Automate All The Things
A contract that nobody checks is a paper tiger, worth no more than a wiki page. The article's automation list:
- Generate HTML documentation and make it available on the intranet.
- Publish to metadata repositories such as data product marketplaces or catalogs.
- Test the test environment against the contract in CI/CD on each pull request, and block the merge on failure.
- Test production every day, and inform the owner and downstream consumers of broken promises.
- Generate SQL DDL so provider tables are contract-compliant by design.
- Derive masking and access policies, such as masking columns tagged `pii:true` for roles without personal-data access.
- Generate consumer queries that avoid `select *`.
- Compare a new contract version with the old one in a pull request, and fail a breaking change in a minor or patch update.
- Validate new data before it reaches consumers, and hold back data that violates the contract.

The open-source Data Contract CLI, with over 1 million downloads per month, natively understands ODCS and supports these tasks. The Thoughtworks Technology Radar rates it Trial (November 2025), especially useful for validation, breaking-change detection and CI/CD governance.

### Quarantine pattern
Validate new data against the contract before consumers see it, and hold back data that violates the contract. The article names the pattern as part of the automation list and gives no further detail.

> ❌ **Anti-pattern — unchecked contract:** keeping the contract as documentation that no test runs against. **Why people do it:** the article does not say. **Consequence:** the first time a consumer finds a broken promise, their trust is gone. **Fix:** turn every promise into an automated test.

## Data Contracts Belong in Your CI/CD Pipeline
Every commit is checked before it ships. The CLI has a command per stage: `datacontract lint` validates the contract, `datacontract breaking` catches breaking changes against the previous version, and `datacontract ci` tests the data against the contract in each environment. Only when all stages pass does the pipeline deploy to production and publish the contract with `datacontract publish`. A second, scheduled pipeline runs `datacontract test --server prod` on a cron and publishes results, informing providers and affected consumers as soon as a promise breaks. Figure 6 shows the flow: checked on every commit, and tested against production every night.

## The Costs and Benefits of Data Contracts
Contracts pay back in trust and automation, but they are not free and are not worth writing for every dataset. Agents amplify both sides of that trade-off.

### The Benefits
When analytical data is described by contracts, it can be tested automatically and monitored continuously, and counter-actions can trigger when quality drops below a promised threshold. Because consumer expectations are written down, breaking changes can be detected and can stop a deployment, instead of propagating into downstream services. Ownership shifts toward providers, which builds trust between providers and consumers, business and IT, and humans and the agents acting for them.

## Generate Your Data Pipeline
Data contracts enable spec-driven development. The contract is the core of the specification: given to a coding agent along with a plugin of skills that describe how pipelines are built in the company, it can generate almost the whole pipeline. Testing the data against the contract during generation gives the agent feedback that steers it. Figure 7 shows this flow, and the article points to an example run on dataproduct.com.

### Spec-driven development
The contract is treated as the specification that drives code generation, rather than documentation written after the code. Testing the generated pipeline against the contract closes the loop, so each run is corrected by the same promises that consumers depend on.

## Chat With Your Data
The hard part of chatting with data is not generating SQL but generating the correct SQL. An agent facing hundreds of tables and thousands of columns must guess column semantics and join logic, and must also decide whether it may answer at all. It first has to find the data. Contracts published to a marketplace that agents can search, for example through MCP, make the data discoverable. Once found, the contract stops the guessing: it gives column meaning, join paths through semantic links, and the terms of use.

### Model Context Protocol
> 🔎 Added context: The Model Context Protocol is an open standard that lets AI applications connect to external tools, files, databases and APIs through one common interface.

In the article, MCP is the route by which agents search a data marketplace and find contracts.

## The Cost
Contracts take time and effort to write and maintain, and owners must keep their promises. Agents raise the stakes, because an agent acts on an outdated contract as confidently as on a correct one. The article advises against contracts for all data: focus on interfaces where one team shares data with another, ideally through data products. Do not roll out everything at once. Start where it hurts, such as quality issues, broken pipelines or miscommunication, or specify the interfaces of a new use case from the start. Introducing contracts is also a cultural shift toward owning data, and making things explicit can expose missing ownership that must be fixed first. Providers are measured on features, so help them with platform templates, workshops with consumers on the service levels they need, reliability shown in the marketplace, and clear ownership of the alert when a promise breaks.

> ❌ **Anti-pattern — contracting all data at once:** rolling out contracts for every dataset in one push. **Why people do it:** the article does not say. **Consequence:** it feels like bureaucracy when a contract does not solve a current problem. **Fix:** start where a problem hurts, or specify a new use case's interfaces from the start.

## Why Data Contracts Will Be Everywhere
The authors expect data contracts to become for data what OpenAPI is for REST APIs today, for three reasons: agents make it worthwhile to write them well, consumers will start writing their own, and an open standard keeps them portable.

## In the Age of Agents, the Incentive Points the Right Way
Effort goes to what can be automated. Schema and quality rules get care, while prose fields such as business names, descriptions and terms of use get less, because only humans read them. Agents take those fields at their word: a vague description leads to the wrong pipeline, a missing terms-of-use clause leads to misuse, and an ambiguous purpose leads to a confidently wrong answer. The article's answer is a feedback loop: ask a question, see a wrong answer, fix the description or terms, and ask again. It notes that ODCS 3.2 added the AI context block, written for agents.

> ❌ **Anti-pattern — filling only the automatable parts:** writing schema and quality rules carefully while leaving prose fields vague. **Why people do it:** the article says effort goes to what can be automated. **Consequence:** agents act on vague descriptions and terms. **Fix:** write prose fields with the same rigor, and test them by asking an agent questions.

## Consumer-Driven Data Contracts Add the Missing Perspective
Trust must also run from consumer to provider, for example when sharing sensitive data, which the terms of use cover. Applying consumer-driven contracts to data, a consumer publishes its own contract of promises and expectations in the same ODCS structure. Only the direction changes: where a provider's schema states a promise, a consumer's states an expectation. This enables contract-first from the consumer side, where a consumer specifies the data it needs without knowing who provides it, and an agent can search for matches. Consumer contracts also show which data is used compared with what was granted, which helps providers optimize their service and automate governance.

### Consumer-driven contract
A consumer-driven contract records what a consumer needs, not what a provider promises. Applied to data, the consumer writes an ODCS document of expectations, reusing the provider's structure. The same format on both sides lets matching be automated.

## Open Standards Enable Metadata Sovereignty
Metadata is small in volume but strategically central: it maps all valuable data and underlies every catalog, quality check, lineage tool and agent. Large vendors give metadata tools away free because, once metadata is in their proprietary format, everything that reads it is locked in. Since metadata is so small, there is no storage reason to keep it proprietary, so doing so is "purely a play for control". An open standard is the real defense: it keeps metadata portable, lets teams combine tools from different vendors, and puts the format under vendor-neutral governance, which ODCS gets through the Linux Foundation. A simple test of lock-in: can you export all contracts as plain files into Git and rebuild your catalog elsewhere from them alone? OpenAPI shows the same for operational systems.

> ❌ **Anti-pattern — proprietary metadata format:** keeping contracts and catalog metadata only in a vendor's own format. **Why people do it:** the article attributes it to vendors wanting control over the data. **Consequence:** everything that reads the metadata is locked in. **Fix:** use an open standard and keep contracts as plain files in Git.

> 🔎 Added context: The article's contents list five sidebars, but their text is not in the extracted article, so they are not covered in this note. The acknowledgments thank 19 contributors and disclose AI assistance for brainstorming, outlines and draft review.

## 🧠 Mental models
### The contract is the product details page
**The model:** In a data marketplace the contract does the job of a product page: it lets a consumer judge whether the data fits before asking for access.
**How to apply it:** Before publishing, check that a consumer could decide fit from the contract alone.

### Unchecked promises are paper tigers
**The model:** A promise counts only when tooling checks it automatically.
**How to apply it:** Turn every quality rule and SLA into a test that runs in CI and on a schedule against production.

### Things that change together live together
**The model:** The contract changes with the pipeline that produces the data, so both belong in one repository.
**How to apply it:** Keep contracts in a subfolder beside the pipeline code, and review them in the same pull request.

### Version by breaking-ness
**The model:** A major version marks a breaking change, a minor version adds compatible change, and a patch fixes something small.
**How to apply it:** Classify each schema change by whether an existing consumer's read or query breaks, then bump the version to match.

## 🔑 Key takeaways
- Combining data from many teams makes trust the bottleneck, and agents amplify the risk because they treat every value as truth.
- A data contract is a machine-readable, verifiable, versioned, provider-driven description, and the single source of metadata truth.
- Rules such as "less than 1% empty name fields" and SLAs such as "within 24 hours" matter only when automated tests check them.
- Breaking changes, such as removing or renaming a column, take a new major version and a new dataset, and the old version is phased out through a status lifecycle.
- The registry default of backward compatibility permits deleting a field, which breaks existing consumers; minor versions need forward compatibility.
- Contracts belong in Git beside pipeline code, and CI/CD runs lint, breaking-change checks and tests before publication to the marketplace.
- An access agreement is a separate, bilateral, time-bound grant that takes effect only on provider approval, and no standard for it exists yet.

## 🏛️ For Kenji
Interpretation, not the author's: the AI context, constraints and terms of use are the parts of a dataset that agents actually read, so they work as a governance surface, similar to a model card, and deserve review rather than just documentation. The authors warn that prose fields get less rigor because only humans read them, which suggests governance review should check contract prose as strictly as schema. The question this raises: which AI-facing policies in your organization could be written as machine-readable terms of use and checked before an agent touches the data?

## 📚 Reading plan
### From the article
- Making Your Data Ready for Agentic AI (Pramod Sadalage and Prem Chandrasekaran) — the source of the "agents treat every value as truth" argument. The extract has no link target, so no URL is given.
- RFC for an Open Access Agreement Standard (Bitol project) — the proposed format for recording who may access which port, for what purpose and for how long. No URL in the extract.
- Building an "Amazon.com" for Your Data Products — the source for marketplace trust signals such as published test results. No URL in the extract.
- Data Product Canvas — the workshop canvas in which the contract is a central part. No URL in the extract.
### To go further
- Open Data Contract Standard (Bitol, Linux Foundation) — the full field reference and JSON Schema for validation.
- Data Contract CLI — the open-source tool for lint, breaking-change checks and CI tests.
- OpenAPI specification — the model the authors compare contracts to.
- Schema registry compatibility modes (forward, backward, full) — needed to reason about event-stream versioning.
- Apache Ossie ontology standard — the semantic layer for linking contracts to ontologies.
- Consumer-driven contract testing — the origin of the consumer-side idea the article applies to data.

## 🔗 Concepts
- [api-versioning](/concepts/backend-systems/api-versioning.md)
- [model-context-protocol](/concepts/ai-ml/model-context-protocol.md)
