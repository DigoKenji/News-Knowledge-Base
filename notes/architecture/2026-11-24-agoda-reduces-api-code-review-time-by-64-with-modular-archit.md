---
type: Article Summary
title: Agoda Reduces API Code Review Time by 64% with Modular Architecture
description: Agoda's modular-monolith framework enforces domain boundaries with automated tests, cutting median review time
  64% without a rewrite.
resource: https://www.infoq.com/news/2026/11/agoda-modular-monolith-framework
tags:
- architecture
- integration-and-software-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-24T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/agoda-modular-monolith-framework
  title: Agoda Reduces API Code Review Time by 64% with Modular Architecture
  author: Leela Kumili
status: stable
source_url: https://www.infoq.com/news/2026/11/agoda-modular-monolith-framework
source: InfoQ
source_type: blog
published: '2026-11-23'
domain: architecture
subdomain: integration-and-software-architecture
concepts:
- bounded-context
- modular-monolith
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/422baf74172d815ebe26f620ce7071e7
foundation_candidates:
- modular monolith
- architectural fitness function
- data mapper pattern
---

## 🎯 Core thesis
Agoda can keep an existing service from getting more tangled without a rewrite, by organizing code into domain components, enforcing their boundaries with automated tests, and migrating incrementally, and this shortened code review time.

## 🧭 Why the author wrote it
The post-booking API (BAPI) had grown hard to maintain: merge requests more than quadrupled between 2022 and 2025, and some files exceeded 4,000 lines. The article argues that a framework with enforced boundaries, adopted gradually, fixes this without a rewrite. If the thesis is right, changes stay local to a business domain and reviews get faster.

## The approach and headline results
Agoda introduced a modular monolith framework in its post-booking API to reduce code coupling and enforce domain boundaries without rewriting the existing service. The approach combines component-based code organization, automated architecture checks, and incremental migration. For framework-backed changes compared with legacy code, the team reported a 64% shorter median code review time and a 34% shorter time to initial substantive feedback.

### Modular monolith
🔎 Added context: A modular monolith is one deployable application split into modules with enforced internal boundaries, rather than separate services. The article uses the term in its opening sentence and describes the boundaries that make it work.

### Incremental migration
The team did not start over. It introduced architectural boundaries step by step, so the existing service keeps running while new code follows the new structure.

## Why the API needed change
BAPI coordinates cancellations, booking amendments, refunds, and customer communications. Agoda extracted the API from its aging core monolith in 2019 through a largely line-by-line migration. Development then increased: merge requests more than quadrupled between 2022 and 2025, and some files exceeded 4,000 lines.

### Line-by-line extraction
A line-by-line migration moves code across mostly as it is, rather than redesigning it. This was fast, but it carried the old structure into the new service, which is why the size and tangle of the code grew with the team's workload.

## A tactical trade-off, and an incremental path
The team describes the 2019 migration as a tactical design trade-off, drawing on John K. Ousterhout's *A Philosophy of Software Design*:

> We had deliberately traded a strategic design approach for a tactical one to prioritize speed to market. This tactical mindset was exactly what the business needed at the time.

Rather than undertake a complete rewrite, the team introduced architectural boundaries incrementally.

### Tactical versus strategic design
Tactical design focuses on getting the next feature out quickly. Strategic design invests up front in structure that makes later change cheaper. The article does not claim the tactical choice was wrong; it was right for the business at the time, and the cost arrived later as maintenance pressure.

### Incremental introduction instead of a rewrite
The team placed boundaries around the existing code gradually. This keeps delivery moving and avoids a risky big-bang replacement, at the price of old and new structures coexisting for a period.

## Platform: package by business domain
The framework, called Platform, organizes code around business domains such as cancellations, rebooking, and payments rather than technical layers. Each component encapsulates its business logic and data access behind defined interfaces, with application, service, repository, and model layers contained within the component. The article includes a diagram of the boundaries inside a domain module; the image itself is not in the extracted text.

### Package-by-component
Instead of grouping all controllers together, all services together and all repositories together, code is grouped by business capability. A change to cancellations then touches one component, not every layer of the application. This aims to localize changes and reduce dependencies between business domains.

### Encapsulation behind interfaces
Each component exposes defined interfaces and keeps its internal layers hidden. Other components call the interface, not the internals, so the internals can change without rippling outward.

## Data Mapper and typed builders
Agoda uses the Data Mapper pattern to keep repository-specific models, including database entities and API payloads, out of business logic. Typed builders centralize object creation and transformations, making it easier for developers to find where domain objects and service inputs are constructed.

### Data Mapper pattern
A data mapper translates between the shape of the stored or transported data and the domain objects that business logic works with. Business logic then never depends on database entities or API payload types, so storage or payload changes stay in the mapping layer.

> ❌ **Anti-pattern — repository models in business logic:** business code reads and writes database entities or API payloads directly. **Why people do it:** the article does not say; it is the shortest path when a model already exists. **Consequence:** the business logic is tied to storage and API shapes, so every schema change ripples through it. **Fix:** route all conversions through a Data Mapper so business logic only sees domain objects.

### Typed builders
A typed builder is a dedicated object that creates a domain object or service input in one place. Centralizing creation makes it easier for a developer to find where an object is constructed and transformed, rather than searching the whole codebase.

## Architecture rules as tests in CI
The team uses ArchUnit to encode architectural constraints as automated tests in continuous integration. For example, repository-specific models can only be accessed by their respective repositories. Violations fail the build rather than relying on code reviewers to identify unauthorized dependencies manually.

> ❌ **Anti-pattern — relying on reviewers for boundaries:** a human reviewer is the only check on unauthorized dependencies. **Why people do it:** the article does not say; review is the default safety net already in place. **Consequence (as implied by the article):** structure drifts, and reviewers catch it late or not at all. **Fix:** codify the rule as a test that fails the build.

### ArchUnit
🔎 Added context: ArchUnit is a library for writing architecture rules as unit-test-style checks, for example that one package may not depend on another. The rules run like any other test, so violations appear in CI.

### Architectural fitness functions
The team described these constraints as architectural fitness functions, a concept discussed by Neal Ford and Mark Richards in *Software Architecture: The Hard Parts*. One engineer explained:

> By codifying our layer boundaries into an ArchUnit test suite, our CI pipeline serves as an automated guardrail, catching structural drift on every pull request before a human reviewer ever sees it.

The idea is that an architectural property is checked automatically and continuously, so it does not erode unnoticed.

## Rollout and measured results
Platform is now the default for new modules, while existing functionality is migrated incrementally when feature development provides an opportunity. Agoda compared framework-backed and legacy merge requests in Q2 2026. Median review completion time was 1.08 hours for framework-backed changes, compared with 3.02 hours for legacy changes. Time to first substantive feedback was 10.34 hours versus 15.77 hours.

### Platform as the default
New modules start on the framework, and migration of old code happens only when a feature gives a natural reason to touch it. This avoids a dedicated migration project while the share of structured code grows.

### AI prompt usage
Between June and August, framework-backed changes used 29% fewer AI prompts per 100 changed lines, although median prompts per merge request were similar (20 vs. 22). Agoda cautioned that the AI findings were directional due to the short observation period. Read the AI result as a signal, not a conclusion.

## A related approach
A related approach is Spring Modulith, which provides module verification and dependency-cycle checks for Spring Boot applications.

### Spring Modulith
🔎 Added context: Spring Modulith is a Spring Boot project for structuring an application into modules. Its verification and cycle checks are the same kind of automated boundary enforcement the article describes with ArchUnit.

## 🧠 Mental models
### Tactical versus strategic design
**The model:** tactical work optimizes for the next delivery, while strategic work invests in structure that pays off over time. Early tactical speed is a legitimate choice when the business needs it, but the debt is real.
**How to apply it:** when you accept a tactical shortcut, record what structure you are deferring and when you will pay it back.

### Architectural fitness functions
**The model:** an architectural property, such as "this layer may not call that one," is a test that runs on every change.
**How to apply it:** pick the two or three boundaries that drift most in your codebase and encode each as a failing test in CI.

### Package-by-component
**The model:** group code by business capability with its layers inside, so a change to one domain stays in one place.
**How to apply it:** before adding a module, ask which business capability owns the change; if the answer is several, the boundary is wrong.

## 🔑 Key takeaways
- Agoda's framework cut median code review time by 64% for framework-backed changes compared with legacy code.
- Boundaries were added without a rewrite, by migrating code incrementally and making the framework the default for new modules.
- Platform groups code by business domain, such as cancellations, rebooking, and payments, not by technical layer.
- The Data Mapper pattern keeps database entities and API payloads out of business logic.
- ArchUnit tests in CI fail the build on forbidden dependencies, so reviewers no longer have to catch them by hand.
- The 2019 extraction was a deliberate tactical trade-off for speed to market, and its maintenance cost surfaced as merge requests quadrupled.
- The AI prompt results are directional only, because the observation period was short.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Application is primary (components, layers, interfaces). Data appears only as repository models and encapsulated data access; no data architecture is discussed. Business appears only as the domains that organize code; no business architecture work is described. Technology is not covered beyond the CI pipeline that runs the tests. |
| Scope | Solution: one API (BAPI) and its domain modules, within one service. |
| Stakeholders and concerns | Developers and reviewers, who care about review time, finding where objects are built, and unauthorized dependencies. The business, which needed speed to market in 2019 and still depends on BAPI's cancellation, refund and communication flows. |
| Frameworks and notations | No modeling notation named (no TOGAF, ArchiMate, BPMN, or C4). Named tools and patterns: ArchUnit, Data Mapper pattern, architectural fitness functions, Spring Modulith. |

### Decisions and trade-offs
- **Tactical versus strategic design (2019):** the team chose a tactical line-by-line extraction to prioritize speed to market. The cost was growing maintenance pressure, with merge requests more than quadrupling and files over 4,000 lines.
- **Incremental change versus a rewrite:** the team chose incremental boundaries over a complete rewrite. The cost is that old and new structures coexist while migration proceeds.
- **Package by component versus package by layer:** the team chose business-domain components. The aim is localized change, at the cost of a new organization to learn.
- **Data Mapper for repository models:** the team keeps persistence and payload models out of business logic. The article does not discuss the cost of the extra mapping layer.
- **Automated boundary tests versus manual review:** the team chose ArchUnit tests that fail the build. The article presents this as catching drift earlier, but does not describe the cost of maintaining the rules.
- **Platform as the default for new modules:** new work starts on the framework, and old code moves only when feature work touches it.

### Foundations it rests on
- [Application Architecture](/foundations/application-architecture.md) — The article uses it as a structure of components that group capabilities and manage their data, which matches the definition.
- [Business capability](/foundations/business-capability.md) — The article organizes code by business domains, but these are code boundaries, not a capability map of the business.
- [Architecture](/foundations/architecture.md) — The boundaries and automated constraints are the design principles the definition refers to, enforced over the system's evolution.
- [Solution Architecture](/foundations/solution-architecture.md) — The post-booking API is one discrete operation scoped to a single service, matching this definition.

## 🏛️ For Kenji
This is a useful case for an architect who has inherited a tangled service and cannot rewrite it. The most transferable idea is that a boundary becomes real only when a build fails on violation, which is also a governance mechanism. The MBA link is the strategy-versus-tactics trade-off in the 2019 decision. This is your interpretation, not the author's. Question for you: which decision in your own platform is tactical today, and what automated check would show when it starts costing review time?

## 📚 Reading plan
### From the article
- [InfoQ: Agoda Reduces API Code Review Time by 64% with Modular Architecture](https://www.infoq.com/news/2026/11/agoda-modular-monolith-framework) — the full news piece, with the same numbers in context.
### To go further
- Agoda's own blog post on Platform — the source of the diagram and the detailed framework description; the article cites it, but no link is given in the extracted text.
- John K. Ousterhout, *A Philosophy of Software Design* — the origin of the strategic versus tactical design distinction.
- Neal Ford and Mark Richards, *Software Architecture: The Hard Parts* — the source of the architectural fitness function idea.
- ArchUnit — the library for encoding architecture rules as tests.
- Spring Modulith — module verification and dependency-cycle checks for Spring Boot.
- Data Mapper pattern — the persistence pattern used to keep storage models out of business logic.

## 🔗 Concepts
- [bounded-context](/concepts/backend-systems/bounded-context.md)
- [Modular monolith](/concepts/architecture/modular-monolith.md)
