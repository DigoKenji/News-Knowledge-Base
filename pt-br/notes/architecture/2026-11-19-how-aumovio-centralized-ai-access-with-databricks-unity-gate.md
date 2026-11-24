---
type: Article Summary
title: Como a AUMOVIO centralizou o acesso à IA com o Databricks Unity Gateway
description: A AUMOVIO consolidou acessos dispersos a LLMs em um único gateway governado do Databricks, comprando a plataforma e construindo apenas as camadas que a diferenciam.
resource: https://www.databricks.com/blog/how-aumovio-centralized-ai-access-databricks-unity-gateway
tags:
- architecture
- technology-architecture
- model-context-protocol
- oidc
- oauth2
- ai-gateway
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://www.databricks.com/blog/how-aumovio-centralized-ai-access-databricks-unity-gateway
  title: How AUMOVIO centralized AI access with Databricks Unity Gateway
  author: Databricks
status: stable
source_url: https://www.databricks.com/blog/how-aumovio-centralized-ai-access-databricks-unity-gateway
source: Databricks
source_type: blog
published: '2026-11-18'
domain: architecture
subdomain: technology-architecture
concepts:
- model-context-protocol
- oidc
- oauth2
- ai-gateway
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d8141978bf9ba64382c08
lang: pt-br
translation_of: /notes/architecture/2026-11-19-how-aumovio-centralized-ai-access-with-databricks-unity-gate.md
---

## 🎯 Tese central
Centralizar o acesso disperso a LLMs atrás de um único gateway com governança central compensa quando você compra o gateway e constrói apenas as camadas que codificam o seu próprio modelo operacional.

## 🧭 Por que o autor escreveu o texto
As equipes da AUMOVIO tinham criado várias formas diferentes de acessar LLMs, e essa variedade dificultava a operação, a atribuição de custos e a governança consistente. Se a tese se confirmar, um gateway compartilhado permite à empresa gerenciar provedores, gastos e aprovações em um só lugar, em vez de reconstruir esse trabalho em cada equipe.

## Introdução
O artigo começa sem título. À medida que a adoção de IA crescia, as equipes da AUMOVIO desenvolveram formas diferentes de acessar modelos de linguagem de grande porte, e o Databricks Unity Gateway agora oferece um ponto de entrada compartilhado para as cargas de trabalho de IA da empresa. O post explica como essas abordagens foram reunidas.

### Databricks Unity Gateway
Este é o produto de gateway do Databricks no centro do artigo. As seções seguintes descrevem o que ele faz pela AUMOVIO: roteia chamadas de modelos para provedores aprovados, aplica aprovações, políticas de acesso, rastreamento de uso, registro e guardrails, e dá à empresa um único lugar para gerenciá-los.

## O desafio: gerenciar o acesso à IA em vários provedores
As equipes tinham criado integrações com provedores e gateways para casos de uso específicos. Cada abordagem atendia a uma necessidade real, mas juntas aumentavam a complexidade da operação, da atribuição de custos e da governança consistente. O artigo lista quatro oportunidades de melhoria.

### Visibilidade compartilhada
O objetivo é reunir o uso de modelos em um só lugar para que os relatórios sejam consistentes. Este item vem primeiro na lista do artigo, antes de custo e operação.

### Transparência de custos
O objetivo é atribuir os gastos a um usuário, equipe ou caso de uso. A atribuição é o que torna visível o custo da IA para as pessoas responsáveis por ele.

### Operações simplificadas
O objetivo é gerenciar atualizações de provedores, incidentes e suporte por meio de um único serviço compartilhado, em vez de separadamente em cada equipe.

### Governança consistente
O objetivo é aplicar aprovações de modelos, registro e guardrails por meio de uma abordagem comum. O artigo combina as quatro oportunidades em "um modelo operacional compartilhado para gerenciar nossas cargas de trabalho de IA com uma única solução de gateway de IA".

## A decisão: um gateway de IA centralizado
A força-tarefa de IA multifuncional da AUMOVIO reuniu segurança, proteção de dados, áreas de negócio e TI. Ela decidiu por um único gateway central de IA como rota compartilhada para o acesso a LLMs e a governança em toda a empresa. O trabalho restante da equipe era tornar esse caminho central útil para as equipes e suas aplicações.

### Força-tarefa de IA multifuncional
A força-tarefa é o mecanismo de governança descrito pelo artigo. Seus membros vieram de segurança, proteção de dados, áreas de negócio e TI, de modo que a decisão refletiu as quatro preocupações. O resultado foi uma direção comum para o acesso a modelos em toda a empresa.

## Por que escolhemos o Databricks Unity Gateway
O ponto central do artigo é que construir um gateway de IA é simples, mas operá-lo como serviço corporativo é um compromisso permanente. Cinco argumentos decidiram a escolha em vez de comprar outro gateway de IA.

### Operar um gateway como compromisso permanente
APIs de provedores mudam, dependências precisam de correções, requisitos de segurança evoluem e os usuários precisam de suporte. Quando as aplicações dependem do gateway, ele precisa de um roadmap, de uma escala de plantão e de um processo de suporte.

> ❌ **Anti-padrão — Precificar o protótipo:** Tratar o gateway como uma construção simples porque a primeira versão é direta. **Por que as pessoas fazem isso:** a primeira versão realmente é direta. **Consequência:** o gateway se torna um compromisso permanente que precisa de roadmap, escala de plantão e processo de suporte. **Correção:** precifique a década, não o protótipo.

### 1. O Databricks já era nossa plataforma de dados
A One AUMOVIO Data Platform (1ADP) roda no Databricks. Colocar o acesso a modelos ao lado de dados corporativos governados significava nenhum novo fornecedor, nenhum novo caminho de rede a revisar e nenhuma pilha isolada para operar.

### 2. Integração com a identidade corporativa
O Databricks está integrado ao tenant corporativo do Microsoft Entra ID, e outras aplicações da AUMOVIO se autenticam no mesmo tenant. Uma aplicação apresenta seu token do Entra ID e obtém acesso por meio de troca de tokens, sem conta separada, sem chave de API para distribuir e sem integração sob medida. Integrar uma nova aplicação habilitada para IA exige apenas uma concessão de permissão, e o acesso a todos os modelos termina automaticamente quando a identidade corporativa termina.

> 🔎 Contexto adicionado: Troca de tokens (token exchange) significa trocar um token por outro, geralmente um com escopo para um serviço diferente. O Microsoft Entra ID é um provedor de identidade que emite tokens OpenID Connect e OAuth 2.0.

> ❌ **Anti-padrão — Credenciais por aplicação:** Cada aplicação recebe sua própria conta e chave de API, além de uma integração sob medida. **Por que as pessoas fazem isso:** é o caminho mais rápido para conectar uma aplicação a um provedor; o artigo não diz isso, é implícito pelo contraste. **Consequência (implícita):** chaves para distribuir e revogar em cada aplicação, e acesso que não termina quando a identidade termina. **Correção:** autenticar as aplicações na identidade corporativa e usar troca de tokens.

### 3. Governança e rastreamento de uso
Aprovações de modelos, políticas de acesso, rastreamento de uso e registro ficam em um só lugar, em vez de serem reimplementados em cada aplicação. O registro de payloads por meio de inference tables vem desativado por padrão e só é habilitado com uma justificativa explícita e aprovada, que, segundo o artigo, é a postura exigida pelos seus acordos de proteção de dados.

### 4. Continuamos livres para escolher provedores
O artigo chama este o argumento mais importante. A AUMOVIO publica um catálogo curado de aliases de modelos 1adp-*, e o Unity Gateway roteia cada alias para o provedor que o atende: modelos fundacionais hospedados no Databricks, Google Vertex AI, Azure OpenAI, AWS Bedrock ou outros. Os usuários programam contra um nome estável e neutro em relação a fornecedores, e a empresa pode reapontar esse nome para outro provedor ou um modelo mais novo sem que nenhum consumidor altere uma linha de código.

> ❌ **Anti-padrão — Nomes de modelos de provedor fixos no código:** O código da aplicação nomeia diretamente um modelo específico de um fornecedor. **Por que as pessoas fazem isso:** funciona imediatamente para o único provedor em uso. **Consequência:** trocar de provedor ou de modelo exige alterar cada consumidor, o que os aliases evitam. **Correção:** programar contra um alias 1adp-*.

### 5. A configuração é infraestrutura como código
O Terraform define tudo o que o Unity Gateway faz para a AUMOVIO: os endpoints por trás de cada alias 1adp-*, o provedor para o qual cada modelo é roteado, a ordem de fallback quando um provedor degrada, os limites de taxa e os guardrails aplicados por endpoint. Adicionar um modelo, reapontar um alias ou endurecer um controle de segurança é um pull request revisado, versionado e reproduzível entre ambientes. Quando a produção se comporta mal, o controle de versão explica a configuração que causou o problema, e o Gateway é apenas mais um módulo para engenheiros que já operam a plataforma.

## Como o Unity Gateway nos ajuda com a gestão de custos de IA e o acesso a modelos
Centralizar o acesso a modelos foi o primeiro passo. A empresa então construiu três capacidades sobre o Unity Gateway.

### Atribuição de custos e repasse (chargeback)
O Unity Gateway informa quantos tokens foram consumidos. Cada usuário atribui esse consumo a um centro de custo pessoal ou a um dos projetos em que trabalha. Pipelines cruzam essas atribuições com as system tables do Databricks e cobram o uso do centro de custo correto. Os resultados são publicados como dashboards que mostram quais modelos são usados, por quem, como o consumo evolui e quanto custam, para que os responsáveis pelo orçamento vejam seus gastos e identifiquem mudanças no consumo.

> 🔎 Contexto adicionado: Chargeback significa cobrar o consumo da unidade de negócio que o utiliza, em vez de absorvê-lo em um orçamento central de TI.

### Um catálogo curado de modelos
O catálogo é a fonte única da verdade sobre quais modelos são aprovados, a que eles correspondem e em qual faixa de consumo operam. As equipes ainda testam sua aplicação com o modelo que escolhem, mas não precisam mais construir infraestrutura para alcançá-lo.

> ❌ **Anti-padrão — Tratar o acesso padronizado como equivalência de modelos:** Presumir que uma única rota torna todos os modelos intercambiáveis. **Por que as pessoas fazem isso:** um único endpoint pode fazer modelos parecerem substitutos diretos (uma implicação do ponto do artigo). **Consequência:** o artigo diz que o acesso padronizado "não torna os modelos intercambiáveis", então o comportamento pode variar entre modelos. **Correção:** continuar testando a aplicação contra o modelo escolhido e usar o catálogo para verificar para o que cada modelo está aprovado.

### Uma interface para os funcionários
O 1ADP AI App roda em produção no mesmo gateway e oferece aos funcionários um jeito governado de usar IA sem escrever código. Os desenvolvedores recebem um endpoint compatível com OpenAI e podem apontar ferramentas de codificação agentivas compatíveis para os modelos centrais. O resumo do artigo: a mesma infraestrutura, dois públicos. A empresa padroniza o acesso aos modelos e deixa aberta a escolha da interface de desenvolvimento.

> 🔎 Contexto adicionado: Um endpoint compatível com OpenAI aceita requisições no formato da API da OpenAI, então ferramentas construídas para esse formato podem ser apontadas para ele.

## Os resultados até agora: governança centralizada de IA, visibilidade de custos e acesso de desenvolvedores
O Unity Gateway fornece uma rota compartilhada para o acesso a LLMs. Os aliases 1adp-* conectam as aplicações aos modelos, os guardrails se aplicam de forma central e o uso é atribuído a centros de custo em quase tempo real.

### Próxima fase: agentes e ferramentas
A próxima fase sobe na pilha de plataforma de IA: compartilhar agentes dentro dos projetos do 1ADP e conectar fontes de conhecimento, busca na web e outras ferramentas por meio de conexões MCP (Model Context Protocol) gerenciadas e trazidas pelo próprio usuário. O acesso aos modelos continua centralizado, mas a gama de ferramentas que um agente pode alcançar cresce. Isso levanta três perguntas: o que um agente pode recuperar, quais ferramentas pode chamar e em nome de quais permissões age. O artigo diz que um framework para respondê-las é o motivo pelo qual a empresa consolidou tudo no início.

> 🔎 Contexto adicionado: MCP é um padrão aberto para conectar aplicações de IA a ferramentas, arquivos, bancos de dados e APIs externas por meio de uma interface comum.

## Lições aprendidas: construir ou comprar um gateway de IA corporativo
Se você está reunindo abordagens diferentes de acesso a modelos, envolva segurança, proteção de dados, áreas de negócio e TI no modelo operacional, e depois torne o serviço compartilhado útil para as equipes que devem usá-lo.

### Precifique a década, não o protótipo
Ao comparar construir com comprar, pergunte quem corrige a integração com o provedor, quem responde ao chamado de suporte, quem trata o incidente e quem vai fazer tudo de novo no ano que vem.

> 📋 **Checklist — Perguntas de construir vs. comprar**
> - [ ] Quem corrige a integração com o provedor
> - [ ] Quem responde ao chamado de suporte
> - [ ] Quem trata o incidente
> - [ ] Quem faz tudo de novo no ano que vem

### Construa o que é genuinamente seu
Decida o que é genuinamente seu para construir. O gateway não era. Atribuição de custos, o catálogo de modelos e o AI App voltado ao funcionário eram, porque codificam o modo de funcionamento da AUMOVIO e nenhum fornecedor os entregaria.

> 📋 **Checklist — Construir vs. comprar um gateway de IA**
> - [ ] Envolver segurança, proteção de dados, áreas de negócio e TI no modelo operacional
> - [ ] Tornar o serviço compartilhado útil para as equipes que devem usá-lo
> - [ ] Precificar a década, não o protótipo
> - [ ] Decidir o que é genuinamente seu para construir

### A responsabilidade continua com a empresa
O artigo diz que a AUMOVIO continua responsável por sua arquitetura, suas decisões de segurança e a qualidade de suas aplicações. O Unity Gateway fornece o serviço compartilhado de acesso a modelos, enquanto as equipes continuam escolhendo as aplicações que constroem.

## Perguntas frequentes
### Por que não construir um gateway próprio
A empresa escolheu o Unity Gateway para centralizar o acesso a modelos sem a manutenção contínua, as atualizações de segurança e a sobrecarga operacional de construir o seu próprio. Como o Databricks já alimentava sua plataforma de dados, ela pôde reutilizar a infraestrutura existente, a integração com o Entra ID e os controles de governança. Isso deixou as equipes livres para construir capacidades específicas da empresa, como atribuição de custos, um catálogo de modelos e uma aplicação de IA para funcionários.

### Governança em vários provedores
O Unity Gateway oferece um único ponto de entrada governado para provedores como Azure OpenAI, Google Vertex AI, AWS Bedrock e modelos hospedados no Databricks. Aprovações centrais, políticas de acesso, registro e guardrails aplicam a mesma governança em todas as aplicações. Aliases curados permitem que os administradores troquem o modelo ou o provedor subjacente sem alterações no código das aplicações.

### Rastreamento e alocação de custos
O Unity Gateway fornece dados de uso de tokens. A AUMOVIO os combina com as system tables do Databricks e com as atribuições de projeto e centro de custo para atribuir os gastos aos responsáveis pelo negócio. Dashboards dão visibilidade quase em tempo real sobre uso, tendências e custos, o que apoia o chargeback e o controle orçamentário.

### Acesso de desenvolvedores sem gerenciar chaves de API
Os desenvolvedores acessam modelos aprovados por meio de um endpoint compatível com OpenAI, então não gerenciam integrações separadas com provedores. As aplicações se autenticam por troca de tokens do Entra ID, o que elimina a necessidade de distribuir chaves de API individuais. A TI gerencia permissões e conexões com provedores de forma centralizada.

### Benefícios de um gateway central para agentes
Um gateway central coloca em um único serviço compartilhado o acesso a vários modelos, a governança, a visibilidade de custos e o gerenciamento operacional. A mesma infraestrutura atende às aplicações dos funcionários e às ferramentas dos desenvolvedores. A próxima fase estende isso para agentes compartilhados e conexões MCP, com foco na governança do acesso a ferramentas e permissões.

## 🧠 Modelos mentais
### Construa o que codifica o seu modelo operacional, compre o resto
**O modelo:** O artigo traça a linha entre construir e comprar perguntando se um componente codifica o funcionamento da empresa. O gateway não codificava, então foi comprado. Atribuição de custos, o catálogo de modelos e o AI App codificavam, então foram construídos.
**Como aplicar:** Para cada componente candidato, pergunte se um fornecedor conseguiria vendê-lo sem conhecer o funcionamento da sua empresa. Se não, construa; se sim, compre.

### Precifique a década, não o protótipo
**O modelo:** O custo real de um serviço compartilhado é o trabalho recorrente de mantê-lo funcionando por anos, não o custo da primeira versão.
**Como aplicar:** Antes de escolher construir em vez de comprar, liste quem corrige, quem dá suporte, quem responde a incidentes e quem refaz o trabalho no ano seguinte, e estime esse custo ao longo de vários anos.

## 🔑 Principais conclusões
- A AUMOVIO consolidou o acesso a modelos em um gateway para perseguir quatro objetivos: visibilidade compartilhada, transparência de custos, operações simplificadas e governança consistente.
- A empresa comprou o gateway porque operá-lo como serviço corporativo é um compromisso permanente. Construiu apenas a atribuição de custos, o catálogo de modelos e o AI App.
- Cinco argumentos decidiram a escolha: uma plataforma de dados já existente, integração com a identidade corporativa, governança e rastreamento de uso, liberdade de provedor e infraestrutura como código.
- Aliases 1adp-* estáveis permitem que a empresa reaponte um nome para outro provedor ou modelo mais novo sem alterar o código dos consumidores.
- A troca de tokens com a identidade corporativa elimina a necessidade de distribuir chaves de API, e o acesso a todos os modelos termina quando a identidade corporativa termina.
- O registro de payloads vem desativado por padrão e só é habilitado com uma justificativa explícita e aprovada, que o artigo vincula aos seus acordos de proteção de dados.
- A próxima fase, agentes e conexões de ferramentas via MCP, desloca a questão da governança para quais ferramentas um agente pode alcançar e em nome de quais permissões ele age.

## 🏗️ Lente de arquitetura
| Aspecto | No artigo |
|---|---|
| Domínios do TOGAF | Negócio (centros de custo, responsáveis por orçamento, a força-tarefa multifuncional); Dados (uso e system tables, a política de registro de payloads); Aplicação (o serviço de gateway compartilhado, o 1ADP AI App, o acesso de desenvolvedores); Tecnologia (Databricks, Microsoft Entra ID, Terraform, vários provedores de modelos). O artigo não usa termos do TOGAF; este mapeamento é uma classificação do seu conteúdo. |
| Escopo | Empresarial: a força-tarefa definiu um único gateway central como rota para o acesso a LLMs em toda a empresa. |
| Stakeholders e preocupações (concerns) | Membros da força-tarefa de segurança, proteção de dados, áreas de negócio e TI; responsáveis por orçamento (visibilidade de gastos); desenvolvedores (acesso à API sem distribuição de chaves); funcionários (uso governado de IA); os engenheiros que operam a plataforma (configuração reproduzível, carga de suporte). Preocupações: governança, proteção de dados, atribuição de custos, troca de provedor sem alteração de código e suporte operacional. |
| Frameworks e notações | Nenhum citado. TOGAF, ArchiMate, BPMN, BIZBOK e C4 não aparecem. Apenas tecnologias citadas: Databricks, Microsoft Entra ID, Terraform, endpoints compatíveis com OpenAI e MCP. |

### Decisões e trade-offs
- **Comprar um gateway em vez de construir um:** As opções eram construir um gateway de IA ou comprar outro. A empresa escolheu o Databricks Unity Gateway, que já tinha por meio de sua plataforma de dados. Custo: o artigo diz que operar um gateway é um compromisso permanente, e não discute o custo de depender do Databricks além dos benefícios de reutilização.
- **Uma rota central em vez de acesso por equipe:** A força-tarefa escolheu um único gateway central de IA para o acesso a LLMs e a governança. Custo: as aplicações passam a depender de um serviço compartilhado que precisa de roadmap, escala de plantão e processo de suporte.
- **Aliases 1adp-* neutros em relação a fornecedores em vez de nomes diretos de provedores:** As aplicações programam contra aliases que o Unity Gateway mapeia para provedores. Custo (implícito): a empresa cura e mantém o catálogo e o mapeamento por conta própria.
- **Registro de payloads desativado por padrão:** O registro só é habilitado com uma justificativa explícita e aprovada. Custo (implícito, não discutido): há menos dados de payload disponíveis na solução de problemas.
- **Terraform para toda a configuração do gateway:** As mudanças são pull requests revisados, versionados e reproduzíveis. Custo (implícito): as mudanças passam por revisão, então são mais lentas do que edições diretas.
- **Construir os diferenciais, comprar o gateway:** A empresa construiu atribuição de custos, o catálogo e o AI App. Custo: o artigo diz que ela continua responsável por esses componentes e pelas suas decisões de arquitetura e segurança.

### Fundamentos em que se apoia
- [Arquitetura Empresarial (EA)](/pt-br/foundations/enterprise-architecture.md) — o artigo a aplica em escala de empresa: um gateway escolhido pela força-tarefa alinha o acesso a LLMs e a governança em toda a empresa.
- [Arquitetura de Tecnologia](/pt-br/foundations/technology-architecture.md) — o gateway, o Databricks, o Entra ID, o Terraform e os provedores de modelos são os serviços e componentes tecnológicos que ele descreve.
- [Arquitetura de Aplicações](/pt-br/foundations/application-architecture.md) — o artigo descreve o AI App e as ferramentas de desenvolvedor usando o gateway compartilhado, mas trata o gateway em si como um serviço compartilhado, e não como uma aplicação.
- [Domínio de arquitetura](/pt-br/foundations/architecture-domain.md) — o artigo toca negócio, dados, aplicação e tecnologia, embora não nomeie os domínios.
- [Capacidade de negócio](/pt-br/foundations/business-capability.md) — o artigo fala em "capabilities" para três funcionalidades construídas (atribuição de custos, catálogo, interface para funcionários), que são funcionalidades de produto, e não o que o negócio faz.
- [Arquitetura de Dados](/pt-br/foundations/data-architecture.md) — o artigo cobre dados de uso, system tables e registro em inference tables, mas como fluxos de dados e política, e não como arquitetura de dados.

## 🏛️ Para o Kenji
*Esta é a sua interpretação, não a do autor.* Os testes do artigo, "precifique a década" e "construa o que codifica como você trabalha", se aplicam diretamente a decisões de construir ou comprar para plataformas compartilhadas e ao trabalho de caso de negócio e alocação de custos de um MBA em arquitetura. Quando ferramentas e agentes ficarem atrás do mesmo gateway, qual modelo de permissões deve decidir o que um agente pode recuperar ou chamar?

## 📚 Plano de leitura
### Do artigo
- Nenhum link é citado no texto extraído, então nenhum pode ser listado aqui.
### Para ir além
- Terraform e infraestrutura como código — como mudanças de configuração revisadas e versionadas funcionam em plataformas compartilhadas.
- Troca de tokens do Microsoft Entra ID e OAuth 2.0 — o mecanismo por trás do acesso sem chaves de API distribuídas.
- Chargeback e showback na gestão de custos de TI — como o uso é atribuído a centros de custo e responsáveis por orçamento.
- Model Context Protocol — o padrão por trás das conexões MCP de que a próxima fase depende.
- Custo total de propriedade de construir versus comprar para plataformas internas — como precificar anos de operação de um serviço compartilhado.

## 🔗 Conceitos
- [model-context-protocol](/pt-br/concepts/ai-ml/model-context-protocol.md)
- [oidc](/pt-br/concepts/security/oidc.md)
- [oauth2](/pt-br/concepts/security/oauth2.md)
- [ai-gateway](/pt-br/concepts/ai-ml/ai-gateway.md)
