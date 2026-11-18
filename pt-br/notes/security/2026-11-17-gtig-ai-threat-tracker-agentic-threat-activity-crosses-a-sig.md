---
type: Article Summary
title: 'Monitor de Ameaças de IA do GTIG: a atividade de ameaças agênticas ultrapassa um limiar significativo'
description: Agentes de ameaça passaram de prompts para LLMs a frameworks autônomos multiagente que executam intrusões em velocidade de máquina, embora troquem furtividade por velocidade.
resource: https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/gtig-ai-threat-tracker-q4-2026/
tags:
- security
- appsec
- model-context-protocol
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/gtig-ai-threat-tracker-q4-2026/
  title: 'GTIG AI Threat Tracker: Agentic Threat Activity Crosses a Significant Threshold'
  author: Google Threat Intelligence Group (Aurora Blum, Kelli Vanderlee, Jess Xia, David Mainor)
status: stable
source_url: https://blog.google/innovation-and-ai/infrastructure-and-cloud/google-cloud/gtig-ai-threat-tracker-q4-2026/
source: Google blog (forwarded)
source_type: blog
published: '2026-11-16'
domain: security
subdomain: appsec
concepts:
- model-context-protocol
selected_by: forwarded
content: handoff
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d81b4b700d5bc26230856
lang: pt-br
translation_of: /notes/security/2026-11-17-gtig-ai-threat-tracker-agentic-threat-activity-crosses-a-sig.md
---

## 🎯 Tese central
Os adversários já passaram da engenharia de prompts manual. Frameworks de LLMs autônomos e multiagente agora automatizam fases de intrusão e pipelines de exploração inteiros em velocidade de máquina e, até agora, trocam furtividade por velocidade.

## 🧭 Por que o autor escreveu
O GTIG e a Mandiant relatam o último trimestre de uso de IA generativa por adversários, dando seguimento a relatórios anteriores sobre a passagem da engenharia de prompts para a autonomia. O problema de partida é que ataques que antes exigiam um operador habilidoso em cada etapa estão se compactando em pipelines automatizados. Se a tese se sustentar, os defensores precisam se preparar para janelas de correção mais curtas, maior volume de ataques e atacantes que precisam de pouca participação humana.

## Resumo Executivo
O artigo afirma que os agentes de ameaça estão indo além da engenharia de prompts básica, em direção a frameworks multiagente autônomos, pipelines de exploração automatizados e monetização alternativa, como o LLMJacking, que ele chama de "ponto de inflexão crítico". O escopo abrange crime cibernético, espionagem e operações de informação. Os achados se baseiam na resposta a incidentes da Mandiant, no rastreamento de atores do GTIG e na telemetria de plataformas. A seção também descreve a posição defensiva do Google: salvaguardas no nível do modelo, inteligência de ameaças, contenção, a arquitetura Google AI Threat Defense (AITD) e o BeyondZero.

Ela lista cinco tendências do último trimestre.

### Mudança para fluxos de trabalho autônomos e frameworks multiagente
Adversários implantam frameworks ofensivos autônomos e multiagente (ARTEX, CAIRN, CyberStrikeAI, PentAGI) para executar intrusões de múltiplas etapas mais rapidamente. A engenharia de prompts manual está sendo substituída por sistemas que encadeiam etapas sem que uma pessoa dirija cada uma delas.

### Pesquisa automatizada de vulnerabilidades e exploração
Atacantes usam LLMs e agentes para fazer engenharia reversa de correções por meio de análise diferencial. Isso lhes permite armar vulnerabilidades n-day rapidamente e executar pipelines de exploração contra servidores expostos à internet, sem humano no circuito.

> 🔎 Contexto adicional: uma vulnerabilidade n-day já tem uma correção pública, então o atacante só precisa encontrar a diferença entre o código corrigido e o não corrigido. Análise diferencial significa comparar as duas versões para localizar essa mudança.

### Monetização e abuso de infraestrutura
Atores motivados financeiramente roubam credenciais de LLMs e recursos de nuvem de empresas comprometidas e revendem acesso a modelos de alto desempenho por meio de serviços de proxy clandestinos, como os "LLM Mixers".

### Integração de LLMs em operações de informação
Grupos patrocinados por Estados, fornecedores comerciais de Influence-as-a-Service (IaaS) e redes de monetização política inautêntica (IPM) usam rotineiramente LLMs para produzir mídia sintética em escala, criar personas localizadas e automatizar a distribuição entre plataformas.

### Espionagem direcionada e evolução da cadeia de suprimentos
Grupos de espionagem usam LLMs para pesquisa de alvos e análise de dados. Eles também descobriram novos vetores na cadeia de suprimentos, como a alteração de configurações de assistentes de programação autônomos para que uma backdoor seja executada durante a execução local.

## Frameworks Multiagente em Intrusões Cibernéticas
O GTIG e a Mandiant observaram vários atores abusando de frameworks de pentest agêntico de código aberto para automatizar fases e tarefas discretas durante intrusões reais. Relatórios anteriores tinham mostrado apenas interesse em capacidades agênticas.

A mecânica não mudou. Atacantes ainda usam credenciais comprometidas e têm como alvo sistemas sem correção. O que muda é a velocidade e a escala com que os frameworks exploram fraquezas conhecidas, e o encadeamento automatizado de caminhos de ataque é o novo problema defensivo. (As Figuras 1 e 2 são um diagrama de framework e uma linha do tempo de maturidade de LLMs. Seu conteúdo está coberto em prosa aqui.)

### Frameworks ofensivos multiagente
Essas ferramentas compartilham três capacidades. Elas descobrem e exploram vulnerabilidades de baixa complexidade, usam LLMs para gerar payloads de uso único sob demanda e usam feedback de execução em tempo real para ajustar táticas. O GTIG observou de forma independente o CAIRN e o PentAGI em uso real e viu experimentação com o CyberStrikeAI e o HexStrike AI. Também viu harnesses construídos sob medida.

### CAIRN
O CAIRN é um framework de pentest multiagente de código aberto. Ele executa um ciclo OODA (Observe, Orient, Decide, Act), no qual agentes especializados colaboram por meio de um "quadro-negro" (blackboard) compartilhado, avançando de um parâmetro inicial, como um IP-alvo, em direção a um objetivo. O GTIG viu o framework sendo usado contra alvos incluindo o setor aeroespacial dos EUA.

> 🔎 Contexto adicional: OODA é um ciclo de decisão da estratégia militar. Aqui, cada agente observa os resultados, se orienta em relação a eles, decide e age.

### CyberStrikeAI
Um framework ofensivo de código aberto nativo de LLM que funciona como um "painel único" (single pane of glass). Um backend em Go atua como servidor de comando e controle (C2) e de orquestração. Ele conecta LLMs comerciais e de pesos abertos a mais de 100 ferramentas de segurança, receitas de ataque em YAML e scripts personalizados para automatizar cadeias de ponta a ponta.

### HexStrike AI
Um harness ofensivo de código aberto que integra o Model Context Protocol (MCP). Ele permite que clientes de LLM chamem mais de 150 ferramentas ofensivas pré-instaladas, empacotadas como funções para reconhecimento, testes web, exploração e análise de binários. Mais de 12 subagentes especializados as coordenam. O GTIG relata experimentação com ele, não implantação confirmada.

### PentAGI
Um framework autônomo de código aberto criado para reduzir a barreira de entrada para operadores de nível básico. Entre o fim de agosto e o início de outubro de 2026, o GTIG rastreou um ator usando uma plataforma personalizada baseada no PentAGI de código aberto para reconhecimento sistemático e varredura de vulnerabilidades contra muitas organizações na Ásia.

### Outras ferramentas e desenvolvimento ligado a Estados
Atores motivados financeiramente e espiões suspeitos de vínculo com a RPC estão ambos experimentando esses frameworks. Um grupo ligado à RPC parece estar construindo seu próprio framework de ataque automatizado. O artigo também cita o ZeroHollow, um serviço de inferência para desenvolvimento de exploits e roteamento de LLMs, e o Agent Zero, um framework de agentes de código aberto com execução autônoma de código e geração dinâmica de ferramentas.

### Furtividade versus velocidade
O GTIG espera uso contínuo contra organizações do mundo inteiro, dadas as baixas barreiras de entrada, a fácil integração de novos exploits e a escala. Ele espera a migração para intrusões totalmente agênticas. A fraqueza atual é a furtividade. A execução autônoma sem restrições cria tráfego forense, identificadores reconhecíveis e rastros de código distintos. Isso aumenta o risco de detecção, mas as ferramentas ainda alcançam seus objetivos. À medida que as ferramentas e os LLMs subjacentes melhoram, o artigo prevê menos participação humana e mais furtividade.

**Mitigações (segundo o artigo):** O Google SecOps implantou rulepacks comportamentais e volumétricos para o ARTEX. A Mandiant Consulting lançou um guia de endurecimento e um framework de defesa arquitetural voltado à velocidade e à escala das ferramentas ofensivas habilitadas por LLMs.

## Uso de Agentes em Pesquisa e Desenvolvimento de Vulnerabilidades
O artigo afirma que agentes de LLM autônomos em pesquisa de vulnerabilidades passaram da teoria para a implantação operacional. Adversários usam sistemas multiagente e pipelines de LLM para análise diferencial de correções, fuzzing dinâmico de protocolos e armamento de prova de conceito (POC). Encadeadas, essas etapas encurtam drasticamente a janela entre a divulgação da correção e a exploração armada.

Atacantes consideram o trabalho com n-day eficiente porque o material de origem é finito: duas versões de código, incluindo canais de pré-lançamento, notas de versão, pesquisa de segurança e código de POC para uma falha. A caça a zero-days exige uma rede muito mais ampla. A barreira mais baixa também produziu uma onda de exploits gerados por LLM que dizem funcionar, mas não funcionam, o que acrescenta trabalho de verificação para os defensores.

### Estatísticas de tendência de exploração
A análise de setembro de 2026 do GTIG constatou que divulgações e exploits quase dobraram entre janeiro e agosto de 2026. Exploits de zero-day subiram apenas marginalmente, de uma média de 8 por mês em 2025 para 11 por mês em 2026, e depois dispararam para 22 em agosto. O ponto do artigo é que os LLMs mudam os tipos e os perfis de risco das falhas encontradas, não apenas a velocidade.

### Exploração totalmente automatizada sem humano no circuito
Esta é a primeira vez, segundo o artigo, que o GTIG identificou atores usando frameworks de LLM para automatizar tanto a descoberta quanto a exploração, sem humano no circuito. A evidência é um servidor de staging exposto, hospedando um framework multiagente chamado `exploit_agent`. Ele se conecta a vários LLMs de fronteira e locais e executa análise diferencial contínua de aplicações web populares. Compara o código antes e depois dos anúncios de correção, encontra potenciais vulnerabilidades, escreve exploits e os executa automaticamente. O servidor também guardava dados de vítimas exfiltrados e scripts para reconhecimento interno, ataques de força bruta, instalação de webshells e remoção de webshells de outros atacantes. A telemetria das vítimas sugeriu interesse em sites de mídia, entretenimento e educação franceses.

O artigo reproduz o código que o framework analisou: uma verificação de permissão da REST API do WordPress no plugin Spectra (Ultimate Addons for Gutenberg), versão 2.19.25.

```php
// Insecure capability check analyzed in v21925.php:
public static function restrict_popup_rest_access( $result ) {
    if ( is_wp_error( $result ) ) {
        return $result;
    }
    $route = isset( $_SERVER['REQUEST_URI'] ) ? sanitize_text_field( wp_unslash( $_SERVER['REQUEST_URI'] ) ) : '';
    if ( false === strpos( $route, '/wp/v2/spectra-popup' ) ) {
        return $result;
    }
    if ( is_user_logged_in() && current_user_can( 'manage_options' ) ) {
        return $result;
    }
    return new WP_Error(
        'rest_forbidden',
        __( 'Sorry, you are not allowed to access popups.', 'ultimate-addons-for-gutenberg' ),
        array( 'status' => rest_authorization_required_code() )
    );
}
```

_Figura 4: Framework agêntico inspecionando implementações de controle de acesso no plugin WordPress Spectra (Ultimate Addons for Gutenberg), versão 2.19.25_

**O que isso faz:**
- Executa como um filtro sobre o resultado da REST e repassa qualquer erro existente sem alterações.
- Lê a URI da requisição e retorna cedo, a menos que a rota seja `/wp/v2/spectra-popup`. Apenas essa rota é controlada.
- Permite a requisição somente se o usuário estiver logado e tiver `manage_options` (um administrador). Todo o resto recebe um erro `rest_forbidden`.
- O artigo sinaliza isso como uma verificação de capacidade insegura. É o tipo de lógica que um agente pode ler e testar automaticamente em muitas versões corrigidas.

O arquivo de ambiente do operador continha placeholders de chaves de API de LLM com comentários em russo, pedindo que as duas chaves fossem inseridas diretamente no arquivo.

### Corroboração no mundo real: PaperCut
No fim de agosto de 2026, atores de ameaça exploraram duas falhas do software de gerenciamento de impressão PaperCut como zero-days. Encadeadas, elas permitem execução remota de código sem autenticação: CVE-2026-81578, um bypass de autenticação, e CVE-2026-82078, uma falha de carregamento dinâmico de classes inseguro. Pesquisadores e a Huntress produziram exploits funcionais com LLMs em até um dia após a divulgação. A GreyNoise relatou, quatro dias após a divulgação, uma campanha provavelmente construída com LLMs, que teve como alvo centenas de organizações. Um dos atores ampliou um ponto de apoio inicial para administrador de domínio em minutos.

### Vulnerabilidades em PHP e a enxurrada de POCs falsos
CVE-2026-109618 e CVE-2026-109619 no PHP foram encontradas com análise habilitada por LLM. Antes das correções oficiais de 21 de outubro de 2026, um vídeo de POC e código corrigido inicial foram publicados. Pesquisadores e atacantes podiam comparar versões e tentar construir exploits antes que existisse uma correção. Nos dias seguintes à correção, centenas de POCs alegados, mas não funcionais, provavelmente gerados por LLM, apareceram. Os defensores tiveram de verificar ou desmentir esses POCs enquanto monitoravam a exploração real.

**Mitigações (segundo o artigo):** O Google apresentou o Big Sleep, um agente de IA do Google DeepMind e do Project Zero que busca vulnerabilidades desconhecidas. O Google Cloud combina revisão de código, bibliotecas seguras por padrão e varredura automatizada com agentes como o CodeMender. A empresa também mantém um Programa de Recompensas por Vulnerabilidades e o Project Zero.

## Integração Total de LLMs em Operações de Informação
O GTIG avalia com alta confiança que atores de OI passaram de experimentos com LLMs para integração operacional completa. Isso abrange grupos patrocinados por Estados (o artigo cita Rússia, RPC e Irã), fornecedores comerciais de IaaS e redes de IPM. Os atores incorporam LLMs na rotina de trabalho, incluindo manipulação de SEO, edição de artigos semeados e geração e publicação de conteúdo de influência. Algumas campanhas de longa duração melhoraram em qualidade, e redes de IPM agora conseguem produzir clickbait semelhante a campanhas patrocinadas por Estados em tema e volume. O autor faz a ressalva de que conteúdo melhor não significa mais impacto.

### Categorias de atores
A Tabela 2 do artigo descreve os três grupos.

| Categoria | Descrição (segundo o artigo) |
| --- | --- |
| **Grupos patrocinados por Estados** | Atores de OI diretamente ligados a aparatos estatais, por muito tempo a principal referência para defensores de plataformas. Para manter a negação plausível, usam cada vez mais empresas terceirizadas. |
| **Fornecedores comerciais de IaaS** | Agências de marketing e relações públicas, empreiteiras de inteligência privada e braços derivados de fornecedores de vigilância, vendendo desde campanhas sob medida até plataformas prontas. Empreiteiras de inteligência e derivados costumam ter OPSEC mais forte que atores estatais. |
| **Redes de monetização política inautêntica (IPM)** | Atores movidos por lucro que geram grandes volumes de clickbait político para engajamento e receita publicitária. O enquadramento político faz deles o grupo mais frequentemente confundido com operações estatais. |

O foco está mudando da geração de conteúdo, que deixou de ser o gargalo, para o uso de LLMs na arquitetura de infraestrutura, nos pipelines de distribuição e na antidetecção. O artigo vê plataformas comerciais de IaaS de ponta a ponta que automatizam todo o ciclo de vida da campanha, o que reduz o custo para os atores e torna mais difícil detectar comportamento inautêntico coordenado.

**Mitigações (segundo o artigo):** O GTIG trabalha com equipes de produto para agir de acordo com as políticas das plataformas. A aplicação incluiu a remoção de canais do YouTube, blogs, contas do AdSense e domínios do Google Notícias, reportada trimestralmente no Boletim de Operações de Influência. O GTIG também compartilha insights com o Google DeepMind para melhorar as recusas do Gemini.

## O Desenvolvimento Comercial de Operações de Influência Prontas e Habilitadas por LLMs
Em agosto de 2026, o GTIG investigou atividades que usavam a API do Gemini junto com ferramentas de vários provedores de IA para construir uma plataforma de IaaS de ponta a ponta. Ela tinha como alvo o sentimento público sobre temas políticos regionais, principalmente na Europa e no Oriente Médio. A plataforma automatizava a criação de personas, a geração de mídia multimodal, a distribuição e a publicação interativa, com prompts voltados a astroturfing e à evasão de regras de detecção. Suas táticas incluíam:
- **Personas localizadas.** Os modelos recebiam instruções para imitar dialetos e gírias regionais, com linguagem informal e erros de digitação deliberados para que a saída não parecesse automatizada.
- **Mídia crível.** Os prompts de imagem acrescentavam falhas de câmera, como granulação e iluminação irregular, e omitiam textos de fundo para evitar marcadores de imagens sintéticas.
- **Evasão de defesas de login.** O tráfego passava por proxies residenciais com cookies de login salvos, mantendo a automação autenticada e evitando CAPTCHAs e nova verificação.

Um contratado comercial separado usava a API do Gemini dentro de uma arquitetura modular e multiprovedor. Ele construía uma plataforma de IaaS pronta para gerenciar mais de 100.000 contas em grandes plataformas, combinando agentes automatizados, antidetecção e falsificação de identificadores de dispositivo e fazendas de celulares na nuvem para milhares de personas. Seus prompts mostravam planos de vender a ferramenta a órgãos governamentais nacionais e a clientes estrangeiros.
- **Telefones virtuais.** Smartphones virtuais hospedados na nuvem, cada um disfarçado de dispositivo legítimo, operavam as contas. Proxies contornavam a detecção de bots e os limites de taxa.
- **Envelhecimento de contas.** As contas eram criadas com e-mails descartáveis e números de SMS virtuais de outros países, e então envelhecidas. LLMs simulavam comportamento normal, como demorar em publicações, rolar a tela e interagir com conteúdo sem relação com a campanha, para construir um histórico antes das mensagens direcionadas.

### Adoção tática de LLMs: microsegmentação demográfica e escala de conteúdo
LLMs comerciais também apoiam o trabalho cotidiano com conteúdo: superar barreiras linguísticas, adaptar mensagens e escalar a produção. O artigo apresenta três casos:
- **Provedor de IaaS de longa duração.** Monitorado desde 2019, ele usa LLMs para elevar a qualidade e o volume de operações de influência executadas por meio de um conjunto que gerencia milhares de perfis sintéticos. Os LLMs produziam personas convincentes e comentários de estilo acadêmico sobre temas políticos, religiosos e de direitos civis, amplificados por meio de personas ocidentais fabricadas.
- **Segmentação demográfica.** Personas de influenciadores sintéticos promoviam narrativas de políticas voltadas a grupos específicos. Uma rede publicou animações no estilo Lego e paródias de futebol ridicularizando figuras políticas, provavelmente para alcançar públicos mais jovens em formatos de vídeo curto.
- **Monetização por IPM.** Operadores pedem a LLMs comerciais temas lucrativos, palavras-chave em alta e nichos de CPM alto. Eles promovem a saída por meio de apresentadores de notícias sintéticos, alguns dos quais imitam a aparência de jornalistas reais.

## Atores de Ameaça Abusando e Monetizando o Acesso a Ferramentas de IA
Credenciais comprometidas continuam sendo a base das intrusões. Atacantes agora também reutilizam credenciais roubadas e poder computacional para alimentar ferramentas de LLM não autorizadas, porque o custo de acesso, os limites de taxa de tokens e as restrições geográficas limitam o que eles conseguem rodar por conta própria. Eles colhem tokens de sessão e credenciais de plataforma de endpoints e ambientes de CI. Mercados clandestinos vendem contas verificadas de IA de fronteira, cotas de API tokenizadas e configurações de infostealers como ferramentas prontas.

### LLMJacking na resposta a incidentes
Ao longo de 2026, a Mandiant respondeu a vários incidentes de LLMJacking em que atores motivados financeiramente consumiram recursos computacionais de nuvem e cotas de LLM das vítimas. O uso e a cobrança podem disparar em horas. Cobranças fraudulentas em um único dia superaram US$ 100.000 em alguns casos, e o abuso acumulado chegou a US$ 1 milhão nos piores casos antes da remediação.

O GTIG e a Mandiant descreveram as TTPs comuns em novembro. As intrusões afetaram organizações dos setores financeiro, varejo, transporte e tecnologia na América do Norte, na América do Sul e na Ásia-Pacífico.

| Estágio do ciclo de ataque | Táticas observadas (segundo o artigo) |
| --- | --- |
| **Acesso inicial** | Roubo de credenciais e sequestro de sessão. Credenciais válidas foram colhidas de repositórios públicos do GitHub, e sites enganosos induziram usuários a entregar tokens de sessão ativos. |
| **Reconhecimento interno e movimento lateral** | Buscas em repositórios de código internos, armazenamentos de dados, serviços de nuvem, contêineres e logs de acesso por credenciais e sistemas. Em um caso, uma falha de aplicação web em um contêiner interno levou a mais sistemas. |
| **Escalonamento de privilégios e persistência** | Contas de IAM e de serviço não autorizadas foram criadas, ou políticas de administrador foram anexadas a funções de serviço. Um ator explorou o CVE-2026-40217, uma fuga de sandbox do LiteLLM que transforma chaves de inferência limitadas ("Virtual Keys") em credenciais de administrador completas. Contas de serviço privilegiadas bastaram para uma persistência sem malware. |
| **Missão completa: abuso de LLM** | Contas comprometidas foram assinadas em planos de LLM pré-pagos por uso. Um ator usou a estratégia "queimar e rotacionar", criando e excluindo contas de serviço de curta duração para evitar limites de taxa. Outros provisionaram instâncias de GPU e solicitaram aumentos de cota para rodar suas próprias ferramentas, incluindo frameworks de exploração agêntica, gateways de API e relays para revender o acesso da vítima. |

Em outro caso, dois atacantes usaram as mesmas credenciais legítimas, provavelmente vendidas ou vazadas. O primeiro enumerou serviços de LLM e verificou os custos de uso. No mesmo dia, uma botnet ou proxy com um User-Agent comum fez milhões de chamadas à API de LLM em cerca de 24 horas, e as credenciais foram revogadas antes que o primeiro atacante conseguisse consultar os modelos. Esse atacante voltou depois e criou contas de serviço com permissões de consulta a LLM.

### Monetizando o LLMJacking por meio de LLM Mixers
"LLM Mixers" são vitrines clandestinas que vendem acesso não autorizado a serviços de LLM e à infraestrutura de proxy por trás deles. Elas roteiam o tráfego dos compradores por credenciais e recursos computacionais roubados e agrupados, distribuindo o volume de consultas por muitos ambientes de vítimas para reduzir a detecção. O ecossistema espelha o cibercrime mais amplo, com especialistas em acesso, compra de credenciais de operadores de infostealers e registro em massa de testes gratuitos.

O GTIG analisou anúncios em mercados de língua russa e chinesa. A maioria oferecia vários modelos, incluindo modelos de fronteira e modelos com jailbreak anunciados para atividades cibernéticas maliciosas. Os preços por milhão de tokens variavam de US$ 0,10 a US$ 25. Credenciais de testes gratuitos eram muito mais baratas que as corporativas. A APIMart aumentou os preços em cinco vezes, tanto na entrada quanto na saída, assim que um prompt ultrapassava 100.000 tokens. A JetNight vende assinaturas mensais por tipo de modelo.

## Atores de Ameaça Continuam Usando IA por Ganhos de Produtividade
Atores seguem usando LLMs para acelerar fases de ataque, mas o uso generalizado tem desvantagens. Processos de ferramentas de LLM forensicamente ruidosos podem degradar a OPSEC, o que importa mais para a espionagem. Os modelos também alucinam detalhes e até alvos.

O autor prevê uma divisão. Atores focados em persistência de longo prazo evitarão ferramentas ruidosas dentro dos alvos, embora possam usá-las seletivamente para ganhar velocidade. Atores menos preocupados com furtividade, incluindo muitos grupos de cibercrime, continuarão adicionando ferramentas autônomas, já que o alcance maior pode compensar as taxas de sucesso menores. O artigo classifica isso com alta confiança.

### Atores apoiados por governos
CALANQUE ION, um ator apoiado pelo governo iraniano anteriormente rastreado como APT42, usa LLMs, incluindo o Gemini, para reconhecimento e engenharia social direcionada. Atores ligados à RPC usam LLMs para reconhecimento e pesquisa, e, no terceiro trimestre de 2026, os usaram para varredura de front-end, pesquisa de alvos e armamento de exploits. Outros casos:
- **Análise pós-comprometimento.** O terceirizado chinês LEAKY CASTLE usou LLMs para analisar, traduzir e interpretar dados exfiltrados, e incorporou os resultados em relatórios finalizados vendidos a clientes.
- **Plataformas de LLM como infraestrutura.** Atores ligados à RPC abusam de plataformas de LLM e de back ends de hospedagem de arquivos para staging, C2 do tipo dead-drop e exfiltração. Um ator armazenou arquivos de isca em um serviço de hospedagem de arquivos de um modelo de fronteira e exfiltrou documentos da intranet por meio de uma API de backend.

Atores norte-coreanos (DPRK) usam ferramentas de LLM em esquemas de trabalhadores de TI, como gerar currículos e respostas de entrevista que soem autênticos. Eles também usam LLMs para iscas e código. Um suposto cluster MIDNIGHT NEPTUNE combinou engenharia social habilitada por LLM com o abuso das ferramentas de IA de uma organização como vetor de cadeia de suprimentos. Em intrusões contra organizações de criptomoedas na América do Norte e na Europa, o ator:
- usou perfis falsos de recrutadores cujas imagens tinham marcas d'água SynthID, o que, segundo o artigo, confirma o uso de LLM para sintetizar identidades.
- enviou os alvos a plataformas falsas de reunião com iscas "ClickFix" que executavam scripts de staging provavelmente escritos por LLM.
- alterou `.claude/settings.json` em repositórios internos para que, quando os usuários iniciassem o Claude Code, ele baixasse a backdoor SOMBERMEME.
- gerou Personal Access Tokens do GitHub com nomes de Microsoft Copilot para ocultar o movimento lateral.

O resumo do artigo: os atores estão tanto escalando operações com ferramentas generativas quanto explorando hooks nativos de execução de agentes de IA como vetores de acesso corporativo.

> ❌ **Anti-padrão — confiar na configuração de agentes de IA no nível do repositório:** A configuração de agentes, como `.claude/settings.json`, é versionada em repositórios compartilhados e roda automaticamente quando um desenvolvedor inicia o agente. **Por que as pessoas fazem isso:** 🔎 Contexto adicional: a configuração compartilhada da equipe é conveniente, e os revisores podem tratá-la como configurações inofensivas, e não como código. **Consequência:** Uma mudança no repositório transformou a inicialização do agente em um download de malware. **Correção:** 🔎 Contexto adicional: revise a configuração do agente e os hooks de inicialização como qualquer código executável, e restrinja quem pode alterá-los. O artigo descreve o ataque, mas não lista correções.

### Kits de LLM oferecem soluções prontas de cibercrime
Operadores de Phishing-as-a-Service (PhaaS) usam LLMs para construir ferramentas para afiliados de baixo conhecimento técnico. O serviço de phishing em chinês Jieyou, visto pela primeira vez em março de 2026 e focado em fraude de e-commerce, adicionou um chatbot de LLM na atualização de back end de junho de 2026. O chatbot é independente de modelo, conecta-se a várias plataformas de fronteira e de código aberto por meio de APIs, e permite que os afiliados configurem painéis de phishing e editem templates com comandos em linguagem natural. Os afiliados podem ajustar iscas e páginas em tempo real, em vez de editar payloads manualmente.

**Mitigações (segundo o artigo):** Essas atividades acionaram as respostas de segurança do Gemini. O Google tomou medidas mais amplas para desarticular as campanhas, desativou os ativos associados e usou os insights para fortalecer classificadores e modelos, de modo que recusem esses ataques.

## Como o Google Protege Contra o Abuso de IA
A defesa do Google combina salvaguardas no nível do modelo, inteligência de ameaças, protocolos de contenção e red teaming. A empresa introduziu o Secure AI Framework (SAIF) e argumenta que o setor precisa de padrões de segurança para a implantação responsável de IA.

### Defesas proativas de modelo e plataforma
O Google alimenta classificadores de segurança e guardrails com o monitoramento ativo de ameaças. Contra ataques de extração de modelo, também chamados de "destilação", implantou defesas em tempo real que degradam modelos "estudantes" não autorizados e detectam tentativas de clonar lógica proprietária. A empresa desativa projetos e contas de atores maliciosos.

Para empresas, o Google construiu o Google AI Threat Defense (AITD), que combina Gemini e outros modelos de fronteira, a priorização de riscos do Wiz, remediação automatizada com Gemini e CodeMender, e a inteligência da Mandiant. Ele usa modelos leves para varredura contínua e modelos de fronteira especializados para vulnerabilidades de alto risco.

O Gemini 4 Carbon, descrito como o modelo mais forte do Google para cibersegurança e tarefas de longo horizonte, tem salvaguardas em camadas: treinamento de segurança, classificadores de entrada e saída, sondas de introspecção baseadas em ativação e monitores de cadeia de pensamento e de trajetórias agênticas. Esses mecanismos visam bloquear pedidos de exploits funcionais ou armados e de malware. Equipes internas de red team e parceiros, incluindo Gray Swan e Trajectory Labs, testaram sob estresse. Defensores, pesquisadores e parceiros governamentais vetados recebem acesso a capacidades de uso duplo pelo Fairwind Program.

## Construindo IA de Forma Segura e Responsável
O Google afirma que seu desenvolvimento de IA segue seus Princípios de IA, com guardrails testados continuamente. Suas diretrizes de política e suas políticas de uso proibido sustentam sua abordagem de segurança. O artigo considera a inteligência de ameaças parte central da postura do Google. A empresa investiga abusos de suas plataformas, trabalha com a aplicação da lei quando apropriado e incorpora as lições de cada contramedida ao desenvolvimento de produtos e às salvaguardas dos modelos.

## 🧠 Modelos mentais
### Velocidade de máquina como a nova referência
**O modelo:** Os frameworks encadeiam reconhecimento, exploração e etapas pós-comprometimento com pouca demora, de modo que a distância entre a divulgação e o ataque se reduz a horas ou minutos.
**Como aplicar:** Meça seu próprio tempo entre a publicação da correção e a implantação, e presuma que o relógio do atacante começa na divulgação.

### Furtividade versus velocidade
**O modelo:** Ferramentas autônomas compram velocidade e escala ao custo de rastros forenses. Atores focados em furtividade as evitam, e atores focados em volume as aceitam.
**Como aplicar:** Espere que a detecção seja mais fácil na automação ruidosa, e espere que o equilíbrio mude à medida que os modelos melhorarem.

### Especialização na cadeia de suprimentos do crime
**O modelo:** A economia criminosa de LLMs segue o mercado mais amplo do cibercrime, com atores separados para acesso, credenciais, infraestrutura e revenda.
**Como aplicar:** Mapeie a exposição por estágio (vazamento de credencial, token de sessão, abuso de cota, revenda), e não como uma ameaça única.

## 🔑 Principais conclusões
- Frameworks multiagente autônomos como CAIRN, CyberStrikeAI, HexStrike AI e PentAGI estão em uso real, não apenas em experimentos.
- O GTIG observou o primeiro caso de um framework automatizando tanto a descoberta quanto a exploração de vulnerabilidades, sem humano no circuito.
- Divulgações e exploits quase dobraram de janeiro a agosto de 2026. Exploits de zero-day subiram de uma média de 8 para 11 por mês, e depois chegaram a 22 em agosto.
- Cobranças de LLMJacking já ultrapassaram US$ 100.000 em um único dia, com abuso acumulado de US$ 1 milhão.
- Operações de influência agora usam plataformas de LLM de ponta a ponta que gerenciam mais de 100.000 contas, as envelhecem e as roteiam por fazendas de celulares na nuvem e proxies residenciais.
- O abuso da configuração de agentes de IA, como `.claude/settings.json`, agora é um vetor de ataque à cadeia de suprimentos.

## 🏛️ Para o Kenji
Este é um modelo de ameaças para sistemas que rodam agentes de LLM ou guardam credenciais de LLM. O artigo mostra chaves de API, contas de serviço e tokens de CI sendo roubados, e configurações de agentes versionadas em repositórios. As revisões de arquitetura devem cobrir o escopo e a rotação de chaves, alarmes de cota e se os hooks de inicialização de agentes podem ser alterados em um repositório compartilhado sem revisão. 🔎 Esta é minha interpretação, não a do autor. A pergunta que ela levanta: se um atacante consegue rodar um pipeline multiagente contra o seu ciclo de correções, a sua implantação de correções termina primeiro?

## 📚 Plano de leitura
### A partir do artigo
- [ARTEX and Friends: Enter Agentic Offensive Frameworks](https://cloud.google.com/blog/topics/threat-intelligence/artex-and-friends-enter-agentic-offensive-frameworks) — análise do framework e o guia de endurecimento por trás da seção multiagente.
- [From Prompting to Autonomy: The Evolution of Adversarial AI](https://cloud.google.com/blog/topics/threat-intelligence/from-prompting-to-autonomy-the-evolution-of-adversarial-ai) — o relatório anterior no qual esta atualização se apoia.
- [The Rise of LLMJacking: How Threat Actors Monetize Compromised Cloud and Model Resources](https://cloud.google.com/blog/topics/threat-intelligence/llmjacking-monetizing-compromised-cloud-models) — TTPs completas, orientações de endurecimento e a economia dos mixers.
- [Vulnerability discovery and exploitation trends in the AI era](https://cloud.google.com/blog/topics/threat-intelligence/vulnerability-discovery-and-exploitation-trends-in-the-ai-era) — as estatísticas por trás dos números de zero-days.
- [GreyNoise: AI-orchestrated campaign against PaperCut NG/MF](https://www.greynoise.io/blog/ai-orchestrated-campaign-against-papercut-ng-mf) — cronologia e escala da campanha contra o PaperCut.
- [Huntress: PaperCut actively exploited](https://www.huntress.com/blog/papercut-actively-exploited) — a rapidez com que surgiram exploits funcionais.
- [Influence Operations Bulletin Q2 2026](https://blog.google/security/influence-operations-bulletin-q2-2026/) — dados trimestrais de aplicação sobre atividades de OI.
- [Gemini 4 Carbon Safety & Security Report](https://storage.googleapis.com/deepmind-media/gemini/gemini_4_carbon_safety_report.pdf) — os testes de segurança por trás das defesas no nível do modelo.

### Para ir além
- OWASP Top 10 for LLM Applications — uma checklist de riscos em nível de aplicação para sistemas baseados em LLM, incluindo as questões de agentes e credenciais acima.
- MITRE ATT&CK — mapeie a tabela de TTPs de LLMJacking do artigo para técnicas e detecções que sua equipe já acompanha.
