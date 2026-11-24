---
type: Article Summary
title: Probing DNS at Cloudflare
description: Cloudflare extended its Flamingo testing framework to probe DNS propagation continuously, and uses those SLO
  measurements to gate releases.
resource: https://blog.cloudflare.com/probing-dns-at-cloudflare
tags:
- cloud-infra
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://blog.cloudflare.com/probing-dns-at-cloudflare
  title: Probing DNS at Cloudflare
  author: Cloudflare
status: stable
source_url: https://blog.cloudflare.com/probing-dns-at-cloudflare
source: Cloudflare
source_type: blog
published: '2026-11-23'
domain: cloud-infra
concepts:
- control-plane
- canary-deploy
- slo-error-budget
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d814e84b6f9d9508f2e14
---

## 🎯 Core thesis
Probing DNS continuously from every data center, against production, with the same modular testing framework used for HTTP lets SLO measurements decide whether a release is promoted or rolled back.

## 🧭 Why the author wrote it
The story starts with a real incident: a scheduling bug in the service that publishes DNS records to Cloudflare's edge left some record changes unpublished for hours. The service stayed up and healthy, but new hostnames resolved slowly and certificates could not be issued until the bug was fixed. If the thesis is right, DNS regressions are caught automatically by the same kind of measurement that gates other releases, before they spread.

## The opening: a release that broke production
The article has no heading for its opening, so the title here is a label added for the note. It starts with a familiar scenario: you ship a release, the unit tests pass, the dashboards are green, and then your phone starts blowing up because production is broken. The Cloudflare DNS team has lived this. The bug was a scheduling problem that left record changes sitting unpublished for hours. Nothing looked down in the usual sense, yet the failure was real for customers.

The author then explains why this class of failure is hard to catch. DNS failures are slow, spread across the network, and partial in where they cause impact. The goal they set is to test DNS the way customers experience it: "constantly, from every data center, and against production."

### DNS failure modes
DNS maps hostnames to records, and a fault in it rarely crashes a service outright. Instead it shows up as answers that are slow or stale, in some places and not others. The article's point is that the delay is built in, because a change has to travel to the authoritative servers and then through caches before everyone sees it. A service can therefore look healthy from the inside while new names or certificates fail for customers. That is why the author wants measurement from the outside rather than checks inside the service.

### Measuring from the customer's side
The probes run against production, not a copy of it. The measurement is what a customer would see from each location, such as whether a record is fresh enough to resolve. This is the foundation for the later SLO sections, where the same probes feed release decisions.

## A little background
Cloudflare is focused on performance and safety for customers, so its code has to be tested constantly, both at scale and against the real edge its users run on. Flamingo was built for that purpose: to measure the behavior of production and to confirm that code in lower-tier environments behaves as expected. The article notes that an earlier post covered Flamingo's use in CDN stack upgrades, but that this is only a small part of how it is used internally. One of its features is a modular architecture that lets teams teach it other network protocols, and DNS is the protocol this post is about.

### Testing at scale against the real edge
When a system is widely distributed, requests take many paths and cross several systems before reaching the origin. Testing at that scale is harder than testing one component, and the article's argument is that constant automated tests against real systems, with the outcomes measured, are needed to judge a service's health.

## What is Flamingo?
Flamingo began as a tool for complex, multi-step HTTP testing, and its scope has grown considerably since. It was built from the start for Cloudflare's infrastructure. It has a control plane and scheduler that let teams target specific environments, data centers, or percentages of metals (the article's word; most likely bare-metal machines, given the next sentence). It also integrates with custom reporting, metrics and analytics, and it has to perform predictably on bare-metal servers in resource-constrained environments.

### Control plane and scheduler
The control plane decides which work runs, where and when, while the workers execute it. In Flamingo, this is the layer that lets a team point a test at one environment, one data center, or a fraction of machines. Separating it from execution is what makes targeted, canary-style runs possible without each team building its own scheduling. The article describes this as a core part of Flamingo's infrastructure design.

### Unit tests versus end-to-end testing
Unit tests check code in isolation and do not show how requests behave across real pathways. The article says plainly that unit testing alone is not nearly enough at scale. The point is to measure the outcomes of real requests against real systems and use those outcomes to decide whether a service is healthy.

> ❌ **Anti-pattern — Unit tests as the only safety net:** Shipping a release on the strength of passing unit tests and green dashboards. **Why people do it:** not discussed in the article; unit tests are fast and familiar, so they feel sufficient. **Consequence:** the opening incident shows the cost, since the break reached production with every test green. **Fix:** run constant automated tests against the real system and measure the outcomes.

## Being delightful
The author wants internal tooling to be easy to use but powerful enough for over 80% of use cases. A new edge acceptance-test framework also has to push teams toward common guidelines and reusable scripts, in an environment that is stable, scalable, and manages complexity. The author's view is that debugging a test is the opposite of delightful, and the design choices below follow from that.

> ❌ **Anti-pattern — Tests written in a general-purpose language:** Letting each team write tests as ordinary programs in a traditional language. **Why people do it:** not discussed in the article; general-purpose languages are flexible and familiar. **Consequence:** when a test fails, the engineer must debug both the upstream system under test and the test code itself. **Fix:** use a constrained format for tests, as Flamingo does with its DSL.

### Setup-request-response DSL
Flamingo uses a custom domain-specific language built around a simple setup, request, and response format. The article's example makes sure all unencrypted traffic is redirected to its encrypted counterpart:

```text
TEST test-redirect
SET $hostname www.cloudflare.com
GET "http://$(hostname)/"
ASSERT STATUS IS 301
ASSERT HEADER Location IS "https://$(hostname)/"
```

**What this does:**
- `TEST test-redirect` names the test.
- `SET $hostname` stores the host once, and the `GET` line builds its URL from it with `$(hostname)`, so the host is defined in one place.
- `GET "http://..."` is the request step, sent over plain HTTP so the redirect behavior is exercised.
- The two `ASSERT` lines are the response step: a 301 status, and a `Location` header that points to the HTTPS version of the same host.
- The format is declarative, so a failure points at the outcome rather than at program logic.

> 🔎 Added context: HTTP status 301 means a permanent redirect. The `Location` header tells the client where to go instead.

The article says this five-line script can run anywhere on Cloudflare's edge and scale to thousands of requests per minute. Millions of data points from such runs can be turned into a graph of how the service actually performs in the real world.

### Modules
The DSL's middle step is normally a request. The article's move is to treat that step as an action instead, so Flamingo stops being an HTTP tool and becomes a system that can perform any action whose outcome you want to measure. These pluggable pieces are called modules. The author has put real effort into making them easy for anyone to write.

Flamingo is written in Rust because of its consistent performance characteristics and strong type system. The article says the type system suits modularity: strongly typed enumerations for input and output define the action runner's contract.

### Typed action dispatch
Each protocol gets a variant of an action enum for its inputs, and a matching output enum for its results. A handler type is an async function from an action, a shared context and a config to a result, and a lookup function maps an action to the handler that performs it. The article's Rust code, as printed:

```rust
pub enum Action {
  Query {
    method: String,
    url: Expression,
    parameters: Vec<http::QueryParameters>,
  },
  DnsQuery {
    query_name: Expression,
    query_type: Expression,
    parameters: Vec<dns::DnsQueryParameters>,
  },
}
pub enum ActionOutput {
  Http(http::FetchResponse),
  Dns(dns::QueryResponse),
}
pub type ActionHandlerFunction = Arc<
  dyn Fn(
      Action,
      Arc<Ctx>,
      InterpreterConfig,
    )
      -> BoxFuture<'static, Result<ActionOutput>>
    + Send
    + Sync
    + 'static,
>;
pub type ActionHandlerLUT = Arc<
  dyn Fn(&Action) -> ActionHandlerFunction
    + Send
    + Sync
    + 'static,
>;
```

**What this does:**
- `Action` is a closed set of variants: `Query` carries an HTTP method, URL and parameters, and `DnsQuery` carries a query name, query type and DNS parameters.
- `ActionOutput` mirrors it, returning either an `Http` or a `Dns` response, so each handler's result type is checked at compile time.
- `ActionHandlerFunction` is a boxed, shareable async function, which lets handlers be stored and called independently of one another.
- `ActionHandlerLUT` maps an action to its handler, so the runner picks the right implementation from the action's type.
- The `Arc`, `Send` and `Sync` bounds let handlers be shared safely across threads. This is a Rust detail the article does not explain.

The author's summary is that the logic for performing an action is decoupled into a dynamically dispatched function that can be called independently of other actions. Test scheduling and resource management are handled at a higher level, so a module author only has to focus on the logic.

The module handler for HTTP is shown next, as an excerpt:

```rust
pub async fn query(
  action: Action,
  ctx: Arc<Ctx>,
  cfg: InterpreterConfig,
) -> Result<ActionOutput> {
  let Action::Query {
    method,
    url,
    parameters,
  } = action
  else {
    bail!("Invalid action for module-http::query: {action:?}");
  };
  ...
}
pub async fn execute_module<F>(
  ctx: Arc<Ctx>,
  cfg: &InterpreterConfig,
  handler_lut: F,
) -> Result<ActionOutput>
where
  F: (Fn(&Action) -> ActionHandlerFunction),
{
  let func = handler_lut(action);
  let action_out = (func)(action.clone(), ctx.clone(), cfg.clone()).await?;
  ...
}
```

**What this does:**
- `query` destructures the `Query` variant with `let ... else`, and bails with an error if it receives any other action, so a module only handles its own action type.
- The `...` marks the elided implementation in the excerpt.
- `execute_module` takes the lookup function, finds the handler for the action, and calls it with the action, context and config.
- In the excerpt as printed, `action` is used in `execute_module` without being bound as a parameter, so the snippet is illustrative rather than complete.
- The design keeps the module focused on one action's logic, while the runner owns scheduling and resources.

## Using Flamingo to probe DNS
The article opens this section with the adage that when issues arise, it is always DNS, and notes there is truth behind the joke. DNS is central to the Internet, and because it depends on a network of interconnected sources of truth, a single misstep can cause a storm of failures. It is finicky because it is hierarchical: changing a record in a Cloudflare zone means propagating it to the authoritative servers so the updated record becomes widely available.

### DNS record propagation
Records are stored in caches that sit closer to users, to reduce latency. Propagation is the time it takes for a changed record to reach the authoritative servers and then the caches that serve queries. Delays matter because time-sensitive operations, such as certificate issuance and traffic failover, depend on the new record being visible.

### TTL and caching
Each record has a time to live (TTL) that tells the local cache how long it may keep the record before refetching it from the authoritative server. A long TTL means changes take longer to appear, which is why the article treats propagation delay as something to measure rather than assume.

### Authoritative servers and public resolvers
The probe measures two things: how quickly a change reaches the authoritative nameservers, and how quickly it appears through the public resolver 1.1.1.1. These are separate checks because the two hops have different freshness expectations in the article's design.

The DNS team added a DNS module rather than a new probing service. Because the framework already handled scheduling, reporting and pipeline integration, the team got those capabilities "for free." The team also did not need to manage a new probing service or integrate one with reporting.

The probe works like this. It continuously updates a TXT record with a timestamp, then queries three authoritative nameserver IPs and 1.1.1.1 from every data center. It asserts freshness within 90 seconds for the authoritative servers and within 600 seconds for the resolver.

### DNS propagation test
The article's example probes one authoritative nameserver directly:

```text
TEST test-propagation-delay-direct-108-162-193-62
MOD dns
SET $hostname "some.probe.domain"
SET $authNs 108.162.193.62:53 # amir.ns.cloudflare.com
QUERY $hostname TXT
TARGET $authNs
ASSERT DNS.RCODE IS NOERROR
ASSERT DNS.FLAGS CONTAINS AA
ASSERT DNS.ANSWERS[0] GT SUBTRACT(TIMESTAMP(), "90")
```

**What this does:**
- `MOD dns` selects the DNS module, which is the mechanism that lets the same DSL run a DNS action.
- `SET $hostname` names the probe record. The value `some.probe.domain` is a placeholder in the article.
- `SET $authNs` and `TARGET $authNs` pin the query to one authoritative server on port 53. The trailing comment names that server, which is useful when reading results.
- `QUERY $hostname TXT` asks for the TXT record, which carries the timestamp the probe writes.
- The first two `ASSERT` lines check that the response is successful and that it is authoritative. The third checks freshness: the timestamp in the answer must be later than the current time minus 90 seconds.

> 🔎 Added context: `NOERROR` is the DNS response code for success. The `AA` flag means the answering server claims authority for the zone.

## Catching bugs (SLO and steady)
Constant synthetic traffic through production builds a picture of what real users experience, and feeds the SLOs. When the synthetic traffic hits problems, the author is confident that customers are hitting them too, so the team can triage accordingly.

### SLI and SLO
A service-level objective (SLO) is a percentage target for some metric. A service-level indicator (SLI) is the measurement that tracks it, focused on the end-to-end experience of a user. The article's point is that the probes are SLIs in this sense, because they measure what a user would see rather than what the service reports about itself.

### Error budget and burn rate
An error budget is the share of downtime a service will tolerate. The burn rate is how fast that budget is being used at a given moment. The article's example contrasts two cases: burning 10% of the budget over three days is a long-running issue that is not urgent but still affects customers, while burning 10% within an hour means an incident is occurring and customer impact will be substantial unless fixed immediately. Because the same numbers carry both meanings, an automated system can judge severity from them.

### Release gating
During a deployment, the team looks at SLOs to judge the health of the service. Based on that, the releasing systems are told to either promote the change or roll it back. The article presents this as the reason the measurements matter: they turn observation into a decision that does not need a person to interpret every dashboard.

## The road ahead
Moving DNS monitoring to Flamingo means the system can detect DNS problems before they spread across the edge. The article gives an example: if Flamingo detects a spike in propagation latency in a specific canary data center, it can automatically halt the rollout of a new software version before it affects the rest of the Internet.

### Canary halt
A canary data center is a small, early location where a change is deployed first. A latency spike there is treated as a signal to stop the rollout. The article's claim is that this protects the wider Internet from a bad release, because the probes catch the problem in the canary before the change reaches other locations.

### Protocol coverage
DNS is only the start. Flamingo already has modules for TCP, UDP, WebSockets and several more. The article's conclusion is that quick, automated testing and monitoring of services lets teams deploy large changes safely without a human keeping constant watch.

## 🧠 Mental models
### Synthetic probes as the customer's view
**The model:** Probes that run continuously against production measure what users experience, so they can stand in for waiting for user reports.
**How to apply it:** Measure the outcome a customer would see, such as freshness, status or resolve time, from each location, rather than trusting the component's own health reports.

### Error budget burn rate
**The model:** An error budget is how much failure a service can tolerate, and the burn rate is how quickly it is being spent.
**How to apply it:** Judge severity by rate as well as total. The article's example: 10% of budget over three days is a slow problem, while 10% within an hour is an incident.

### Modules over bespoke tests
**The model:** Keep one fixed setup-request-response script shape and swap in an action module for each protocol.
**How to apply it:** When a new protocol needs testing, write a module that performs the action and returns typed output. The scheduling, reporting and DSL come with it, as they did for DNS.

## 🔑 Key takeaways
- DNS failures can leave a healthy service unable to make new hostnames resolve or to issue certificates, so they need their own measurement.
- Probing from every data center against production catches partial failures that unit tests and green dashboards can miss.
- Adding a DNS action module to Flamingo gave the DNS team scheduling, reporting and scale without building a new probing service.
- The probe asserts freshness within 90 seconds for authoritative servers and within 600 seconds for the 1.1.1.1 resolver.
- Error budgets and burn rates let automated systems judge severity: 10% of budget in one hour is an incident, while 10% over three days is not urgent.
- Continuous synthetic probes can gate releases, promoting a change or rolling it back based on SLO measurements.
- A propagation latency spike in a canary data center can halt a rollout before it reaches the rest of the Internet.

## 🏛️ For Kenji
This is your interpretation, not the author's. In architecture governance, the article shows a concrete way to turn reliability from a dashboard into a release gate: define an outside-in measure of what users see, and let the deployment pipeline act on it. The same pattern could apply to an AI gateway or agent platform, where a healthy-looking service may still give stale or wrong answers. The question it raises for you: which of your systems can fail in ways that unit tests and a healthy status page would never reveal, and what outside-in probe would expose that failure?

## 📚 Reading plan
### From the article
- No URLs are present in the extracted text, so no links are written here. The article mentions an earlier Cloudflare post on Flamingo's use in CDN stack upgrades, and a careers page; those links were not extracted.

### To go further
- Service level objectives, error budgets and burn-rate alerting (SRE practice; the Google SRE book covers these in depth) — the article's SLO model, explained at length.
- DNS caching and TTL behavior (the core DNS specifications, RFC 1034 and RFC 1035) — to understand how authoritative answers and TTLs interact.
- Canary and progressive delivery with automated rollback — the release-gating pattern the article depends on.

## 🔗 Concepts
- [control-plane](/concepts/control-plane.md)
- [canary-deploy](/concepts/canary-deploy.md)
- [SLO error budget](/concepts/slo-error-budget.md)
