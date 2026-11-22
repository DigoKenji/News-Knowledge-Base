---
type: Article Summary
title: How United Airlines streams database changes to Apache Iceberg with Amazon Data Firehose
description: A serverless CDC pipeline from Aurora MySQL into Iceberg gives near-real-time data that several query engines
  can read.
resource: https://aws.amazon.com/blogs/architecture/how-united-airlines-streams-database-changes-to-apache-iceberg-with-amazon-data-firehose
tags:
- data
- dead-letter-queue
- change-data-capture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/how-united-airlines-streams-database-changes-to-apache-iceberg-with-amazon-data-firehose
  title: How United Airlines streams database changes to Apache Iceberg with Amazon Data Firehose
  author: AWS Architecture
status: stable
source_url: https://aws.amazon.com/blogs/architecture/how-united-airlines-streams-database-changes-to-apache-iceberg-with-amazon-data-firehose
source: AWS Architecture
source_type: blog
published: '2026-11-18'
domain: data
concepts:
- dead-letter-queue
- change-data-capture
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d8141800adc0b590c2faa
---

## 🎯 Core thesis
A serverless change data capture pipeline from Aurora MySQL into Apache Iceberg tables on Amazon S3 can replace stale batch loads of reference data with near-real-time data that several query engines can read, and the Firehose buffer interval lets you trade latency against cost.

## 🧭 Why the author wrote it
United Airlines needed reference data from its operational Aurora MySQL database to reach analysts within minutes while it migrated to a new analytics platform. The legacy batch process could not keep pace, so analysts worked from stale market data and business decisions were delayed. If the thesis holds, a managed streaming pipeline closes that gap without a custom application or servers for the delivery step.

## Introduction and scope notes
The opening has no heading in the original. It introduces United Airlines, which the article describes as the world's largest airline by revenue passenger miles, and the problem above. United's pipeline sends database changes from Aurora MySQL through Kinesis Data Streams, Lambda, Firehose and Iceberg on S3. The reported results are these. Queries scanning more than 10 million rows no longer hit the legacy SQL bottlenecks. Several hundred business analysts can run complex queries over a broader data domain. Average query latency improved 5x in United's testing. Because those queries run inside downstream AWS Glue ETL jobs, total job processing time dropped 2x while billed Glue run time went down. Once landed, the data is queryable from AWS Glue, Amazon Redshift and Amazon Athena, and any Iceberg-compatible engine can read it.

### Change data capture (CDC)
CDC is the practice of reading a database's change log and emitting each insert, update and delete as an event, instead of periodically copying whole tables. A consumer applies those events to a copy of the data, so the copy stays current with far less work than a full reload. It matters when source changes must reach analysts within minutes. In this architecture the CDC engine is AWS DMS, which reads the MySQL binary log.

### Amazon Kinesis Data Streams
Kinesis Data Streams is a managed, durable stream that sits between the CDC engine and the delivery service. It buffers change events, so source replication and downstream processing are decoupled. It can run in on-demand mode, which scales capacity automatically as change volume grows. The template instead uses four provisioned shards with 24-hour retention, which the article says are not sized for any specific throughput.

### Amazon Data Firehose
Firehose reads change events from the stream, batches them, optionally transforms each batch with Lambda, writes Parquet files to the Iceberg table, applies each insert, update and delete, and commits a new snapshot. The article's point is that you build no separate application and manage no servers for that delivery step. Its batching is governed by a buffer size and interval, covered under the processing section below.

### Apache Iceberg
Iceberg is an open table format whose data files and metadata live on object storage. The article credits it with ACID transactions and time travel, which it calls useful for regulatory requirements. Because the tables are open and cataloged in the AWS Glue Data Catalog, any Iceberg-compatible engine can read them. The template requires format-version 2 and a unique key.
> 🔎 Added context: An Iceberg table records each commit as a snapshot, so readers see a consistent state. Time travel reads an earlier snapshot, which is what the article means by time travel.

### Amazon S3 and Parquet
S3 holds the Iceberg data and metadata files. The data is stored in columnar Parquet format, which the article credits with durability, scalability and low storage cost. United's tables now hold over 2 TB. Storage is not the only cost: every update also writes a delete file, which adds S3 PUT charges.

### AWS DMS Serverless
DMS Serverless runs the replication without replication-instance sizing or management, and the walkthrough uses it for that reason. United itself runs standard provisioned DMS. The serverless mode has limits: it does not support custom CDC start points, so a replication stopped longer than the binlog retention window needs a full reload. It also runs inside your VPC with no public IP.

**Note on this walkthrough.** The article grounds the walkthrough in United's pre-production build but simplifies it.
- CDC engine: any engine that can write change events to Kinesis and supports your source works. Only the Lambda transform is engine-specific, so it is rewritten when you change engines. The downstream pipeline stays the same.
- Query engine: United's analysts mainly use Redshift Serverless. The walkthrough validates only with Athena.
- Scope: the CloudFormation template is a representative starting point, not the full production solution.
- Operational settings: the template uses demo values. It logs at debug level, uses four provisioned shards with 24-hour retention, and sets the Firehose Iceberg buffer to 1 MB or 3 seconds with a 300-second retry duration. A short buffer commits a snapshot and delete files every few seconds.
- Single table: the template replicates one source table into one Iceberg table. More tables require extending it.
- Schema evolution: not covered by the walkthrough (see the concept below).

### Schema evolution
Schema evolution means handling source changes made with DDL statements over time. The article says Firehose handles some of these itself. If a source record has a new field that is not in the table, but all other column names and data types match, Firehose skips the new field. If column names or types do not match, Firehose returns an error and sends the record to the S3 error bucket. At United, schema evolution is handled outside this pattern, not in the Lambda transform.

> ❌ **Anti-pattern — handling DDL in the transform:** putting schema changes inside the Lambda function that rewrites each record. **Why people do it:** the article does not say; the transform is the one code the pipeline owns. **Consequence:** the article says United keeps schema evolution outside the pattern, so the transform stays engine-specific. **Fix:** keep schema change handling in a separate process, as United does.

## Solution walkthrough
The walkthrough implements near-real-time CDC from Aurora MySQL to Iceberg with managed services. CloudFormation automates the replication pipeline, while you configure the source and destination endpoints by hand. The data flow has seven stages. The CDC engine reads the Aurora MySQL binary log. Kinesis Data Streams receives and buffers the events. Firehose reads the stream and batches records. Firehose invokes a Lambda function that is coded for the CDC engine's record format. Firehose writes Parquet files to the Iceberg table in S3. Firehose commits each write as a new Iceberg snapshot, updating the table's metadata pointer in the Glue Data Catalog. Athena and Redshift Serverless look up metadata in the catalog and read the data files from S3. The article refers to Figure 1 for this pipeline, but the figure is not in the extracted text.

## Data ingestion and streaming
The CDC engine captures changes with minimal impact on the source database. Changes stream continuously into Kinesis Data Streams, which provides durable, scalable real-time streaming and buffers between source replication and downstream processing. Firehose consumes the stream and batches records based on the configured buffer size and interval, which reduces infrastructure overhead.

### Firehose buffer interval
The buffer interval sets the trade-off between latency and cost. In a low-volume test of about 120 rows, United set a 1-second interval and saw end-to-end latency as low as 10 seconds. For an initial load of about 1.5 million rows, United used a 60-second interval to balance latency, cost and file size. That load finished in about 2 hours 22 minutes, which is about 176 rows per second. The article says United measured these latencies manually against average row throughput, so treat them as indicative.

## Processing and storage
Lambda and Firehose process the stream in near real time. Lambda parses each CDC record, extracts the operation (INSERT, UPDATE or DELETE), and converts the record to an Iceberg-compatible format with the right operation field. Firehose then commits the writes as Iceberg snapshots. Both services scale with the stream. The fixed limits are the template's four Kinesis shards and its DMS capacity range of 1 to 2 DCUs.

### AWS Lambda transform
The Lambda function is the only engine-specific piece of the pipeline. It passes each row through and adds the destination table and operation that Firehose needs to route it. Firehose uses that metadata to insert, update or delete the row in the right Iceberg table. Because the function parses one engine's record format, you rewrite it whenever you change CDC engines. The template runs it on Python 3.14.

### ACID transactions and optimistic concurrency control
Firehose commits each write as an ACID transaction, meaning atomic, consistent, isolated and durable, using optimistic concurrency control. Table metadata updates automatically with each commit. This is why readers see whole batches or nothing. The article does not describe conflict handling beyond this, so do not assume concurrent writers are safe without checking.

### Merge-on-read
Firehose applies CDC operations with merge-on-read. An UPDATE is written as a delete file plus a new data file, and a DELETE is written as a delete file. The query engine merges these at read time to resolve the current row state. The mode requires the Iceberg table to be format-version 2 and a unique key, which the template sets on the delivery stream.

### DMS partition key
DMS sends each record to Kinesis with a partition key. The template defines no DMS partition-key rule, so DMS uses the schema and table name. Every change for the table then lands on one shard, and the other three shards stay idle. Throughput is limited to what one shard supports. The fix is to set the partition key to the table's primary key in a DMS object-mapping rule. That spreads the load and keeps per-row order.

> ❌ **Anti-pattern — default partition key:** leaving DMS on the schema-and-table partition key, so one table's change stream lands on one shard. **Why people do it:** the article does not say why; the template ships without the rule. **Consequence:** three of four shards sit idle and throughput is capped at one shard. **Fix:** set the partition key to the table's primary key in an object-mapping rule.

United pre-created its Iceberg tables with Athena, and those tables now hold over 2 TB of data.

## Metadata and analytics
The article describes three query surfaces over the same tables. AWS Glue Data Catalog is the central metadata repository. It stores Iceberg table metadata, schema versions and partition information, and supports schema evolution and governance across the organization. Redshift Serverless queries the same tables through the catalog. Athena provides pay-per-query SQL for ad hoc analysis and data validation.

### AWS Glue Data Catalog
The catalog is the shared metadata layer. Iceberg tables are registered here, and the pointer to the current table metadata moves with each Firehose commit. Engines look up the table in the catalog and then read the data files from S3. The article also suggests adding business context to the catalog to help AI agents discover data.

### Amazon Redshift Serverless
Redshift Serverless reads the Iceberg tables in two ways. You can reference the table through the auto-mounted awsdatacatalog database, or create an external schema on the Glue database. United's analysts mainly use Redshift Serverless.

### Amazon Athena
Athena queries the same tables through the catalog. It needs no infrastructure to manage, and you pay per query. In the walkthrough's test, an UPDATE and a DELETE at the source returned the correct current rows from both Athena and Redshift Serverless.

## Solution creation
The creation steps come in order: prerequisites, then Steps 1 to 10. They mix console work, AWS CLI commands and one CloudFormation deployment.

## Prerequisites
> 📋 **Checklist — Prerequisites** (items abridged to their requirement; full wording is in the article)
> - [ ] An Aurora MySQL cluster with binary logging enabled.
> - [ ] A Secrets Manager secret holding the Aurora connection details, created manually, with its ARN noted.
> - [ ] An IAM access role that DMS can assume to read that secret.
> - [ ] A VPC with private subnets, security group rules allowing port 3306, and a Kinesis VPC interface endpoint and a Secrets Manager VPC interface endpoint.
> - [ ] Permission to create the DMS endpoint and replication, Kinesis stream, Lambda, Firehose, log groups, a KMS key, three IAM roles, and the stack, with CAPABILITY_NAMED_IAM.
> - [ ] AWS CLI installed and configured.

The article adds that the Aurora-managed secret omits host and port and will not work with DMS, so the secret must be created manually. The access role's trust policy allows the regional DMS service principal, for example dms.us-east-1.amazonaws.com, and its permissions policy grants secretsmanager:GetSecretValue on the secret's ARN. Because the stack creates IAM roles, the deploying identity also needs iam:CreateRole, iam:PutRolePolicy and iam:PassRole.

### VPC interface endpoints
DMS Serverless runs inside your VPC with no public IP, and the replication runs privately with no internet access. So the walkthrough creates a Kinesis Data Streams interface endpoint and a Secrets Manager interface endpoint. Both need private DNS. The Aurora cluster's own security group must also allow inbound MySQL on TCP 3306 from the replication's security group, because the cluster sits outside the stack.

> ❌ **Anti-pattern — Aurora-managed secret for DMS:** pointing DMS at the secret that Aurora creates. **Why people do it:** it is the default secret Aurora provides. **Consequence:** the article says it omits host and port, so DMS will not work with it. **Fix:** create a secret with username, password, host and port, and reference its ARN.

## Step 1: Configure source database
The article assumes an existing Aurora MySQL cluster, tested on version 3.08.0, which is compatible with MySQL 8.0. This step is not automated.

The first part sets binary logging. In a custom DB cluster parameter group, set binlog_format to ROW, binlog_row_image to FULL and binlog_checksum to NONE. These come from the cluster parameter group and cannot be set with SET GLOBAL. Associate the group with the cluster, then reboot the writer, because binlog_format is static.

### Binary log and ROW format
The binary log records the changes made to the database, and CDC reads it. ROW format records the actual row changes rather than the SQL statements. FULL row image records every column, so each change event carries the full row. Together they give the CDC engine enough data to build correct inserts, updates and deletes.

### Binlog retention window
The retention window is the period the database keeps the binary log. The article sets it to 24 hours with a stored procedure and says it should be longer than the longest time replication might be stopped. Aurora MySQL version 3 allows up to 2,160 hours, which is 90 days. DMS Serverless does not support custom CDC start points, so a replication that stays stopped beyond the window must restart with a full reload.

After the reboot, check that log_bin is ON, binlog_format is ROW, binlog_row_image is FULL and binlog_checksum is NONE. The log_bin check matters most, because it confirms binary logging is active, not just configured. If log_bin is OFF, confirm the parameter group is attached and reboot the writer again.

The article notes the walkthrough uses the Aurora admin user for both this SQL and the DMS source endpoint. In production the article recommends a dedicated replication user with only REPLICATION CLIENT, REPLICATION SLAVE and SELECT on the source table.

> ❌ **Anti-pattern — admin user for CDC:** using the database admin account as the DMS source user. **Why people do it:** it is the simplest option for a walkthrough, and the article says it uses it for simplicity. **Consequence:** the replication holds far more privilege than it needs. **Fix:** create a least-privilege user with only the three listed privileges.

## Step 2: Create CDC engine source endpoint
The source endpoint is created by hand in the DMS console, before the stack is deployed. The article's settings are: source endpoint type, identifier mysql-source-endpoint, source engine Amazon Aurora MySQL, the writer endpoint as server name, and port 3306. For credentials, reference the Secrets Manager secret and the access role, which is recommended. You can instead enter the username and password directly.

### SSL mode
The walkthrough leaves SSL mode at none, which is the default for Aurora MySQL endpoints. The article says that leaves the connection between DMS and Aurora unencrypted. For production it recommends verify-ca or verify-full, with the Amazon RDS CA certificate attached. On Aurora MySQL 8.4, require_secure_transport is on by default and rejects SSL mode none, so SSL must be enabled there.

> ❌ **Anti-pattern — demo SSL setting:** leaving SSL mode at none for production. **Why people do it:** it is the default and the walkthrough keeps it. **Consequence:** the change stream crosses the network unencrypted, and on 8.4 the connection is rejected outright. **Fix:** set verify-ca or verify-full and attach the RDS CA certificate.

Save the endpoint ARN for the DmsSourceEndpointArn stack parameter. The secret's ARN goes in DmsSourceSecretArn.

## Step 3: Set up VPC infrastructure
The VPC with private subnets is assumed and not created by CloudFormation. First-time DMS users must create a service role named exactly dms-vpc-role, with a trust policy for dms.amazonaws.com and the AWS-managed policy AmazonDMSVPCManagementRole. Without it, subnet group creation fails with an "IAM Role dms-vpc-role is not configured properly" error. The console usually creates this role automatically, but the CLI and API do not.

### DMS subnet group
The subnet group needs at least two private subnets in different Availability Zones. Its ARN has the form arn:aws:dms:<region>:<account-id>:subgrp:<subnet-group-name>, and you save it for the stack.

### DMS security group
The replication's security group allows inbound HTTPS on 443 from the VPC CIDR, so the replication can reach the interface endpoints. Outbound allows HTTPS on 443 to AWS services and MySQL on 3306 to the Aurora cluster. The default all-traffic outbound also works. Then add an inbound MySQL rule on the Aurora cluster's security group from this group.

Both interface endpoints show Pending briefly, then Available.

## Step 4: Create destination infrastructure
These resources must exist before deployment: an S3 bucket, a Glue Data Catalog database, and the Iceberg table. Athena needs a results bucket first. The commands in items 1 and 2 are AWS CLI commands. The bucket name is a placeholder, amzn-s3-demo-bucket, because S3 names are global. Use your own name in the Glue table location and in the S3BucketName parameter.

The Glue database, market_db, is a different resource from the Aurora database of the same name. The names match by convention only. The Iceberg table is created with Athena DDL, which registers it in the Glue Data Catalog.

The article warns that if the table's S3 location is registered with Lake Formation, you must create the table before registering it. Athena does not support DDL on Iceberg tables whose location is registered with Lake Formation.

## Step 5: Deploy CloudFormation stack
The template provisions the Kinesis stream, DMS Serverless replication, Lambda function, Firehose delivery stream, IAM roles and CloudWatch logs. It does not create the source endpoint, VPC infrastructure, S3 bucket, Glue database or Iceberg table. The stack's resources are a Kinesis stream with four shards and 24-hour retention, DMS Serverless replication at 1 to 2 DCUs in full-load-and-CDC mode, a Kinesis target endpoint, the Lambda function, a Firehose delivery stream with an Iceberg destination, IAM roles, a KMS key for log encryption, and log groups.

### CloudFormation
You create the stack in the console by uploading the template from aws-samples, naming it iceberg-replication-stack, and setting parameters. These include DmsSourceEndpointArn, DmsSubnetGroupArn, DmsVpcSecurityGroupId, DmsSourceSecretArn, the DMS source table market_rule, Glue database market_db, Glue table market_rule, IcebergUniqueKey rule_id, and your S3 bucket. You then acknowledge IAM resource creation. The stack reaches CREATE_COMPLETE when it is done.

### Lake Formation
If the account uses Lake Formation permissions for the Glue catalog instead of the IAM-only default, the Firehose role must be authorized before deployment. Otherwise stack creation can fail. You either grant the Firehose role Lake Formation ALL on market_db.market_rule, or switch the database and table to IAM-based access by granting IAM_ALLOWED_PRINCIPALS ALL. The article says the second option weakens Lake Formation governance, because IAM policies alone then control access. It also says that, in production, you should grant only the minimum permissions needed.

> ❌ **Anti-pattern — IAM_ALLOWED_PRINCIPALS shortcut:** granting IAM_ALLOWED_PRINCIPALS ALL so stack creation succeeds. **Why people do it:** it is the quickest way past a failing stack deployment. **Consequence:** Lake Formation governance is weakened, because IAM alone controls access. **Fix:** grant only the minimum Lake Formation permissions the Firehose role needs.

## Step 6: Validate CDC engine replication
Start the DMS task from the console and choose Start task. You can leave the premigration assessment on or turn it off. Serverless capacity provisioning can take up to about 40 minutes on first start, and the task shows "Provisioning capacity" during that time. The article says this is normal, not a failure.

### DMS table statistics
In the replication's detail view, check three things. Load state should read "Table completed". The full-load row count should match the source table. CDC operations should show ongoing INSERT, UPDATE and DELETE activity. The article's example shows market_rule with Table completed and Total rows 2.

Then run SELECT * FROM market_db.market_rule in Athena and check that the row count matches the DMS full-load count of 2. The article says this confirms the rows reached the Iceberg table, not only the Kinesis target.

## Step 7: Generate a CDC event
With full load complete and the task showing "Load completed, replication ongoing", insert a test row into the source Aurora MySQL database. The CDC engine reads the change from the binary log and streams it through the pipeline. The following steps check each stage. The insert statement is not in the extracted text.

## Step 8: Verify Lambda transformation
In the Lambda console, open the function named [stack-name]-lambda-processor and check its Monitor tab. Invocations should be registered, errors should be 0, and the success rate should be 100%. The logs should show INSERT, UPDATE or DELETE detection and successful transformation with no errors. The article refers to Figure 9, which is missing from the extracted text.

## Step 9: Monitor Firehose delivery
In the Firehose console, open the stream's Monitoring tab. Incoming records should show data flowing from Kinesis. Delivery to Iceberg should match incoming records, with Failed at 0. The article highlights DataFreshness. It should stay steady, and a rising value means Firehose is falling behind. Once Firehose has delivered the record, query for it in Athena. The new record appearing in the Iceberg table confirms near-real-time replication.

The article suggests an optional test: update one row and delete another at the source, then query both Athena and Redshift Serverless with SELECT * FROM awsdatacatalog.market_db.market_rule, and confirm both return current rows.

## Step 10: Set up monitoring dashboard
This step is optional. Build a CloudWatch dashboard with these metrics:
- DMS CDCLatencySource and CDCLatencyTarget, which show how far replication lags the source and the target.
- Kinesis GetRecords.IteratorAgeMilliseconds, which shows how far Firehose is behind the stream.
- Lambda Invocations and Errors for the transform function.
- Firehose DeliveryToIceberg.DataFreshness and DeliveryToIceberg.FailedRowCount.

The article says to set alarm thresholds from your own latency requirements.

## Operational monitoring
After deployment, United set up procedures to keep Iceberg tables performing. Each Firehose commit creates new data files, and each update also writes a delete file, so a busy table accumulates many small files that slow reads. Compaction fixes that, either through automatic compaction in the Glue Data Catalog or a scheduled Athena OPTIMIZE. Storage is reclaimed with Athena VACUUM, which expires old snapshots and removes orphan files. United also monitors replication health and has a defined procedure to restart and reconcile data if a task stops or fails.

> ❌ **Anti-pattern — no table maintenance:** running the pipeline without compaction or VACUUM. **Why people do it:** the pipeline works on day one, so maintenance is easy to defer. **Consequence:** small files and delete files pile up and slow reads. **Fix:** schedule OPTIMIZE or enable catalog compaction, and run VACUUM to expire snapshots.

## Cost considerations
The article gives us-east-1 list prices at the time of writing and asks readers to check current rates.
- DMS Serverless: $0.0819 per DCU-hour for Single-AZ and $0.1639 for Multi-AZ. The template is Multi-AZ at 1 to 2 DCUs, so roughly $0.16 to $0.33 per hour while replication runs.
- Kinesis Data Streams: $0.015 per shard-hour, or $0.06 per hour for four shards, plus $0.014 per million PUT payload units.
- Lambda: billed per request and duration.
- Firehose: $0.045 per GB for Iceberg delivery from Kinesis (first 250 TB per month). Extra bytes added by the Lambda transform are billed at the same rate.
- S3: storage plus PUT requests. Each update's delete file adds PUT charges.
- VPC endpoints: the Kinesis and Secrets Manager interface endpoints are billed hourly per Availability Zone, plus $0.01 per GB processed.
- Table maintenance: Glue compaction or Athena OPTIMIZE add Glue or Athena charges.

## Cleanup
> 📋 **Checklist — Cleanup order**
> - [ ] Stop the DMS Serverless replication and wait until it shows Stopped. DMS can delete a serverless replication only when it is stopped or failed.
> - [ ] Delete the CloudFormation stack. This removes the replication, Kinesis target endpoint, Kinesis stream, Lambda, Firehose stream, IAM roles, KMS key and log groups.
> - [ ] Delete the manual resources: the DMS source endpoint, the Kinesis and Secrets Manager interface endpoints, the DMS subnet group and security group, the Glue table and database, the S3 bucket (empty it first, including the firehose-errors/ and processing-failed/ prefixes), the Secrets Manager secret and its access role, any Lake Formation grants added for Firehose, dms-vpc-role if no other DMS resource uses it, and the Aurora cluster and parameter group if you do not want to keep them.

The article's verification step is to confirm the CloudFormation stack shows as deleted, and that the console shows no billable resources still active.

## Conclusion
The article's conclusion is that this serverless architecture gives near-real-time replication from Aurora MySQL to Iceberg, with end-to-end latency as low as about 10 seconds. Latency can be traded against cost through the Firehose buffer interval. Pay-per-use components keep spend tied to usage. Iceberg adds ACID transactions and time travel, and AWS Glue, Amazon Redshift and Amazon Athena can query the same data. United used the result to expand its Iceberg use for business analytics and to give several hundred users of its Revenue Management data platform near-real-time reference data.

## 🧠 Mental models
### The buffer interval is a latency dial
**The model:** Firehose batches records for a set interval before committing, so each commit costs money and each batch adds delay. Short intervals cut latency and raise commit and file overhead; long intervals do the reverse.
**How to apply it:** Set the interval per table from its latency target, and check file size and cost, as United did when it moved from 1 second in testing to 60 seconds for the large initial load.

### The engine-specific edge
**The model:** Only the Lambda transform knows the CDC engine's record format. Everything from Kinesis through Iceberg is engine-neutral.
**How to apply it:** Keep engine parsing in one function, so a change of CDC engine touches one component.

### The partition key decides both load and order
**The model:** The partition key determines which shard a change lands on. Setting it to the primary key spreads load across shards while keeping changes to one row in order.
**How to apply it:** Choose the table's primary key in the DMS object-mapping rule instead of accepting the default.

## 🔑 Key takeaways
- A DMS-driven CDC pipeline into Kinesis, Firehose and Iceberg delivered end-to-end latency as low as about 10 seconds in United's tests.
- Firehose commits each batch as an ACID Iceberg snapshot and does the delivery without a custom application or servers.
- The Firehose buffer interval sets the latency-cost trade-off: United used 1 second for a 120-row test and 60 seconds for a 1.5 million row load.
- Iceberg with merge-on-read needs format-version 2 and a unique key, and each update writes a delete file.
- The default DMS partition key sends a table's whole change stream to one shard, and the fix is to use the primary key.
- Demo settings (debug logging, four shards, 1 MB or 3-second buffer, SSL none) are not production settings; the article lists what to review first.
- Lake Formation governance should be kept, not bypassed with IAM_ALLOWED_PRINCIPALS, and table maintenance must be scheduled.

## 🏛️ For Kenji
This is the pattern for a near-real-time reference-data feed into a multi-engine Iceberg platform: an architect would weigh the buffer interval as a latency-cost decision, the partition key as a throughput decision, and the Lake Formation shortcut as a governance decision. Your interpretation, not the author's: the article keeps schema evolution outside the pipeline on purpose. Who owns that boundary when the source schema changes, and does your governance model cover it?

## 📚 Reading plan
### From the article
- Setting up binary log replication (no URL in the extracted text) — the binlog settings and retention rules that Step 1 depends on.
- AWS DMS Serverless limitations (no URL in the extracted text) — the start-point and deletion limits that shape the replication runbook.
- Considerations and limitations for Apache Iceberg tables (no URL in the extracted text) — the Firehose schema-evolution and buffering rules to review before production.
- Changing the default settings for your data lake (no URL in the extracted text) — the Lake Formation defaults that can break stack creation.
- Using SSL with AWS Database Migration Service (no URL in the extracted text) — the certificate setup for production endpoints.
### To go further
- Merge-on-read versus copy-on-write in Apache Iceberg — the write-mode trade-off behind the delete files this pipeline writes.
- Kinesis partition keys and shard hot spots — how the key choice decides whether shards share load.
- Iceberg table maintenance (compaction and snapshot expiry) — how to keep small files and delete files from slowing reads.

## Concepts
- [Dead-letter queue](/concepts/dead-letter-queue.md)
- [Change data capture](/concepts/change-data-capture.md)
