---
type: Article Summary
title: 15 projetos open source para agentes de IA em produção
description: Escolha ferramentas para agentes em produção pela tarefa que cada componente executa, e separe implantações nomeadas de simples afirmações de fornecedores.
resource: https://medium.com/@unicodeveloper/15-open-source-projects-for-ai-agents-in-production-fd8a329d00da
tags:
- ai-ml
- ai-agent
- agent-harness
- model-context-protocol
- ai-gateway
- mlops-llmops
- model-evaluation
- observability
- idempotency
- least-privilege
- indirect-prompt-injection
- ai-security
- software-testing
- durable-execution
- sandboxed-execution
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-26T06:47:00-03:00'
sources:
- resource: https://medium.com/@unicodeveloper/15-open-source-projects-for-ai-agents-in-production-fd8a329d00da
  title: 15 Open Source Projects for AI Agents in Production
  author: unicodeveloper
status: stable
source_url: https://medium.com/@unicodeveloper/15-open-source-projects-for-ai-agents-in-production-fd8a329d00da
source: Medium (unicodeveloper)
source_type: medium
published: '2026-11-02'
domain: ai-ml
concepts:
- ai-agent
- agent-harness
- model-context-protocol
- ai-gateway
- mlops-llmops
- model-evaluation
- observability
- idempotency
- least-privilege
- indirect-prompt-injection
- ai-security
- software-testing
- durable-execution
- sandboxed-execution
selected_by: saved-backlog
content: member-session
complete: 'yes'
notion_url: https://app.notion.com/p/424baf74172d817f967dd049a93b9fd9
lang: pt-br
translation_of: /notes/ai-ml/2026-11-26-15-open-source-projects-for-ai-agents-in-production.md
---

## 🎯 Tese central
Escolher ferramentas para agentes de IA em produção deve começar pela tarefa que cada componente precisa cumprir (execução, trabalho durável, acesso a ferramentas, observabilidade, avaliação, roteamento de modelos), e não por uma busca pelo único "melhor" framework.

## 🧭 Por que o autor escreveu
O autor parte de um mercado que se move rápido: o artigo afirma que dois em cada cinco desenvolvedores já executam agentes de IA em produção, e as ferramentas mudam quase tão depressa quanto os agentes. Se a pergunta certa é "o que este agente precisa fazer para cumprir sua função de forma confiável?", a resposta varia de empresa para empresa. Por isso, uma lista de projetos open source precisa ser agrupada por responsabilidade e sustentada por evidências sobre onde cada projeto é de fato usado.

## Introdução (sem título no artigo)
A seção de abertura faz três movimentos. Primeiro, afirma que "qual ferramenta é a melhor?" é o ponto de partida errado. Segundo, apresenta dois exemplos contrastantes. Os agentes da Genspark precisam executar código, trabalhar com arquivos e manter estado ao longo de tarefas com várias etapas; o artigo diz que o relato publicado pela Genspark sobre sua implementação descreve o uso do E2B para esses ambientes de execução. Os agentes de suporte da Canva precisam de tracing, gestão de prompts e avaliações, que a equipe executa com o Langfuse. Terceiro, separa as funções: o software que roda o código de um agente não explica por que o agente cometeu um erro, e uma plataforma de observabilidade não fornece um ambiente de execução.

O artigo então delimita seu escopo. As entradas de infraestrutura se apoiam em implantações nomeadas e dependências diretas. As aplicações (monitoramento de cibersegurança, pesquisa geopolítica, análise de mercados de previsão) mostram fluxos de trabalho completos; o artigo diz que elas não são apresentadas como dependências usadas pela maioria dos agentes.

> 🔎 Contexto adicional: aqui, "dependência" significa um pacote que o projeto declara como necessário, encontrado em seu manifesto. "Implantação documentada" significa uma empresa que descreve publicamente como usa a ferramenta.

## TL;DR
O resumo do artigo traz uma linha por responsabilidade:

- **Construir o agente:** LangGraph e Pydantic AI oferecem abordagens para fluxo de controle com estado, ferramentas tipadas e integração com aplicações.
- **Manter o trabalho em execução:** Temporal lida com fluxos de trabalho duráveis em segundo plano, com um papel documentado na arquitetura do Content Agent da Sanity.
- **Conectar ferramentas e executar tarefas:** Pydantic, o SDK Python do MCP, Playwright, Browser Use e E2B fornecem contratos de dados, conectividade com ferramentas, controle de navegador e ambientes de execução.
- **Operar e melhorar o sistema:** Langfuse, promptfoo e LiteLLM cobrem observabilidade, testes e acesso a modelos. O artigo ressalta que eles cumprem papéis diferentes e não são intercambiáveis.
- **Acrescentar pesquisa e estudar aplicações completas:** o SDK TypeScript da Valyu fornece APIs de pesquisa; Global Hack Map, Global Threat Map e Polyseer mostram interfaces e saídas específicas de domínio.

O artigo diz que a lista é agrupada por responsabilidade, não ordenada por popularidade.

## Frameworks de agentes

### 1. LangGraph: estado explícito e fluxo de controle
LangGraph é um framework para orquestração de agentes com estado. Ele modela o trabalho do agente como um grafo de etapas, de modo que as transições são explícitas, e oferece checkpointing, que salva o estado da execução para que o trabalho possa ser retomado, e interrupções que aguardam a entrada humana. O checkpointing importa porque uma tarefa de agente que abrange várias chamadas de ferramentas e aprovações não é uma única resposta do modelo; o estado entre as etapas é o que precisa sobreviver.

Os exemplos de implantação publicados no artigo incluem o assistente de recrutamento da LinkedIn, os fluxos de teste de código da Uber e o copiloto de administração de imóveis da AppFolio. O artigo descreve essas aplicações como aplicações com várias etapas e ferramentas, em que o estado da execução importa além da última resposta do modelo.

O artigo diz que o LangGraph vale a pena ser avaliado quando uma aplicação precisa se ramificar, pausar para aprovação ou continuar uma tarefa parcialmente concluída. Seu conselho é definir o estado que deve sobreviver e tornar as transições visíveis no código da aplicação.

> ❌ **Anti-padrão — restaurar o estado como se fosse toda a recuperação:** presumir que, porque o estado do grafo tem checkpoint, uma execução retomada é segura. **Por que as pessoas fazem isso:** o checkpointing é o recurso visível, e parece uma recuperação completa. **Consequência:** o artigo alerta que restaurar o estado do grafo não impede, por si só, um pagamento repetido ou uma mensagem duplicada. **Correção:** use armazenamento persistente de checkpoints e projete as ações externas para novas tentativas, de modo que repetir uma etapa não repita seu efeito.

### 2. Pydantic AI: agentes Python tipados
Pydantic AI oferece ferramentas tipadas, saídas estruturadas e injeção de dependências para agentes em Python. Injeção de dependências significa que os componentes de que um agente precisa (clientes, configuração, handles de banco de dados) são passados de fora, em vez de criados dentro do agente, o que facilita os testes. O artigo observa que o Pydantic AI é um projeto separado da biblioteca de validação Pydantic, abordada mais adiante na lista.

A evidência do artigo é um relato publicado de migração da MindsDB, que descreve a adoção do Pydantic AI para obter controle programático sobre o estado do agente e para validar saídas intermediárias. Ele também cita código público de terceiros: o manifesto de dependências do marimo inclui o Pydantic AI Slim para seus recursos de IA.

O encaixe prático, nos termos do artigo, é uma aplicação Python em que a saída de um componente se torna entrada de outro. Resultados tipados permitem que o código comum da aplicação decida o que acontece em seguida, em vez de pedir a outra chamada de modelo que interprete um parágrafo.

O artigo alerta para manter a validação de regras de negócio separada da validação estrutural. Um ID de fornecedor corretamente tipado ainda pode identificar o fornecedor errado, e um schema de ação válido não autoriza a ação.

## Execução durável e trabalho em segundo plano

### 3. Temporal: recuperar fluxos de trabalho de longa duração
Temporal separa a coordenação durável do fluxo de trabalho das atividades que executam trabalho externo. Ele registra o histórico do fluxo, de modo que a execução pode ser recuperada após interrupções, como um worker que travou. Nesse modelo, o código do fluxo decide a sequência de etapas, e as atividades são as chamadas individuais que tocam o mundo externo; é nas atividades que novas tentativas e falhas são tratadas.

O relato de implantação da Sanity no artigo descreve o uso do Temporal para calcular um grafo de schema de conteúdo e executar tarefas de mutação em paralelo em segundo plano. Esses trabalhos dão suporte ao agente sem exigir que toda a operação permaneça dentro de uma requisição de chat ao vivo.

O artigo diz que o Temporal vale a pena ser considerado quando uma tarefa cruza fronteiras de serviços, aguarda um evento ou aprovação, ou precisa sobreviver a falhas de workers. Seu exemplo: um usuário envia um job, sai da aplicação e volta mais tarde para verificar o andamento ou o resultado.

As ressalvas do artigo são que as equipes precisam entender o código determinístico de fluxo de trabalho, o comportamento de novas tentativas das atividades e o versionamento dos fluxos. Efeitos colaterais externos ainda exigem idempotência adequada. Ele também observa que executar o serviço do Temporal por conta própria é um compromisso diferente de usar o Temporal Cloud.

## Contratos de dados e conectividade com ferramentas

### 4. Pydantic: uma dependência compartilhada entre provedores de modelos
Pydantic oferece modelos de dados tipados, validação, serialização (conversão de objetos de e para um formato de transporte, como JSON) e geração de JSON Schema. JSON Schema é uma descrição padrão do que um documento JSON pode conter, e é assim que muitas definições de ferramentas informam ao modelo quais argumentos ele pode enviar.

A evidência de adoção do artigo é direta: o Pydantic é uma dependência declarada nos SDKs Python oficiais da OpenAI, da Anthropic e do Google, bem como no Agents SDK da OpenAI.

Para uma aplicação com agentes, o artigo defende que contratos de dados explícitos são úteis sempre que uma requisição de ferramenta, uma resposta do modelo ou um resultado de API entra no código comum. Eles tornam testáveis os campos aceitos e os valores inválidos, e dão aos componentes seguintes uma representação previsível.

A ressalva do artigo é decidir o que a validação significa em cada fronteira. Forma, correção de negócio e autorização são verificações diferentes. Ele também observa que essas dependências de SDK não revelam como os serviços privados de inferência dos provedores são implementados.

### 5. SDK Python do MCP: uma interface de ferramentas reutilizável
O SDK Python oficial do Model Context Protocol fornece software para construir clientes e servidores MCP. O artigo deixa explícito que o projeto desta entrada é o SDK, não o protocolo em abstrato. MCP é um protocolo baseado em JSON-RPC que padroniza a forma como aplicações de IA se conectam a ferramentas, fontes de dados e prompts.

O artigo cita uso por terceiros: o Agents SDK da OpenAI e o Browser Use declaram o pacote Python `mcp` como dependência de execução. O artigo trata esses manifestos como evidência de uso em software de agentes, e não como prova de que os produtos são apenas compatíveis.

O argumento é que uma interface padrão de ferramentas permite que um serviço exponha suas operações a clientes diferentes. A aplicação pode reutilizar a implementação da ferramenta em vez de escrever um adaptador separado para cada framework de agentes.

A ressalva do artigo: implemente você mesmo a autenticação, a autorização, a validação de entrada e o isolamento de tenants. O MCP descreve a comunicação; ele não decide quais registros de clientes um chamador pode acessar. Fixe versões compatíveis do SDK e revise as atualizações de segurança.

> 🔎 Contexto adicional: "isolamento de tenants" significa garantir que os dados de um cliente não possam ser alcançados por meio das requisições de outro cliente em um sistema multicliente.

## Controle de navegador e execução de código

### 6. Playwright: automação de navegador programada
Playwright oferece primitivas de automação de navegador para navegação, interação com páginas, downloads e inspeção. Uma primitiva é uma operação básica que a biblioteca expõe, a partir da qual scripts maiores são construídos.

O artigo cita duas dependências concretas por terceiros: o manifesto do OpenClaw lista o `playwright-core` como dependência de execução, e a implementação Playwright MCP da Microsoft depende do Playwright. O artigo acrescenta que isso não significa que toda tarefa de agente abra um navegador.

O argumento: para um fluxo conhecido em um portal, ações programadas no navegador podem ser mais fáceis de testar do que pedir a um modelo que escolha cada clique. Navegue pelas páginas esperadas, baixe o documento e verifique o resultado antes de passá-lo ao componente seguinte.

As ressalvas do artigo são isolar as sessões do navegador, proteger o estado autenticado e usar portões de aprovação para ações com consequências. Ele também observa que nem todo agente de navegador usa o Playwright; alguns usam seus próprios drivers do Chrome DevTools Protocol.

### 7. Browser Use: interação com navegador guiada por tarefa
O Browser Use oferece uma biblioteca para agentes de navegador e ferramentas relacionadas, com serviços em nuvem separados. A evidência do artigo é a documentação de terceiros de primeira mão do Hermes Agent, que identifica o browser-harness do Browser Use como o driver por trás de sua ferramenta de execução de navegador e o lista como dependência Python comum. O Hermes explica como o código escrito pelo modelo opera um navegador local ou um navegador configurado na nuvem.

O artigo apresenta isso como útil quando um agente precisa navegar por um site desconhecido, inspecionar uma interface que muda ou concluir uma tarefa cuja rota não pode ser totalmente roteirizada antecipadamente. O driver executa as operações; a aplicação ao redor determina o que o agente tem permissão para tentar.

As ressalvas do artigo: trate o conteúdo das páginas como entrada não confiável, restrinja os destinos e limite a execução. Separe a pesquisa somente leitura de compras ou alterações de conta. Uso de navegador hospedado e uso de biblioteca local são escolhas de implantação diferentes.

> ❌ **Anti-padrão — tratar o conteúdo da página como instruções:** deixar que o texto de uma página visitada dirija as próximas ações do agente. **Por que as pessoas fazem isso:** um agente de navegador lê páginas para agir, então o texto da página pode parecer parte da tarefa. **Consequência:** o agente pode seguir instruções que um atacante colocou na página; este é o risco de injeção indireta de prompt que a orientação do artigo aborda. **Correção:** trate o conteúdo da página como entrada não confiável, mantenha as ações permitidas ao agente definidas pela aplicação e exija aprovação para as que têm consequências.

### 8. E2B: ambientes de execução para agentes
E2B oferece SDKs para criar e controlar ambientes de execução isolados, incluindo capacidades de interpretador de código. Um ambiente isolado é um lugar separado para executar código, de modo que o código gerado não roda com o acesso do próprio servidor web.

A evidência de implantação do artigo é o relato da Genspark sobre o uso do E2B para agentes que executam código, trabalham com arquivos e concluem tarefas com várias etapas. Ele também cita contas de clientes do E2B envolvendo Gumloop e Lindy.

O artigo apresenta o requisito concreto que isso atende: código gerado precisa de um lugar para rodar sem herdar a autoridade do servidor web. Seu exemplo: um agente de análise de dados recebe um arquivo, executa a análise em um ambiente separado e devolve artefatos para o usuário inspecionar.

As ressalvas do artigo: decida quais credenciais, dados e acesso à rede entram em cada ambiente, e defina limites de recursos e de tempo de vida. Ele também observa que as evidências de clientes se referem ao serviço do E2B e não estabelecem que esses clientes hospedem por conta própria os componentes open source do E2B.

## Observabilidade, testes e operações de modelos

### 9. Langfuse: rastrear e avaliar o comportamento do agente
Langfuse oferece tracing, gestão de prompts, datasets e avaliações para aplicações baseadas em modelos. Um trace registra as etapas de uma requisição, como recuperação, chamadas de modelo e operações com ferramentas, para que uma falha possa ser localizada.

A conta da Canva no artigo descreve o uso do Langfuse em um Java Help Assistant e em um agente de suporte assíncrono em Python. A equipe rastreia a execução, versiona prompts e roda avaliações online e offline (online significa sobre tráfego real; offline, sobre conjuntos de teste armazenados). A conta também explica a migração da Canva de hospedagem própria para o Langfuse Cloud.

A prática operacional do artigo: quando um agente devolve um resultado ruim, rastreie a recuperação, as chamadas de modelo e as operações com ferramentas para encontrar a falha. Preserve falhas representativas como casos de teste, para que a próxima versão possa ser verificada contra elas.

As ressalvas do artigo: planeje a redação de dados sensíveis, o acesso e a retenção antes de registrar conversas de clientes ou respostas de ferramentas. Instrumente a lógica da aplicação além das chamadas de modelo, porque um trace que contém apenas a geração final pode deixar passar o problema.

> ❌ **Anti-padrão — rastrear apenas a saída final do modelo:** registrar a última geração e nada mais. **Por que as pessoas fazem isso:** a resposta final é o artefato visível, e é a coisa mais fácil de capturar. **Consequência:** a causa raiz, muitas vezes um resultado anterior de recuperação ou de ferramenta, fica ausente do registro. **Correção:** instrumente a lógica da aplicação ao redor, além das chamadas de modelo.

### 10. promptfoo: avaliações e testes adversariais
promptfoo é uma CLI e uma biblioteca para avaliar e fazer red-teaming de aplicações baseadas em modelos, com suporte a fluxos de desenvolvimento automatizados. Red-teaming significa tentar deliberadamente fazer o sistema falhar ou se comportar mal, por exemplo, empurrando-o para ações proibidas.

O artigo relata que as declarações de adoção publicadas pelo promptfoo identificam a OpenAI e a Anthropic como usuárias. Ele classifica isso como uma afirmação publicada pelo fornecedor, não como um manifesto público de dependências nem como um relato detalhado de uma implantação interna específica. Acrescenta que um software de testes pode apoiar um produto em produção sem estar presente em seu caminho de requisição.

O fluxo prático: transforme falhas em testes repetíveis. Guarde exemplos de requisições válidas, afirmações sem suporte e ações proibidas, e execute-os sempre que um prompt, um modelo ou a implementação de uma ferramenta mudar.

As ressalvas do artigo: teste o comportamento observável, incluindo ações de ferramentas quando a integração as expõe. Uma resposta final convincente não basta. Passar em uma suíte de avaliação estabelece desempenho nesses testes, não proteção contra todos os ataques. Revise para onde os avaliadores configurados enviam dados.

> ❌ **Anti-padrão — confundir uma suíte aprovada com segurança:** tratar uma execução de avaliação sem falhas como prova de que o sistema está protegido. **Por que as pessoas fazem isso:** uma suíte dá um resultado claro e contável. **Consequência:** a suíte cobre apenas os casos que contém, então ataques e falhas não testados passam despercebidos. **Correção:** trate a suíte como evidência sobre casos nomeados, continue ampliando-a a partir de falhas reais e teste ações além de textos.

### 11. LiteLLM: acesso compartilhado a modelos e roteamento
LiteLLM oferece uma interface unificada de modelos e um servidor proxy com roteamento, chaves virtuais, controle de gastos e balanceamento de carga. Uma chave virtual é uma credencial no nível da aplicação que o gateway mapeia para as chaves reais dos provedores.

O depoimento de cliente no artigo inclui a NVIDIA, que descreve uma interface consistente para endpoints de modelos em provedores de nuvem, implantações open source e serviços internos. O artigo diz que isso sustenta o papel do LiteLLM como infraestrutura compartilhada de acesso a modelos, e não faz nenhuma afirmação sobre todos os agentes da NVIDIA.

O argumento: para uma equipe que opera várias aplicações, um gateway comum torna o uso atribuível e facilita a gestão da configuração dos provedores. Também dá aos operadores um único lugar para configurar o roteamento aprovado e investigar falhas, em vez de procurar em várias implementações de cliente não relacionadas.

As ressalvas do artigo: avalie modelos de fallback nas tarefas que de fato vão receber. Formatos de API compatíveis não garantem uso equivalente de ferramentas nem qualidade de saída equivalente. Verifique o licenciamento de recursos específicos e teste a imposição de orçamento, em vez de tratar um painel como limite rígido de gastos.

## Ferramentas de pesquisa e aplicações de referência

### 12. SDK TypeScript da Valyu: pesquisa dentro de um agente existente
O SDK TypeScript da Valyu, publicado como `valyu-js`, expõe busca, extração de conteúdo e DeepResearch assíncrono por meio de interfaces tipadas em JavaScript e TypeScript. Assíncrono, aqui, significa que a tarefa de pesquisa é submetida, roda por algum tempo e é recuperada depois pelo seu ID.

O artigo identifica uma implementação concreta para inspecionar: o cliente do lado do servidor do Global Threat Map importa o SDK para recuperar eventos e executar pesquisas. O artigo apresenta isso como evidência de uso em uma aplicação baseada na Valyu, não como uma afirmação independente sobre a participação de mercado do SDK.

O argumento: para um assistente de pesquisa que prepara um briefing sobre uma empresa, o SDK permite que o código da aplicação solicite material de fontes para o próprio raciocínio, ou crie uma tarefa de pesquisa mais longa e recupere o resultado concluído. Mantenha o ID da tarefa, o status e os links das fontes junto com o trabalho, para que o usuário possa inspecionar as evidências e reabrir o resultado.

As ressalvas do artigo: mantenha as chaves de API no servidor, limite o uso e trate tarefas incompletas ou com falha. A licença do SDK não torna os serviços hospedados da Valyu gratuitos nem auto-hospedáveis.

### 13. Global Hack Map: uma interface de monitoramento de segurança
Global Hack Map apresenta vulnerabilidades, exploits, vazamentos e outros desdobramentos de segurança em uma interface baseada em mapa. Seu painel separa novas vulnerabilidades críticas de relatos de exploração, e distingue incidentes confirmados, relatados e alegados.

O enquadramento do artigo é a triagem. Uma equipe de segurança que opera infraestrutura de agentes deve revisar desdobramentos que afetam os produtos de sua pilha, acompanhar o aviso ou relatório vinculado e decidir se o item justifica investigação adicional. O artigo diz que a interface mostra como um produto de monitoramento pode organizar a pesquisa em torno de uma pergunta operacional, em vez de devolver um resumo de notícias indiferenciado.

A ressalva do artigo: verifique as versões afetadas e as alegações de exploração contra as fontes originais. Um evento relatado não estabelece que o seu ambiente é vulnerável, e um marcador geográfico não prova atribuição.

### 14. Global Threat Map: contexto geográfico para pesquisa
Global Threat Map é uma aplicação de consciência situacional que combina um mapa, um feed de eventos, pesquisas por país e dossiês de entidades. O artigo diz que o repositório documenta pesquisas com fontes vinculadas e artefatos de relatório exportáveis.

O exemplo do artigo: um analista de risco regional usa o feed de eventos para identificar um desdobramento, inspeciona suas fontes e solicita um dossiê sobre uma organização ou um país antes de preparar um briefing. O artigo apresenta a implementação como uma demonstração de como resultados de busca, localizações e pesquisas de longa duração podem aparecer em uma mesma interface, com saídas que um colega pode revisar fora da aplicação.

O artigo enquadra o repositório como uma referência de aplicação, não como evidência de que outros agentes de produção dependam dele.

As ressalvas do artigo: revise a geocodificação, a atualidade das fontes e as integrações de API em relação aos contratos de serviço atuais. Ele observa que o modo auto-hospedado documentado não tem autenticação e cobra as chamadas da chave do operador, de modo que são necessários controles de acesso e limites de uso antes de uma implantação compartilhada. A renderização do mapa também exige um token do Mapbox.

### 15. Polyseer: pesquisa estruturada para mercados de previsão
Polyseer aceita uma URL do Polymarket ou da Kalshi, recupera a pergunta do mercado, os preços, a data de fechamento e as regras de resolução, e envia uma tarefa de pesquisa ao Valyu DeepResearch.

O artigo descreve sua implementação do lado do servidor como uma separação entre a recuperação de dados do mercado e a criação da pesquisa e o acompanhamento de status. A saída documentada inclui uma previsão estruturada, evidências a favor e contra ela e artefatos de relatório para download. Um pesquisador pode comparar a análise com os termos de resolução reais do mercado antes de decidir quais premissas merecem verificação adicional.

O artigo apresenta isso como um padrão de aplicação útil: fatos externos definem uma pergunta de pesquisa delimitada, uma tarefa assíncrona realiza a investigação e um resultado estruturado permanece disponível sob um ID de tarefa estável.

A ressalva do artigo: avalie a qualidade das previsões em mercados já resolvidos, porque uma probabilidade gerada não é evidência de calibração. O modo auto-hospedado documentado não tem autenticação e usa a chave de API da Valyu do operador, então revise o acesso e os controles de gastos antes de expô-lo a outras pessoas.

## Perguntas frequentes

### Qual framework de agentes open source devo começar a usar?
O artigo sugere avaliar o Pydantic AI para código de aplicação Python tipado, e o LangGraph quando o fluxo de controle explícito e com estado é central. Ele aconselha executar uma tarefa representativa e testar seus caminhos de falha antes de se comprometer com um framework. Acrescenta que um SDK de pesquisa ou uma aplicação de referência atende a uma camada diferente e não necessariamente exige adotar outro framework de agentes.

### Adoção em produção significa que um projeto é seguro para implantar como está?
Não, segundo o artigo. A adoção estabelece um uso ou uma dependência relatada, não uma garantia de segurança. Uma implantação ainda precisa de permissões adequadas, isolamento, limites de recursos e testes operacionais. Serviços gerenciados e componentes auto-hospedados também implicam responsabilidades diferentes.

### Esses projetos são gratuitos para operar?
Licenças open source não cobrem custos operacionais como inferência de modelos, hospedagem, armazenamento ou serviços de navegador e sandbox. O artigo observa que as aplicações baseadas na Valyu também dependem de APIs hospedadas. Verifique as declarações de licença e a disponibilidade do código-fonte do projeto exato que você planeja usar; o artigo registra a limitação de acesso ao código do Global Hack Map na respectiva entrada.

## 🧠 Modelos mentais

### Escolha pela tarefa, não pela marca
**O modelo:** cada componente de um sistema de agentes tem uma responsabilidade distinta (executar código, manter o trabalho vivo, validar dados, operar o sistema), e a ferramenta certa decorre de qual responsabilidade está faltando.
**Como aplicar:** antes de avaliar um projeto, escreva a tarefa que ele precisa cumprir no seu sistema e a parte da aplicação que precisa de apoio.

### Evidências têm graus
**O modelo:** o artigo separa relatos nomeados de implantação, manifestos de dependências diretas, documentação de terceiros e declarações de adoção publicadas por fornecedores. Cada uma sustenta uma afirmação diferente.
**Como aplicar:** quando um projeto é dito "usado em produção", pergunte que tipo de evidência é citado e afirme apenas o que essa evidência sustenta.

### Valide em camadas
**O modelo:** validação estrutural (o valor tem a forma certa), validação de negócio (o valor está correto para esta situação) e autorização (este chamador pode fazer isto) são verificações separadas, e passar em uma não diz nada sobre as outras.
**Como aplicar:** coloque uma verificação para cada camada em cada fronteira onde os dados entram no código comum ou disparam uma ação.

### Transforme falhas em testes
**O modelo:** uma falha de produção, depois de rastreada, torna-se um caso armazenado que a próxima versão precisa passar.
**Como aplicar:** mantenha requisições válidas representativas, afirmações sem suporte e ações proibidas como uma suíte, e execute-a sempre que um prompt, um modelo ou uma ferramenta mudar.

## 🔑 Principais conclusões
- A pergunta inicial certa é o que um agente específico precisa fazer para cumprir sua função de forma confiável; o artigo diz que a resposta pode variar bastante entre empresas.
- A infraestrutura de agentes se divide em tarefas distintas: orquestração com estado, trabalho durável em segundo plano, contratos de dados, conectividade com ferramentas, execução isolada, observabilidade e avaliação, e acesso compartilhado a modelos.
- Fluxos de trabalho duráveis servem para trabalho que cruza fronteiras de serviços, aguarda eventos ou aprovações, ou precisa sobreviver a falhas de workers; e efeitos colaterais externos ainda exigem idempotência.
- Pydantic, o SDK Python do MCP, Playwright e Browser Use são cada um citados com dependências concretas por terceiros, e o artigo observa que nenhum deles mostra que todo agente os utiliza.
- Observabilidade e avaliação têm papéis diferentes: traces localizam uma falha, e avaliações verificam que uma correção se sustenta nos casos preservados; uma suíte aprovada não é garantia de segurança.
- APIs hospedadas, serviços auto-hospedados e licenças open source trazem responsabilidades, custos e necessidades de controle de acesso separados.

## 🏛️ Para o Kenji
Para um arquiteto, o movimento útil é a separação de responsabilidades feita pelo artigo: a mesma revisão que pergunta se um agente precisa de fluxos de trabalho duráveis, execução isolada ou um gateway de modelos também pergunta onde devem viver a autorização e a idempotência de cada fronteira. A pergunta que isso levanta para o seu trabalho é qual camada de um sistema de agentes você esperaria que falhasse primeiro em um incidente de produção, e se seus traces mostrariam essa falha. *Esta é a sua interpretação, não a do autor.*

## 📚 Plano de leitura
### A partir do artigo
- O texto extraído do artigo não cita hiperlinks, então nenhum link é listado aqui. Leia o artigo para conhecer as contas de implantação nomeadas (LinkedIn, Uber, AppFolio, Sanity, MindsDB, Canva, Genspark, Hermes Agent, NVIDIA) como fontes primárias de cada afirmação.

### Para ir além
- Documentação do LangGraph: para ver como o checkpointing e as interrupções são configurados em um agente com estado.
- Modelo de fluxo de trabalho e atividade do Temporal: para entender determinismo, novas tentativas e versionamento de fluxos antes de executar trabalhos duráveis.
- Especificação do Model Context Protocol e SDK Python: para implementar autenticação e autorização em torno das ferramentas que você expõe.
- Orientação da OWASP sobre injeção de prompt: para projetar defesas para agentes de navegador e de leitura de documentos.
- Prática de observabilidade para aplicações de LLM (traces, versões de prompts, avaliação online e offline): para planejar a redação de dados sensíveis, a retenção e a instrumentação da lógica da aplicação.

## Conceitos
- [ai-agent](/pt-br/concepts/ai-ml/ai-agent.md)
- [agent-harness](/pt-br/concepts/ai-ml/agent-harness.md)
- [model-context-protocol](/pt-br/concepts/ai-ml/model-context-protocol.md)
- [ai-gateway](/pt-br/concepts/ai-ml/ai-gateway.md)
- [mlops-llmops](/pt-br/concepts/ai-ml/mlops-llmops.md)
- [model-evaluation](/pt-br/concepts/ai-ml/model-evaluation.md)
- [observability](/pt-br/concepts/cloud-infra/observability.md)
- [idempotency](/pt-br/concepts/backend-systems/idempotency.md)
- [least-privilege](/pt-br/concepts/security/least-privilege.md)
- [indirect-prompt-injection](/pt-br/concepts/security/indirect-prompt-injection.md)
- [ai-security](/pt-br/concepts/security/ai-security.md)
- [software-testing](/pt-br/concepts/software-engineering/software-testing.md)
- [durable-execution](/pt-br/concepts/backend-systems/durable-execution.md)
- [sandboxed-execution](/pt-br/concepts/security/sandboxed-execution.md)
