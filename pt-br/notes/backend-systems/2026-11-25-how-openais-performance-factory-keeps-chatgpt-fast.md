---
type: Article Summary
title: Como a Performance Factory da OpenAI mantém o ChatGPT rápido
description: A Performance Factory da OpenAI usa agentes Codex em fluxos de tarefas rastreados para investigar, corrigir e verificar regressões de performance, com revisão adversarial e aprovação de engenheiros.
resource: https://openai.com/index/how-openai-performance-factory-keeps-chatgpt-fast
tags:
- backend-systems
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-25T06:47:00-03:00'
sources:
- resource: https://openai.com/index/how-openai-performance-factory-keeps-chatgpt-fast
  title: How OpenAI’s Performance Factory keeps ChatGPT fast
  author: OpenAI
status: stable
source_url: https://openai.com/index/how-openai-performance-factory-keeps-chatgpt-fast
source: OpenAI News
source_type: blog
published: '2026-11-19'
domain: backend-systems
concepts:
- ai-agent
- agent-harness
- agent-memory
- llm-as-a-judge
- model-evaluation
- mlops-llmops
- observability
- least-privilege
- event-driven-architecture
- version-control-code-review
- caching
- latency-throughput
- silent-failure
- ci-cd
- performance-regression
- adversarial-review
selected_by: theme
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/423baf74172d811d82b4d44fdb14e40f
lang: pt-br
translation_of: /notes/backend-systems/2026-11-25-how-openais-performance-factory-keeps-chatgpt-fast.md
---

## 🎯 Tese central
A Performance Factory da OpenAI usa agentes Codex dentro de fluxos de trabalho rastreados, de ponta a ponta, para investigar, corrigir e verificar regressões de performance, com revisão adversarial e aprovação de engenheiros nos pontos em que é preciso julgamento.

## 🧭 Por que o autor escreveu
O artigo diz que o ritmo do desenvolvimento agêntico tornou a engenharia de performance tradicional insustentável, porque engenheiros que investigam cada lentidão manualmente não conseguem acompanhar o roadmap. Ele conta como o uso do Codex pela equipe evoluiu até virar a Performance Factory, percorre o primeiro fluxo em execução e oferece lições iniciais para outras equipes de serviços que estão adotando fluxos agênticos. Se a tese se sustenta, a maior parte da investigação e da correção passa para os agentes, e os engenheiros dedicam o tempo às decisões.

## Introdução: o problema do ritmo e o primeiro fluxo
O artigo começa afirmando que o "ritmo impressionante do desenvolvimento agêntico" (staggering pace of agentic development) tornou insustentáveis as abordagens tradicionais de engenharia de performance. Os autores construíram a Performance Factory, que descrevem como um sistema autônomo que usa todo o potencial do Codex e de seus modelos para encontrar e corrigir regressões em velocidade sem precedentes. Ele roda na nuvem, automatiza tanto a investigação quanto a resolução de problemas de performance, e permite acompanhar a ambição do roadmap.

### A Performance Factory
A Factory é o sistema que executa tarefas recorrentes como fluxos de agentes. Os engenheiros definem cada fluxo e escrevem as instruções que os agentes Codex seguem, e a Factory cuida de rastrear o trabalho, atribuir agentes a ele e levar o trabalho de uma etapa à outra. Seu primeiro fluxo é o Regression Workflow, que o artigo detalha mais adiante. O artigo apresenta o sistema inteiro como uma forma de tornar o trabalho agêntico confiável o bastante para problemas de performance em produção.

### Regressão de performance
> 🔎 Contexto adicionado: Uma regressão de performance é uma lentidão mensurável em relação a uma linha de base anterior, normalmente causada por uma mudança de código, de configuração ou de tráfego. O artigo usa o termo para situações como o aumento da latência até o primeiro token do ChatGPT.

O artigo não define o termo, mas seu fluxo depende dele: o trabalho consiste em descobrir quando a lentidão começou, quais requisições ela afetou e qual foi a causa, e depois confirmar que a performance voltou à linha de base após a correção. A escala é o dado de destaque. Em pouco mais de três meses, o Regression Workflow acompanhou mais de 1.700 investigações e identificou a causa raiz de mais de 600 casos, o que os autores estimam em 17 investigações automatizadas por dia.

O artigo então antecipa três pontos: como o uso do Codex evoluiu até a Factory, o primeiro fluxo em execução e as lições iniciais de rodar fluxos de agentes em produção.

## A progressão: seis etapas, das ferramentas manuais à fábrica
Os autores descrevem uma progressão na forma como a equipe trabalhou com o Codex, e esperam que outras equipes de serviços aprendam com ela ao adotar fluxos agênticos em outros domínios.

### 1. Pré-Codex: investigação manual
Antes do Codex, os engenheiros examinavam manualmente métricas e logs, comparavam traces, depuravam máquinas em produção e liam código para investigar uma lentidão. O trabalho consumia muito tempo, e saber onde procurar exigia com frequência um conhecimento profundo do serviço, de modo que sinais relevantes eram fáceis de perder. Quando surgiam problemas recorrentes, como regressões na latência até o primeiro token do ChatGPT, a equipe criava ferramentas avançadas, como profiladores de CPU e de alocação, para facilitar essas investigações. Com o tempo, acumulou mais de 30 ferramentas de performance. Cada ferramenta ajudava com um tipo de investigação, mas o engenheiro ainda precisava saber qual usar e quando.

### 2. Codex como parceiro
Com o Codex como parceiro, um engenheiro podia perguntar "por que isso ficou mais lento?" e chegar à resposta junto com ele, mesmo em um sistema que não conhecia bem. O Codex conseguia pesquisar em grandes bases de código, encontrar métricas e traces relevantes, acompanhar interações entre serviços e executar profiling adequado para reconstruir o que tinha acontecido. Os engenheiros ainda precisavam conduzir o processo, decidindo quais ferramentas de performance usar e como orientar a investigação. O ganho foi que investigações difíceis passaram a ser depuráveis por mais pessoas, exigindo menos experiência e menos tempo.

### 3. Skills reutilizáveis: `perf-forensics`
Para automatizar mais, a equipe de performance registrou seu processo de investigação em uma skill compartilhada chamada `perf-forensics`. A skill informa ao Codex quais ferramentas estão disponíveis, quando usá-las e como analisar uma lentidão por vários ângulos:
- Descobrir quando a lentidão começou
- Identificar as requisições afetadas
- Investigar se deploys ou mudanças de configuração poderiam explicá-la

> 🔎 Contexto adicionado: Um eval é um conjunto de testes repetível, com resultados esperados conhecidos, usado para medir se uma mudança em um prompt, em uma skill ou em um modelo melhora ou piora os resultados.

A equipe testou e refinou a skill com evals construídos a partir de regressões passadas. Para os tipos de regressão em que se concentrou, como latência do ChatGPT e quedas de disponibilidade, um engenheiro podia agora entregar um alerta ao Codex e receber um diagnóstico em uma única execução, sem precisar guiá-lo em cada passo.

### 4. O proxy humano
À medida que a equipe acrescentava e refinava skills relacionadas à performance, passou a se sentir como um proxy entre os alertas e o Codex. Um alerta disparava, um engenheiro copiava e colava o alerta no Codex com a skill certa e depois colava as conclusões de volta no Slack.

> ❌ **Antipadrão — o proxy humano de copiar e colar:** um engenheiro repassa manualmente cada alerta ao agente e cada conclusão de volta à equipe. **Por que as pessoas fazem isso:** o artigo não diz; é o primeiro passo natural quando o agente funciona, mas nada conecta o agente aos alertas. **Consequência:** o número de alertas investigados fica limitado à atenção de uma pessoa, o que o artigo sugere ter sido a restrição que a automação precisava eliminar. **Correção:** iniciar as investigações automaticamente a partir do alerta, como na etapa seguinte.

### 5. Investigações automatizadas
A equipe então levou as investigações para a nuvem, de modo que um alerta pudesse iniciar uma sessão do Codex automaticamente. O Codex investigava a lentidão e publicava suas conclusões na thread do Slack, mas a automação terminava nessa resposta. Um engenheiro ainda precisava pegar o diagnóstico, desenvolver uma correção e verificar se ela funcionava depois do deploy. Conforme os alertas se acumulavam, até decidir quais regressões mereciam atenção exigia vasculhar threads do Slack.

> ❌ **Antipadrão — automatizar apenas o diagnóstico:** a execução do agente termina com uma mensagem em uma thread de chat, e a correção e a verificação pós-deploy continuam manuais. **Por que as pessoas fazem isso:** o artigo não diz; a investigação é a etapa mais fácil de automatizar isoladamente. **Consequência:** o acúmulo de conclusões pendentes cresce, e a priorização volta a depender de pessoas lendo threads. **Correção:** manter o trabalho como uma tarefa persistente que atravessa a correção e a verificação, o que a Factory faz.

### 6. A fábrica
A equipe construiu a Performance Factory para executar fluxos de engenharia de performance agêntica de ponta a ponta. Ela acompanha o que cada tarefa ativa precisa em seguida e atribui um agente quando a etapa está pronta. Cada tarefa mantém seu estado e suas conclusões, de modo que o trabalho pode passar entre agentes e engenheiros e ser retomado após uma revisão de código ou um deploy. No caso de uma regressão, o trabalho começa com a investigação de um alerta e termina com a verificação da recuperação em produção. Otimizações comuns podem se tornar fluxos que rodam regularmente em vários serviços, e um fluxo que identifica uma possível otimização pode abrir uma tarefa em outro fluxo para testá-la e implementá-la.

## Anatomia da Performance Factory
O artigo diz que a Factory tem quatro componentes: um rastreador de tarefas com fluxos configuráveis, um loop de despacho que atribui trabalho pronto a agentes, uma API que executa o Codex na nuvem e assinaturas que acordam agentes quando algo muda, como a publicação de um comentário de revisão no GitHub ou a conclusão de um deploy.

### Rastreador de tarefas e configuração de fluxos
Os engenheiros definem um fluxo adicionando arquivos de configuração e prompts para os agentes, escritos em Markdown. A configuração lista os estados pelos quais uma tarefa pode passar, as transições permitidas entre eles e quais etapas exigem um engenheiro. Prompts separados dizem aos agentes o que fazer em cada estado. Cada item de trabalho vira uma tarefa dentro de um fluxo, e todos os fluxos ficam visíveis em uma interface, onde os engenheiros veem o que está aberto, acompanham o progresso e compartilham uma tarefa com um colega que pode ter mais contexto.

### Loop de despacho
Um loop de despacho roda periodicamente e verifica quais tarefas estão prontas para a próxima etapa. Para cada tarefa pronta, atribui um agente iniciando um turno do Codex com as instruções para o estado atual da tarefa. O agente registra seu progresso na tarefa e a move para o próximo estado se concluir a etapa.

### API do Codex na nuvem e identidade de serviço
Uma vez atribuído, o agente precisa de um lugar para trabalhar, com as mesmas ferramentas que um engenheiro usaria. A equipe construiu uma API interna que invoca um turno do Codex programaticamente na nuvem. O turno roda em um ambiente dentro da rede deles, com acesso a repositórios de código e a ferramentas de observabilidade, como métricas, logs, traces, perfis de CPU e snapshots de memória. O agente roda sob uma identidade de serviço, de modo que suas permissões podem ser restritas e suas ações podem ser distinguidas das de engenheiros. Rodar na nuvem permite que o trabalho avance em paralelo, sem depender de os laptops dos engenheiros estarem abertos nem de gerenciar máquinas de desenvolvimento individuais na nuvem.

> 🔎 Contexto adicionado: Uma identidade de serviço é uma conta não humana, com credenciais e permissões próprias, separada da conta de qualquer pessoa.

### Assinaturas: esperar sem executar
Parte do trabalho precisa esperar por uma pessoa ou por um deploy, e um agente não deveria continuar rodando nesse intervalo. Uma tarefa pode assinar eventos, como comentários de revisão ou checks de CI que falharam em um pull request do GitHub, e ser notificada quando for hora de retomar. Quando um evento recebido corresponde à assinatura, a tarefa volta a ficar pronta e o loop de despacho pode atribuir um agente para continuar o trabalho.

## O Regression Workflow em detalhe
Encontrar, identificar a causa raiz e resolver regressões é o primeiro fluxo construído dentro da Factory. Uma regressão passa por quatro estágios: **needs investigation** (precisa de investigação), **root caused** (causa raiz identificada), **fix proposed** (correção proposta) e **resolved** (resolvida). O fluxo reflete esses mesmos estágios e move cada tarefa por eles.

### Estágios e estado da tarefa
Quando um dos alertas de performance configurados dispara, o Regression Workflow abre uma tarefa, e os agentes a pegam e a movem pelos estágios. A tarefa registra seu status e as conclusões principais fora do contexto de qualquer agente individual, de modo que outros agentes ou engenheiros podem continuar o trabalho. Os engenheiros podem acompanhar o progresso no Slack ou na interface do Regression Workflow, adicionar contexto e revisar as mudanças propostas. Depois que a correção é implantada, um agente verifica se a performance se recuperou antes de resolver a tarefa.

### Encontrando a causa raiz
Esta é a primeira grande etapa. O agente descobre quais métricas pioraram, quando começaram a mudar e quais requisições de serviço foram afetadas. Um alerta dispara quando uma métrica ultrapassa um limite, mas a regressão pode ter começado horas antes, então o agente volta nos dados até o primeiro ponto em que a métrica piorou. Em seguida, compara grupos de requisições. Requisições semelhantes ficaram de fato mais lentas, ou estavam sendo atendidas mais requisições que já eram lentas? O artigo observa que esses dois achados apontam para causas diferentes. Quando sabe quando e onde olhar, o agente examina deploys e mudanças de feature flags, lê o código relevante, verifica quando uma mudança de fato atingiu as requisições afetadas e usa logs ou traces para testar se o código poderia explicar o que aconteceu. A skill `perf-forensics` dá ao Codex acesso às ferramentas que engenheiros de performance usam e a um playbook para essas perguntas.

### Revisão adversarial do diagnóstico
No começo, os agentes às vezes paravam na primeira explicação plausível que encontravam, e alguns desses diagnósticos estavam errados. A correção foi uma revisão adversarial: uma sessão separada do Codex cujo trabalho é refutar o diagnóstico proposto. O revisor reverifica as premissas e testa cenários contrafactuais. Se um rollout causou a lentidão, requisições semelhantes que não foram expostas continuaram saudáveis? Se a mudança suspeita foi revertida, a métrica afetada se recuperou?

> ❌ **Antipadrão — parar na primeira explicação plausível:** o agente aceita a primeira causa que se encaixa e marca a investigação como concluída. **Por que as pessoas fazem isso:** uma história plausível parece terminada, e o artigo relata que os primeiros agentes faziam isso. **Consequência:** diagnósticos errados avançam para correções. **Correção:** um revisor independente que tenta refutar o diagnóstico e executa verificações contrafactuais.

O artigo dá um exemplo. Um agente rastreou conversas do ChatGPT que falhavam até erros em downloads de imagens e marcou a causa como encontrada. O revisor percebeu que o serviço interno responsável por localizar arquivos já tinha se recuperado enquanto as requisições de imagem continuavam falhando. O diagnóstico não explicava as falhas que persistiam, então o revisor reabriu a investigação e registrou o que ainda precisava ser estabelecido.

Dividir trabalhos maiores em turnos focados do Codex também ajudou. Em uma revisão manual de 100 sugestões de causa raiz, a equipe classificou 70% como precisas, e 30% precisavam de mais refinamento ou não estabeleciam totalmente a causalidade. No Regression Workflow, turnos diferentes investigam regressões, questionam diagnósticos e propõem correções, o que os autores descrevem como um sistema de freios e contrapesos para a precisão.

> 🔎 Contexto adicionado: LLM-as-a-judge significa usar um modelo, guiado por uma rubrica, para avaliar a saída de outro modelo. O revisor adversarial aplica a mesma ideia, mas seu objetivo é encontrar a falha, e não atribuir uma nota.

### Propondo e revisando a correção
Depois que o diagnóstico passa pela revisão adversarial, o fluxo pede ao Codex que implemente uma correção e abra um pull request. Um engenheiro revisa o PR e decide se faz o merge. A equipe investiu bastante trabalho para tornar essa passagem fácil. Cada tarefa tem um relatório com um resumo gerado automaticamente sobre a regressão, seu impacto, a causa raiz e a correção proposta. Os resumos são ajustados para serem fáceis de escanear e incluem séries temporais de métricas, flame graphs de CPU e traces, para que os engenheiros possam conferir as evidências rapidamente. Os engenheiros podem editar o PR diretamente ou usar o diagnóstico para escrever a própria correção, e, se deixarem comentários no PR, o fluxo aciona um agente para tratá-los.

### Revisão adversarial da correção
O artigo diz que acertar a correção tem sido mais difícil do que encontrar a causa. Mudanças propostas por agentes às vezes foram mais complicadas do que o necessário ou introduziram novos problemas. Por isso, cada correção proposta passa por uma revisão adversarial que verifica se ela resolve a causa raiz e se apanha bugs ou consequências não intencionais. Algumas correções também exigem decisões arquiteturais. Adicionar um cache, por exemplo, pode melhorar a performance, mas introduzir dados desatualizados, e essas mudanças se beneficiam de um engenheiro que conhece o serviço e pode avaliar os trade-offs.

> ❌ **Antipadrão — correções não verificadas ou excessivamente complicadas:** o agente propõe uma mudança mais elaborada do que o problema exige, ou que introduz novos bugs. **Por que as pessoas fazem isso:** o artigo não diz; uma correção que parece completa é fácil de aceitar. **Consequência:** novos problemas chegam à revisão, e o revisor assume o custo extra. **Correção:** uma verificação adversarial em cada correção, mais o julgamento do engenheiro sobre os trade-offs arquiteturais.

### Verificando a recuperação após o deploy
Depois que um engenheiro implanta a correção, o fluxo aciona um agente para verificar se a performance voltou à linha de base anterior. Se voltou, o agente marca a tarefa como resolvida. Se a performance se recuperou apenas parcialmente, o agente resolve a regressão original e abre uma nova tarefa para a lentidão remanescente, que pode ter outra causa. Se não houver melhora, o agente registra o que aconteceu e devolve a regressão para investigação.

### O ciclo de avaliação
A equipe está construindo um ciclo de avaliação e melhoria a partir desses casos. Ao comparar as correções propostas pelos agentes com as mudanças que os engenheiros acabam fazendo merge, consegue ver onde as propostas ficam aquém, criar evals e melhorar o fluxo. Também está trabalhando em testes sintéticos de performance, para que os agentes possam medir uma correção proposta e revisá-la antes de pedir a revisão de um engenheiro. Por fim, está explorando como os agentes poderiam usar suas outras ferramentas de revisão agêntica para identificar mudanças de baixo risco e, eventualmente, entregar essas correções sem um humano no loop.

## Lições para equipes que constroem fluxos agênticos
Os autores encerram a parte técnica com lições para equipes que constroem os próprios fluxos agênticos.

### Torne a saída do agente pronta para decisão
A saída bruta de uma investigação do Codex pode ser um bloco de texto difícil de interpretar. Os engenheiros precisam descobrir o que o agente está afirmando e encontrar os gráficos que mostram se os dados sustentam a afirmação. O objetivo é que os engenheiros gastem sua atenção em decisões, e não em reconstruir o trabalho do agente.

> ❌ **Antipadrão — uma interface cheia de conclusões sem ranking:** a primeira versão apresentava muitas conclusões aos engenheiros, mas alguém ainda precisava decidir quais importavam. **Por que as pessoas fazem isso:** o artigo não diz; produzir mais saída é mais fácil do que decidir o que importa. **Consequência:** o peso da revisão passa para as pessoas. **Correção:** resumos estruturados com as evidências anexadas, como nos relatórios do Regression Workflow.

### Agrupe incidentes relacionados e ordene por impacto
Agentes conseguem investigar mais regressões do que uma equipe tem tempo de ler. Uma única regressão também pode aparecer como vários incidentes em alertas, monitores e canais diferentes. O fluxo agora vincula incidentes relacionados, agrupa-os pelas métricas com que as equipes se importam e ordena-os pelo impacto estimado, para que os engenheiros possam se concentrar em acrescentar contexto, pesar os trade-offs e decidir o que entregar.

> ❌ **Antipadrão — tratar cada alerta como um problema separado:** a mesma regressão é investigada várias vezes sob alertas diferentes. **Por que as pessoas fazem isso:** cada alerta chega como um item próprio. **Consequência:** investigação duplicada e uma fila mais longa. **Correção:** vincular e agrupar incidentes relacionados antes de investigar.

### Teste antes de mudar uma skill ou um modelo
Atualizar uma skill de investigação ou trocar o modelo pode tornar o agente melhor em diagnosticar alguns casos e pior em outros, então o impacto precisa ser testado antes de a mudança ir para produção. Os autores testam duas coisas separadamente: quão bem o agente investiga e quão útil são os resumos para os engenheiros.

> ❌ **Antipadrão — publicar uma atualização de skill ou de modelo sem testes:** presume-se que a nova versão é melhor em todos os casos. **Por que as pessoas fazem isso:** o artigo não diz; um modelo mais novo parece uma atualização. **Consequência:** alguns casos pioram sem que ninguém perceba. **Correção:** rodar a suíte de avaliação antes da atualização e pontuar investigação e resumos separadamente.

### Faça backtest das investigações com incidentes passados
Para as investigações, a equipe faz backtest com incidentes de produção passados. Ela transforma esses incidentes em uma suíte de casos de teste que podem ser executados novamente sempre que a skill muda. Um agente Codex tenta investigar e resolver cada incidente, e seu comportamento é comparado com os dados reais de resolução passada. Cada caso começa com um investigador Codex novo e com o sinal original, sem nenhum detalhe que possa revelar a resposta.

Testes históricos exigem cuidado. Métricas e traces expiram, e evidências coletadas depois da recuperação podem revelar a causa. Um teste só é útil se você souber quais evidências o investigador podia ver. Na configuração deles, o agente pode inspecionar código e consultar telemetria de produção dentro de limites definidos. Ele não pode usar memórias salvas (anotações de investigações anteriores), ler o relatório do incidente nem ver resultados de execuções anteriores. A equipe também inspeciona cada transcrição depois, em busca de informações que possam ter estragado o teste.

> ❌ **Antipadrão — um backtest vazado:** o agente de teste pode ver evidências posteriores à recuperação, o relatório do incidente ou resultados anteriores, e pode passar lendo a resposta. **Por que as pessoas fazem isso:** o artigo não diz; a evidência mais rica também é a mais tentadora. **Consequência:** os números de acurácia parecem melhores do que o agente realmente é. **Correção:** limitar as evidências visíveis, começar com um investigador novo e auditar a transcrição.

### Pontue as dimensões separadamente
A equipe avalia quando o problema começou, qual foi a sua magnitude e quais requisições foram afetadas. Também avalia como o agente usa as evidências para ranquear possíveis causas e como explica o que continua incerto. A razão para separar essas dimensões é que um agente pode acertar o momento e, ainda assim, culpar o deploy errado.

> ❌ **Antipadrão — uma pontuação única misturada:** tempo, escopo e causa são somados em um único número. **Por que as pessoas fazem isso:** um número só é fácil de reportar. **Consequência:** um tempo correto pode esconder uma causa errada. **Correção:** pontuar cada dimensão isoladamente.

### Avalie resumos a partir de snapshots salvos
A equipe avalia os resumos de tarefas usando snapshots de tarefas salvas. Elas são backtests especialmente bons, porque a sumarização não depende de métricas ou traces externos, que expiram. Um avaliador separado verifica se um engenheiro que não conhece o sistema conseguiria entender o que aconteceu e por que isso importa, se os gráficos úteis foram mantidos e se o próximo passo proposto é sustentado pelas evidências.

### Monitore os agentes além dos serviços
Agentes precisam de monitoramento tanto quanto os serviços que investigam. Um profiler quebrado ou uma permissão ausente podem impedir um agente de verificar o que causou uma lentidão, e falhas assim podem ser difíceis de notar no relatório final. A equipe roda diariamente um agente para analisar mensagens e logs de execuções recentes, buscando falhas de ferramentas e erros de acesso.

> ❌ **Antipadrão — presumir que agentes falham de forma explícita:** um agente que não consegue rodar suas ferramentas ainda produz um relatório que parece completo. **Por que as pessoas fazem isso:** o artigo não diz; a saída final é a única coisa que a maioria das pessoas lê. **Consequência:** uma causa fica sem verificação, e a lacuna é difícil de enxergar. **Correção:** uma verificação agendada dos logs de execução em busca de erros de ferramentas e de acesso.

### Deixe a fábrica se consertar
Quando o mesmo erro de ferramenta bloqueia várias investigações, o agente cria uma tarefa para corrigi-lo. Se a causa estiver clara, um agente propõe a correção, e um engenheiro revisa a mudança e decide se faz o merge. Depois do merge, a equipe usa evals para verificar se os agentes agora conseguem passar pela etapa que antes os bloqueava.

## Para onde a fábrica vai
O Regression Workflow é uma instância do que o artigo chama de padrão agent factory, que move tarefas por estados explícitos. Os agentes avançam uma tarefa quando há mais trabalho pronto, revisores separados questionam as conclusões, e os engenheiros intervêm quando a próxima decisão exige julgamento. Outros fluxos podem reutilizar a mesma engrenagem com estados, instruções e eventos diferentes.

### Detecção proativa sem alerta
Hoje o Regression Workflow começa quando um alerta dispara. A equipe quer que a Factory encontre lentidões e oportunidades de otimização mesmo quando nenhum alerta dispara. Comparando lotes de perfis de CPU e de memória, heap dumps e traces de requisições entre releases, um agente poderia perceber um pequeno aumento de CPU para uma determinada requisição. Ele então poderia abrir uma tarefa do Regression Workflow para rastrear o aumento até a causa, implementar uma correção e abrir um PR para revisão.

### Codificando sinais conhecidos
Quando a equipe já sabe o que procurar, ela está codificando esses sinais em mais fluxos. O exemplo do artigo é uma skill de rightsizing de serviços, que ajuda agentes a identificar configurações de autoscaling que mantêm mais réplicas em execução do que um serviço precisa. Depois que um padrão desses é reconhecido, a equipe quer construir e executar um fluxo para ele em todos os serviços, para que continue otimizando em um ambiente que muda rapidamente.

### Lições que se acumulam
O que um engenheiro aprende com uma lentidão pode moldar a forma como a próxima é investigada e corrigida, mesmo meses depois, em outro serviço. A conclusão do artigo é que o gargalo de hoje pode ser esperar que um engenheiro revise e faça o merge de uma correção, e a equipe já está explorando se os agentes podem fazer isso sozinhos. A última frase do artigo diz: "Codex already helps us build ChatGPT faster. Now it's helping us keep ChatGPT fast." (O Codex já nos ajuda a construir o ChatGPT mais rápido. Agora ele nos ajuda a mantê-lo rápido.)

## 🧠 Modelos mentais
### O padrão agent factory
**O modelo:** uma tarefa passa por estados explícitos. Agentes a avançam quando o trabalho está pronto, revisores separados questionam suas conclusões, e engenheiros intervêm quando uma decisão exige julgamento.
**Como aplicar:** antes de escrever qualquer prompt de agente, defina os estados, as transições permitidas e quais transições exigem um humano, e depois reutilize a mesma engrenagem em cada novo fluxo.

### Estado durável da tarefa em vez do contexto do agente
**O modelo:** é a tarefa, e não o agente, que guarda o status, as conclusões e as evidências, de modo que qualquer agente ou engenheiro pode retomar o trabalho depois.
**Como aplicar:** armazene conclusões, evidências e o próximo passo no item de trabalho, para que um novo agente comece a partir do registro da tarefa, e não de uma transcrição de chat anterior.

### Desafio contrafactual
**O modelo:** uma conclusão só é confiável se sobreviver a verificações que pareceriam diferentes caso ela fosse falsa.
**Como aplicar:** para cada causa proposta, anote qual observação deveria ser diferente se a causa estiver errada, como requisições semelhantes não expostas continuarem saudáveis ou a métrica se recuperar depois de um rollback, e verifique isso.

### Pontue cada dimensão separadamente
**O modelo:** uma resposta pode estar certa em um eixo e errada em outro, e uma pontuação combinada esconde qual é qual.
**Como aplicar:** meça tempo, magnitude, escopo afetado e ranking de causas como métricas separadas, e reporte a incerteza junto com elas.

## 🔑 Principais conclusões
- O Regression Workflow acompanhou mais de 1.700 investigações em pouco mais de três meses e identificou a causa raiz de mais de 600, o que os autores estimam em 17 investigações automatizadas por dia.
- Automatizar apenas o diagnóstico move o gargalo para as pessoas; o ganho veio de um estado de tarefa persistente que leva o trabalho até a correção e à verificação pós-deploy.
- Agentes iniciais paravam na primeira explicação plausível, e um revisor adversarial que tenta refutar o diagnóstico e testa cenários contrafactuais foi adicionado para pegar esse problema.
- Em uma revisão manual de 100 sugestões de causa raiz, 70% foram classificadas como precisas e 30% precisavam de refinamento ou não estabeleciam totalmente a causalidade.
- Um backtest só é significativo se você controlar quais evidências o agente pode ver: um investigador novo, um sinal sem detalhes, sem memórias salvas nem resultados anteriores, e uma auditoria da transcrição.
- Tempo, magnitude, requisições afetadas e ranking de causas devem ser pontuados separadamente, porque um agente pode acertar o tempo e, ainda assim, culpar o deploy errado.
- Agentes precisam de monitoramento próprio, já que um profiler quebrado ou uma permissão ausente podem interromper silenciosamente uma investigação antes que ela verifique sua causa.

## 🏛️ Para o Kenji
Esta é a minha interpretação, não a do autor. A Factory é um fluxo governado: estados declarados, identidades de serviço com escopo restrito, pontos de aprovação humana e um registro auditável do que cada agente fez. Isso é estruturalmente próximo dos controles de governança de IA com que você já trabalha, e sugere uma questão de nível arquitetural: quais das suas próprias decisões precisariam dos mesmos estados explícitos, pontos de aprovação e ciclo de avaliação antes que um agente pudesse agir sobre elas sem uma pessoa no loop?

## 📚 Plano de leitura
### Do artigo
- O artigo não cita fontes externas. Seu único link é um "View all" (ver tudo) que leva ao índice de notícias da OpenAI, sob um rodapé "Keep reading" (continue lendo). Isso é navegação do site, por isso não está listado aqui.

### Para aprofundar
- *Site Reliability Engineering* (Google, 2016), para as práticas de SLO, alertas e postmortem que enquadram o ciclo de alerta até correção que a Factory automatiza.
- "Dapper, a Large-Scale Distributed Systems Tracing Infrastructure" (Google, 2010), como base sobre os traces distribuídos que os agentes leem durante as investigações.
- *Designing Data-Intensive Applications* (Martin Kleppmann, 2017), sobre os trade-offs de replicação e consistência por trás do exemplo de cache e do risco de dados desatualizados.
- Backtesting de agentes de LLM contra incidentes históricos e manutenção dos dados de avaliação livres de vazamento, o método prático que o artigo descreve.
- Profiling contínuo (CPU e heap) como sinal de regressão, a base da detecção proativa que os autores estão construindo.

## Conceitos
- [ai-agent](/pt-br/concepts/ai-ml/ai-agent.md)
- [agent-harness](/pt-br/concepts/ai-ml/agent-harness.md)
- [agent-memory](/pt-br/concepts/ai-ml/agent-memory.md)
- [llm-as-a-judge](/pt-br/concepts/ai-ml/llm-as-a-judge.md)
- [model-evaluation](/pt-br/concepts/ai-ml/model-evaluation.md)
- [mlops-llmops](/pt-br/concepts/ai-ml/mlops-llmops.md)
- [observability](/pt-br/concepts/cloud-infra/observability.md)
- [least-privilege](/pt-br/concepts/security/least-privilege.md)
- [event-driven-architecture](/pt-br/concepts/architecture/event-driven-architecture.md)
- [version-control-code-review](/pt-br/concepts/software-engineering/version-control-code-review.md)
- [caching](/pt-br/concepts/backend-systems/caching.md)
- [latency-throughput](/pt-br/concepts/backend-systems/latency-throughput.md)
- [silent-failure](/pt-br/concepts/backend-systems/silent-failure.md)
- [ci-cd](/pt-br/concepts/software-engineering/ci-cd.md)
- [performance-regression](/pt-br/concepts/software-engineering/performance-regression.md)
- [adversarial-review](/pt-br/concepts/ai-ml/adversarial-review.md)
