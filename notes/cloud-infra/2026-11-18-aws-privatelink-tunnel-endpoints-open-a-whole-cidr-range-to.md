---
type: Article Summary
title: AWS PrivateLink Tunnel Endpoints Open a Whole CIDR Range to Another Account
description: AWS tunnel endpoints let a consumer VPC reach a whole shared CIDR range one way, without peering or per-resource
  configuration.
resource: https://www.infoq.com/news/2026/11/aws-privatelink-tunnel-endpoints/
tags:
- cloud-infra
- cloud-platforms
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/aws-privatelink-tunnel-endpoints/
  title: AWS PrivateLink Tunnel Endpoints Open a Whole CIDR Range to Another Account
  author: InfoQ
status: stable
source_url: https://www.infoq.com/news/2026/11/aws-privatelink-tunnel-endpoints/
source: InfoQ
source_type: blog
published: '2026-11-18'
domain: cloud-infra
subdomain: cloud-platforms
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d81f982d3d31524991487
---

## 🎯 Core thesis
A CIDR-scoped tunnel endpoint lets a consumer VPC reach every address in a range that another account shares through AWS Resource Access Manager, including resources created after the share, in one direction and without listing each resource.

## 🧭 Why the author wrote it
Vendors and service providers need to reach applications inside a customer's network. Per-resource PrivateLink configurations break down when the resources are ephemeral or the customer does not want to enumerate them. If the article's thesis holds, vendor access can be defined as a network perimeter instead of an inventory of hosts.

## Overview: a perimeter instead of a resource list
AWS published a technical walkthrough of tunnel endpoints, a VPC endpoint type launched in September. The provider shares a CIDR-type resource configuration through RAM, and the consumer creates a tunnel endpoint against it. Bruce Pinsky and Ashish Kumar of AWS frame the feature as a vendor-access problem, for "a vendor or service provider whose software must reach applications and resources in a customer's network." The customer defines the perimeter in address ranges, and "resources located outside the perimeter can't be reached by the vendor."

### CIDR resource configuration
A resource configuration is the provider-side object that describes what can be reached. A CIDR-type configuration describes an address range rather than one host or database. Once shared, anything inside the range is reachable, including resources that did not exist when the share was made. This removes the need to update access each time a host is replaced or scaled.

### Overlapping CIDR blocks
The consumer and provider VPCs can use overlapping address blocks, which is common when many customer networks use the same private ranges. The tunnel endpoint translates addresses at the boundary, so the two networks do not need to be renumbered. The article presents this as a design property, and the field tests in S5 show what it costs in practice.

### One-directional initiation
Connections "can start only from the consumer VPC, not from the provider network." The provider can respond but cannot open a session into the consumer. This matters for vendor access, because a compromised provider cannot initiate traffic into the customer network.

## Comparison with VPC peering and VPC Lattice
A Reddit thread on the r/aws forum about the September launch opened with the question of what this does that VPC peering does not. The answers gave the use cases. A peering connection exposes the whole VPC in both directions. A tunnel endpoint shares a subset of the address space in one direction only.

### VPC peering
Peering joins two VPCs at the network level, so routing and reachability cover everything in both. That is the source of the risk. A commenter at a bank wrote that "peering with unregulated partners is a massive no-no, the auditors will rip you to shreds for it." Treat peering as a full network trust decision, not a service integration.

> ❌ **Anti-pattern — peering with an external partner for one service:** a full VPC peering to give a vendor access to one application. **Why people do it:** peering is the familiar tool and is quick to set up. **Consequence:** the partner can reach every routable resource in the peered VPC, in both directions. **Fix:** share only the address range the partner needs with a CIDR resource configuration, which runs one way.

### VPC Lattice and layer 4 versus layer 7
Lattice services work at layer 7, while tunnel endpoints work at layer 4 and need no resource configuration per target. Another commenter pointed to self-managed clusters that do client-side load balancing, such as Kafka or Aerospike. Those clusters previously needed port-based routing on a Network Load Balancer and a separate port-mapping scheme for connections that arrived over an interface endpoint.

## The consumer-side mechanics: GENEVE wrapping
The unusual part is on the consumer side. The application's traffic must reach the tunnel endpoint wrapped in GENEVE, over UDP port 6081, in the same Availability Zone.

### GENEVE encapsulation
GENEVE is the encapsulation protocol defined in RFC 8926. It wraps an inner packet in an outer UDP header so the inner packet can cross a network that does not understand it. The tunnel endpoint removes the outer header, checks that the inner destination is inside the shared range, rewrites the source to a resource gateway address, and forwards a plain packet into the provider VPC. The client only needs to produce the wrapper.

### Linux network namespace routing
Linux can produce the wrapper in the kernel. The walkthrough creates a device with `ip link add ... type geneve id 0 device` inside its own network namespace and sets a default route through it. Only the provider's ranges enter the tunnel, so the instance's other traffic is unaffected. This isolation is what makes the approach workable on a shared host.

### Security groups on the resource gateway
From the provider's side, the resources see the resource gateway as the client, not the instance behind the endpoint. The post advises writing security group rules that reference the gateway's security group or subnet, not a single address. A rule on one address breaks as soon as the gateway changes.

> ❌ **Anti-pattern — allowing a single gateway address:** a security group rule that names one resource gateway IP. **Why people do it:** it looks like the most precise rule. **Consequence:** the rule fails when the gateway's address changes, and it does not describe the actual source. **Fix:** reference the gateway's security group or subnet, as the post advises.

## Constraints and limitations
The documentation lists several hard limits. Inner traffic must be TCP, with UDP permitted only for DNS, so ICMP and ping fail. IP fragments are dropped. Private DNS names are not supported on tunnel endpoints. The endpoint's subnets must sit in Availability Zones that overlap with the resource gateway's, or the association shows as inaccessible. The resource gateway's DNS setting cannot be changed after creation.

### Access logs and attribution
The access logs identify the consumer's endpoint but have no field for the inner source address. A provider can attribute traffic to a vendor, but not to the workload behind that vendor's endpoint. Any audit that needs per-workload attribution must be built elsewhere.

### The 0.0.0.0/0 warning
Sharing 0.0.0.0/0 grants access to everything the provider VPC can route to, including networks behind VPC peering, Direct Connect or a Site-to-Site VPN. The post recommends sharing the narrowest ranges that work.

> ❌ **Anti-pattern — sharing 0.0.0.0/0:** a share that covers the whole address space. **Why people do it:** it avoids working out which ranges the vendor needs. **Consequence:** the consumer reaches everything the provider routes, including networks behind peering, Direct Connect and VPN. **Fix:** share the narrowest ranges that work, and review them when the vendor's needs change.

## Field tests with overlapping CIDRs
Ryo Suzuki of the Japanese consultancy Classmethod tested the design, and the article's claims held up in practice.

### Overlapping VPCs
He connected two VPCs that both use 10.0.0.0/16 and got HTTP through. Three approaches made it work: running a GENEVE relay as root on the client, routing a /32 per destination host, or putting an HAProxy or NAT instance in front. Each adds a hop or a single point of failure. His conclusion was that "route management according to the number of destinations is required."

> ❌ **Anti-pattern — per-destination routing or a relay:** a /32 route for each destination host, a GENEVE relay as root on the client, or an HAProxy or NAT instance in front. **Why people do it:** these are the only ways the test got overlapping networks to talk. **Consequence:** each adds a hop or a single point of failure, and the route count grows with the number of destinations. **Fix:** the article does not offer a clean fix for overlapping ranges; the author's conclusion is that the routing effort scales with destinations, so plan for it.

### Sharing 0.0.0.0/0 in practice
In a second experiment, Suzuki shared 0.0.0.0/0 from a VPC with a NAT gateway and reached the internet from a VPC with no gateway at all. This confirmed AWS's warning from the other side. He judged it workable as a temporary path and "difficult to use in a permanent production setup."

## Pricing, availability and Terraform support
Pricing is an hourly charge per tunnel endpoint plus a per-GB charge for data processed, under PrivateLink pricing. The provider side is billed per GB through the resource gateway under VPC Lattice pricing. The feature is available in 28 regions.

### Terraform support
Terraform support came from the community. A contributor request was opened two days after launch, and another contributor implemented it. It shipped in version 6.71.0 of the AWS provider, as a Tunnel endpoint type on `aws_vpc_endpoint` and a `cidr_resource` block on the Lattice resource configuration.

## 🔑 Key takeaways
- A CIDR resource configuration gives a consumer reach to a whole range, including resources created after the share.
- Tunnel endpoints run one way, and connections start only from the consumer VPC.
- Overlapping CIDR blocks are supported, but the field tests show that routing effort grows with the number of destinations.
- Sharing 0.0.0.0/0 exposes everything the provider VPC can route to, so share the narrowest ranges.
- Access logs identify the consumer's endpoint but not the inner source, so per-workload attribution needs another source.
- The consumer must produce GENEVE on UDP 6081 in the same Availability Zone as the endpoint, and inner traffic must be TCP, or UDP for DNS only.

## 🏛️ For Kenji
This is your interpretation, not the author's. Vendor access to internal AI services, such as model endpoints or data pipelines run by an outside provider, fits the perimeter model well, and it limits what a vendor can initiate. The gap in attribution is the harder problem for a governance review: if the access logs cannot name the workload behind a vendor's endpoint, how would you prove which internal workload reached which resource during an audit?

## 📚 Reading plan
### From the article
- No links survived extraction. The article names the AWS Networking & Content Delivery blog, the r/aws thread on the September launch, RFC 8926, and the PrivateLink and VPC Lattice pricing pages, but gives no URLs to cite.
### To go further
- RFC 8926 (Geneve: Generic Network Virtualization Encapsulation) — the encapsulation spec behind the consumer-side wrapping.
- AWS PrivateLink and VPC Lattice documentation on resource configurations — the product docs for the constraints in this article.
- AWS Terraform provider release notes for 6.71.0 — the resource and argument names for automating tunnel endpoints.
