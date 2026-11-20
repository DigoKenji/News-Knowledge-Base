---
type: Article Summary
title: 'Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover'
description: Observability should trigger automated action and confirmation, because multi-region failover only works when
  tested under real load.
resource: https://www.infoq.com/articles/observability-control-plane-failover/
tags:
- cloud-infra
- chaos-engineering
- silent-failure
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/articles/observability-control-plane-failover/
  title: 'Building Reliability Into the Cloud: Observability as the Control Plane for Multi-Region Failover'
  author: InfoQ
status: stable
source_url: https://www.infoq.com/articles/observability-control-plane-failover/
source: InfoQ
source_type: blog
published: '2026-11-20'
domain: cloud-infra
concepts:
- chaos-engineering
- silent-failure
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d8194a4b2e310479cc345
---

## 🎯 Core thesis
Observability should drive system behavior, not only inform humans: a signal should trigger an automated action, a follow-up check should confirm the action worked, and multi-region failover is only trustworthy after it has run under real production load and has been tested end to end.

## 🧭 Why the author wrote it
The author's team lost a Terraform module registry to an S3 behavior they had never modeled, then spent hours assuming the fault was theirs. The article argues that the assumption "cloud infrastructure is reliable by default" is what makes teams under-build detection, response and confirmation, and that a regional outage forced the team to design failover around real traffic instead of diagrams.

## Key Takeaways (author's preface)
The article opens with five claims. Each is expanded below and carried into the rest of the note.

- A 99.99 percent SLA is a statistic, not a promise. Design for the tail of that distribution, because at sufficient scale you will eventually land in it.
- An alert that wakes a human only speeds up notification. Resilience comes from automation that acts on the signal and informs humans afterward.
- A passive region that has never carried production traffic is an untested hypothesis. Active-active keeps both regions continuously validated under real load.
- DNS TTL can nullify a perfectly executed failover. Choose it deliberately against your RTO, health-check interval, and false-positive tolerance, rather than accepting the module's default.
- Faster detection only sharpens the post-mortem timeline. Reliability requires automated recovery plus a confirmation loop that verifies the failover actually worked.

🔎 Added context: RTO (recovery time objective) is the maximum acceptable time to restore service after a failure. Route 53 is AWS's DNS service, and its health checks and weighted records are used for the routing examples below.

## The Failure Nobody Expected From a Service Everyone Trusted
The author almost titled this section "The Failure I Should Have Expected." The author's team owned a Terraform execution platform whose module registry was backed by S3, which the author describes as standard, with nothing exotic added. One morning the dashboard showed registry failures and artifacts not downloading. The team declared an incident and started a process of elimination. After about two hours, the author was still convinced the team had caused the problem through a bucket policy or a permissions boundary. The problem was not in their changes, config or code.

### SLA as a statistic
The author's main lesson is about the assumption under the incident. The team had treated cloud reliability as a guarantee. The author points out that a 99.99 percent SLA is "essentially always works" only in the casual reading. It is a statistical claim about a distribution, and the tail of that distribution is where incidents live. The article puts the figure concretely: 99.99 percent allows 52 minutes and 36 seconds of downtime per year. At the team's scale, the author had already hit that window more than once, but had not treated those hits as evidence that the design needed to change.

> ❌ **Anti-pattern — SLA as guarantee:** Reading a provider's availability percentage as a promise that the dependency will always work, and designing with no plan for the tail. **Why people do it:** The SLA is a number on a contract page and it sounds like a commitment. **Consequence:** Detection, failover and recovery are underbuilt, and every tail event looks like a surprise. **Fix:** Treat the SLA as a distribution, compute the downtime budget it allows, and design for the failures that fall inside it.

### S3 signed URLs and the HEAD request
The mechanism was narrow. S3 signed URLs worked when called by hand, and an artifact downloaded without issue. Terraform, however, calls those URLs in sequence. When it resolves module dependencies, it fires several requests back to back. Some of them failed silently, not all of them, and not in a consistent pattern. That was enough to break pipelines. Every dashboard pointed at the registry, so the failure looked like a registry bug rather than an S3 bug.

The author reproduced the problem on a laptop. The failures were intermittent and appeared only in production, where experimentation is discouraged, so the author built a safe reproduction. The reproduction was a small Terraform configuration that downloads modules from the same registry path the pipelines use, run under a load test. The author also checked out the Terraform source and added logging for HTTP request and response headers. The failure reproduced within ten minutes. The logs traced it to a HEAD request, not to the registry. Terraform hands module downloads to HashiCorp's go-getter library, which sends a HEAD request to each signed URL before the GET to check whether the download can be resumed. The author proposed that a recent change in how AWS handled that HEAD request produced the downstream 404.

🔎 Added context: A HEAD request asks for the headers of a resource without its body. Here it was used as a resumability check before the actual download.

The hypothesis was not obvious even to AWS support. The author backed it with clear reproduction steps and diagnostic evidence. After support escalated internally to the architecture team, AWS confirmed that a recent change in request handling was the root cause and rolled it back. The lesson the author draws: when a managed dependency fails intermittently, reproduce your client's exact call pattern outside production, and instrument the client, not only the server.

### Detection and recovery timeline
Detection took a few hours. Recovery took nearly 24 hours. Most of that time went to support calls proving the problem was on AWS's side, waiting for AWS to acknowledge that other customers were affected, and waiting for the rollback to ship. Once AWS rolled back the S3 deployment, concurrent signed-URL requests stabilized, and the registry recovered without any change on the team's side.

## Observability Should Drive Decisions, Not Just Display Them
The author's experience is that every organization makes the same mistake. Someone builds a good observability system early. The dashboards are thoughtful, the alerts are calibrated and the runbooks are linked. Then the system grows, engineers move on, and new services are wired in without anyone updating what came before. Alerts drift out of calibration, and dashboards show issues nobody on the current team knows how to read. When something breaks at a bad hour, the on-call engineer may spend twenty minutes working out what the dashboards mean. The author calls this a design failure, not a skills gap. The dashboards were built as a reporting layer. They were never connected to anything that acts on what they show, so a human is still the bridge between signal and response.

The mistake has nothing to do with tooling, query language or layout. It is treating observability as a way to inform humans rather than a way to drive system behavior.

### Reporting layer vs control plane
The article contrasts two designs. In reporting, a human receives an alert, interprets it under stress, decides what to do and acts. In a control plane, the system receives the signal, evaluates it against a defined policy, acts, and then tells the human what happened. The author's point is about scaling: the reporting model scales with headcount and on-call rotation, while the control-plane model keeps working when both are stretched thin.

The author also notes that the tools already exist. Prometheus has had alerting rules and webhook routing for years. Terraform can reconfigure routing weights. The components have not been wired together with automation as the explicit goal. The same pattern applies at every scale, from pod autoscaling to regional failover: observe, decide, act and confirm.

> ❌ **Anti-pattern — alert as the end of the chain:** An alert that pages a human and nothing else, so response depends on the on-call person being available and fast. **Why people do it:** Paging is the familiar, well-understood last step of monitoring. **Consequence:** Response time scales with human availability, and the team carries the cognitive load at bad hours. **Fix:** Attach an automated action to the alert and let the human receive a result instead of a task, where the action is safe to automate.

### Observability ownership
The author's second point is that the observability stack concentrates expertise. When the person who really knows the stack leaves, calibration drifts silently until something breaks at the worst moment. The article's prescription is to give the observability stack the same ownership model as production services: regular reviews, real failure testing and collective ownership that does not expire when the original author leaves. The author has run Prometheus and Grafana across hundreds of services, large builds and deployment pipelines with thousands of executions per day, and says these tools carry an operational cost that teams consistently underestimate.

> ❌ **Anti-pattern — single-owner observability:** Dashboards and alerts that only the person who built them understands and maintains. **Why people do it:** The original builder is the only one who knows the context, so it feels efficient. **Consequence:** Alerts drift, nobody dares to change them, and the signal becomes unreliable without anyone noticing. **Fix:** Review alerts and dashboards on a schedule, test them under real failures, and share ownership across the team.

Figure 1 in the article shows the reporting layer and the control plane side by side. The difference is whether a human or the system bridges signal to response.

## Designing Multi-Region Failover That Survives Real Failures
The author notes that no one needs a whitepaper to understand why multi-region matters, only an outage. After a major regional failure at one cloud provider, the team had the conversation about moving the Terraform registry to an active-active architecture. The registry is called during every pipeline execution, including every build, deployment and release. When it goes down, all of them stop across every team. The blast radius of that dependency was much larger than the architecture diagram suggested.

### Active-active vs active-passive
The team chose active-active. The author stresses that the reason for this choice comes up less often than it should: the team wanted the secondary region under real production load before it was needed for failover.

Active-passive looks good in diagrams, but its failure mode is subtle. The passive region has never handled real traffic. Its configuration may have drifted unnoticed. Caches are cold, and downstream dependencies have not been exercised under anything close to real load. Staging tests use synthetic load that does not match the real traffic profile. When the failover happens under pressure, all of these problems surface at once. The author has been on that incident bridge and describes it as trying to recover from one problem while discovering a second set you did not know you had.

Active-active costs more. Regions must be kept in sync, and data consistency under partition needs careful thought. But when one region degrades, the load shifts within a system that has run in two regions all along. The team ran 50/50 under normal operation. Both regions were exercised constantly, which is the continuous validation the author wants. The author adds that 50/50 is the steady state, not the failure state. When a region actually degrades, traffic moves to 100/0.

> ❌ **Anti-pattern — untested passive region:** A standby region that has never carried production traffic and is assumed to work when needed. **Why people do it:** It is cheaper to run, and it looks right in the architecture diagram. **Consequence:** Config drift, cold caches and unexercised dependencies surface during the incident, when they are hardest to handle. **Fix:** Put the secondary under real production load (active-active), or at minimum test it under realistic traffic on a schedule.

### 100/0 and chaos testing
The article says the 100/0 transition is what the design is building toward, and it has to be tested deliberately. The team ran chaos testing for this exact case: cut one region entirely, verify that the other absorbs the full load cleanly, and confirm recovery when the region returns. The author's conclusion: if you have not tested the 100/0 state, you have not tested your disaster recovery system. You have tested your routing configuration.

🔎 Added context: Chaos testing means deliberately injecting faults into a running system to observe how it behaves. The article applies it to a full region loss.

The author is candid that multi-region is expensive to build and maintain. For a critical build-time dependency like this registry, it was the right call, but the article argues it should be a deliberate decision rather than a default.

### Correlated failure across regions
The article names two failure modes that a clean chaos test does not reveal. The first is a failure that is not regional. The S3 incident hit both regions at the same time because both depended on the same S3 behavior. Regional failover cannot fix that. Failover shifts traffic to the other region, which is failing the same way, so failover triggers there as well and traffic shifts back. The automation keeps moving traffic between two broken regions and adds noise to an incident it cannot resolve. Route 53 behaves the same way at the DNS layer: when every weighted record is unhealthy, it returns all of them rather than none. The correct automated response to a shared-dependency failure is to hold traffic where it is and page a human.

🔎 Added context: A correlated failure is one that takes out several redundant components at once because they share a dependency, which defeats redundancy built only at the component level.

> ❌ **Anti-pattern — failover into a shared dependency:** Automated failover that keeps moving traffic between regions that depend on the same broken service. **Why people do it:** Failover is the expected automated response to any regional health alert. **Consequence:** Traffic oscillates, noise increases, and the incident is never resolved by the automation. **Fix:** Detect when all regions degrade together, stop the failover, hold traffic in place, and escalate to a human.

### Health checks that check the right thing
The second failure mode is a health check that reports the wrong thing. The team's /health endpoint confirmed the application process was running, not that it could do its job. If the backend database in one region goes down, the application keeps answering /health with 200 OK while real requests fail. This is exactly the case regional failover exists for, but Route 53 sees a healthy endpoint and keeps sending traffic to the broken region. The confirmation Lambda, described below, checks the same endpoint, so it would also report a failover into that region as successful.

The fix is a health check that exercises the functionality the service delivers, such as a lightweight database read. The article warns that deep checks carry their own risk: if the dependency they test is shared by both regions, they bring back the correlated-failure problem above.

> ❌ **Anti-pattern — process-level health check:** A /health endpoint that returns 200 whenever the process is running, regardless of whether it can serve requests. **Why people do it:** It is the default in many frameworks and it is cheap to implement. **Consequence:** Routing and failover see a healthy region while requests fail, and confirmation logic reports success falsely. **Fix:** Check the service's real capability with a lightweight dependency call, and weigh the risk of a shared dependency in deeper checks.

### DNS TTL
The article calls DNS TTL the single most common disaster recovery misconfiguration in otherwise well-designed systems. TTL controls how long a resolver caches a DNS record before requerying. If the TTL is too high, for example 300 or 600 seconds, or whatever the module default was, the failover logic can fire correctly and the routing weights can update correctly, yet clients keep hitting the degraded region for minutes because their resolvers still hold the old record. The failover ran, but traffic never moved.

If the TTL is too low, the system is overreactive. A transient latency spike triggers a DNS change, the issue clears thirty seconds later, and the regions flip-flop, amplifying instability that would have cleared on its own. The author's conclusion is that the TTL should be a deliberate number chosen against RTO, the health-check interval and the tolerance for false positives. Most teams run whatever IaC module default was set years earlier, and they only learn its value while debugging a failover that did not go as expected.

Figure 2 in the article shows how a weight change that fires correctly can still leave traffic stranded in the degraded region until resolver caches expire.

> ❌ **Anti-pattern — module-default TTL:** Accepting the DNS TTL that an infrastructure module ships with, without checking it against recovery objectives. **Why people do it:** The default is set once, works for normal operation, and nobody revisits it. **Consequence:** Failover is slower than designed, or so fast that it flaps. **Fix:** Choose the TTL against RTO, health-check interval and false-positive tolerance, and document the choice.

## Disaster Recovery as an Automated, Observable System
The article's view of DR is that what separates working systems from paper ones is automation with confirmation. Detection alone is not enough. The system also has to act on the signal and verify that the action worked. Many teams with serious DR investment have detection, such as alerts when error rates spike or health checks fail. Fewer have built the response layer. Fewer still have built the confirmation layer, which checks whether the response actually worked.

The confirmation layer matters more than people realize. Automated failover without confirmation is a system that acts and then goes quiet. Did the secondary receive the traffic? Did DNS propagate within the TTL window? Are the downstream dependencies responding correctly? A system that keeps observing after it acts is one you can trust. A system that fires and goes silent is, in the article's words, an untested hypothesis.

### Composite alerts with a duration
The article's pattern starts with a Prometheus alert that evaluates a composite condition: error rate above a threshold, AND p99 latency elevated, AND sustained for a defined window. The author explains why. Single-metric alerts are noisy, and a transient spike fires them constantly for problems that clear within a minute or two. The composite condition with a duration requirement triggers only on degradation that has persisted.

🔎 Added context: p99 latency is the response time that 99 percent of requests beat, so it describes the slow tail rather than the average.

Most implementations stop at the alert. In this design the alert is the entry point to an automated chain. How traffic moves depends on the topology. In active-active, Route 53 does the switching: each regional record carries a weight and a health check, and when a region's health check fails, Route 53 stops returning that record, so traffic moves with no custom code. In active-passive, the alert drives the switch: Alertmanager routes it to a webhook, which invokes a Lambda function that flips the Route 53 failover record to the secondary region. In both cases the alert also triggers a confirmation Lambda that polls the target region's health endpoint until it returns healthy responses. The human is notified only after the confirmation completes. The article's summary is that detection, action and confirmation form one connected loop, in which observability drives the action and then verifies the outcome.

Figure 3 in the article shows this loop: Prometheus fires, Route 53 shifts traffic, Lambda polls until the target region is healthy, then the team is notified.

### The detect-act-confirm loop in code
The article gives a confirmation Lambda in Python:

```python
import urllib.request
import time

def lambda_handler(event, context):

    # CONFIRM: Route 53 or the failover webhook has already moved traffic
    # Lambda confirms the failover succeeded and notifies
    confirmed = False
    for attempt in range(10):
        time.sleep(30)
        try:
            response = urllib.request.urlopen(
                'https://registry-secondary.internal.example.com/health',
                timeout=5
            )
            if response.status == 200:
                confirmed = True
                break
        except Exception:
            continue

    return {
        'failover_executed': True,
        'confirmation_status': 'healthy' if confirmed else 'unconfirmed',
        'attempts': attempt + 1
    }
```

**What this does:**
- Runs after the failover has been triggered, so its job is to verify the outcome rather than to act.
- Sleeps 30 seconds before each of up to 10 checks, which gives DNS and the target region time to settle. The first check happens after 30 seconds, not immediately.
- Uses a 5-second timeout on each request, so one hung connection cannot stall the loop.
- Treats any exception as "not yet healthy" and retries, which absorbs transient errors during the switch.
- Returns `unconfirmed` if the loop never gets a 200. The article says this status should itself become an alert rather than a silent failure. The `failover_executed` field is always `True` in this sketch, so it records the trigger, not the outcome.

The loop does not close until the health check returns 200. If it never does, the function returns "unconfirmed," and that signal becomes an alert.

The article also gives the Route 53 side as Terraform. Both regions carry equal weight, and Route 53 stops returning a region's record when its health check fails:

```hcl
resource "aws_route53_health_check" "primary" {
  fqdn              = "registry-primary.internal.example.com"
  port              = 443
  type              = "HTTPS"
  resource_path     = "/health"
  failure_threshold = 3  
  request_interval  = 10  # or 30, fine tune based on the system
}

resource "aws_route53_health_check" "secondary" {
  fqdn              = "registry-secondary.internal.example.com"
  port              = 443
  type              = "HTTPS"
  resource_path     = "/health"
  failure_threshold = 3
  request_interval  = 10
}

resource "aws_route53_record" "registry_primary" {
  zone_id        = var.zone_id
  name           = "registry.internal.example.com"
  type           = "A"
  set_identifier = "primary"

  weighted_routing_policy {
    weight = 50
  }

  health_check_id = aws_route53_health_check.primary.id
  ttl             = 30  # or 60 can be fine tuned based on the system.
  records         = [var.primary_region_ip]
}

resource "aws_route53_record" "registry_secondary" {
  zone_id        = var.zone_id
  name           = "registry.internal.example.com"
  type           = "A"
  set_identifier = "secondary"

  weighted_routing_policy {
    weight = 50
  }

  health_check_id = aws_route53_health_check.secondary.id
  ttl     = 30   # or 60 can be fine tuned based on the system.
  records = [var.secondary_region_ip]
}
```

**What this does:**
- Two HTTPS health checks poll each region's /health path every 10 seconds and mark it unhealthy after 3 consecutive failures.
- Each region gets its own weighted A record with weight 50, so normal traffic splits 50/50 and both regions stay exercised.
- Each record is tied to its health check, so an unhealthy region's record stops being returned.
- The TTL of 30 seconds is a short value, which speeds failover but raises the flip-flop risk discussed above. The inline comments say to tune it to the system.

The article then gives the Prometheus alert rule. It is shown after the Terraform in the source, under a "Prometheus rule:" label:

Prometheus rule:

```yaml
groups:
  - name: registry_availability
    rules:
      - alert: RegistryRegionalDegradation
        expr: |
          sum by (region) (rate(http_requests_total{job="registry", status=~"5.."}[2m]))
            /
          sum by (region) (rate(http_requests_total{job="registry"}[2m]))
            > 0.03
          and
          histogram_quantile(0.99,
            sum by (le, region) (rate(http_request_duration_seconds_bucket{job="registry"}[2m]))
          ) > 0.8
        for: 90s
        labels:
          severity: critical
        annotations:
          summary: "Registry degradation in region {{ $labels.region }}"
          description: "Error rate above 3% and p99 latency above 800ms sustained for 90 seconds in {{ $labels.region }}"
```

**What this does:**
- Computes the 5xx error ratio per region over a 2-minute window and fires only when it exceeds 3 percent.
- Requires the p99 latency to exceed 0.8 seconds (800 ms) in the same region, so the alert needs both symptoms.
- The `by (region)` clause evaluates the condition separately for each region, so the alert names the degraded region and the automation knows which one to act on.
- `for: 90s` means the condition must hold for a minute and a half. Without it the rule fires on blips; with it, the rule fires only on sustained degradation.
- The `critical` severity label and the annotations give the human a readable summary once the automation has acted.

### Game days
The article's preferred test practice is the game day: a scheduled exercise in which the team deliberately injects real failures into production and works through detection, response and recovery in real time, with the people who would handle a real incident in the room. It is not a runbook walk-through in staging.

Game days find human gaps as reliably as technical ones. Who has decision authority when the incident commander cannot be reached? Where is the runbook for this failure mode, exactly, and does everyone know it? The first time most teams run one, these answers are uncomfortable, and that is the point, because finding them during a game day costs an afternoon.

The article says the most consistently skipped test is the confirmation layer. Teams test that failover fires, but rarely test what happens when it fires and the system concludes that it did not work. That path can create cascading failures the team caused itself.

> ❌ **Anti-pattern — testing only the trigger:** Validating that a failover fires without validating that the confirmation layer correctly recognizes success and stops acting. **Why people do it:** Firing is the visible behavior, and it is easy to demonstrate. **Consequence:** A false "failed" verdict can keep the automation acting, creating a cascading failure. **Fix:** On game days, include a case where the system believes the failover did not work and verify that it stops taking action.

## Conclusion
The author concludes that the S3 incident was not something the team had modeled. Nobody had a runbook for "S3 silently breaks concurrent signed URL requests under load following an internal deployment." The assumption behind most system design, that the infrastructure below is basically reliable, is correct most of the time, which is why it is rarely questioned.

The article ends with three steps for turning a failover plan into a working system:

> 📋 **Checklist — make the failover plan a working system**
> - [ ] Pick one alert that pages a human today and wire it to an automated response with a confirmation step.
> - [ ] Put the secondary region under real production traffic before you need it for failover.
> - [ ] On your next game day, test the confirmation layer, not just the trigger. Verify that the system recognizes a successful failover and stops acting.

The author's closing line: if you have not validated all three, you have a plan, not a system. Infrastructure will fail, and the next failure will probably be one you have not seen before. Whether a person has to piece together the response manually, or the system handles it, was decided long before the incident started.

## 🧠 Mental models
### Observe, decide, act, confirm
**The model:** Every reliability mechanism in the article is a loop of four steps: observe a signal, decide against a defined policy, act automatically, and confirm the outcome before notifying a human.
**How to apply it:** For each alert, name the decision it drives, the action that follows, and the check that proves the action worked. If any step is missing, the alert is a reporting element, not a control element.

### Reporting layer vs control plane
**The model:** A reporting layer tells a human what happened, and a control plane acts on what happened and then tells the human. The first scales with headcount, and the second keeps working when headcount is stretched.
**How to apply it:** For each dashboard or alert, ask who acts on it and how long that takes at 3 a.m. If the answer is "a person, after twenty minutes of reading," consider whether a safe automated action exists.

### Active-active keeps the failover path warm
**The model:** A failover path that carries real traffic all the time is validated continuously. A standby that carries none is a hypothesis.
**How to apply it:** Test the 100/0 state deliberately with a region cut, and treat the normal split as the steady state that makes failure states believable.

### Shared dependencies defeat redundancy
**The model:** Redundancy built at the region level does not protect against a failure in a service that all regions share, and automated failover can make that case worse.
**How to apply it:** List the dependencies every region shares, including the cloud provider's storage and DNS services. For each one, decide what automation should do when all regions degrade together, which is usually to hold and page.

### Composite, sustained signals
**The model:** An alert on one metric, without a duration, fires on noise. A composite condition with a sustained window fires on degradation that persists.
**How to apply it:** Combine an error-rate signal with a latency signal, require a duration, and label the alert per region so the automation knows its target.

## 🔑 Key takeaways
- A 99.99 percent SLA allows 52 minutes and 36 seconds of downtime per year. Design the failover and recovery for that tail rather than treating the number as a guarantee.
- Failures in managed dependencies can be intermittent and client-visible only, so reproduce the client's exact call pattern outside production and instrument the client as well as the server.
- Run the secondary region under real production load (the article uses 50/50 active-active) so failover is validated continuously, then test the 100/0 state with a deliberate region cut.
- Failover cannot fix a shared-dependency failure. When all regions degrade together, automation should hold traffic and page a human.
- Choose the DNS TTL and the health-check design against RTO, check interval and false-positive tolerance. A /health endpoint that only proves the process is up will keep routing traffic to a broken region.

## 🏛️ For Kenji
This applies directly to architecture work on any shared control-plane dependency, such as a registry, artifact store, identity provider or DNS. The article's question for an architecture review is which failover decisions the system makes automatically, which it holds, and how it confirms each outcome. *(This is your interpretation, not the author's.)* A useful question to raise: for a service your platform depends on, what does the confirmation layer do when it wrongly concludes that a failover failed?

## 📚 Reading plan
### From the article
- The article's body cites no external links, so there is no link to follow from it.

### To go further
- AWS Route 53 health checks and weighted routing policies: how failing health checks remove records from answers, and how TTL and resolver caching affect failover timing.
- Terraform module downloads and the go-getter library: how Terraform fetches modules, including the HEAD request used to check resumability.
- Prometheus alerting rules and Alertmanager routing: `for` durations, composite expressions and webhook receivers.
- Chaos engineering and game days: how teams plan and run controlled failure exercises in production.
- Correlated failures in multi-region design: shared dependencies that defeat regional redundancy.
