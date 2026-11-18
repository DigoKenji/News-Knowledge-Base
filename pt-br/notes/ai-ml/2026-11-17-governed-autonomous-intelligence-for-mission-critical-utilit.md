---
type: Article Summary
title: Inteligência Autônoma Governada para Utilidades de Missão Crítica
description: Operações agênticas governadas mantêm o raciocínio da IA separado da execução, de modo que um portão de políticas e um único gateway de ferramentas controlam cada mudança em produção.
resource: https://www.infoq.com/articles/utility-governed-autonomous-intelligence-ai/
tags:
- ai-ml
- ml-governance
- rag
- multi-agent-orchestration
- concept-drift
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T07:30:00-03:00'
sources:
- resource: https://www.infoq.com/articles/utility-governed-autonomous-intelligence-ai/
  title: Governed Autonomous Intelligence for Mission-Critical Utility
  author: Purnang Borah
status: stable
source_url: https://www.infoq.com/articles/utility-governed-autonomous-intelligence-ai/
source: InfoQ
source_type: blog
published: '2026-11-16'
domain: ai-ml
subdomain: ml-governance
concepts:
- rag
- multi-agent-orchestration
- concept-drift
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41bbaf74172d81ed941fc5270812f39f
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-governed-autonomous-intelligence-for-mission-critical-utilit.md
---

# Inteligência Autônoma Governada para Utilidades de Missão Crítica

**Fonte:** https://www.infoq.com/articles/utility-governed-autonomous-intelligence-ai/
**Autor:** Purnang Borah
**Data:** 2026-11-16
**Conceitos:** [Geração aumentada por recuperação (RAG)](/pt-br/concepts/rag.md) · [Orquestração multiagente](/pt-br/concepts/multi-agent-orchestration.md) · [Deriva de conceito (concept drift)](/pt-br/concepts/concept-drift.md)

---

## 🎯 Tese central
Em uma utilidade regulada, um agente pode raciocinar sobre operações e propor mudanças em produção, mas somente um portão de políticas determinístico e um único gateway de ferramentas governado podem autorizar a execução, e a autonomia cresce uma classe de ação por vez.

## 🧭 Por que o autor escreveu
As equipes de TI e de plataforma de utilidades já têm mais dados de monitoramento do que conseguem usar, então o problema difícil é conectar sinais com rapidez suficiente para agir sobre eles. A IA agêntica pode fazer essa conexão, mas uma ação errada em produção pode afetar serviços voltados ao cliente, fluxos de trabalho relacionados à segurança ou relatórios regulatórios. Se a tese se sustentar, um erro do modelo vira uma proposta rejeitada ou revisada, em vez de uma mudança executada.

## Principais aprendizados
*(O resumo do próprio autor, reproduzido de forma próxima ao original.)*
- Em um ambiente de utilidade regulado, uma plataforma de operações agênticas mantém o raciocínio separado da execução: os agentes correlacionam sinais, avaliam o impacto no negócio e propõem remediação, mas toda ação em produção atravessa uma fronteira determinística de políticas que o modelo não consegue sobrescrever.
- Essa fronteira é definida fora do modelo. Cada ação proposta é mapeada para uma classe de risco e um requisito de aprovação em um catálogo de ações predefinido e versionado, o que mantém as decisões de aprovação previsíveis, testáveis e revisáveis antes da implantação.
- Um gateway de ferramentas governado impõe a fronteira como o único caminho entre a proposta de um agente e uma mudança em produção, com privilégio mínimo, autorização independente, verificação de aprovação e evidência completa de cada ação anexada a essa única porta.
- Como toda ação passa por um único caminho governado, a plataforma produz as evidências necessárias para atender aos requisitos regulatórios e de governança de cada sistema, sem afirmar que a própria plataforma cria conformidade.
- A autonomia é conquistada, não presumida: a plataforma começa somente leitura e expande uma classe de ação por vez, guiada pela qualidade das recomendações, pela aceitação dos operadores, pelos resultados das execuções e pela segurança de reversão, e essa autonomia pode ser revogada com a mesma facilidade com que foi concedida.

## Introdução
As equipes de operações de utilidades, ou seja, as equipes de TI e de plataforma que dão suporte a aplicações corporativas, integrações e infraestrutura, já têm mais dados de monitoramento do que conseguem consumir com conforto. Logs de aplicações, métricas de infraestrutura, telemetria de APIs, eventos de segurança, falhas de jobs, chamados de gestão de serviços e alertas de negócio descrevem, cada um, uma parte do mesmo ambiente operacional. O problema difícil não é coletar mais um sinal. É conectar esses sinais com rapidez suficiente para entender o que está acontecendo, o que pode ser afetado e o que deve acontecer em seguida.

A IA agêntica é atraente porque consegue raciocinar sobre evidências fragmentadas, recuperar conhecimento operacional, chamar ferramentas e coordenar trabalhos de múltiplas etapas. Uma utilidade não pode tratar um agente desses como um assistente conversacional. Um resumo errado é inconveniente. Uma ação errada em produção pode afetar um serviço voltado ao cliente, um fluxo de trabalho relacionado à segurança, relatórios regulatórios ou um sistema que participa da entrega confiável de energia.

### IA agêntica em operações de utilidades
Neste artigo, um agente é um modelo que pode recuperar contexto, chamar ferramentas e executar trabalho de múltiplas etapas. A preocupação central é o passo de responder para agir, porque é nesse passo que os erros ganham consequências.
> 🔎 Contexto adicionado: "Agêntico", aqui, significa que o modelo pode chamar ferramentas, não apenas produzir texto. O restante do artigo trata de controlar esse poder de chamada de ferramentas.

## Patrocinadores relacionados
Apesar do rótulo, esta seção traz a declaração de arquitetura do artigo, e não uma propaganda de produto. O autor descreve o padrão usado em uma plataforma de operações agênticas em um ambiente de utilidade regulado. A implementação de referência usa o Amazon Bedrock AgentCore, o Amazon Bedrock, recuperação sobre runbooks corporativos e histórico de incidentes, e interfaces de ferramentas do Model Context Protocol (MCP).

A seção enuncia o princípio em três frases: "The model can propose anything. It can execute only what the policy gate authorizes, through the governed tool path, with human approval wherever policy requires it. Authority never lives inside the model." (O modelo pode propor qualquer coisa. Ele só pode executar o que o portão de políticas autorizar, pelo caminho de ferramentas governado, com aprovação humana sempre que a política exigir. A autoridade nunca reside dentro do modelo.) Ela também afirma que a previsão e a remediação autônoma são habilitadas de forma incremental e apresenta a capacidade de expandir a autonomia gradualmente e revogá-la rapidamente como parte da arquitetura.

### Amazon Bedrock AgentCore
O AgentCore é a plataforma gerenciada da AWS sobre a qual a implementação de referência é construída. O autor o usa como um exemplo concreto do padrão, e não como um requisito.

### Model Context Protocol (MCP)
O MCP padroniza como as ferramentas são descritas e invocadas na fronteira de ferramentas. Uma interface padrão única dá à plataforma um lugar único para restringir operações, validar argumentos, aplicar políticas e registrar evidências.
> 🔎 Contexto adicionado: MCP é um protocolo aberto para conectar modelos a ferramentas e fontes de dados externas. O artigo o usa como a camada de descrição de ferramentas, e não como um controle de segurança por si só.

## Por que operações de utilidades precisam de um padrão agêntico diferente
Um grande parque de sistemas de uma utilidade abrange aplicações customizadas, produtos SaaS, plataformas de nuvem, camadas de integração, bancos de dados, ferramentas de observabilidade, sistemas de campo e serviços de terceiros. Cada domínio pode ser monitorado bem isoladamente, mas o serviço de ponta a ponta continua difícil de analisar. O autor lista lacunas recorrentes:

- Sinais relacionados aparecem em ferramentas separadas e são correlacionados manualmente.
- A severidade técnica é visível, mas a consequência para o negócio não é.
- Runbooks e lições aprendidas estão espalhados por wikis, históricos de chamados, drives compartilhados e pela memória de engenheiros experientes.
- Limiares (thresholds) informam uma condição conhecida depois que ela já ocorreu, enquanto sinais precursores mais fracos entre sistemas dependentes são fáceis de perder.
- As evidências que ligam uma mudança à sua justificativa, autorização, aprovador e resultado podem ser montadas somente depois do fato.

Os requisitos regulatórios também variam conforme o setor, o ambiente e a jurisdição. Por isso, a arquitetura precisa correlacionar entre domínios, raciocinar com conhecimento operacional confiável, mapear eventos técnicos para impacto no negócio, oferecer suporte à previsão sem fingir que ela é certa e manter um modelo de execução que segurança, conformidade, operadores e auditores consigam entender. A Figura 1 do original mostra a observabilidade fragmentada obrigando as equipes a correlacionar sinais manualmente.

### NERC CIP e BES Cyber Systems
A NERC é a North American Electric Reliability Corporation. Seus requisitos de Critical Infrastructure Protection (CIP) se aplicam ao Bulk Electric System (BES) e aos BES Cyber Systems definidos como no escopo. O escopo é definido por sistema, e não por empresa: outros sistemas da mesma utilidade podem ser regidos por regras diferentes.
> 🔎 Contexto adicionado: O artigo referencia o CIP-002-5.1a, onde os sistemas no escopo são definidos. Leia-o antes de decidir quais sistemas uma plataforma como esta pode tocar.

## O padrão de plano de controle: separar raciocínio de execução
A ideia organizadora é um plano de controle de IA, termo emprestado de redes e infraestrutura de nuvem, em que um plano decide o que deve acontecer e um plano separado o executa. A separação serve à segurança, à auditabilidade e à contenção do raio de impacto (blast radius). A Figura 2 do original mostra a autoridade de raciocínio separada da autoridade de execução.

No plano de raciocínio, o orquestrador e os agentes de domínio analisam eventos, recuperam contexto, comparam incidentes com o histórico, estimam o impacto no negócio, preveem modos de falha e propõem remediação. Nenhum deles consegue alterar diretamente um sistema de produção. Uma ação proposta só passa para o plano de execução por meio de um portão de políticas determinístico e de um gateway de ferramentas governado. O portão decide se a ação é permitida e qual aprovação ela exige. O gateway aplica a autorização final, obtém credenciais com escopo restrito, invoca o alvo, registra o resultado e executa ou solicita a validação pós-ação.

O ganho: uma causa-raiz alucinada em um projeto autônomo convencional pode se tornar uma ação ruim, mas aqui ela se torna uma proposta ruim. A política pode rejeitá-la, um humano pode revisá-la ou o gateway pode negar a autorização.

### Portão de políticas
O portão de políticas mapeia cada ação proposta para uma classe de risco e um requisito de aprovação. Ele é determinístico e fica fora do modelo, então a mesma proposta sempre recebe a mesma classificação.

### Gateway de ferramentas governado
O gateway é o caminho único da proposta até a mudança em produção. Ele aplica autorização, aprovações, credenciais com escopo, validação de schema e captura de evidências. O princípio do autor: "one door to production, with every control attached to that door" (uma única porta para a produção, com todos os controles anexados a essa porta).

> ❌ **Anti-padrão: gateway opcional:** Agentes alcançam sistemas-alvo por meio de chaves de API diretas, canais laterais ou ferramentas que ignoram o gateway. **Por que as pessoas fazem isso:** o acesso direto é mais rápido para protótipos. **Consequência:** os controles do gateway podem ser contornados, então não há uma única fronteira para auditar. **Correção:** tornar o gateway a única rota, sem credenciais de alvo no runtime de raciocínio e com caminhos de rede que forcem o tráfego a passar por ele.

## Implementação de referência na AWS
A implementação de referência roda no Amazon Bedrock AgentCore, que oferece runtime gerenciado de agentes, memória, gateway, identidade e integrações de observabilidade. O Amazon Bedrock fornece acesso aos modelos, e as Bedrock Knowledge Bases fornecem recuperação sobre o conhecimento corporativo.

A fronteira é coberta pelo AgentCore Policy, disponível em geral desde março de 2026. As regras são escritas em linguagem natural ou como policy-as-code, compiladas para Cedar e anexadas a um AgentCore Gateway, que avalia cada requisição agente-ferramenta antes de permiti-la ou negá-la. O autor aconselha as equipes que estão começando hoje a avaliar esse recurso antes de construir um equivalente. O AgentCore Gateway é a implementação da AWS do gateway de ferramentas. A propriedade de "lugar único" é a mais importante: o projeto não tem credenciais de sistemas-alvo no runtime de raciocínio, possui políticas de identidade com negação por padrão, caminhos de rede restritos e credenciais expostas apenas à implementação de ferramenta que precisa delas. A Figura 3 mapeia o padrão para os serviços da AWS.

### AgentCore Policy e Cedar
O Cedar é a linguagem de políticas de autorização de código aberto que a AWS usa para as regras compiladas. A aplicação acontece no gateway, e não na lógica do agente, então um erro de raciocínio não consegue convencer a plataforma a contornar a regra.

### AgentCore Gateway como gateway de ferramentas
O AgentCore Gateway é onde cada requisição agente-ferramenta é permitida ou negada. Ele funciona com interfaces de ferramentas MCP, que lhe dão uma fronteira definida por schema para verificar.

### AgentCore Evaluations
O AgentCore Evaluations oferece suporte à avaliação contínua e sob demanda do comportamento dos agentes, usada nos controles de ciclo de vida descritos mais adiante.

### Serviços equivalentes em outras plataformas
O padrão precisa de um runtime gerenciado de agentes, uma camada de recuperação e um gateway de ferramentas que aplique políticas e que os agentes não consigam contornar. No Microsoft Azure, o Microsoft Foundry Agent Service fornece o runtime, o Microsoft Entra fornece as identidades dos agentes, o Azure AI Search cuida da recuperação e o Azure API Management atua como gateway de ferramentas e gateway de IA para o tráfego de modelos e de MCP. Alguns desses recursos de governança ainda estão em preview. No Google Cloud, a Gemini Enterprise Agent Platform fornece o Agent Runtime e a Agent Identity, com o Agent Gateway como gateway de ferramentas, aplicando políticas de acesso IAM e a triagem do Model Armor contra prompt injection.

### Responsabilidades de ponta a ponta
A fronteira que importa fica entre a camada agêntica e o gateway de ferramentas. Tudo acima dela raciocina, e nada acima dela pode agir. A tabela do autor sobre o pipeline governado:

| Camada | Responsabilidade |
|---|---|
| Fonte | Alertas, logs, eventos e telemetria de aplicações, infraestrutura, nuvem, monitoramento e sistemas de segurança. |
| Integração | Ingere e normaliza sinais por meio de APIs, webhooks e streaming em um esquema comum. |
| Fila | Armazena eventos para processamento assíncrono e absorve picos sem perda. |
| Orquestrador | Recebe eventos, determina as necessidades de análise, direciona para agentes de domínio e consolida as conclusões. |
| Camada agêntica | Agentes delimitados para correlação, análise de causa-raiz, impacto no negócio, previsão e propostas de remediação. |
| Base de conhecimento | Runbooks, SOPs, documentos de arquitetura, histórico de incidentes e mapas de dependências como contexto confiável. |
| Acesso a modelos | Raciocínio, interpretação e recomendações fundamentadas no conhecimento corporativo. |
| Portão de políticas | Mapeia ações propostas para classes de risco e requisitos de aprovação fora do modelo. |
| Gateway de ferramentas (MCP; AgentCore Gateway na AWS) | Caminho único de produção governado, que aplica autorização, aprovações, credenciais com escopo, validação de schema e captura de evidências. |
| Sistemas-alvo | Aplicações, serviços de nuvem, bancos de dados, redes, ITSM e plataformas de observabilidade que recebem a remediação. |
| Aprovação humana | Revisão pelo operador ou pelo responsável, com decisão, identidade e carimbo de data/hora registrados. |
| Visão operacional | Saúde, previsões, impacto no negócio, aprovações, status da remediação e resultados validados. |

*Tabela 1 do original: responsabilidades de ponta a ponta ao longo do pipeline governado.*

## Raciocínio multiagente fundamentado em conhecimento operacional
O plano de raciocínio usa uma estrutura multiagente hierárquica, em vez de um agente grande com acesso amplo. Um agente restrito, com responsabilidade delimitada, é mais fácil de testar, restringir, avaliar e explicar. O Orchestrator Agent é o ponto de entrada para os eventos normalizados. Ele determina a análise necessária, direciona o trabalho para os agentes de domínio e consolida as conclusões, mas não executa. O Application Reliability Agent analisa falhas de aplicações com base em padrões conhecidos e no histórico. O Platform Intelligence Agent examina falhas de APIs e interfaces, latência, degradação parcial e dependências. O Business Impact Agent traduz o estado técnico em consequências para o atendimento ao cliente, a cobrança, programas relacionados à segurança, compromissos de confiabilidade ou prazos de relatórios. O Remediation Agent é o único componente de raciocínio autorizado a propor uma ação que toque a produção, e essa proposta não tem autoridade até passar pela política e pela aprovação.

### Orquestração multiagente hierárquica
O orquestrador coordena e os agentes de domínio se especializam. Cada agente é mantido restrito para que possa ser testado contra seus próprios modos de falha, e apenas um deles pode propor mudanças em produção.

> ❌ **Anti-padrão: super-agente monolítico:** Um agente com acesso a todos os domínios e a todas as ferramentas. **Por que as pessoas fazem isso:** é mais simples de montar e impressiona em demonstrações. **Consequência:** é difícil de testar, restringir ou explicar, e toda ferramenta se torna alcançável a partir de qualquer caminho de raciocínio. **Correção:** dividir as responsabilidades em agentes delimitados, roteados por um orquestrador.

### Conhecimento fundamentado por recuperação (RAG)
Todos os agentes compartilham contexto operacional recuperado de runbooks, SOPs, documentos de arquitetura, histórico de incidentes, regras de negócio, mapas de dependências e procedimentos de recuperação. O autor argumenta que uma recomendação fundamentada no procedimento documentado daquele ambiente é um artefato muito diferente de uma resposta plausível baseada no treinamento geral. A recuperação só é tão boa quanto o seu conteúdo.

### Higiene do conhecimento
Runbooks desatualizados, contraditórios ou incompletos produzem instruções desatualizadas, contraditórias ou incompletas, recuperadas fielmente. Por isso, cada runbook tem um responsável nomeado e uma data da última revisão, e a recuperação devolve ambos junto com cada trecho. Runbooks vencidos são sinalizados ao responsável, e as rejeições de operadores que citam um runbook defeituoso são encaminhadas a esse responsável.

> ❌ **Anti-padrão: runbooks sem dono:** Runbooks sem responsável nomeado, data de revisão ou histórico de versões. **Por que as pessoas fazem isso:** a documentação é tratada como um projeto paralelo. **Consequência:** o agente recupera passos desatualizados com total confiança, e ninguém é responsável por corrigi-los. **Correção:** atribuir responsáveis, registrar datas de revisão, sinalizar itens vencidos e devolver as rejeições aos responsáveis.

### Contrato de evento normalizado
Os sinais de entrada são normalizados em um schema pequeno: componente ou serviço afetado, ambiente, tipo de evento, carimbo de data/hora, origem, severidade técnica, identificadores de dependência, chaves de correlação, mapeamento para o serviço de negócio e uma referência à evidência original. O payload bruto é preservado. O parsing determinístico e os adaptadores de origem, e não o LLM, estabelecem fatos básicos como qual sistema emitiu um evento. O mapeamento para o serviço de negócio vem do catálogo de serviços. O exemplo simplificado do cenário apresentado:

```json
{
  "event_id": "evt-000193",
  "timestamp": "2026-04-12T14:03:27Z",
  "source": "apm",
  "source_event_ref": "apm://alerts/88213",
  "service": "customer-payments-api",
  "environment": "prod",
  "event_type": "latency_degradation",
  "technical_severity": "warning",
  "dependency_ids": ["billing-integration", "payments-queue"],
  "correlation_keys": ["chain:bill-payment-flow"],
  "business_service": {
    "name": "Customer bill payment",
    "criticality": "high",
    "mapping_source": "service-catalog"
  }
}
```

**O que isso faz:**
- `source_event_ref` aponta de volta para o alerta original do APM, de modo que a evidência continua rastreável.
- `service`, `environment` e `event_type` são os fatos que o parsing determinístico deve estabelecer.
- `dependency_ids` e `correlation_keys` ligam este evento a sinais relacionados na mesma cadeia de dependências.
- `business_service` traz `mapping_source: "service-catalog"`, para que um auditor veja que o mapeamento veio de uma fonte controlada.

> ❌ **Anti-padrão: fatos estabelecidos pelo modelo:** Deixar o LLM decidir qual sistema emitiu uma linha de log, se dois identificadores nomeiam o mesmo serviço ou qual serviço de negócio um componente apoia. **Por que as pessoas fazem isso:** logs em texto livre são confusos, e o modelo lida bem com eles. **Consequência:** um palpite probabilístico dirige a correlação e o risco, sem nenhum registro confiável da origem. **Correção:** fazer parsing determinístico, consultar os mapeamentos no catálogo e manter o payload bruto.

### Percorrendo um evento governado
A latência de uma API está subindo, uma integração downstream faz mais retentativas e uma fila de processamento cresce. Cada sinal, isoladamente, está abaixo de um limiar que justificaria acionar alguém, mas juntos sugerem que um fluxo de trabalho voltado ao cliente pode degradar. O orquestrador identifica a cadeia de dependência compartilhada e pede ao Platform Intelligence Agent que analise a interface. O Application Reliability Agent verifica incidentes anteriores com padrão semelhante, e o Business Impact Agent mapeia a dependência para o serviço de negócio. A recuperação traz o runbook atual e os incidentes mais relevantes. A saída é uma hipótese estruturada: o modo de falha provável, as evidências, os serviços dependentes e as opções documentadas. Evidências fracas ou contraditórias são carregadas adiante, não ocultadas.

O runbook permite diagnósticos e, se uma condição for confirmada, um restart sem estado. O catálogo classifica os diagnósticos como de baixo risco, então eles rodam automaticamente, e associa o restart a uma aprovação humana. O Remediation Agent pode propor o restart, mas não pode rebaixar o seu risco porque o modelo está confiante. Quando um operador aprova, o gateway verifica a referência de aprovação e a política, obtém a identidade com escopo, invoca apenas a operação permitida e captura a resposta. Em seguida, o fluxo de trabalho verifica a latência, o comportamento das retentativas, a tendência da fila e a saúde do serviço. Se eles não se recuperarem, a remediação não é marcada como bem-sucedida apenas porque o comando de restart foi concluído.

## Previsão sem conceder autoridade de previsão
A previsão leva a plataforma além de uma sumarização mais rápida. Ela procura combinações de sinais fracos que historicamente precedem falhas, como latência crescente, crescimento de filas, retentativas, saturação de recursos, avisos repetidos ou anomalias que só importam quando correlacionadas ao longo de uma cadeia de dependências. Ela pode usar incidentes históricos, tendências de séries temporais, dados de dependências e análise semântica de logs para afirmar o que pode falhar, em qual horizonte, por qual mecanismo, com qual confiança e com qual consequência para o negócio. Falsos positivos são esperados e devem ser medidos e reduzidos ao longo do tempo.

### Detecção preditiva
As previsões entram no mesmo caminho governado dos incidentes observados, carregando evidências, confiança e risco ponderado pelo negócio, para que os operadores possam decidir como responder.

> ❌ **Anti-padrão: vender previsão em excesso:** Apresentar previsões como resultados quase certos. **Por que as pessoas fazem isso:** previsões confiantes impressionam os stakeholders. **Consequência:** os operadores aprendem que os alertas não são confiáveis e param de agir sobre eles. **Correção:** reportar evidências, confiança, antecedência (lead time) e taxas de falsos positivos. O autor considera essa uma das formas mais rápidas de perder a confiança dos operadores.

### Confiança não é autorização
Uma pontuação de confiança não é um token de autorização. A previsão não traz permissão para agir: é a mesma separação entre os dois planos, aplicada a um único número.

> ❌ **Anti-padrão: confiança como permissão:** Deixar que uma previsão de alta confiança dispare a execução. **Por que as pessoas fazem isso:** se o modelo costuma acertar, pular a revisão parece eficiente. **Consequência:** o raro erro confiante chega à produção sem verificação. **Correção:** manter as previsões no caminho governado, com a execução controlada somente por políticas e aprovações.

## Remediação governada: risco determinístico e aprovação humana
O controle humano não deve ser apenas uma frase em um documento de governança. Ele deve estar diretamente no caminho de execução. A plataforma mapeia cada ação candidata para uma classe de risco predefinida fora do modelo, guiada por um catálogo de ações aprovado que pondera a operação, o ambiente-alvo, o raio de impacto, a reversibilidade, o impacto nos dados e a sensibilidade das dependências. A política é fixa, versionada, testável e revisável.

| Classe de risco | Exemplos | Governança |
|---|---|---|
| Baixo | Diagnósticos somente leitura, verificações de saúde, telemetria adicional | Pode executar automaticamente; registro completo |
| Médio | Mudanças reversíveis, como restarts aprovados de serviços sem estado ou escalonamento dentro de limites | Aprovação conforme a política |
| Alto | Mudanças de configuração em produção, failover, operações com dados sensíveis ou mudanças de amplo impacto | Aprovação explícita; controle dual quando a política exigir |

O autor chama isso de intencionalmente conservador. Mesmo algumas ações de baixo risco podem continuar exigindo apenas aprovação no início da implantação. A Figura 5 do original mostra a política mapeando cada ação para uma classe de risco antes que o gateway possa executá-la.

### Classificação determinística de risco
Os fatos que determinam o risco vêm de configuração controlada, inventário ou contexto verificado. O agente pode fornecer parâmetros como o nome de um serviço ou a capacidade solicitada, mas a política decide se eles estão dentro do envelope autorizado. Cada operação permitida tem um registro de política que pode ser testado sem um LLM: identificador da ferramenta, ambiente-alvo, escopo permitido, classe de risco, tipo de aprovação, timeout de execução, pré-condições, pós-condições esperadas e um link para o rollback, quando a reversão fizer sentido.

> ❌ **Anti-padrão: risco autorrotulado:** Pedir ao modelo que classifique a própria proposta como baixo, médio ou alto. **Por que as pessoas fazem isso:** o modelo tem o contexto, então perguntar parece eficiente. **Consequência:** a decisão de autoridade passa para o modelo, justamente o que o projeto busca restringir. **Correção:** derivar o risco do catálogo e de fatos verificados, e tratar qualquer rótulo do modelo apenas como consultivo.

### Aprovação humana no caminho de execução
O gateway verifica novamente as aprovações exigidas no momento da execução, em vez de confiar na afirmação de um componente anterior. Cada aprovação é registrada com identidade e carimbo de data/hora.

> ❌ **Anti-padrão: confiar em aprovações anteriores:** Aceitar uma flag de aprovação de um componente anterior sem revalidá-la. **Por que as pessoas fazem isso:** o componente anterior é interno e confiável. **Consequência:** uma aprovação desatualizada ou mal aplicada pode autorizar uma ação que ela nunca cobriu. **Correção:** verificar a referência de aprovação e a política no gateway, na execução.

### Validação pós-ação
"A API retornou 200" não é o mesmo que "o sistema está saudável". O fluxo de trabalho valida o resultado que deveria produzir e escala o caso se o estado esperado não for restaurado.

> ❌ **Anti-padrão: sucesso pelo código de saída:** Marcar a remediação como bem-sucedida porque o comando foi concluído. **Por que as pessoas fazem isso:** códigos de saída são imediatos e fáceis de ler. **Consequência:** a latência ou a profundidade da fila continuam degradadas enquanto o incidente é fechado. **Correção:** verificar os sinais que motivaram a ação e escalar se eles não se recuperarem.

### Defesa em profundidade e prompt injection
O gateway aplica credenciais de privilégio mínimo a ferramentas específicas, autoriza cada par agente-ação de forma independente, valida os argumentos contra schemas e registra identidade, decisão de política, referência de aprovação, entradas, resultado e status de saúde de cada chamada. Ferramentas tipadas restringem o que um agente manipulado consegue solicitar, mas não eliminam o prompt injection, porque um agente ainda pode usar indevidamente uma operação dentro do seu escopo. O controle é em camadas: tratamento de dados não confiáveis, ferramentas restritas, validação de argumentos, privilégio mínimo, políticas, aprovações, restrições de rede e validação pós-ação.

> ❌ **Anti-padrão: ferramentas tipadas como correção para injeção:** Tratar uma interface definida por schema como proteção suficiente contra prompt injection. **Por que as pessoas fazem isso:** interfaces tipadas parecem uma fronteira completa. **Consequência:** instruções injetadas podem conduzir uma operação legítima com argumentos prejudiciais ou no momento errado. **Correção:** empilhar as camadas, de modo que nenhum controle isolado carregue toda a carga.

### Inspeção de conteúdo
Desde junho de 2026, o AgentCore Policy pode avaliar os Bedrock Guardrails sobre as entradas e saídas do gateway, em busca de prompt injection e exposição de dados sensíveis, inicialmente em um subconjunto de Regiões da AWS. O autor trata isso como um controle na porta, e não como solução para a injeção.

## Segurança, auditabilidade e conformidade
Em operações reguladas, a governança é a razão pela qual a arquitetura tem esse formato.

### Disciplina de mudanças para agentes
Mudanças em modelos e agentes seguem a disciplina de software de produção: prompts e definições versionados em controle de versão, mudanças revisadas, avaliação antes da promoção e rollback. Incidentes históricos e casos adversariais servem como testes de regressão. Traces de produção são amostrados e avaliados quanto à conclusão da tarefa, à seleção de ferramentas, à fundamentação (groundedness), à segurança e à aderência às políticas. A confiança e a qualidade das recomendações são monitoradas quanto a deriva após a implantação.

### Minimização de dados e orientações da OWASP
Logs e chamados podem conter dados de clientes, de funcionários ou sensíveis do ponto de vista de segurança que são irrelevantes para a tarefa. As equipes devem estabelecer classificação de dados, isolamento de domínios, criptografia e regras de retenção antes de uma ingestão ampla. O autor aponta o OWASP Top 10 for LLM Applications, que trata prompt injection e excesso de agência (excessive agency) como riscos distintos, e cujas orientações sobre agentes acrescentam sequestro de objetivo (goal hijacking), uso indevido de ferramentas, abuso de identidade e privilégios e interações multiagente.

### Evidências de auditoria
As evidências de auditoria devem responder a mais do que "o que o sistema fez?". Para cada ação relevante, a organização deve reconstruir o sinal de origem, as evidências recuperadas, a recomendação, a política aplicável, a decisão humana, a operação executada, a identidade usada e o resultado validado. A imutabilidade depende do controle de armazenamento; no mínimo, o registro deve ser durável, com acesso controlado e à prova de adulteração (tamper-evident).

### Mapeamento de evidências para os requisitos do CIP
Para os BES Cyber Systems no escopo, o CIP-010-4 trata da gestão de mudanças de configuração e das avaliações de vulnerabilidade, o CIP-004-7 cobre a gestão de pessoal e de acessos, o CIP-011-3 trata da proteção das informações de BES Cyber System e o CIP-008-6 cobre a notificação de incidentes e o planejamento de resposta. A plataforma pode oferecer suporte às evidências que esses requisitos exigem, mas não cria conformidade. Programas voltados ao cliente podem enfrentar outras obrigações jurisdicionais, e aplicações corporativas fora do escopo do CIP podem estar sujeitas a controles internos de mudanças, privacidade ou financeiros.

> ❌ **Anti-padrão: rótulo de conformidade genérico:** Descrever todo o parque de sistemas da utilidade como "em conformidade com o CIP" porque uma plataforma está em uso. **Por que as pessoas fazem isso:** um único rótulo é fácil de comunicar. **Consequência:** sistemas fora do escopo herdam controles que não se encaixam no seu regime, e sistemas no escopo podem passar despercebidos. **Correção:** mapear as evidências para o regime que governa cada sistema específico.

### NIST AI RMF e ISO/IEC 42001
O NIST AI Risk Management Framework (Govern, Map, Measure, Manage) e a ISO/IEC 42001:2023 podem organizar o programa mais amplo de governança de IA. Eles complementam, em vez de substituir, os requisitos específicos de cada setor.

## Medir o valor e conquistar autonomia
Meça o valor da IA agêntica em relação a linhas de base operacionais, e não a percentuais de melhoria sem comprovação. O autor acompanha o tempo de detecção, o tempo de resposta até a resolução validada, a precisão e a antecedência das previsões, o esforço de engenharia em trabalho manual e medidas de governança como latência de aprovação, negações de política, ações não autorizadas bloqueadas, completude das evidências e tempo de reconstrução para auditoria.

### Autonomia em fases
O autor expandiu a autonomia uma classe de ação por vez, e nenhuma classe avançou apenas com base na confiança do modelo. Cada etapa exigiu o acordo do dono do sistema e dos revisores de gestão de mudanças.

| Fase | Classe de ação | Modo de execução | Evidência necessária para avançar |
|---|---|---|---|
| 1 | Todas as ações | Somente leitura: observar, correlacionar, recuperar, prever e recomendar | Os operadores aceitaram as recomendações de forma consistente, e os motivos de rejeição foram revisados e tratados |
| 2 | Diagnósticos somente leitura e verificações de saúde | Somente com aprovação → automático | Nenhum diagnóstico rodou fora do escopo autorizado; os registros de evidência estavam completos |
| 3 | Mudanças reversíveis, como restarts de serviços sem estado | Introduzidas somente com aprovação | A validação pós-ação confirmou a recuperação; os procedimentos de rollback foram testados |
| 4 | Mudanças reversíveis selecionadas, de baixo impacto, como reiniciar um serviço sem estado aprovado ou escalar dentro de limites predefinidos | Somente com aprovação → automático, dentro de escopo e janelas de tempo definidos | Aprovadas de forma consistente, sem modificação; a validação pós-ação confirmou a recuperação; o rollback teve sucesso quando testado; nenhuma negação de política ou solicitação fora do escopo |
| 5 | Mudanças de alto risco, como mudanças de configuração e failover | Sempre com aprovação explícita | Não elegível para execução automática |

*Tabela 3 do original: progressão da autonomia por fases e requisitos de evidência.*

> ❌ **Anti-padrão: autonomia baseada apenas na confiança do modelo:** Conceder execução automática porque o modelo teve bom desempenho em testes ou em conversas. **Por que as pessoas fazem isso:** resultados iniciais fortes criam pressão para avançar mais rápido. **Consequência:** a autonomia ultrapassa as evidências de que operadores, responsáveis e auditores precisam. **Correção:** exigir a evidência de cada fase e manter as mudanças de alto risco sob aprovação explícita.

### Percentuais de melhoria sem comprovação
O artigo não traz números de desempenho, e o ponto do autor é que as equipes devem definir linhas de base antes de reivindicar ganhos.

> ❌ **Anti-padrão: percentuais de melhoria sem comprovação:** Citar ganhos sem uma linha de base medida. **Por que as pessoas fazem isso:** números de destaque convencem no planejamento e nos relatórios. **Consequência:** a afirmação não pode ser verificada, e a confiança cai quando ela não se sustenta. **Correção:** medir em relação a uma linha de base capturada antes da mudança.

## Conclusão: lições para equipes que adotam este padrão
A IA agêntica consegue correlacionar sinais, avaliar impacto e recomendar remediação, mas em ambientes de missão crítica seu valor depende de quão bem a sua autoridade é governada. As quatro recomendações do autor:

- Estabeleça a governança primeiro: construa o caminho de execução governado, aplique privilégio mínimo e defina o catálogo de ações antes de introduzir agentes.
- Aplique políticas determinísticas: gerencie a classificação de risco e os requisitos de aprovação por meio de configuração versionada, e não por prompts de modelo.
- Valide os resultados: confirme a recuperação do serviço em vez de confiar na execução bem-sucedida de um comando.
- Conquiste a autonomia gradualmente: habilite uma classe de ação por vez, com a capacidade de revogar a autorização quando necessário.

O objetivo, nas palavras do autor, "não é a autonomia máxima, mas a autonomia baseada em evidências que pode ser governada com segurança e revogada".

## Sobre o autor
Purnang Borah escreveu o artigo, e Rafał Gancarz consta como revisor. A página extraída não tem texto biográfico além do nome.

## 🧩 Modelos mentais
### Plano de controle versus plano de execução
**O modelo:** decidir e fazer são sistemas separados, como em redes, onde o plano de controle decide e outro plano executa a decisão.
**Como aplicar:** pergunte, em qualquer projeto de agentes, qual componente decide e qual componente age, e se o lado que age tem controles determinísticos próprios.

### Uma porta para a produção
**O modelo:** toda mudança em produção passa por um único gateway, e todos os controles estão anexados a essa porta.
**Como aplicar:** audite a existência de outras rotas até os sistemas-alvo, incluindo credenciais, caminhos de rede e ferramentas laterais, antes de confiar em qualquer controle no gateway.

### Uma proposta ruim, não uma ação ruim
**O modelo:** um erro do modelo deve custar uma proposta rejeitada ou revisada, nunca uma mudança executada.
**Como aplicar:** projete de forma que cada saída de raciocínio passe por uma verificação independente antes de qualquer efeito em produção.

### Autonomia conquistada e revogável
**O modelo:** a autonomia é concedida uma classe de ação por vez, respaldada por evidências, e pode ser retirada com a mesma facilidade com que foi concedida.
**Como aplicar:** registre por escrito as evidências necessárias para cada fase antes de a fase começar, e defina também o caminho de revogação.

## 🔑 Principais takeaways
- A autoridade vem de um portão de políticas determinístico e de um único gateway de ferramentas governado, e não do modelo. O modelo pode propor, mas não pode executar por conta do seu próprio julgamento.
- O gateway é a única porta para a produção, e os agentes não podem ter nenhuma outra rota até os sistemas-alvo.
- As classes de risco e as aprovações ficam em um catálogo versionado que pode ser testado sem um LLM, e o modelo nunca rotula as próprias propostas.
- Sucesso significa que os sinais que motivaram a ação se recuperaram, e não que um comando retornou com sucesso.
- A previsão é uma hipótese com evidências, confiança e antecedência. A confiança nunca funciona como permissão.
- As evidências de conformidade são mapeadas sistema por sistema. A plataforma apoia as evidências, mas não torna uma utilidade em conformidade.
- A autonomia se expande uma classe de ação por vez, com evidências específicas de cada fase, e pode ser revogada com a mesma facilidade com que foi concedida.

## 🏛️ Para o Kenji
*Esta é a sua interpretação, não a do autor.* O padrão se encaixa no trabalho de governança de IA de um Staff Architect: um gateway de policy-as-code, um catálogo de ações versionado e autonomia em fases são controles que um programa de governança de IA precisa especificar e comprovar. A questão em aberto é como você mostraria que as classes de risco de um catálogo vieram de fatos verificados, e não de rótulos de agentes, e quem é o responsável por essa evidência quando um auditor perguntar.

## 📚 Plano de leitura
### Do artigo
- [Amazon Bedrock AgentCore](https://aws.amazon.com/bedrock/agentcore/) — a plataforma de referência; mapeie cada componente para a sua própria stack.
- [Model Context Protocol (MCP)](https://www.anthropic.com/news/donating-the-model-context-protocol-and-establishing-of-the-agentic-ai-foundation) — o padrão de interface de ferramentas no qual a fronteira é construída.
- [AgentCore Policy](https://aws.amazon.com/about-aws/whats-new/2026/03/policy-amazon-bedrock-agentcore-generally-available/) — a camada de políticas aplicada pelo gateway, que o autor aconselha avaliar primeiro.
- [Conceitos centrais da política Cedar](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/policy-core-concepts.html) — como as regras compiladas são estruturadas.
- [AgentCore Evaluations](https://aws.amazon.com/about-aws/whats-new/2026/03/agentcore-evaluations-generally-available/) — avaliação contínua e sob demanda do comportamento dos agentes.
- [Bedrock Guardrails](https://aws.amazon.com/bedrock/guardrails/) — a triagem de conteúdo que o autor trata como um controle entre vários outros.
- [OWASP Top 10 for LLM Applications](https://genai.owasp.org/resource/owasp-top-10-for-llm-applications-2025/) — os riscos por trás dos controles de injeção e de excesso de agência.
- [Núcleo do NIST AI RMF](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/) — a estrutura Govern, Map, Measure, Manage.
- [ISO/IEC 42001:2023](https://www.iso.org/standard/42001) — a norma de sistema de gestão de IA que o autor combina com o NIST.
- [CIP-002-5.1a](https://www.nerc.com/standards/reliability-standards/cip/cip-002-5.1a) — onde os BES Cyber Systems são definidos; leia antes de delimitar qualquer sistema.
- [CIP-010-4](https://www.nerc.com/standards/reliability-standards/cip/cip-010-4) — gestão de mudanças de configuração e avaliação de vulnerabilidades.
- [CIP-004-7](https://www.nerc.com/globalassets/standards/reliability-standards/cip/cip-004-7.pdf) — gestão de pessoal e de acessos.
- [CIP-011-3](https://prod.nerc.com/standards/reliability-standards/cip/cip-011-3) — proteção das informações de BES Cyber System.
- [CIP-008-6](https://www.nerc.com/globalassets/standards/reliability-standards/cip/cip-008-6.pdf) — notificação de incidentes e planejamento de resposta.

### Para ir além
- Orientações de segurança agêntica da OWASP — sequestro de objetivo, uso indevido de ferramentas, abuso de identidade e privilégios, interações multiagente.
- Autorização como código (policy-as-code) — escrever, versionar e testar regras de negação por padrão para ações de agentes.
- Medição de alertas preditivos — tempo de detecção, antecedência e taxa de falsos positivos em relação a uma linha de base.
- Governança de conteúdo de runbooks — responsabilidade, intervalos de revisão e versionamento de documentos que os agentes recuperam.
