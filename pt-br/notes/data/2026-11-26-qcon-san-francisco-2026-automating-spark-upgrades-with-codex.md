---
type: Article Summary
title: 'QCon San Francisco 2026: Automatizando upgrades do Spark com agentes Codex na OpenAI'
description: Agentes geram mais valor em operações de plataforma de escopo aberto quando sistemas determinísticos fornecem as garantias e os guardrails.
resource: https://www.infoq.com/news/2026/11/openai-spark-fleet
tags:
- data
- ai-agent
- shadow-testing
- authentication-authorization
- observability
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-26T06:47:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/openai-spark-fleet
  title: 'QCon San Francisco 2026: Automating Spark Upgrades with Codex Agents at OpenAI'
  author: Robert Krzaczyński
status: stable
source_url: https://www.infoq.com/news/2026/11/openai-spark-fleet
source: InfoQ
source_type: blog
published: '2026-11-25'
domain: data
concepts:
- ai-agent
- shadow-testing
- authentication-authorization
- observability
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/424baf74172d812f95adf975b1319a75
lang: pt-br
translation_of: /notes/data/2026-11-26-qcon-san-francisco-2026-automating-spark-upgrades-with-codex.md
---

## 🎯 Tese central
Agentes de IA são mais valiosos em trabalho operacional de escopo aberto, que não pode ser reduzido a playbooks fixos, mas somente quando sistemas determinísticos fornecem as garantias e os mecanismos de segurança por baixo deles.

## 🧭 Por que o autor escreveu o texto
O artigo parte da plataforma de dados da OpenAI, que opera em escala de exabytes, com centenas de milhares de datasets e uma frota Spark em milhões de núcleos. A palestra de Mouli Mukherjee na QCon San Francisco 2026 defende não usar agentes para reconstruir essa plataforma. Se a tese estiver certa, as equipes constroem garantias rígidas como sistemas comuns e deixam que agentes as usem como ferramentas no trabalho de julgamento ao redor delas.

## A plataforma e a afirmação
Na QCon San Francisco 2026, Mouli Mukherjee, Engineering Manager de Data Infrastructure na OpenAI, argumentou que agentes são mais úteis não para reconstruir uma plataforma de dados, mas para trabalho operacional de escopo aberto que é difícil de transformar em playbooks fixos. Ela mostrou como agentes guiados pelo Codex podem assumir esse trabalho quando os sistemas certos estão por baixo. A escala que ela descreve é de exabytes, centenas de milhares de datasets e uma frota Spark em milhões de núcleos.

### Agente de IA
Um agente, no sentido deste artigo, é um sistema em que um modelo de linguagem decide, passo a passo, o que fazer em seguida. Ele escolhe uma ação, usa uma ferramenta, lê o resultado e então decide o que tentar a seguir. O ponto do artigo é que esse ciclo se encaixa em tarefas nas quais o próximo passo depende do que o passo anterior revelou. Não se encaixa em tarefas que precisam devolver a mesma saída para a mesma entrada sempre.

### Apache Spark
> 🔎 Contexto adicionado: Spark é um motor distribuído que executa jobs de processamento de dados em muitas máquinas ao mesmo tempo. O artigo trata a frota Spark como o objeto que está sendo atualizado e ajustado, e não discute os detalhes internos do Spark.

## Sistemas determinísticos e agentes
Mukherjee separou dois tipos de componentes. Tudo que precisa de saídas consistentes para as mesmas entradas, como controle de acesso, resolução de configuração e garantias de correção dos dados, pertence a sistemas apropriados. Agentes usam esses sistemas como ferramentas e não os reimplementam. Ela formulou o princípio assim:

> Definitivamente devemos construir os sistemas onde queremos garantias e guardrails.

Agentes se justificam onde modos de falha e remediações não podem ser listados de antemão. Depuração é o exemplo: o agente segue as evidências, decide o que tentar a seguir e permanece dentro das garantias de suas ferramentas.

### Sistemas determinísticos
Um sistema determinístico devolve a mesma saída para a mesma entrada, então seu comportamento pode ser previsto, testado e auditado. O artigo coloca controle de acesso, resolução de configuração e correção dos dados nessa categoria porque uma resposta errada em qualquer um deles é cara e difícil de notar. Manter isso em código comum significa que as garantias são escritas uma vez e verificadas da mesma forma toda vez. Os agentes então as chamam, em vez de tentar chegar por raciocínio a uma resposta equivalente.

> ❌ **Anti-padrão — Agente reimplementa um sistema determinístico:** o agente deduz decisões de acesso, valores de configuração ou verificações de correção por conta própria, em vez de chamar o sistema que é dono deles. **Por que as pessoas fazem isso:** o artigo não discute a motivação; é um atalho tentador quando um modelo já consegue produzir uma lógica plausível. **Consequência:** a posição do artigo é que a resposta pode não ser consistente para as mesmas entradas, e assim as garantias se perdem. **Correção:** mantenha a garantia em um sistema e exponha-a ao agente como uma ferramenta.

## Tornando a atualização do Spark segura
A atualização do Spark é o exemplo trabalhado. Antes de qualquer agente agir, a equipe construiu três salvaguardas. Cada uma estava no lugar antes que o passo seguinte dependesse dela, e essa sequência é o núcleo do exemplo.

### Execução shadow
Uma execução shadow é uma cópia de um job que roda em um sistema novo enquanto o sistema de produção ainda controla o resultado real. O serviço de execução shadow da equipe reutilizava as entradas de produção, então o teste via os mesmos dados que a produção via. Ele rodava em uma fila de baixa prioridade para que o trabalho de teste não afetasse as operações ao vivo.

> 🔎 Contexto adicionado: uma execução shadow testa a nova versão com tráfego real sem deixar que os resultados dela cheguem aos usuários. Neste artigo, o "tráfego" são as entradas de jobs de produção.

### Serviço de sobrescrita de configuração
A equipe construiu um serviço de sobrescrita de configuração para que os experimentos não pudessem alterar configurações globais. Sem ele, um experimento em um job poderia alterar o comportamento de todos os outros. O serviço mantém cada teste local ao job que ele testa.

### Skill de validação
A equipe também construiu uma skill de validação que compara a execução de produção com a execução shadow. É a verificação que decide se a nova versão do Spark se comporta de forma aceitável. O artigo não descreve os critérios de comparação.

### Sequência de migração conduzida por agentes
Só depois que essas salvaguardas estavam no lugar os agentes começaram o trabalho. Eles reivindicaram jobs, lançaram execuções shadow, revisaram relatórios de validação, classificaram erros e migraram jobs para a nova versão do Spark. O papel dos agentes era a coordenação repetitiva e a triagem, e os sistemas definiam o que contava como seguro.

> ❌ **Anti-padrão — Deixar agentes agirem antes que existam os mecanismos de segurança:** agentes lançam e promovem atualizações enquanto não há teste shadow, nem isolamento de experimentos, nem comparação. **Por que as pessoas fazem isso:** o artigo não discute a motivação; agentes avançam rápido em trabalho visível. **Consequência:** a preocupação do artigo é que os experimentos se tornem não repetíveis, não observáveis e difíceis de reverter. **Correção:** construa primeiro as execuções shadow, o isolamento e a validação, e então deixe o agente operar dentro deles.

## Operações rotineiras da plataforma
Mukherjee relatou que as mesmas primitivas agora cobrem mais do trabalho rotineiro. A equipe as usa para ajuste, releases, falhas e suporte.

### Ajuste contínuo de jobs
Agentes ajustam a eficiência dos jobs inspecionando planos de consulta, testando uma mudança de configuração, comparando as execuções e iterando. O ciclo é o mesmo da depuração: observar, mudar uma coisa, medir, repetir. O artigo apresenta isso como trabalho contínuo, e não como um projeto pontual.

### Promoção direta para releases de baixo risco
Para releases incrementais de bibliotecas de menor risco, a equipe às vezes pula a etapa shadow e promove diretamente. Isso troca capacidade, ou seja, os recursos excedentes que uma execução shadow consumiria, por maior risco de rollback. É uma escolha deliberada feita por tipo de release, não um padrão.

### Rollback automático
Um job que passa na validação shadow mas depois falha em produção pode voltar automaticamente para a versão anterior do Spark. Os pipelines continuam rodando sem intervenção manual. O mecanismo é o que torna aceitável a troca da promoção direta: um release ruim tem um caminho de volta definido.

### Observabilidade para suporte
Agentes pesquisam relatórios de incidentes anteriores, sugerem atualizações de runbooks e rastreiam por que uma tabela está desatualizada por meio dos logs do orquestrador e do scheduler. Esses logs são a evidência que o agente segue. O artigo apresenta isso como uma forma de responder "por que isso está desatualizado" sem escavação manual de logs.

### Controle de acesso e credenciais de usuário
Agentes ajudam usuários a entender negações de controle de acesso. As permissões continuam aplicando as credenciais próprias do usuário, então o agente explica uma decisão que o sistema tomou e não concede acesso por conta própria.

## Recomendação: construa os limites primeiro
Mukherjee recomendou começar por tarefas repetitivas e de escopo aberto, em que o julgamento importa e os modos de falha são numerosos demais para serem codificados. Construa primeiro os mecanismos de segurança e então deixe os agentes operarem dentro desses limites.

### Experimentação repetível, observável e reversível
A meta declarada do artigo é que a experimentação seja repetível, observável e reversível. As execuções shadow a tornam repetível, os logs e relatórios a tornam observável, e o rollback a torna reversível. Cada salvaguarda do artigo corresponde a uma dessas três propriedades.

## 🧠 Modelos mentais
### Garantias em sistemas, julgamento em agentes
**O modelo:** tudo que precisa dar a mesma resposta para a mesma entrada pertence a um sistema determinístico. Os agentes lidam com o julgamento aberto ao redor desses sistemas.
**Como aplicar:** para cada passo de um fluxo de trabalho, pergunte se a resposta precisa ser consistente. Se precisar, torne-a uma ferramenta que o agente chama, em vez de lógica que o agente improvisa.

### Guardrails antes da autonomia
**O modelo:** agentes não devem receber autonomia antes que existam os mecanismos de segurança. O artigo construiu primeiro as execuções shadow, o isolamento e a validação.
**Como aplicar:** antes de habilitar um agente em um sistema ao vivo, liste os mecanismos que tornam uma ação ruim visível e reversível. Se algum estiver faltando, construa-o primeiro.

### Trocar capacidade por risco de rollback
**O modelo:** pular uma etapa de validação economiza capacidade, mas aumenta o custo de uma falha. O artigo faz essa troca por tipo de release, aceitando-a apenas para releases de menor risco.
**Como aplicar:** classifique as mudanças por risco. Mantenha o caminho completo de validação para mudanças de alto risco e use o caminho mais barato somente onde existe um rollback automático.

## 🔑 Principais conclusões
- A divisão de Mukherjee é explícita: funções críticas para consistência, como controle de acesso, resolução de configuração e correção dos dados, ficam em sistemas determinísticos, e os agentes as usam como ferramentas.
- Agentes se justificam onde modos de falha e remediações não podem ser totalmente listados de antemão, como na depuração.
- A OpenAI construiu um serviço de execução shadow em uma fila de baixa prioridade, um serviço de sobrescrita de configuração e uma skill de validação antes que agentes operassem a atualização do Spark.
- Execuções shadow reutilizam entradas de produção e mantêm o trabalho de teste fora das operações ao vivo, então a atualização é testada sem afetar a produção.
- Para releases incrementais de bibliotecas de menor risco, a equipe às vezes promove diretamente sem uma etapa shadow, trocando capacidade por maior risco de rollback.
- Jobs que passam na validação shadow mas depois falham em produção podem voltar automaticamente para a versão anterior do Spark, sem intervenção manual.
- A recomendação de Mukherjee é começar por tarefas repetitivas e de escopo aberto, construir primeiro os mecanismos de segurança e então deixar os agentes operarem dentro desses limites.

## 🏛️ Para o Kenji
> 🔎 Esta é a sua interpretação, não a do autor. No trabalho de um arquiteto, a lição útil é onde traçar a linha entre um sistema que é dono de uma garantia e um agente que opera sobre ele, como um pipeline de atualização ou uma revisão de configuração. A pergunta que isso levanta: para um fluxo de trabalho que você é dono, quais de seus passos precisam devolver a mesma resposta sempre, e essa garantia está hoje em um sistema ou apenas implícita no comportamento de um agente?

## 📚 Plano de leitura
### Do artigo
- [QCon San Francisco 2026](https://qconsf.com/presentation/nov2026/building-data-platform-ai) — a página da sessão da palestra de Mukherjee, fonte das afirmações resumidas aqui.

### Para aprofundar
- Shadow testing e execução de um novo sistema contra tráfego de produção — o mecanismo por trás da verificação de segurança da atualização do Spark.
- Componentes determinísticos versus agênticos em sistemas de produção — como decidir quais partes precisam ser garantidas e quais podem ficar com o julgamento.
- Rollback e entrega progressiva — os padrões de release por trás do rollback automático e da promoção direta.

## Conceitos
- [ai-agent](/pt-br/concepts/ai-ml/ai-agent.md)
- [shadow-testing](/pt-br/concepts/backend-systems/shadow-testing.md)
- [authentication-authorization](/pt-br/concepts/security/authentication-authorization.md)
- [observability](/pt-br/concepts/cloud-infra/observability.md)
