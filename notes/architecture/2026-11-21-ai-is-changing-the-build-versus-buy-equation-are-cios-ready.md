---
type: Article Summary
title: AI is changing the build-versus-buy equation. Are CIOs ready?
description: AI lowers the cost of building software but not owning it, so CIOs must judge build-versus-buy by total cost
  of ownership.
resource: https://www.cio.com/article/4250943/ai-is-changing-the-build-versus-buy-equation-are-cios-ready.html
tags:
- architecture
- enterprise-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.cio.com/article/4250943/ai-is-changing-the-build-versus-buy-equation-are-cios-ready.html
  title: AI is changing the build-versus-buy equation. Are CIOs ready?
  author: CIO.com
status: stable
source_url: https://www.cio.com/article/4250943/ai-is-changing-the-build-versus-buy-equation-are-cios-ready.html
source: CIO.com
source_type: blog
published: '2026-11-20'
domain: architecture
subdomain: enterprise-architecture
concepts: []
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d816aa601e81890e61555
foundation_candidates:
- total cost of ownership
- build-versus-buy decision
- shadow IT
---

## 🎯 Core thesis
AI is lowering the cost of creating software but not the cost of owning it, so CIOs should reconsider build-versus-buy decisions using total cost of ownership rather than initial development cost.

## 🧭 Why the author wrote it
The article starts from a long-standing preference for buying SaaS over building. AI-assisted development now challenges that preference, because applications that once took months can sometimes be prototyped in hours or days. If the thesis is right, SaaS spending deserves review, but judged on the full lifetime cost of an application rather than on how fast code can be generated.

## Opening (untitled in the original)
### Build versus buy
Enterprise software has always raised the question of whether to build an application internally or buy it from a provider. The answer has often favored buying, because commercial software, particularly software as a service (SaaS), provides mature capabilities, ongoing enhancements and technical support. Building custom applications typically requires significant investment in developers, infrastructure, testing, security and maintenance.

### Vibe coding
Vibe coding is the article's name for building software with AI from natural-language instructions. Employees with limited programming experience can describe what an application should do and receive a working solution, while professional developers can use AI to generate code, build interfaces and automate testing. The author asks whether, if AI makes development faster and cheaper, enterprises should reconsider how much they spend on SaaS. The answer is yes, with a qualification: AI reduces the cost of creating software but has not eliminated the cost of owning it.

## AI is changing the economics of software development
### Why organizations adopted SaaS
Companies embraced SaaS largely for development economics. Instead of building and maintaining applications, they subscribed to established products and benefited from the vendor's continuing investment in functionality, security, infrastructure and support.

### SaaS portfolio complexity
Over time, SaaS portfolios have grown complex. Many enterprises run hundreds of applications, some with overlapping capabilities, limited adoption or subscription costs that keep rising. Business units often buy specialized tools for narrow requirements, which creates additional integration and governance challenges.

### AI-assisted development as an alternative
The example is a department paying an annual subscription for a simple application that manages approvals, tracks requests or coordinates a specialized workflow. A replacement once needed a project team and several months of development. With AI-assisted development it may be economically attractive, particularly if the requirements are straightforward and the organization already has the necessary infrastructure and expertise. Across a whole portfolio, replacing selected applications, eliminating overlapping functions or building targeted capabilities around existing platforms could generate meaningful savings. The author adds that the productivity benefits deserve careful examination.

### The METR productivity study
A 2025 randomized controlled study by METR examined how AI tools affected experienced open-source developers working on familiar codebases. Developers using AI tools took approximately 19% longer to complete their assigned tasks, even though they expected the tools to make them faster. The author limits the claim: the study covers one development environment and is not evidence that AI generally slows software development. Its point is that generating code faster does not necessarily mean delivering reliable software faster, so CIOs should measure actual development productivity rather than assume lower costs.

> 🔎 Added context: In a randomized controlled study, participants are assigned to conditions at random, so differences in outcomes can be attributed to the condition rather than to who happened to be in each group.

> ❌ **Anti-pattern — assuming AI lowers cost automatically:** Choosing to build because AI-generated code looks fast, without measuring how long reliable delivery takes. **Why people do it:** a working demonstration is vivid, and the article notes that watching AI produce an application can make traditional development look unnecessarily complicated. **Consequence:** the expected savings may not appear, since faster code generation does not guarantee faster or more reliable delivery. **Fix:** measure actual development productivity before assuming a lower cost.

## The hidden costs and risks of replacing SaaS
### Security of AI-generated code
A 2025 report from Veracode examining AI-generated code found that 45% of the code samples evaluated introduced security vulnerabilities. The author concludes that organizations cannot assume AI-generated code follows established security practices, even when the application appears functional. Enterprise applications need authentication, authorization, encryption, vulnerability management and data protection. Depending on their purpose, they may also need to meet regulatory, privacy and audit requirements. These responsibilities stay with the enterprise however quickly the application was built.

> ❌ **Anti-pattern — judging an application by whether it works:** Accepting a replacement because it functions in a demonstration. **Why people do it:** a functional demo is the most visible evidence available, and the article notes that enterprise requirements extend well beyond whether software works during a demo. **Consequence:** missing authentication, encryption or data-protection controls surface only after deployment. **Fix:** assess security and compliance requirements alongside functionality before deciding.

### Maintenance as an internal obligation
A SaaS subscription typically includes product enhancements, infrastructure management, patches and technical support. When an organization builds its own application, those become internal obligations. Business requirements change, vulnerabilities emerge and integrations break, so applications must be monitored, tested and supported for their whole useful lives. The author accepts that AI may help with these activities but says it does not eliminate the need for accountability.

### Shadow IT from vibe coding
Vibe coding can let non-IT professionals solve departmental problems without waiting for formal development resources, especially for simple needs. It can also create a new generation of shadow IT, with applications deployed without sufficient oversight. The article asks what happens when such an application becomes essential: who maintains it after its creator leaves, who ensures the data stays protected, and whether it keeps working when underlying systems change. These questions matter more when applications support financial transactions, customer information or other business-critical processes.

> ❌ **Anti-pattern — an essential application with no owner:** An employee builds an application that a department comes to depend on, and no one is accountable for it. **Why people do it:** formal development resources may not be available quickly, so building directly looks faster. **Consequence:** maintenance, data protection and continuity depend on one person and can fail when that person leaves or systems change. **Fix:** the article's direction is to limit non-IT builds to lightweight applications within defined boundaries, with governance matched to the work.

### Integration and scale
A seemingly simple SaaS application may connect to ERP, CRM, identity management and other systems. Replacing it may mean rebuilding integrations, migrating data and making sure existing workflows keep functioning. An application that works for a few users may also behave differently when supporting thousands of employees or large transaction volumes.

### Total cost of ownership
The article's conclusion is that vibe coding "may make software easier to build" but "doesn't necessarily make software easier to own." For CIOs, the relevant measure is total cost of ownership, not initial development cost.

## Rethinking the build-versus-buy decision
The author does not recommend replacing major SaaS platforms indiscriminately. Mature ERP, HR, payroll and financial applications hold years of accumulated functionality, business rules and compliance capabilities. Recreating them internally would add complexity and risk, even with better development productivity. The more immediate opportunity lies in specialized SaaS applications with narrow, non-critical requirements. Some duplicate capabilities already available in enterprise platforms, and others could be developed internally at a lower long-term cost.

Rather than applying the criteria used five years ago, the author proposes five questions for evaluating a portfolio.

### Is the capability strategically differentiating?
Custom development may provide greater flexibility and control when an application supports a unique business process. Standardized capabilities that offer little differentiation may be better served by commercial products.

### How complex is the application?
Simple workflows, reporting tools and departmental applications may suit AI-assisted development. Applications involving complex transactions, regulatory requirements or mission-critical operations need greater scrutiny.

### What is the true total cost of ownership?
The analysis should include subscription fees, development, infrastructure, integration, security, maintenance and ongoing support. The internal resources needed to operate the application must also be counted.

### Do we have the capabilities to support what we build?
AI may reduce manual programming, but organizations still need architecture standards, secure development practices, testing and operational support. Without these, technical debt can accumulate quickly.

### Can we achieve the same outcome by extending what we already own?
Before replacing a SaaS application, organizations should check whether existing enterprise platforms can provide the required functionality. Many companies already pay for capabilities they have not fully adopted. The author's summary: the best alternative to buying another SaaS application may be making better use of technology already available, not building a new one.

> 📋 **Checklist — five questions for the build-versus-buy decision**
> - [ ] Is the capability strategically differentiating?
> - [ ] How complex is the application?
> - [ ] What is the true total cost of ownership?
> - [ ] Do we have the capabilities to support what we build?
> - [ ] Can we achieve the same outcome by extending what we already own?

### Two modes of AI-assisted development
The article distinguishes professional developers, who use AI while applying established engineering practices, from business users (described as non-IT professionals, or "users"), who create lightweight applications within defined boundaries. Both approaches have value, but they require different levels of governance.

### Pricing pressure from vendors
The AI capabilities that make custom development more attractive are also available to SaaS providers, who can use them to accelerate development, automate support and add functionality. The author suggests CIOs should question whether traditional SaaS pricing still fits. Will efficiencies reach subscription prices, or will vendors keep raising prices as they add AI features and new licensing models? These questions are worth raising at software renewals and commercial negotiations.

### Combining building and buying
In many cases the best answer combines both. Organizations may keep core enterprise platforms while developing targeted applications, workflows and AI agents that extend them. This preserves stable systems while letting the business innovate more quickly.

The closing argument is that AI creates opportunities to simplify application portfolios, reduce unnecessary software spending and build capabilities that serve the business. Realizing them requires disciplined evaluation rather than enthusiasm for how quickly software can be generated. The question is no longer simply whether an application is cheaper to build than to buy, but whether building, buying or extending existing technology will deliver the best business outcome over the application's entire life.

## 🧠 Mental models
### Creation cost is the first line, not the total
**The model:** The price of building software is only the start of its cost. It continues through integration, security, maintenance and support for as long as the application is used.
**How to apply it:** Before comparing a build estimate with a subscription, list every ownership cost from the article's TCO list and compare full lifetime figures.

### Working is not the same as shippable
**The model:** An application that works during a demonstration says little about security, integration, scale or long-term support.
**How to apply it:** Test a candidate replacement against authentication, data protection, integration and load before counting it as an alternative to SaaS.

### Build, buy, or extend
**The model:** Build versus buy is incomplete. The article adds a third option: extending technology the organization already owns.
**How to apply it:** Check whether an existing enterprise platform can deliver the need before approving a new build or purchase.

## 🔑 Key takeaways
- AI reduces the cost of creating software, but according to the author it "has not eliminated the cost of owning software."
- The 2025 METR study found experienced developers using AI tools took about 19% longer on their assigned tasks, despite expecting to be faster. The author limits this to one development environment.
- A 2025 Veracode report found that 45% of the AI-generated code samples evaluated introduced security vulnerabilities.
- Replacing a SaaS application can mean rebuilding integrations with ERP, CRM and identity systems, migrating data and handling much larger user and transaction volumes.
- Mature ERP, HR, payroll and financial platforms should not be replaced indiscriminately. The opportunity lies in specialized, non-critical SaaS applications.
- Five questions frame the decision: differentiation, complexity, total cost of ownership, ability to support what is built, and whether existing platforms can deliver the outcome.
- SaaS vendors have access to the same AI capabilities, so SaaS pricing should be questioned at renewal.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Business (build-versus-buy as a business decision, capabilities, business outcome) and Application (SaaS portfolio, replacement, integration). Technology and Data appear only as cost items (infrastructure, data migration and protection), not as analysed domains. |
| Scope | Enterprise: the whole software portfolio, with individual applications as the units of decision. |
| Stakeholders and concerns | CIOs (portfolio and renewal decisions, total cost); business units (narrow tools, requirements); employees and non-IT builders (speed, ownership after they leave); security and compliance (vulnerabilities, audit); SaaS vendors (pricing under AI). |
| Frameworks and notations | none named |

### Decisions and trade-offs
- **Build, buy or extend, per application:** The options are keeping SaaS, building with AI, or extending an existing platform. The author recommends evaluating each on the five questions and, in many cases, combining build and buy. The cost is internal ownership of maintenance, security and integration, plus technical debt if support capabilities are missing.
- **Replacing mature enterprise platforms:** The options are replacing or keeping them. The author keeps them and targets narrow SaaS first. The cost is forgone savings, in exchange for avoiding the complexity and risk of recreating years of functionality and compliance.
- **Who may build:** Professional developers using AI within engineering practices, versus business users within defined boundaries. The cost is two governance levels to run and maintain.
- **Subscription renewals:** The options are accepting current pricing or challenging it given AI-driven development economics. The author recommends raising the question at renewals and negotiations. The article gives no quantified cost for either choice.

### Foundations it rests on
- [Enterprise Architecture](/foundations/enterprise-architecture.md) — The article applies enterprise-wide scope to a software portfolio and its spending, not to aligning processes, information and technology with strategy.
- [Business Capability](/foundations/business-capability.md) — The article uses "capabilities" for organizational abilities (for example, "the capabilities to support what we build"), a looser sense than the foundation's definition of what the business does.
- [Application Architecture](/foundations/application-architecture.md) — The article treats applications and their integrations as the units to keep, replace or build, which fits the foundation's grouping of applications around business functions.

## 🏛️ For Kenji
In an architect's work, this applies when a departmental SaaS tool comes up for renewal, consolidation or replacement. The TCO list and the integration check are reusable in portfolio reviews. It also touches business architecture and the financial case that an architecture MBA would evaluate. Question raised: how would you estimate the total cost of an internally built application that must keep running after its builder leaves? This is your interpretation, not the author's.

## 📚 Reading plan
### From the article
- The extracted text keeps the link text for the 2025 METR randomized controlled study and the 2025 Veracode report on AI-generated code, but no URLs survived, so none are listed here. Read those two sources first to check the 19% and 45% figures.
### To go further
- Total cost of ownership for software portfolios — to ground the cost comparison in a standard model.
- Shadow IT governance for business-built applications — to set boundaries for non-IT builders.
- Integration and data migration when replacing SaaS — to size the hidden costs the article describes.
