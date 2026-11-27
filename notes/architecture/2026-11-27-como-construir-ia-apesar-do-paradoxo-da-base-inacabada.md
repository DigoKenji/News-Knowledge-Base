---
type: Article Summary
title: Como construir IA apesar do paradoxo da base inacabada
description: Generative AI is not yet a mature platform, so firms should invest in learning, complementary assets and organizational
  capability that survive architectural change.
resource: https://mitsloanreview.com.br/como-construir-ia-sem-ter-a-base-pronta/
tags:
- architecture
- enterprise-architecture
- artificial-intelligence
- large-language-model
- ai-agent
- model-context-protocol
- training-vs-inference
- architecture-options
- economies-of-speed
- cloud-computing
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- resource: https://mitsloanreview.com.br/como-construir-ia-sem-ter-a-base-pronta/
  title: Como construir IA apesar do paradoxo da base inacabada
  author: unknown
status: stable
source_url: https://mitsloanreview.com.br/como-construir-ia-sem-ter-a-base-pronta/
source: MIT Sloan Management Review Brasil
source_type: blog
domain: architecture
subdomain: enterprise-architecture
concepts:
- artificial-intelligence
- large-language-model
- ai-agent
- model-context-protocol
- training-vs-inference
- architecture-options
- economies-of-speed
- cloud-computing
selected_by: architecture-lane (PC run)
content: pc-run
complete: 'yes'
notion_url: https://app.notion.com/p/425baf74172d81508ae1f30aae8ab09c
foundation_candidates:
- platforming
- complementary assets
- general-purpose technology
---

## 🎯 Core thesis
Generative AI is not yet a mature platform, so firms should invest in what they learn, the complementary assets they own and the capabilities they build, because those keep their value whichever architecture eventually wins.

## 🧭 Why the author wrote it
Kevin J. Boudreau starts from the fast commercial growth of generative AI, which already looks like the early stage of a platform ecosystem. He argues that the real promise of AI is to become a general-purpose technology, like electricity, that transforms organizations and whole economies, and that the structures needed for that are still far from built. If the thesis is right, firms cannot wait for the architecture to settle; they need to act now on investments that stay useful under several possible outcomes.

## Opening: AI looks like a platform race

### Adoption and revenue signals
The article opens with traction figures. By its estimates, about 2.4 billion people use generative AI platforms every month, and coding agents have already changed how software is written. It cites Cursor reaching a revenue run rate above US$2 billion per year in early 2026, and Perplexity passing 100 million monthly users in April 2026 in a search market long considered consolidated. Salesforce's Agentforce reached US$1.2 billion in annual recurring revenue, Harvey gained ground in large law firms, and Shopify now treats AI use as a basic expectation in all its operations. Under many conventional criteria, these signals resemble the early formation of a platform ecosystem.

> 🔎 Added context: "Run rate" means the revenue of a period (here, the most recent one) multiplied out to a full year; the article describes the Cursor figure as projected over one year.

### General-purpose technology as the real ambition
The author's point is that the bigger goal is not to become one more successful technology platform but to become a general-purpose technology (GPT), like electricity or the internal combustion engine, that transforms organizations, sectors and the economy as a whole. Measured against that standard, progress is still limited: the complementary innovation, organizational integration and economic transformation expected of such a technology are, in the author's words, still in their infancy. Early-stage problems such as immature models are real but are not the main obstacle. Broad applicability alone does not produce economic transformation; the potential also depends on building, around the technology, an environment that favours innovation and large-scale adoption. The rest of the article explains what that structuring requires and where AI stands on it.

## Plataforma, uma tecnologia de uso geral

### General-purpose technology
Economists have long argued that general-purpose technologies can transform economies because they apply across many sectors and drive successive waves of complementary innovation, as happened with electricity, the steam engine and the internet. Realising that potential is not simple. A large-scale transformation requires many independent organizations to make interdependent investments, redesign products, processes and business models, build new capabilities and coordinate, all under heavy uncertainty about how the technology and its ecosystem will evolve. The central challenge is therefore to build the foundations that make an ecosystem possible: standards, coordination mechanisms and conditions under which different organizations can integrate the technology, create complementary innovations and build solutions together.

### Platforming (estruturação como plataforma)
The article calls this process platforming. A technology becomes a platform when the technological, industrial and institutional architecture that forms around it becomes stable enough for independent organizations to build on it with confidence:

> Uma tecnologia se estrutura como plataforma quando a arquitetura tecnológica, industrial e institucional que se forma ao seu redor cria condições suficientemente estáveis para que organizações independentes possam desenvolver soluções a partir dela com confiança.

Stability reduces uncertainty by setting clearer expectations about how the technology can be used and how it will evolve. It also defines the paths for complementary innovation (where to innovate, where to rely on others, what can be treated as stable), the governance rules, and the incentives that let each organization capture value from its investment while still coordinating with others. When the three structures align, decentralised investment and experimentation stop being isolated initiatives and can reach scale, driving a wider transformation.

### Electrification as the reference case
Electricity was technically proven and commercially viable in 1882, yet took almost four decades to reach large-scale adoption. The technological architecture stabilised with the Niagara Falls hydroelectric project of 1895 to 1896, which showed that polyphase alternating current was viable at commercial scale. The industrial architecture matured as a division of labour settled between utilities, equipment manufacturers and financiers, under a regulated public-utility service model consolidated between 1898 and 1907. The institutional architecture took shape in 1907 with the creation of the first broad state public-service commissions. As these three layers aligned, organizations gained the confidence to invest and experiment, which accelerated electrification. The platforming was the joint work of inventors, manufacturers, utilities, financiers, standards bodies and regulators.

### Personal computing as an alternative path
Other general-purpose technologies followed a different route. In personal computing, platform leaders such as Microsoft and Intel orchestrated the structuring directly to drive ecosystem growth. The article therefore suggests two routes to platforming: distributed institutional effort, as with electricity, or a leading firm that orchestrates the ecosystem, as with personal computing.

## A estruturação da IA como plataforma: em que estágio estamos?

The AI platform is not yet consolidated, but recognisable technological, industrial and institutional structures are beginning to appear. Knowing what has stabilised and what is still open helps identify the opportunities and challenges of building AI-based solutions before the structure is fully defined.

### Technological architecture is still developing
The dominant architecture, mainly among the leading frontier-model developers, is: pre-trained foundation models, predominantly language-based; specialised hardware; training and inference in the cloud; and distribution through APIs (application programming interfaces). The stack has layers: chips, cloud infrastructure, foundation models and the applications built on them. The article refers to a figure, "Elementos-chave da pilha de IA" (key elements of the AI stack), which is not reproduced in the text. The three lower layers converge around a centralised base: model development and most of the compute sit with a few frontier models hosted in the cloud, and most organizations access that intelligence remotely through APIs rather than building and operating their own models.

This breaks the logic that digital services used to take for granted. Instead of distributing software that runs locally at little extra cost per use, AI delivers intelligence through continuous inference hosted in the cloud, doing heavy computation every time it is used. The likely alternatives, open-weight ecosystems and parallel stacks built by Chinese companies, are variations on the same model rather than fundamentally different trajectories. As this architecture consolidates, much of the uncertainty around the lower layers should fall.

The application and deployment layer, where most organizations expect to build complementary products, is still evolving. Emerging layers of orchestration, agents and middleware compete to define how AI is integrated into products, workflows and enterprise systems. Some firms build on chatbot interfaces, while others work directly with foundation-model APIs. At the same time, frontier models keep absorbing capabilities that many expected other layers to provide, such as enterprise search, retrieval from knowledge bases and persistent memory, instead of these being done by separate software. It is still unclear which abstractions, interfaces and patterns will consolidate into a stable base for large-scale complementary innovation.

### Industrial architecture is still forming
Technological architecture determines how intelligence is built: the trajectory, the approach and how the large problem is divided into components. Industrial architecture determines who builds what: how the ecosystem divides problem-solving and commercial activity among specialised organizations, and how value creation and capture are distributed among them. Whether organizations can take on these roles depends on the capabilities they develop, the skills available in the labour market, how well they align with each other and with the wider economy, and the returns that sustain those activities. Where these misalign, complementary innovation falls well short of expectations.

A recognisable division of tasks has started to appear in the lower layers: Nvidia in AI accelerators; Amazon Web Services, Microsoft and Google Cloud in computing; OpenAI, Anthropic, Google DeepMind and xAI in frontier models; and Meta (Llama), Mistral, DeepSeek and Alibaba (Qwen) in the open-weight ecosystem. Much of the measurable investment in the ecosystem sits in these foundational layers: chips, compute, energy, data centres, cloud and frontier models.

> The same foundation models that lower the cost of innovation also lower the cost of imitation.

Some firms have clearly established themselves as suppliers of products and services that complement foundation models. But AI has not yet produced anything like the app stores of iOS and Android, or the backward-compatible Windows APIs, with clear categories, rules and stable interfaces that gave thousands of earlier developers the certainty to commit. OpenAI's GPT Store has stayed small. Meanwhile a different candidate layer is forming above, around AI application builders such as Replit and Lovable, where non-developers can generate and launch software. Whether a lasting application economy consolidates there, among model providers, or within established software vendors' suites is still unresolved.

For now, AI reaches users through a heterogeneous mix: the frontier providers' own applications, applications built on frontier models, agents, vertical applications, AI embedded in existing software and proprietary enterprise deployments. Even what counts as the application layer is uncertain, since it may be a simple chatbot interface, AI features inside existing tools, orchestration and middleware, wrapper applications (apps that serve as an interface to third-party models) or third-party agents. Vertical integration blurs these divisions further, as frontier developers move into user-facing products and established firms bundle foundation models into their offerings. Rather than competing inside established categories, firms compete to define them, testing rival hypotheses about how foundation-model capabilities should be organised, accessed and turned into value.

Returns on complementary investment remain uncertain, especially regarding the eventual division of labour, the dominant application architecture and the sources of lasting advantage. The article notes a familiar dilemma: the same foundation models that cut the cost of innovation also cut the cost of imitation, which makes differentiation harder.

Investment in the lower layers faces uncertainty for other reasons. Frontier developers are spending heavily on future scale, ecosystem leadership and pricing power, expectations that depend on an industrial structure that has not yet emerged. Switching costs remain modest, customers often use several models, and the mechanisms that historically produced lasting platform leadership have not yet developed. Training and inference costs weigh on profitability, and increasingly capable open-weight models keep pressure on proprietary ones. Ultimately, these investments are a bet that AI will become platform-like enough to generate enough value to justify current spending.

### Institutional architecture is emerging slowly
Institutional architecture defines how independent organizations coordinate their actions, invest and build solutions on a common base. Successful platform ecosystems need more than technology and market forces: they need institutions and governance mechanisms that let organizations invest independently while the collective stays coordinated. In AI this is taking shape through standards bodies, consortia, technology providers and governments developing protocols. It is still much less developed than the technological and industrial architectures.

Coordination can come from several mechanisms. Governments can set legal frameworks and public standards; sector alliances, standards bodies and multi-party initiatives can coordinate interoperability and common conventions. The article expects platform leadership to matter as well. A central firm organises the ecosystem by setting stable interfaces, defining rules of participation, signalling the direction of the architecture, stating what it does not intend to absorb, and creating reliable incentives for complementary innovation. Leadership requires power to define and enforce the rules, and incentives to bear the costs of coordination. Microsoft and Intel played this role in personal computing, Apple did so for the iPhone ecosystem and Google for Android. Today, although many governments have taken a less rigid approach, sector consortia, evaluation frameworks and private protocols are starting to appear.

A shared protocol such as Anthropic's Model Context Protocol (MCP) is a useful standard, but platform leadership goes much deeper. It involves active coordination and orchestration of an ecosystem that extends beyond the boundaries of the platform's owner: setting rules of participation, aligning incentives, indicating the architecture's direction, committing to what will not be absorbed, and giving many independent firms enough confidence to build on the base.

> ❌ **Anti-pattern — protocol as platform leadership:** treating adoption of a shared standard such as MCP as if it amounted to governing an ecosystem. **Why people do it:** not discussed in the article. **Consequence:** the coordination work the article describes (rules, incentives, direction, commitments) goes undone while the standard is treated as sufficient. **Fix:** adopt the protocol, but also specify who sets participation rules, incentives and architectural direction, and what your organization commits not to absorb.

The article's view is that frontier firms are still investing relatively little in this deeper work. The ecosystem lacks not technological leadership but coordination leadership. Historically, someone has always performed that function: leading firms in personal computing, public authorities and engineering bodies in electricity. In AI, organizations have turned to vertical integration, which is not the same as coordinating an ecosystem. Vertical integration gathers frontier models, cloud infrastructure, developer tools, enterprise software, consumer applications and distribution channels into one integrated offering. But the direct control of a single company is not ecosystem governance: it gives up the diversity and decentralised contribution of many independent partners, which is the engine of large-scale transformation, and it can delay the mature platform that it tries to replace.

> ❌ **Anti-pattern — vertical integration as ecosystem governance:** assuming that controlling the whole stack in one company amounts to coordinating an ecosystem. **Why people do it:** in the absence of mature platform leadership, many firms use vertical integration to solve coordination problems. **Consequence:** the diversity of independent partners is lost, and the mature platform may be postponed. **Fix:** distinguish control of your own stack from the rules, interfaces and incentives that let others build on it.

## Construir e inovar antes que a IA esteja plenamente estruturada como plataforma

Most firms will not compete by building frontier models. They will compete by building solutions on top of them, integrating those models into products and operations and developing complementary goods and services. That puts them in a challenging position, precisely in the least stable part of the architecture. The lower layers are converging, but the application and integration layer, where most of this work happens, is still forming.

It may seem natural to expect the costs, risks and uncertainties of building AI solutions to fall as the technology consolidates as a platform. The real challenge, however, is to recognise these conditions and act despite them: invest in ways that strengthen the firm's position while managing the difficulty of building on a general-purpose technology whose architecture is not yet fully defined. That means prioritising investments that keep generating value whichever way the architecture eventually consolidates.

### Learn faster than you commit
When the architecture is not yet defined, what a firm learns is worth more than the decisions it commits to, and the cost of learning is exceptionally low right now. Much of the experimentation can run on open-weight models such as Llama, Mistral or DeepSeek, executed on the company's own hardware, where the marginal cost of a query is close to zero and proprietary data stays inside the organization. Frontier models can be reserved for the work that really needs them.

One of the most valuable assets a firm can accumulate is a record of the situations where its specialists corrected the model's answers and of the reasons for those corrections. This information is especially valuable now, while models still make enough mistakes to generate corrections. These interventions also help map what the article calls the irregular frontier of AI capabilities: what the system can or cannot do reliably in the firm's specific tasks.

> 🔎 Added context: "Jagged frontier" is the common English name for this uneven boundary of capability; the article uses the Portuguese phrase "fronteira irregular".

### Build assets that survive architectural change
No one knows which model, technology stack or way of organising AI will win, so firms should be careful about making specific bets on any of them. The risk, however, is subtler than it looks. Most organizations ask one technical question: "If the model were replaced tomorrow, would the system keep working?" The article argues this is not enough, because even a perfectly interchangeable model can create commercial dependence, through the vendor's pricing structure or through contractual restrictions. Two examples are given. Salesforce's Agentforce agents are billed in "agentic work units" defined by Salesforce itself, and SAP's 2026 policy restricts what external AI systems may do with data stored in its software. The firm should also keep flexibility at the application layer. Postponing decisions that depend on an architecture that is still undefined is, in itself, a strategy, not a sign of inertia.

> ❌ **Anti-pattern — technical-only swap test:** judging lock-in only by whether the model can be replaced. **Why people do it:** not explained in the article, which notes that most organizations stop at this question. **Consequence:** commercial dependence persists through pricing structures and contract terms that survive a technical swap. **Fix:** also assess pricing models, contractual restrictions and data-access terms before committing.

### Invest in complements, not in intelligence
Almost every firm will buy its AI rather than build it, and its competitors will often buy the same models. An intelligence that any company can contract is not, by itself, a competitive advantage for anyone: the same models raise the bar for a firm and its rivals alike, and the tasks they perform well tend to become standardised. Advantage has to come from what shared models cannot offer, such as proprietary data, sector-specific expertise, customer trust relationships, distribution channels, brands and specific workflows. The article notes this is an old idea: value capture tends to favour holders of co-specialised complementary assets, not of widely available inputs.

Where to invest depends on the junction points between the layers of the architecture. A junction point able to sustain a competitive advantage depends on something the upper layers cannot easily reproduce: credibility with regulators, an audit trail, a billing relationship embedded in the business, or a proprietary dataset. This is why, according to the article, many hospitals use Epic as the platform for clinical AI applications: even a smarter interface has to meet trust requirements that Epic has already been able to satisfy.

By contrast, an illusory competitive protection can be a thin layer built on top of another company's model. It may be useful this quarter but not the next, when the model supplier adds the same functionality to its product at no extra charge. Before investing in this kind of solution, the firm should ask who could eliminate it: the model supplier moving up into application layers, or the platform owner expanding into the lower layers. It should also assess which of its differentiators competitors could reproduce.

> ❌ **Anti-pattern — thin wrapper as moat:** building a thin application layer on a third-party model and treating it as lasting protection. **Why people do it:** not discussed in the article. **Consequence:** the supplier can absorb the feature at no extra cost, and the advantage disappears. **Fix:** ask who could eliminate the layer and which differentiators could be copied; invest where the model cannot internalise the value.

> ❌ **Anti-pattern — investing in intelligence as the advantage:** spending mainly on access to the most capable model. **Why people do it:** not discussed in the article. **Consequence:** the same model is available to rivals, so the spend creates no differentiation. **Fix:** direct investment toward complementary assets such as data, expertise, trust and workflow.

The logic holds even in the case that seems to undermine it: a model advanced enough to carry out integration, make decisions and do the work itself, leaving little apparent need for a broader ecosystem. Even then, the assets that stay valuable are the ones the model cannot internalise: regulators' trust, audit trails, established contracts and proprietary data. Investing in intelligence itself pays off only for whoever wins the race to build frontier models. Investing in complementary assets can generate value regardless of who wins. The question, in the article's words, is not how to own AI but how to own the assets that become more valuable as artificial intelligence becomes abundant.

### Build organizational capability
The most valuable use of AI in this period is also the least obvious. It is not about executing today's work faster, but about turning the company into an organization that understands how it works: where its knowledge is, how its decisions are made, what its bottlenecks are and how work flows between teams. These are the assets on which the initiatives above depend. Technology vendors already have a name for this work, forward-deployed engineer, the professional they send to work directly at the customer's facilities. The article says that firms adopting AI will also need this role internally.

The capability a firm develops now, such as organizational memory, routines and the judgement to identify where AI can or cannot contribute, is itself a complementary asset that a shared model cannot reproduce. It is also what allows the organization to develop new assets as the technological architecture evolves. Learning generates capabilities, and those capabilities give rise to lasting complementary assets.

> ❌ **Anti-pattern — tool accumulation:** adopting as many AI tools as possible and trying to predict in advance the final shape of the technology. **Why people do it:** not discussed in the article. **Consequence:** the article explicitly rejects this path, so it does not build the organizational capability that the author considers the lasting asset. **Fix:** prioritise learning and the organizational understanding that makes future adoption possible.

> ❌ **Anti-pattern — using AI only to speed up today's work:** applying AI mainly to execute existing processes faster. **Why people do it:** not discussed in the article. **Consequence:** the firm improves current operations but does not learn how its own work and knowledge are structured. **Fix:** use AI to make the organization's knowledge and decision flows explicit.

### Reorganize before the architecture matures
The deep gains of a general-purpose technology always arrive late, and they require two things: the surrounding architecture has to stabilise, and organizations have to restructure around the technology. A firm cannot speed up the first process, but it can start the second now. History shows that firms that do this get ahead. Manufacturers that learned to reorganise their factories around electric power obtained gains long before electrification was universal. Likewise, companies that restructured around information technology reaped benefits well before personal computing matured.

The firms that stand out in their use of AI may look less like today's tech companies and more like true knowledge factories: organizations whose competitive advantage lies not in production volume but in how quickly they turn their own operations into validated knowledge.

### Building before the platform is complete
Building before AI is fully structured as a platform means, necessarily, investing in what the technology cannot offer: learning, complementary assets and organizational capabilities that accumulate while the architecture is still changing. It does not mean adopting the largest possible number of tools, nor trying to predict in advance what the final configuration of the technology will be.

## 🧠 Mental models

### Platforming
**The model:** A technology becomes a platform when the technological, industrial and institutional structures around it stabilise enough for independent organizations to invest with confidence. Each layer answers a different question: how the intelligence is built, who builds what, and how the participants coordinate.
**How to apply it:** Before committing to an AI bet, check which of the three layers are stable and which are still open, and which of your investments depend on the open ones.

### Learning over commitment
**The model:** When the architecture is undefined, the learning a firm gathers is worth more than the decisions it commits to, and cheap experimentation (for example, on open-weight models on your own hardware) is the cheapest way to get it.
**How to apply it:** Keep frontier-model spend for the work that needs it, and log every expert correction to a model answer, since those records map where the model is reliable and where it is not.

### Complementary assets and co-specialisation
**The model:** Value flows to holders of assets that complement the shared technology and are hard to copy, such as proprietary data, trust relationships, audit trails and workflows. It does not flow to access to inputs that every competitor can rent.
**How to apply it:** For each AI investment, ask who could eliminate it (the model supplier moving up, or the platform owner moving down) and which of your differentiators a competitor could reproduce.

### Swap test versus commercial lock-in
**The model:** A technical swap test (could the model be replaced?) is necessary but not sufficient. Commercial lock-in can persist through pricing units and contract terms even when the model is technically interchangeable.
**How to apply it:** Review pricing units, contractual restrictions and data-access terms alongside the technical interfaces before you commit to a supplier.

## 🔑 Key takeaways
- AI's lower stack (chips, cloud, frontier models) is consolidating around a few providers, while the application layer where most firms build is still undefined.
- Platforming requires technological, industrial and institutional structures to align. Electricity was viable in 1882 and took almost four decades to reach large-scale adoption.
- The same foundation models that lower the cost of innovation also lower the cost of imitation, so differentiation has to come from elsewhere.
- Commercial lock-in can survive a technical model swap through pricing units and contract terms, such as Agentforce's agentic work units and SAP's 2026 data restrictions.
- Firms should invest in proprietary data, sector expertise, trust relationships and audit trails, not in access to intelligence that competitors can rent.
- A log of expert corrections to model answers is a valuable asset while models still make enough errors to produce corrections.
- Organizational self-understanding (where knowledge lives, how decisions are made, where the bottlenecks are) is an asset that outlives architectural change.

## 🏗️ Architecture lens
| Aspect | In this article |
|---|---|
| TOGAF domains | Technology (chips, cloud, foundation models, APIs); Application (orchestration, agents, middleware, the application layer); Business (capabilities, value capture, complementary assets, organizational capability); Data (proprietary datasets, audit trails, records of expert corrections). The article barely addresses data architecture in its own right. |
| Scope | Enterprise. The subject is an industry-wide ecosystem that spans many organizations, but the recommendations are for a single firm building on that ecosystem, so the decisions sit at enterprise level. |
| Stakeholders and concerns | AI-building firms (investment returns, differentiation); frontier and cloud providers (scale, pricing power, ecosystem leadership); standards bodies, governments and regulators (interoperability, legal frameworks); customers and partners (switching costs, multi-model use); investors (whether the spending is justified). |
| Frameworks and notations | None named. The article names the Model Context Protocol as a protocol, not as an architecture framework. |

### Decisions and trade-offs
- **Where to experiment:** Open-weight models on your own hardware versus frontier-model APIs. The article recommends cheap experimentation on open-weight models and reserving frontier models for the work that needs them. The cost is operating your own infrastructure.
- **Buy intelligence or build complements:** Buy access to models and invest the effort in complementary assets. The cost is that the model alone gives no advantage, and the value depends on the firm's own assets.
- **Commit now or defer:** Postpone decisions that depend on an undefined architecture, and keep application-layer flexibility. The cost is slower commitment and the risk of losing early learning.
- **Vertical integration or ecosystem governance:** The article treats vertical integration as a poor substitute for coordinating an ecosystem. It gives control but loses the diversity of independent partners and may postpone a mature platform.
- **Thin layer on a vendor's model or deeper co-specialised asset:** A thin layer is quick to build but can be absorbed by its supplier; a co-specialised asset (trust, audit trail, proprietary data) is harder to build but harder to copy.
- **Technical swap or commercial flexibility:** Evaluating only technical replaceability is quicker, but the article shows that pricing units and contract terms can keep a firm dependent after a technical swap.

### Foundations it rests on
- [Enterprise Architecture](/foundations/enterprise-architecture.md): the article stretches it from a single enterprise to an industry ecosystem that spans many firms.
- [Architecture](/foundations/architecture.md): the article uses the word for the technological, industrial and institutional structures that form around a technology across organizations, a wider sense than the foundation's single-system definition.
- [Architecture domain](/foundations/architecture-domain.md): the three layers (technological, industrial, institutional) do not map cleanly onto TOGAF's four domains; the industrial and institutional layers have no direct domain.
- [Business Architecture](/foundations/business-architecture.md): the article's analysis of value capture, complementary assets and the division of labour between firms.
- [Business capability](/foundations/business-capability.md): the organizational capability the author describes (memory, routines, judgement) fits the foundation's idea of what the business can do, independent of who does it.
- [Application Architecture](/foundations/application-architecture.md): the article's orchestration, agents and middleware layer is an application-architecture concern.
- [Technology Architecture](/foundations/technology-architecture.md): the chips, cloud and model-serving stack matches the foundation's technology services and components.
- [Data Architecture](/foundations/data-architecture.md): proprietary datasets and expert correction records are the data assets the article treats as durable.
- [Solution Architecture](/foundations/solution-architecture.md): the article's focus on what a single firm builds on top of models matches a solution-level scope.
- [Enterprise](/foundations/enterprise.md): the article's argument that value depends on a whole ecosystem, beyond any one organization, is the foundation's idea of an enterprise as a collection of organizations.

## 🏛️ For Kenji
Your interpretation, not the author's: in a Staff Architect role, this article gives a test for AI dependencies. Ask whether each dependency would survive a change of model vendor on pricing and contract terms, not just on the technical interface, and where your organization's real differentiators sit. The article also raises a question for your governance work: when a vendor's terms become the architecture's real constraint, who in your organization owns that decision?

## 📚 Reading plan
### From the article
The article cites no links.
### To go further
- Electrification and general-purpose technologies (electricity, the steam engine, the internet) — the historical analogy the whole argument rests on.
- Platform leadership and two-sided ecosystems (Microsoft and Intel in personal computing, Apple and Google in mobile) — the precedents for how a central firm coordinates an ecosystem.
- TOGAF 9.2 architecture domains and the Architecture Development Method — the course baseline for placing the article's layers into business, data, application and technology architecture.

## Concepts
- [artificial-intelligence](/concepts/ai-ml/artificial-intelligence.md)
- [large-language-model](/concepts/ai-ml/large-language-model.md)
- [ai-agent](/concepts/ai-ml/ai-agent.md)
- [model-context-protocol](/concepts/ai-ml/model-context-protocol.md)
- [training-vs-inference](/concepts/ai-ml/training-vs-inference.md)
- [architecture-options](/concepts/architecture/architecture-options.md)
- [economies-of-speed](/concepts/software-engineering/economies-of-speed.md)
- [cloud-computing](/concepts/cloud-infra/cloud-computing.md)
