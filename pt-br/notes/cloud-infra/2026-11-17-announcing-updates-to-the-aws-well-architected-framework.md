---
type: Article Summary
title: Anúncio de atualizações do AWS Well-Architected Framework
description: A AWS reorganizou o Well-Architected Framework em Foundations válidos para todo o ambiente e orientações por workload, para que as revisões possam ter escopo definido e ser automatizadas.
resource: https://aws.amazon.com/blogs/architecture/announcing-updates-to-the-aws-well-architected-framework-3/
tags:
- cloud-infra
- cloud-platforms
- well-architected
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://aws.amazon.com/blogs/architecture/announcing-updates-to-the-aws-well-architected-framework-3/
  title: Announcing updates to the AWS Well-Architected Framework
  author: AWS Architecture Blog
status: stable
source_url: https://aws.amazon.com/blogs/architecture/announcing-updates-to-the-aws-well-architected-framework-3/
source: AWS Architecture
source_type: blog
published: '2026-11-16'
domain: cloud-infra
subdomain: cloud-platforms
concepts:
- well-architected
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81c6b6b5ce6f714ab326
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-17-announcing-updates-to-the-aws-well-architected-framework.md
---

# Anúncio de atualizações do AWS Well-Architected Framework

**Fonte:** https://aws.amazon.com/blogs/architecture/announcing-updates-to-the-aws-well-architected-framework-3/
**Autor:** AWS Architecture Blog
**Data:** 2026-11-16
**Conceitos:** [Well-Architected Framework](/pt-br/concepts/well-architected.md)

---

## 🎯 Tese central
A AWS dividiu o Well-Architected Framework para que as decisões válidas para todo o ambiente sejam avaliadas uma única vez em uma nova área, Cloud Foundations, e cada revisão de workload cubra apenas as boas práticas técnicas aplicáveis àquele workload.

## 🧭 Por que o autor escreveu o texto
As revisões Well-Architected são valiosas, mas lentas, porque as práticas de nível de conta e de organização eram repetidas para cada workload, e o Framework misturava orientações técnicas com orientações de negócio e de pessoas. Se a tese estiver certa, as revisões ficam mais rápidas de executar e mais fáceis de automatizar, e o Framework passa a refletir como as equipes constroem e operam na nuvem hoje.

## Um breve histórico
O AWS Well-Architected Framework é um conjunto de boas práticas que organizações de todos os portes e setores usam para projetar, construir e operar workloads seguros, confiáveis, de alto desempenho, custo-efetivos, sustentáveis e simples de operar. A AWS lançou a primeira versão em 2012. Depois, publicou o whitepaper do Framework, acrescentou o pilar de Excelência Operacional e lançou whitepapers por pilar e Lenses para tecnologias e setores específicos. A AWS então atualizou o Framework para que as boas práticas se tornassem sua unidade central de orientação, e acrescentou Sustentabilidade como o sexto pilar. Quando os clientes começaram a construir com IA generativa e agêntica, a AWS introduziu novas lenses. O artigo inclui a Figura 1, uma linha do tempo das orientações Well-Architected de 2012 a 2026; o texto a descreve apenas pela sua legenda.

### Well-Architected Framework
O Framework é o corpo central de conhecimento arquitetural. Seus seis pilares são Excelência Operacional, Segurança, Confiabilidade, Eficiência de Desempenho, Otimização de Custos e Sustentabilidade. O artigo o descreve como um corpo de orientações que cresceu de um único conjunto de recomendações até se tornar uma coleção completa ao longo dos anos. A atualização de 2026 é o passo mais recente desse crescimento.

### Well-Architected Lenses
Uma Lens é um conjunto de orientações que aplica o Framework a uma tecnologia ou setor específico. O artigo cita as Lenses para IA generativa e agêntica como exemplo dessa abordagem.

🔎 Contexto adicionado: uma Lens estende o Framework geral com orientações mais específicas, então uma equipe que constrói um tipo particular de sistema pode usar os princípios do Framework junto com esse conselho especializado.

## O que há de novo
Os clientes disseram que valorizam as revisões Well-Architected, mas querem uma forma mais eficiente de executá-las. As boas práticas de nível de conta e de organização se aplicam a todo o ambiente, então os clientes querem avaliá-las uma vez e depois concentrar cada revisão no workload. Para isso, a versão organiza o Framework em duas áreas: Well-Architected Cloud Foundations e Well-Architected Cloud Workloads. Uma revisão de workload agora cobre apenas as práticas que se aplicam àquele workload, o que a torna mais rápida de executar e de automatizar.

### Duas áreas
Cloud Foundations cobre decisões válidas para todo o ambiente, como a estrutura de múltiplas contas, identidade e governança de custos. Ela define guardrails que cada workload herda, e esses guardrails são refinados à medida que o ambiente amadurece. Cloud Workloads cobre como cada workload é projetado, construído e operado, incluindo padrões de arquitetura atualizados e focados em resultados de negócio específicos. Nas duas áreas, as boas práticas são agrupadas em capacidades, que são conjuntos de boas práticas relacionadas com um objetivo comum.

### Foco técnico
A versão concentra-se na arquitetura técnica porque agentes de IA cada vez mais executam as revisões. A orientação é mais útil para um agente quando pode ser verificada em relação à forma como o ambiente e os workloads são construídos e operados. Práticas centradas em estratégia de negócio, pessoas e processos organizacionais dependem de julgamento humano e de contexto que configuração ou telemetria não capturam. A AWS aposentou essas práticas e continua oferecendo orientações sobre elas por outros recursos. Cada boa prática restante agora trata de uma decisão técnica que uma pessoa, ou um agente agindo em nome dela, pode revisar e sobre a qual pode agir.

> ❌ **Anti-padrão — misturar orientações não técnicas em revisões técnicas:** Práticas de negócio, de pessoas e de processo aparecem na mesma lista que as decisões de arquitetura. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** o artigo afirma que essa mistura torna as revisões mais difíceis de automatizar. **Correção:** mantenha a revisão restrita a decisões técnicas e leve as orientações não técnicas para outros recursos.

A versão também atualizou as referências a serviços em todo o Framework. Ela removeu serviços que não aceitam mais novos clientes ou que atingiram o fim do suporte, e acrescentou orientações para serviços lançados desde a versão anterior.

## Well-Architected Cloud Foundations
As boas práticas para decisões de nível de conta e de organização estavam antes espalhadas pelo Framework e eram revisadas para cada workload. Cloud Foundations as consolida em orientações que você aplica ao configurar ou amadurecer um ambiente de múltiplas contas. Dessas boas práticas, 33 vêm de mover ou unir práticas existentes dos pilares. O artigo diz que o restante são orientações fundacionais recém-escritas, mas não dá essa divisão diretamente; por subtração, 12 das 45 são as recém-escritas.

As práticas realocadas foram consolidadas e reescritas a partir de uma perspectiva de todo o ambiente. Novas práticas cobrem monitoramento centralizado de eventos, um registro auditável do inventário de nuvem e gestão centralizada de DNS. O resultado são 45 boas práticas distribuídas em sete capacidades: Ambiente de múltiplas contas; Gestão de identidade e acesso; Gestão centralizada de observabilidade; Gestão de recursos e inventário; Segurança e conformidade; Gestão financeira em nuvem; e Redes e conectividade.

> ❌ **Anti-padrão — revisar práticas de todo o ambiente para cada workload:** Práticas de nível de conta e de organização são reavaliadas em cada revisão de workload. **Por que as pessoas fazem isso:** o artigo diz que essas práticas estavam antes espalhadas pelo Framework e eram revisadas por workload, então o hábito seguia a estrutura antiga. **Consequência:** o artigo diz que os clientes querem avaliá-las uma vez, então repeti-las torna cada revisão mais lenta. **Correção:** avalie-as uma vez em Cloud Foundations, ao configurar ou amadurecer o ambiente, e herde-as nas revisões de workload.

### 🔎 Contexto adicionado sobre guardrails
Um guardrail, aqui, é um controle permanente de todo o ambiente, como a estrutura de contas ou a política de identidade, que se aplica automaticamente a cada workload abaixo dele.

## Well-Architected Cloud Workloads
Cloud Workloads agora tem 216 boas práticas, contra 307 antes, sem perda de cobertura técnica. As práticas que se aplicam a todo o ambiente foram para Cloud Foundations, práticas semelhantes foram combinadas e práticas fora do escopo técnico do Framework foram aposentadas. As mudanças de cada pilar estão abaixo.

### Excelência Operacional
Este pilar introduz novas capacidades e boas práticas que seguem o ciclo de vida das operações: observação, extração de insights, gestão do fluxo de mudanças, implantação, suporte, resposta e evolução. Ele acrescenta uma boa prática para gerenciar as atualizações de dependências externas. Amplia a orientação sobre o uso de assistentes e agentes de IA para instrumentar telemetria, rascunhar dashboards e investigar incidentes, com validação contra linhas de base e responsabilidade humana nomeada em cada etapa.

### Segurança
Preocupações de toda a conta passam para Cloud Foundations. Elas incluem proteção da conta root, mecanismos de login, guardrails de permissão, acesso de emergência, registro centralizado de logs e prontidão forense, que entram nas capacidades de Gestão de identidade e acesso e de Segurança e conformidade. Dentro de Cloud Workloads, o pilar acrescenta boas práticas para gestão centralizada de permissões de workload, métricas e dashboards de detecção e diretrizes de codificação segura.

### Confiabilidade
Três práticas de rede (alocação de sub-redes IP, topologias hub-and-spoke e faixas de endereços não sobrepostas) passam para a capacidade de Redes e conectividade de Cloud Foundations, onde são consolidadas em uma única boa prática de estratégia de rede. Dentro de Cloud Workloads, o pilar acrescenta uma boa prática sobre resiliência cibernética para dados de backup, que ajuda a proteger backups contra ransomware e outras ameaças cibernéticas.

### Eficiência de Desempenho
A prática de políticas e arquiteturas de referência passa para Cloud Foundations. Dentro de Cloud Workloads, a seleção de arquitetura agora se concentra em validar os requisitos do workload e acompanhá-los com indicadores-chave de desempenho (KPIs). Práticas relacionadas são combinadas: a configuração de computação se funde com a seleção de computação, e os testes de carga se fundem com a avaliação de desempenho do workload.

### Otimização de Custos
Preocupações de nível de conta passam para Cloud Foundations, incluindo estrutura de contas, atribuição de custos, ferramentas de faturamento e controles de custo, onde formam o núcleo da capacidade de Gestão financeira em nuvem. Dentro de Cloud Workloads, práticas sobrepostas são combinadas; a orientação sobre descomissionamento se une em uma única boa prática de gestão do ciclo de vida de recursos. Uma nova boa prática trata do uso de IA generativa para eficiência operacional.

### Sustentabilidade
Este pilar introduz capacidades e boas práticas que seguem como as equipes reduzem o impacto dos workloads: medir e monitorar, provisionar recursos, melhorar a eficiência do workload e otimizar dados e processamento. A orientação geral anterior foi convertida em boas práticas com base no feedback dos clientes. Uma nova boa prática pede que as equipes usem a abordagem computacional mais eficiente em recursos, incluindo um método menos intensivo em recursos ou um modelo de machine learning menor, quando um deles atender ao requisito. O pilar também amplia a orientação sobre combinar métricas de eficiência de uso de recursos com dados de emissões de carbono do AWS Sustainability Console e da API, e do AWS Data Exports. Linhas de base e metas tornam o progresso mensurável ao longo do tempo.

## Chamada à ação
O artigo apresenta o Framework como mais do que um conjunto de boas práticas e ferramentas para encontrar oportunidades de melhoria. Ele o descreve como um mecanismo para fortalecer a governança e gerar melhores resultados em toda a organização. Avaliar a arquitetura de forma consistente em relação às boas práticas cria uma cultura de melhoria contínua e expõe problemas críticos antes que afetem os workloads. O Framework atualizado estará disponível em 11 idiomas: inglês, espanhol, francês, alemão, italiano, japonês, coreano, indonésio, português brasileiro, chinês simplificado e chinês tradicional.

## Sobre os autores
O artigo extraído tem títulos para as seções de autores, mas nenhum texto de autor abaixo deles.

## 🧠 Modelos mentais
### Guardrails herdados
**O modelo:** Controles de todo o ambiente são definidos uma vez na fundação, e cada workload os herda, de modo que cada revisão de workload verifica apenas o que é específico daquele workload.
**Como aplicar:** Ao revisar um workload, primeiro confirme que os guardrails de conta, identidade e custo existem e só então gaste o tempo da revisão com as decisões de arquitetura do próprio workload.

### Orientação verificável
**O modelo:** Uma boa prática só é útil para um agente ou revisor quando nomeia uma decisão técnica que pode ser verificada em relação à configuração ou à telemetria.
**Como aplicar:** Reescreva qualquer prática que dependa de julgamento sobre estratégia ou pessoas como um item de governança separado e não automatizado, e mantenha a lista técnica testável.

### Limite a revisão ao que se aplica
**O modelo:** Uma revisão deve cobrir apenas as práticas relevantes para o sistema revisado, o que a mantém curta o bastante para ser executada com regularidade.
**Como aplicar:** Antes de uma revisão, filtre a lista de práticas pela arquitetura do workload e descarte os itens fora do seu escopo, em vez de percorrer o Framework inteiro a cada vez.

## 🔑 Principais conclusões
- O Framework agora tem duas áreas, Cloud Foundations para decisões de todo o ambiente e Cloud Workloads para decisões por workload, para que as revisões possam ter escopo definido.
- Cloud Foundations tem 45 boas práticas em sete capacidades, e 33 delas vêm de mover ou unir práticas existentes.
- Cloud Workloads cai de 307 para 216 boas práticas sem perda de cobertura técnica, por meio de realocações, fusões e aposentadorias.
- Práticas de estratégia de negócio, de pessoas e de processo organizacional foram aposentadas do Framework, e não movidas, porque exigem julgamento humano que configuração ou telemetria não conseguem capturar.
- Cada prática restante trata de uma decisão técnica que uma pessoa ou um agente agindo em nome dela pode revisar e sobre a qual pode agir.
- Novas práticas cobrem codificação segura, resiliência cibernética para dados de backup, IA generativa para eficiência operacional e computação eficiente em recursos.

## 🏛️ Para o Kenji
As revisões de arquitetura de vocês provavelmente misturam controles de todo o ambiente com controles específicos de workload, e a divisão do artigo sugere definir os controles do ambiente uma vez e herdá-los. O artigo também sugere que revisões executadas por agentes funcionam melhor com critérios técnicos e verificáveis, o que importa para a governança de IA. Esta é a sua interpretação, não a do autor. Pergunta: quais dos critérios de revisão atuais da sua equipe um agente consegue verificar a partir de configuração ou telemetria, e quais exigem julgamento humano que deve ficar fora da revisão automatizada?

## 📚 Plano de leitura
### A partir do artigo
- [Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/) — o ponto de entrada para o corpo completo de orientações e sua história.
- [Whitepaper do AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/framework.html) — o texto atualizado do Framework, incluindo os seis pilares.
- [Cloud Foundations](https://docs.aws.amazon.com/wellarchitected/latest/framework/cloud-foundations.html) — as orientações de todo o ambiente que você configura uma vez por ambiente.
- [Cloud Workloads](https://docs.aws.amazon.com/wellarchitected/latest/framework/workloads.html) — as orientações por workload nas quais as revisões agora se concentram.
- [Excelência Operacional](https://docs.aws.amazon.com/wellarchitected/latest/framework/operational-excellence.html) — o pilar com as novas orientações de operações assistidas por IA.
- [Segurança](https://docs.aws.amazon.com/wellarchitected/latest/framework/security.html) — o pilar com as novas práticas de codificação segura e de permissões.
- [Confiabilidade](https://docs.aws.amazon.com/wellarchitected/latest/framework/reliability.html) — o pilar com a nova prática de resiliência cibernética de backups.
- [Eficiência de Desempenho](https://docs.aws.amazon.com/wellarchitected/latest/framework/performance-efficiency.html) — o pilar com a mudança na seleção de arquitetura baseada em KPIs.
- [Otimização de Custos](https://docs.aws.amazon.com/wellarchitected/latest/framework/cost-optimization.html) — o pilar cujas práticas de nível de conta passaram para Cloud Foundations.
- [Sustentabilidade](https://docs.aws.amazon.com/wellarchitected/latest/framework/sustainability.html) — o pilar com as orientações de eficiência de recursos e dados de carbono.
- [Referência da API do AWS Sustainability](https://docs.aws.amazon.com/sustainability/latest/APIReference/Welcome.html) — para obter dados de emissões de carbono junto com as métricas de uso de recursos.
- [AWS Data Exports](https://docs.aws.amazon.com/cur/latest/userguide/what-is-data-exports.html) — a fonte que o artigo cita para os dados de uso de recursos a combinar com os de emissões.
### Para aprofundar
- AWS Well-Architected Lenses — as extensões específicas de tecnologia e setor citadas no artigo, incluindo as lenses para IA generativa e agêntica.
- O processo de revisão Well-Architected — a prática que a nova estrutura de duas áreas foi desenhada para tornar mais rápida e mais fácil de automatizar.
