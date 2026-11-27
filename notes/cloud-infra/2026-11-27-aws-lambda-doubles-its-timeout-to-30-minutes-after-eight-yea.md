---
type: Article Summary
title: AWS Lambda Doubles Its Timeout to 30 Minutes after Eight Years at 15
description: AWS doubled Lambda's timeout to 30 minutes, but queues, streams and concurrency limits must be checked before
  the new value is safe.
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
---

## 🎯 Core thesis
AWS doubled the maximum timeout of on-demand Lambda event functions from 15 to 30 minutes, but the queues, streams, concurrency and connection limits around each function decide whether a longer run is safe.

## 🧭 Why the author wrote it
The 15-minute ceiling had held since 2018, and AWS names agentic AI requests that chain model and tool calls, media transcoding, Monte Carlo simulations and report generation as workloads that outgrew it. The change is a per-function setting, but the authors argue that everything wired to the function determines whether a longer run is safe.

## The change and where it applies
The new limit covers synchronous, asynchronous and event source mapping invocations in all commercial regions, at standard pricing and without an opt-in. The 15-minute ceiling followed a 60-second limit at launch in 2014 and five minutes from 2015.

Lambda rejects any value above 1,800 seconds with a validation error. The article shows the change as one CLI call:

```bash
aws lambda update-function-configuration \
    --function-name my-demo-function \
    --timeout 1800
```

**What this does:**
- `aws lambda update-function-configuration` changes the settings of an existing function.
- `--function-name my-demo-function` selects the function; the name is the article's placeholder.
- `--timeout 1800` sets the limit in seconds, which is 30 minutes and the maximum Lambda accepts.
- The change is made per function, so each function needs its own update.

The change covers only one of three Lambda shapes. Web Functions keep 15 minutes, Lambda MicroVMs allow eight hours, and on Lambda Managed Instances synchronous invocations now get 30 minutes while asynchronous ones keep the 90 minutes introduced in September.

### Serverless functions
Lambda is a serverless function service: you upload code, AWS runs it when a trigger fires, and you do not provision or manage servers. The provider scales capacity with demand, so the settings you control are things like memory and timeout. The timeout is the maximum time one invocation may run before Lambda stops it.

### Event source mapping
An event source mapping is the Lambda resource that reads from a source such as a queue or stream and invokes the function with batches of records. The new limit applies to these mappings too. This is where the timeout meets the rest of the system, which the third section covers.

## Capacity comes first
The authors name capacity as the first thing to check:

> Concurrency equals requests per second multiplied by average duration in seconds. At the same request rate, a function that runs for 30 minutes instead of 15 needs twice the concurrency. For example, 1 request per second at 30 minutes each needs 1,800 concurrent executions.

### Concurrency
Concurrency is the number of executions running at the same moment. The article's formula is requests per second times average duration in seconds, so the same traffic held for twice as long needs twice the concurrent executions. Its example, 1 request per second at 30 minutes each, needs 1,800 concurrent executions.

> 🔎 Added context: this formula is a form of Little's law, which relates average concurrency, arrival rate and time spent in the system.

## What sits around the function
The rest of the post covers the systems around the function:
- An SQS visibility timeout must be at least six times the function timeout, which means three hours at 30 minutes.
- Kinesis and DynamoDB Streams read one batch per shard at a time, so a slow function slows the shard.
- Amazon MQ, DocumentDB and Kafka mappings created before November 19 stay at 14 minutes.
- A NAT gateway still drops idle connections after 350 seconds.
- The Init phase keeps its 10-second limit.

### Message queue
A message queue lets senders put messages on a queue and lets consumers take them later, so the two sides do not need to be available at the same moment. With Lambda, the queue feeds the function through an event source mapping, and the queue's settings must fit the function's timeout. The next section covers the visibility timeout.

### Batch vs stream processing
Streams are ordered, unbounded sequences of records, and Kinesis and DynamoDB Streams are read shard by shard. The article's point is that the function processes one batch per shard at a time, so a function that takes longer per batch makes the whole shard fall behind.

### NAT gateway
A NAT gateway lets private resources reach the internet. The article says it drops idle connections after 350 seconds, so a function that waits on a quiet connection for longer may find that the connection is gone.

### Init phase
The Init phase is the startup work Lambda does before the handler runs. The article says it keeps its 10-second limit even though the invocation timeout has doubled, so heavy startup work stays capped.

## The SQS rule, tested
Wibo van der Sluis, an independent AWS practitioner, tested the SQS rule in eu-west-1. He raised a function to 1,800 seconds behind a queue with a 90-minute visibility timeout, then lowered the queue to 60 seconds. Both changes were accepted with the mapping still enabled, while a new mapping from the same queue was refused.

> So the check exists, but only at creation. One more thing the test showed: at creation, Lambda compared the queue with the function timeout itself, not with six times it. I did not test where exactly that line sits, so treat the six times as guidance you enforce yourself.

### Visibility timeout
The visibility timeout is how long a received SQS message stays hidden from other consumers while one consumer processes it. If processing runs past that window, the message can become visible again and be delivered a second time. The article asks for at least six times the function timeout, and it does not explain where that factor comes from.

### Dead-letter queue
A dead-letter queue receives messages that could not be processed after a set number of attempts, so they stop blocking the main queue. Retries multiply the wait: van der Sluis notes that an asynchronous function which times out three times has run for 90 minutes before the event reaches a dead-letter queue.

> ❌ **Anti-pattern — raising the timeout for slow code behind an API:** increasing the function timeout to 30 minutes so that slow code finishes. **Why people do it:** it is one configuration value per function, as the article says. **Consequence:** van der Sluis concludes that 30 minutes is the wrong fix for slow code behind an API. **Fix:** fix the slow code; the article offers no other fix here.

> ❌ **Anti-pattern — trusting the creation-time check:** assuming Lambda will refuse an unsafe queue and timeout pairing whenever one changes. **Why people do it:** a validation error exists, which suggests the rule is always enforced. **Consequence:** in the test, both changes were accepted while the mapping stayed enabled. **Fix:** enforce the six-times rule in your own deployment checks.

## Practitioner reactions
Luc van Donkersgoed, principal engineer at NS and AWS Serverless Hero, writes:

> Hell froze over. AWS Lambda now supports 30-minute timeouts. I am not sure how I feel about this.

Magnus Eriksson, architect and former AWS employee, notes that Lambda "is anyhow quite expensive per CPU second compared to EC2" and would still reserve it for short-lived spiky loads. Matthew Bonig, AWS DevTools Hero and co-author of The CDK Book, looked further ahead: "Eventually it'll be 8 hours, and then nobody will be able to understand when to use it vs. MicroVMs."

On Reddit, the most-upvoted reply welcomed the change because it avoids Fargate and ECS for hour-long batch jobs. The answer under it was that Fargate is beautifully simple. One commenter sets out where the longer limit fits:

> A 30 minute run still bills for the full duration at your configured memory, so check the cost per invocation before moving batch style work here, and keep the handler idempotent with a dead letter queue, because a failure or timeout partway through a long run can leave partial work behind. For multi step work, long running jobs, or anything that needs clear state and retries, Step Functions with Fargate or ECS is still usually cleaner.

### Cost per invocation
Lambda bills for the time a run takes at the memory you configure, so a 30-minute run costs far more per invocation than a short one. Magnus's CPU-second remark points the same way. The Reddit commenter's advice is to check cost per invocation before moving batch-style work here.

### Idempotency
Idempotency means that repeating an operation has the same effect as doing it once. The Reddit commenter advises keeping the handler idempotent, because a failure or timeout partway through a long run can leave partial work behind. This is a commenter's advice, not the article's.

## Competitive context and open questions
Joseph Emison, CTO of Branch Insurance, reads the move as the Lambda team recognising that Cloud Run is its most compelling competitor. Cloud Run services allow 60 minutes per request, and Azure Functions on the Flex Consumption and Premium plans enforce no maximum, with a 60-minute grace period during scale-in. AWS has not said whether the Lambda limit will move again.

### Cloud Run and Azure Functions limits
Cloud Run runs containers on demand, and the article's comparison uses its 60-minute per-request limit. Azure Functions' Flex Consumption and Premium plans have no stated maximum timeout.

> 🔎 Added context: scale-in is the removal of instances as load falls, and the grace period gives running work time to finish.

## 🧠 Mental models
### The timeout chain
**The model:** A function's timeout is one link in a chain of timeouts, and the links around it must be sized from it. The article's example is the SQS visibility timeout, set at six times the function timeout.
**How to apply it:** List every queue, stream, connection and startup limit that touches the function, and check each one against the new value before deploying.

### Concurrency is rate times duration
**The model:** Concurrent executions equal request rate times average duration in seconds, so a longer runtime needs proportionally more capacity.
**How to apply it:** Before changing a timeout, multiply the expected request rate by the new duration and compare the result with the concurrency you can actually run.

### Retries multiply the wait
**The model:** Retries and timeouts multiply. An asynchronous function that times out three times has run for 90 minutes before its event reaches a dead-letter queue.
**How to apply it:** Compute the worst-case time to failure, which is the timeout times the number of attempts, and decide whether that delay is acceptable.

## 🔑 Key takeaways
- The limit for on-demand event functions doubled to 30 minutes across synchronous, asynchronous and event source mapping invocations, with no opt-in.
- Concurrency equals requests per second times average duration, so 1 request per second at 30 minutes each needs 1,800 concurrent executions.
- SQS visibility timeouts must be at least six times the function timeout, which means three hours at 30 minutes.
- Kinesis and DynamoDB Streams read one batch per shard at a time, so a slow function slows its whole shard.
- The doubled limit applies to one of three Lambda shapes: Web Functions keep 15 minutes and MicroVMs allow eight hours.
- Van der Sluis's test shows the queue rule is checked only when a mapping is created, so you must enforce it yourself.

## 🏛️ For Kenji
*This is your interpretation, not the author's.* Moving toward a Staff Architect role, the useful habit here is to treat a timeout change as a platform decision and trace every queue, stream and connection the function touches before approving it. The article names agentic AI chains of model and tool calls as a driver, which is exactly the kind of workflow where a longer ceiling changes the design of the whole path. Which timeout in your own event-driven or AI workflows would break first if one link were raised?

## 📚 Reading plan
### From the article
- [AWS Lambda Doubles Its Timeout to 30 Minutes after Eight Years at 15](https://www.infoq.com/news/2026/11/aws-lambda-30-minute-timeout/) — the source article; the extracted text contains no other links, so read it for the reactions and the SQS test.

### To go further
- Amazon SQS visibility timeout and its interaction with Lambda event source mappings — to see where the six-times guidance comes from.
- Little's law — the queueing identity behind the concurrency formula.
- Step Functions with Fargate or ECS — the alternative a Reddit commenter recommends for multi-step, long-running work.
- Idempotent handler design for queue-driven functions — how to make partial work safe to retry.

## Concepts
- [serverless](/concepts/cloud-infra/serverless.md)
- [message-queue](/concepts/backend-systems/message-queue.md)
- [dead-letter-queue](/concepts/backend-systems/dead-letter-queue.md)
- [idempotency](/concepts/backend-systems/idempotency.md)
- [batch-stream-processing](/concepts/data/batch-stream-processing.md)
- [scalability](/concepts/backend-systems/scalability.md)
- [visibility-timeout](/concepts/backend-systems/visibility-timeout.md)
