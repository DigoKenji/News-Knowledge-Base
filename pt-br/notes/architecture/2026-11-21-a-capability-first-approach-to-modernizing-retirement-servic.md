---
type: Article Summary
title: Uma abordagem centrada em capacidades para modernizar plataformas tecnológicas de serviços de aposentadoria
description: A modernização deve começar por uma avaliação de capacidades de negócio, ordenando-as por maturidade, criticidade
  e valor estratégico antes de escolher as plataformas.
resource: https://www.architectureandgovernance.com/uncategorized/a-capability-first-approach-to-modernizing-retirement-services-technology-platforms
tags:
- architecture
- business-architecture
- bounded-context
- ai-agent
- strangler-fig
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.architectureandgovernance.com/uncategorized/a-capability-first-approach-to-modernizing-retirement-services-technology-platforms
  title: A Capability-First Approach to Modernizing Retirement Services Technology Platforms
  author: Karthikeyan Marappan
status: stable
source_url: https://www.architectureandgovernance.com/uncategorized/a-capability-first-approach-to-modernizing-retirement-services-technology-platforms
source: Architecture & Governance Magazine
source_type: blog
published: '2026-11-20'
domain: architecture
subdomain: business-architecture
concepts:
- bounded-context
- ai-agent
- strangler-fig
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d81daa9ffc65a10c7e8f6
foundation_candidates:
- domain-driven design
- capability maturity model
- orchestration layer
lang: pt-br
translation_of: /notes/architecture/2026-11-21-a-capability-first-approach-to-modernizing-retirement-servic.md
---

## 🎯 Tese central
Os provedores de serviços de aposentadoria devem avaliar suas capacidades de negócio, ordenando-as por maturidade, criticidade e valor estratégico, antes de escolher as plataformas e a arquitetura que vão sustentá-las.

## 🧭 Por que o autor escreveu o artigo
Programas de replataformação costumam partir das aplicações existentes e acabam reproduzindo os mesmos limites operacionais em um novo sistema. Se a tese estiver correta, a modernização é sequenciada pela necessidade de negócio, e cada decisão tecnológica pode ser rastreada até a capacidade que ela deve melhorar.

## Abertura: contexto de negócio antes da replataformação
> 🔎 Contexto adicionado: O artigo não tem título sobre os parágrafos iniciais, então este título foi acrescentado para referência. O subtítulo (standfirst) enuncia a tese: "Uma avaliação baseada em capacidades pode ajudar organizações de serviços de aposentadoria a identificar prioridades de modernização, alinhar investimentos em tecnologia às necessidades de negócio e sequenciar as mudanças de acordo com o risco operacional e regulatório."

### Replataformação sem contexto de negócio
Discussões sobre replataformação têm mais sucesso quando, desde o início, estão alinhadas com a forma como o negócio pretende crescer, quais produtos vai suportar e como quer atender seus mercados. Sem esse contexto, a avaliação de fornecedores, o planejamento de integração e as decisões de migração se apoiam em um quadro incompleto, e a nova plataforma tende a herdar as limitações que o programa deveria resolver. O argumento do autor é que o alinhamento precisa vir antes da escolha da plataforma, e não depois dela.

> ❌ **Anti-padrão — Substituição like-for-like:** substituir uma tecnologia envelhecida e melhorar a manutenibilidade sem tratar das limitações operacionais que motivaram o investimento. **Por que as pessoas fazem isso:** o artigo descreve programas enquadrados em torno de tecnologia envelhecida, o que torna a tecnologia o ponto de partida. **Consequência:** as novas plataformas "muitas vezes têm dificuldade para atender patrocinadores de planos, servir participantes, lançar produtos ou gerenciar requisitos regulatórios com eficiência", e o negócio continua contornando as mesmas restrições. **Correção:** definir primeiro as limitações operacionais e a direção do negócio, e só então decidir o que a substituição deve fazer.

### Estratégia de clientes e de mercado como requisitos
A estratégia de clientes cria requisitos próprios. Um provedor que atende pequenas e médias empresas empregadoras precisa de um modelo de inscrição e atendimento diferente daquele de um provedor que atende grandes patrocinadores de planos por meio de equipes de relacionamento dedicadas. Clientes de educação K-12 e de saúde trazem estruturas de plano, requisitos de contribuição e relações de distribuição diferentes. Quando a direção está clara, a modernização pode ser fundamentada nas capacidades necessárias para executá-la: onde a organização tem bom desempenho, onde os sistemas existentes a limitam e quais lacunas têm importância de negócio ou regulatória suficiente para justificar investimento.

## Comece pelo que o negócio precisa ser capaz de fazer

### Capacidades de negócio
Uma capacidade de negócio descreve o que uma organização precisa ser capaz de fazer, independentemente das aplicações ou sistemas que a sustentam. A inscrição de participantes continua sendo uma capacidade central, esteja ela em uma plataforma de recordkeeping legada ou em tecnologia introduzida por um programa de modernização. O cálculo de uma distribuição mínima obrigatória funciona da mesma forma: a obrigação do negócio permanece constante mesmo quando o motor de cálculo muda. Separar a capacidade da sua tecnologia é o primeiro passo da análise.

### Capacidades que abrangem vários sistemas
A separação importa em serviços de aposentadoria porque uma mesma atividade costuma se estender por vários sistemas e funções. Um saque por dificuldade financeira (hardship withdrawal) pode envolver atendimento ao participante, regras de elegibilidade e conformidade, processamento de pagamentos e dados mantidos em várias aplicações, todos servindo a uma mesma obrigação perante o participante. Analisar cada aplicação isoladamente torna essas dependências difíceis de enxergar, enquanto tratar as funções como uma capacidade compartilhada mostra como elas trabalham juntas.

> ❌ **Anti-padrão — Revisão aplicação por aplicação:** avaliar cada aplicação de forma independente, mesmo quando várias delas servem à mesma atividade de negócio. **Por que as pessoas fazem isso:** inventários de aplicações são o ponto de partida mais fácil, e o artigo observa, mais adiante, que eles não revelam como o trabalho realmente acontece. **Consequência:** as dependências entre funções permanecem ocultas. **Correção:** agrupar as aplicações sob a capacidade de negócio compartilhada que elas servem.

### Mapeando a operação como um todo
Uma plataforma de recordkeeping pode suportar grande parte do atendimento a planos e participantes, transações, pedidos de benefício (claims), investimentos e finanças. Capacidades como apoio à pré-venda e a solicitações de propostas (request for proposal, RFP), ou a integração de novos negócios (new-business onboarding), costumam ser atendidas por sistemas de CRM, de apoio a vendas e outros. Mapear por capacidade torna essas relações visíveis e oferece uma visão mais completa da tecnologia que as entrega.

O artigo se refere à Figura 1, que mostra o panorama mais amplo de capacidades e a parte normalmente suportada pela plataforma de recordkeeping. Só a legenda sobrevive no texto extraído:

> Figura 1. Um mapa de capacidades de serviços de aposentadoria mostra como a plataforma típica de recordkeeping suporta apenas parte do panorama mais amplo de capacidades de negócio.

### Ver o panorama completo antes da substituição
Um mapa de capacidades permite que as equipes vejam todo o panorama operacional antes de avaliar plataformas de substituição, o que ajuda a evitar que o programa reproduza limitações existentes. Em uma substituição like-for-like, tecnologia mais nova pode preservar lacunas operacionais se a estrutura atual de aplicações se tornar a base para definir o que a substituição deve suportar. Entender essas lacunas no nível da capacidade dá às equipes uma base mais clara para decidir quais aplicações manter, estender ou substituir.

> 🔎 Contexto adicionado: "Like-for-like" significa substituir um sistema por outro que faz o mesmo trabalho, com a mesma forma. O artigo usa o termo para descrever uma substituição que copia a estrutura existente em vez da necessidade do negócio.

## Investir de acordo com o valor para o negócio

### Capacidades diferenciadoras e commodities
O mapeamento de capacidades é mais útil quando reflete a forma como a organização compete, porque os recursos de modernização e a capacidade de engenharia não devem ser distribuídos igualmente por todas as funções. Algumas capacidades apoiam diretamente os produtos, serviços ou a experiência de clientes que diferenciam a empresa. Outras são essenciais para a operação, mas podem ser bem atendidas por soluções padrão. A classificação varia entre provedores porque seu mix de produtos, base de clientes e estratégia de crescimento são diferentes.

O artigo traz dois exemplos contrastantes. Um provedor de annuities pode precisar investir fortemente em precificação de produtos e administração de pagamentos, porque essas capacidades moldam sua posição competitiva. Uma empresa cujo crescimento depende de patrocinadores de planos K-12 ou de saúde pode dar mais valor ao atendimento a patrocinadores e às comunicações com participantes, refletindo o que esses mercados exigem. Funções essenciais, como contabilidade geral (general ledger) e digitalização de documentos (document imaging), muitas vezes podem ser atendidas por soluções padrão.

### O investimento molda a arquitetura
Decisões de investimento tomadas no nível da capacidade moldam tanto a arquitetura quanto o roteiro de modernização. Um investimento desproporcional em uma função commodity tira recursos de capacidades que sustentam o crescimento ou diferenciam o negócio. Investimento insuficiente em uma capacidade de alto valor deixa a organização limitada pelas mesmas restrições que pretendia resolver. Direção de produto, distribuição, segmentos de clientes e prioridades de crescimento dão às equipes de tecnologia a base para decidir onde um investimento especializado se justifica e onde soluções padrão bastam.

> ❌ **Anti-padrão — Distribuição uniforme do investimento:** distribuir recursos de modernização e de engenharia igualmente por todas as funções. **Por que as pessoas fazem isso:** o artigo não diz por quê, mas esforço distribuído igualmente é o padrão quando não existe classificação de capacidades. **Consequência:** capacidades de alto valor ficam subfinanciadas, e funções commodity absorvem a capacidade disponível. **Correção:** classificar as capacidades pela forma como sustentam a posição competitiva e então financiá-las de acordo.

> ❌ **Anti-padrão — Personalizar funções commodity:** customização extensa de funções padrão, como contabilidade geral ou digitalização de documentos. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** a customização adiciona custo e consome capacidade de engenharia sem fortalecer o que diferencia o negócio. **Correção:** usar soluções padrão para funções commodity.

## Transformando a análise de capacidades em um roteiro de modernização
O autor usa um Modelo de Alinhamento Capacidade-Aplicação (Capability-Application Alignment Model), construído em torno de cinco áreas relacionadas: inventário de capacidades, maturidade, criticidade, alinhamento com aplicações e sequenciamento. O inventário precisa ter detalhe suficiente para expor como o trabalho realmente acontece e estrutura suficiente para orientar decisões de investimento.

### Modelo de Alinhamento Capacidade-Aplicação
As cinco áreas formam uma cadeia única. O inventário define as capacidades. A maturidade mostra o quão bem cada uma funciona hoje. A criticidade mostra o quanto uma fraqueza importa. O alinhamento com aplicações liga cada capacidade aos sistemas que a suportam, e o sequenciamento transforma essas conclusões em uma ordem de trabalho. O modelo é apresentado como prática do próprio autor ("Na prática, eu uso..."), portanto é o método de um arquiteto, e não um padrão publicado.

### Construindo o inventário de capacidades
Construir o inventário exige um trabalho estruturado entre equipes de negócio e de tecnologia. O autor afirma que, em sua experiência, oficinas de Domain-Driven Design são uma forma eficaz de reunir especialistas no assunto em torno do ambiente operacional como ele funciona na prática. As oficinas examinam processos de negócio, pessoas, aplicações e as dependências a montante e a jusante por trás de cada capacidade. Isso costuma revelar detalhes que os inventários de aplicações não mostram. Um processo que parece automatizado ainda pode depender de planilhas, de conciliação manual ou do conhecimento de algumas pessoas. Várias aplicações podem sustentar partes de uma mesma capacidade, e uma única aplicação pode ter responsabilidades que pertencem a vários domínios de negócio.

> ❌ **Anti-padrão — Inventário construído apenas a partir de aplicações:** tratar a lista de aplicações como uma descrição do negócio. **Por que as pessoas fazem isso:** a lista de aplicações é o artefato que já existe. **Consequência:** atalhos manuais e responsabilidades compartilhadas permanecem invisíveis. **Correção:** conduzir oficinas estruturadas que percorram como cada capacidade é de fato entregue.

### Arquitetura de referência de negócio
Uma arquitetura de referência de negócio acrescenta a visão de tecnologia, mapeando os ativos existentes para as capacidades que eles suportam. O resultado é um retrato compartilhado do estado atual que mostra às equipes de negócio e de tecnologia onde estão as lacunas.

### Maturidade de capacidades
A maturidade pode ser avaliada ao longo de uma progressão prática: manual, padronizada, automatizada e, por fim, adaptativa ou aumentada por IA. O autor coloca a precisão antes da aspiração. Um processo que depende da experiência de funcionários que sabem contornar limitações do sistema é fundamentalmente diferente de uma capacidade genuinamente automatizada, mesmo quando ambos parecem bem-sucedidos em um painel executivo.

> ❌ **Anti-padrão — Notas de maturidade aspiracionais:** registrar a maturidade que uma capacidade deveria ter, e não a maturidade que ela tem. **Por que as pessoas fazem isso:** o artigo não diz, embora contraste essas notas com o que os executivos veem nos painéis. **Consequência:** processos dependentes de contornos parecem automatizados, e a modernização é planejada a partir de uma linha de base falsa. **Correção:** atribuir a maturidade com base em como o trabalho realmente funciona.

### Criticidade
A maturidade só é útil para o planejamento quando considerada junto com a criticidade, porque uma capacidade imatura pode trazer pouco risco ao negócio, enquanto outra pode afetar diretamente a conformidade regulatória, a experiência do participante ou a receita. O autor avalia a criticidade pela exposição regulatória, pelo impacto no participante e pela sensibilidade a custos ou receita. O processamento de distribuições mostra bem essa relação. Um erro pode gerar exposição de conformidade, afetar diretamente um participante e gerar custos substanciais de remediação, enquanto uma limitação temporária em uma função de relatórios internos carrega risco operacional consideravelmente menor. Avaliar maturidade e criticidade em conjunto oferece uma base mais sólida para priorizar do que a idade da aplicação isoladamente.

### De lacunas a decisões sobre aplicações
Uma vez concluídas as avaliações, as lacunas de capacidade podem ser associadas a decisões sobre aplicações, com base no que o negócio realmente precisa melhorar. A resposta pode ser estender uma aplicação existente, introduzir uma camada de integração, substituir parte do ambiente legado ou mover a capacidade para uma nova plataforma. Em conjunto, essas decisões estabelecem a sequência de modernização.

> 🔎 Contexto adicionado: Uma camada de integração é um conjunto compartilhado de componentes que intermedeia as conexões entre sistemas, para que eles não precisem se conectar diretamente uns aos outros.

### Restrições práticas e prioridade
Vencimentos de contratos, disponibilidade de fornecedores e janelas de implementação inevitavelmente influenciarão o roteiro. O artigo afirma que eles não devem se tornar a base principal para decidir o que será modernizado primeiro. A maior prioridade vai para capacidades com exposição de negócio significativa e grandes lacunas de maturidade, onde a melhoria reduz riscos relevantes ou gera valor operacional. Isso mantém o roteiro ligado à necessidade do negócio, e não ao ciclo de vida das aplicações existentes.

> ❌ **Anti-padrão — Sequenciar pelo ciclo de vida:** ordenar a modernização por datas de contrato, disponibilidade de fornecedores, janelas de implementação ou idade das aplicações. **Por que as pessoas fazem isso:** essas pressões são reais e visíveis, e o artigo reconhece que elas vão influenciar o roteiro. **Consequência:** o programa pode modernizar sistemas convenientes e deixar intocadas capacidades de alta exposição. **Correção:** ordenar pela exposição de negócio e pela lacuna de maturidade, e tratar o prazo de contratos e fornecedores como restrições dessa ordem.

## Escolhendo o caminho de modernização depois do trabalho de capacidades
As prioridades de capacidade dão o contexto de negócio para decidir como a tecnologia de suporte deve mudar. A resposta não precisa ser a mesma em toda a organização. Algumas partes do ambiente legado podem justificar substituição incremental. Outras podem continuar operando por trás de APIs ou de uma camada de integração. Certas capacidades podem justificar migração para uma plataforma moderna ou SaaS. Cada caminho tem implicações diferentes para custo, risco de implementação, dívida técnica e o nível de controle que a organização mantém.

### Substituição incremental
O autor considera a modernização incremental particularmente útil. Substituir o ambiente legado em etapas permite que o negócio continue operando durante toda a transição. As equipes podem validar capacidades individuais à medida que elas migram para a nova arquitetura, o que reduz a disrupção de uma grande migração. Um sequenciamento cuidadoso é essencial sempre que sistemas antigos e novos precisem coexistir por um período prolongado.

> 🔎 Contexto adicionado: O artigo não menciona o padrão strangler fig, mas descreve a mesma ideia: uma substituição em etapas em que sistemas antigos e novos funcionam lado a lado.

### Envolver um núcleo legado confiável
Envolver o núcleo com APIs ou com uma camada de integração é um caminho diferente. Ele se aplica quando as restrições mais imediatas são o acesso às funcionalidades existentes e a interoperabilidade com aplicações mais novas. A organização pode expor funções existentes a aplicações mais novas e a experiências digitais sem se comprometer de imediato com uma substituição completa. A flexibilidade é real, mas a dívida técnica dentro do núcleo ainda precisa ser tratada.

### Migração completa para uma plataforma moderna ou SaaS
Uma migração completa pode fazer sentido quando grande parte do ambiente não sustenta mais as capacidades de que o negócio precisa, ou quando a organização tem uma razão forte para consolidar a tecnologia em grande escala.

> ❌ **Anti-padrão — Julgar plataformas pela abrangência:** escolher uma plataforma pelo quanto ela cobre. **Por que as pessoas fazem isso:** o artigo não diz, mas a abrangência é uma medida fácil de manchete. **Consequência:** "a abrangência da plataforma por si só ... é uma medida pobre de adequação", porque configuração, extensões, integrações e processos operacionais ainda precisam sustentar os produtos, clientes e o modelo de atendimento específicos do provedor. **Correção:** trazer a decisão de volta para a análise de capacidades.

### Soluções padrão e engenharia sob medida
O caminho certo pode variar de capacidade para capacidade. Soluções padronizadas costumam atender bem funções commodity, enquanto capacidades diferenciadoras às vezes justificam mais controle ou engenharia sob medida. O artigo acrescenta que as ferramentas modernas para desenvolvedores e a engenharia assistida por IA estão mudando a economia dessas escolhas, dando às organizações mais flexibilidade para decidir onde uma plataforma de terceiros oferece valor suficiente e onde mais controle interno se justifica.

### Rastreabilidade
Uma estratégia de modernização é mais defensável quando cada decisão tecnológica pode ser rastreada até a capacidade de negócio que ela deve melhorar. Esse vínculo dá à liderança uma base prática para escolher entre substituição incremental, integração em torno do núcleo legado, migração de plataforma ou desenvolvimento sob medida, de acordo com o valor, a maturidade e a criticidade de cada capacidade.

## Limites de capacidade podem fortalecer o design de APIs
A análise de capacidades também pode orientar a arquitetura de APIs, especialmente quando as integrações foram se acumulando em torno de aplicações individuais ao longo do tempo. APIs projetadas em torno dos dados ou das aplicações que expõem tendem a levar os limites atuais dos sistemas para a camada de integração. Isso cria dependências de tecnologias que a organização pode vir a substituir. Domain-Driven Design organiza as APIs em torno dos domínios de negócio mais duradouros identificados durante o trabalho de capacidades.

### Três camadas fundamentais
Em serviços de aposentadoria, o artigo propõe três camadas fundamentais. APIs de dados oferecem acesso controlado aos repositórios de dados subjacentes. APIs de Serviço de Negócio, ou de Domínio, mantêm as regras e os dados de áreas como empregador ou patrocinador, participante, investimentos e fontes de financiamento. Transações que precisam de informações ou lógica de vários domínios são coordenadas por uma camada de orquestração, de modo que cada domínio mantenha a responsabilidade pelas próprias regras de negócio.

> ❌ **Anti-padrão — APIs moldadas pelas aplicações:** projetar APIs de acordo com os dados que uma aplicação individual possui ou expõe. **Por que as pessoas fazem isso:** o artigo sugere que as integrações se acumulam em torno das aplicações com o tempo. **Consequência:** os limites atuais dos sistemas são levados para a camada de integração e a prendem a tecnologias que podem ser substituídas. **Correção:** organizar as APIs em torno dos domínios de negócio identificados no trabalho de capacidades.

### As regras de negócio permanecem em seu domínio
Elegibilidade de participantes e processamento de contribuições ilustram essa estrutura. As regras dessas atividades permanecem no domínio relevante e se aplicam de forma consistente onde quer que a capacidade seja usada, o que evita várias versões da mesma lógica incorporadas em aplicações de front-end. Mudanças na plataforma de dados subjacente podem então acontecer sem mover as regras do domínio, e uma interface de participante ou de patrocinador pode evoluir sem reescrevê-las.

### Por que os limites estáveis importam
Limites de domínio estáveis permitem que cada camada evolua de forma independente, mesmo quando plataformas, interfaces e regulações mudam em ritmos diferentes. Eles impedem que uma atualização em uma parte do ambiente se transforme em um grande esforço de integração. Também criam pontos mais claros para controles de segurança e de conformidade. Autenticação, autorização e acesso com privilégio mínimo ainda precisam funcionar em toda a arquitetura, especialmente quando há dados financeiros e pessoais de participantes envolvidos. A propriedade do domínio ajuda a estabelecer onde os requisitos de acesso devem ficar e quem é responsável por eles.

### Agentes de IA e permissões
O artigo prevê que agentes de IA interajam diretamente com as APIs de serviços de aposentadoria. Suas permissões devem corresponder às atividades de negócio que estão autorizados a realizar, com acesso restrito, auditabilidade adequada e controles que reflitam a sensibilidade dos dados subjacentes.

> ❌ **Anti-padrão — Acesso amplo para agentes:** dar a um agente de IA acesso a APIs além das atividades que ele deve executar. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** as preocupações do artigo com responsabilização, auditabilidade e sensibilidade dos dados de participantes não são tratadas. **Correção:** restringir as permissões dos agentes a atividades de negócio específicas e auditá-las.

A estrutura de capacidades e domínios criada durante a modernização oferece às organizações uma base prática para definir esses limites à medida que a IA assume um papel maior na atividade de serviços de aposentadoria.

## A modernização deve tornar o negócio mais adaptável
Organizações de serviços de aposentadoria continuarão modernizando ambientes envelhecidos de recordkeeping e atendimento à medida que os requisitos de negócio mudam, as exigências regulatórias crescem e a tecnologia legada fica mais difícil de sustentar. Custo, tempo de implementação e disrupção moldarão esses programas, mas a medida maior de sucesso é se a arquitetura resultante permite ao negócio sustentar seus produtos, clientes e participantes à medida que as necessidades deles mudam.

### Antes da RFP
Antes que uma RFP ou uma plataforma-alvo seja comprometida, a liderança deve ter uma visão clara das capacidades que o negócio precisa, de onde estão as lacunas de maturidade mais consequentes e quais áreas carregam a maior exposição regulatória, de participantes ou financeira. Conectar essas prioridades às aplicações que as suportam oferece uma base defensável para decidir o que manter, estender, substituir, comprar ou construir, e um sequenciamento de modernização fundamentado na necessidade do negócio.

### Capacidades duradouras sobrevivem às plataformas
Plataformas e ofertas SaaS vão mudar, as regulações vão remodelar as operações de aposentadoria, e a IA vai influenciar tanto os modelos de atendimento quanto o desenvolvimento de software. As necessidades subjacentes permanecem as mesmas: inscrever e atender participantes, administrar planos, processar transações e cumprir obrigações regulatórias. Uma arquitetura organizada em torno dessas capacidades duradouras pode absorver a mudança tecnológica sem obrigar o negócio a repensar seu modelo operacional a cada grande mudança de plataforma.

### Agentes inteligentes e confiança
O artigo encerra observando que agentes inteligentes terão um papel maior no atendimento de aposentadoria. A oportunidade de automatizar e se adaptar precisa coexistir com requisitos rigorosos de conformidade, segurança e confiança do participante.

## Sobre o autor
O autor é Diretor de Entrega Técnica na Corebridge Financial e tem 25 anos de experiência em transformação de negócios, modernização de aplicações e entrega técnica. A biografia diz que o autor lidera modernização em recordkeeping, integração de dados, migração para a nuvem, arquitetura orientada a APIs, analytics e IA, e é IEEE Senior Member, tendo apresentado trabalhos técnicos em conferências do IEEE. O artigo termina com quatro tags (Arquitetura de APIs, Modernização de Aplicações, Mapeamento de Capacidades de Negócio, transformação digital) e um espaço publicitário. Nenhuma delas traz um argumento próprio.

## 🧠 Modelos mentais
### Capacidade antes da aplicação
**O modelo:** a capacidade de negócio é aquilo que a organização precisa ser capaz de fazer. Aplicações são uma de várias coisas que a sustentam, e podem mudar enquanto a capacidade permanece a mesma.
**Como aplicar:** antes de uma RFP, liste as capacidades e mapeie cada aplicação sobre elas, depois procure lacunas e capacidades que dependam de vários sistemas.

### Maturidade e criticidade juntas
**O modelo:** maturidade diz o quão bem uma capacidade funciona. Criticidade diz o quanto a sua fraqueza importa. Uma capacidade de baixa maturidade com pouca exposição pode esperar, enquanto uma capacidade moderadamente madura com alta exposição regulatória ou de participantes pode precisar avançar primeiro.
**Como aplicar:** avalie cada capacidade quanto à maturidade e então classifique a criticidade pela exposição regulatória, pelo impacto no participante e pela sensibilidade a custos ou receita, priorizando grandes lacunas em capacidades de alta exposição.

### Três camadas de APIs
**O modelo:** APIs de dados expõem os repositórios de dados. APIs de domínio mantêm as regras de cada área de negócio. Uma camada de orquestração coordena as transações que cruzam domínios.
**Como aplicar:** coloque cada regra de negócio no domínio que a possui, e mantenha a coordenação entre domínios na camada de orquestração, para que as regras não sejam copiadas para as aplicações de front-end.

## 🔑 Principais conclusões
- Defina a direção de negócio, os produtos e os segmentos de clientes antes de qualquer avaliação de plataforma, porque uma plataforma escolhida sem eles herda as limitações que o programa deveria corrigir.
- Mapeie as capacidades em toda a operação, incluindo os sistemas de CRM e de vendas fora da plataforma de recordkeeping, já que a plataforma de recordkeeping normalmente suporta apenas parte do panorama.
- Classifique as capacidades por maturidade e criticidade em conjunto, com a criticidade avaliada pela exposição regulatória, pelo impacto no participante e pela sensibilidade a custos ou receita, e não pela idade da aplicação.
- Avalie a maturidade pela forma como o trabalho realmente funciona, e não por notas aspiracionais, porque atalhos de funcionários podem fazer um processo manual parecer automatizado.
- Escolha o caminho de modernização capacidade por capacidade, entre substituição incremental, envolver o núcleo com APIs ou uma camada de integração, migração para SaaS ou plataforma e engenharia sob medida.
- Organize as APIs em torno de domínios de negócio, com APIs de dados, APIs de domínio e uma camada de orquestração, para que as regras de negócio fiquem em um só lugar e a camada de integração não herde os limites dos sistemas legados.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios TOGAF | Negócio (capacidades, mapa de capacidades, arquitetura de referência de negócio, produtos e segmentos de clientes); Aplicação (alinhamento de aplicações, arquitetura de APIs, camada de integração, decisões de manter, estender, substituir ou comprar); Dados (APIs de dados sobre repositórios de dados, mudanças na plataforma de dados); Tecnologia (SaaS e plataformas modernas, mapeamento de ativos tecnológicos para capacidades). |
| Escopo | Empresarial: a análise abrange toda a operação de serviços de aposentadoria, do recordkeeping ao CRM e às finanças, embora trabalhe no nível de capacidade. |
| Stakeholders e preocupações | Equipes de liderança (investimento e sequenciamento antes de uma RFP); líderes de tecnologia (escolhas de plataforma e integração); equipes de negócio e de produto (adequação a produtos e segmentos de clientes); responsáveis por conformidade e regulação (exposição regulatória); participantes e patrocinadores de planos (atendimento e proteção de dados); equipes de engenharia (capacidade consumida por customização de commodities); responsáveis pelo acesso de agentes de IA (permissões e auditabilidade). |
| Frameworks e notações | Nenhum citado. O artigo usa Domain-Driven Design e uma "arquitetura de referência de negócio", mas não menciona TOGAF, ArchiMate, BPMN, BIZBOK ou C4. |

### Decisões e trade-offs
- **Sequenciamento negócio-primeiro versus plataforma-primeiro:** o autor começa pelas capacidades e pela direção de negócio antes de qualquer avaliação de fornecedores. O custo é um início mais lento, e exige reunir primeiro o contexto de negócio.
- **Caminho de modernização por capacidade:** as opções são substituição incremental, envolver o núcleo legado com APIs ou uma camada de integração, migração para uma plataforma moderna ou SaaS e engenharia sob medida. Cada uma difere em custo, risco de implementação, dívida técnica e controle mantido, e o artigo afirma que a resposta varia por capacidade.
- **Solução padrão versus engenharia sob medida:** funções commodity usam soluções padrão, e capacidades diferenciadoras podem justificar trabalho sob medida. O custo do trabalho sob medida é a capacidade de engenharia; o custo das soluções padrão é menos controle.
- **Base de priorização:** o autor ordena pela exposição de negócio e pela lacuna de maturidade, e trata restrições de contratos, fornecedores e janelas como limites. O custo é que o roteiro pode entrar em conflito com os prazos contratuais.
- **Organização das APIs:** as APIs são organizadas por domínio de negócio e coordenadas por uma camada de orquestração, em vez de pelos dados ou aplicações por trás delas. O artigo não informa o custo dessa estrutura, mas sugere trabalho extra de design e as oficinas necessárias para identificar os domínios.
- **Permissões de agentes de IA:** o artigo recomenda permissões restritas e auditáveis, ligadas a atividades de negócio. O custo é que os agentes podem fazer menos sem acesso mais amplo, o que o artigo não pondera.

### Fundamentos em que se apoia
- [capacidade de negócio](/pt-br/foundations/business-capability.md) — O artigo usa capacidades como unidade para a pontuação de maturidade e criticidade. Sua escala de maturidade mede como o trabalho é feito, o que a definição de trabalho do fundamento, "independentemente de quem o executa ou de como", não cobre.
- [Arquitetura de Negócio](/pt-br/foundations/business-architecture.md) — O mapa de capacidades e a arquitetura de referência de negócio do artigo são artefatos de arquitetura de negócio, usados aqui para definir prioridades de modernização.
- [domínio de arquitetura](/pt-br/foundations/architecture-domain.md) — O artigo cobre preocupações de negócio, aplicação, dados e tecnologia, mas nunca menciona os domínios do TOGAF. Seu uso de "domínio" para áreas de negócio, como participante e patrocinador, segue o Domain-Driven Design, que tem um significado diferente.
- [Arquitetura de Aplicações](/pt-br/foundations/application-architecture.md) — O artigo trata as aplicações como grupos de capacidades, e suas decisões de manter, estender, substituir e de camada de integração são decisões de arquitetura de aplicações.
- [Arquitetura de Dados](/pt-br/foundations/data-architecture.md) — As APIs de dados dão acesso controlado aos repositórios de dados, e o artigo trata as mudanças na plataforma de dados como separadas das regras de negócio.
- [Arquitetura de Tecnologia](/pt-br/foundations/technology-architecture.md) — O artigo usa arquitetura de tecnologia para escolhas de SaaS e de plataformas modernas, e para o mapeamento de ativos tecnológicos para capacidades.

## 🏛️ Para o Kenji
Isso se aplica diretamente ao trabalho de arquitetura no nível Staff, porque o artigo mostra como conectar capacidade de negócio, investimento e decisões de plataforma em uma única cadeia rastreável. Também se aplica às disciplinas de MBA em estratégia e operações de negócio, nas quais o investimento em capacidades é uma escolha estratégica. Considerando o seu trabalho em governança de IA, a parte mais útil é a seção sobre permissões de agentes de IA. A questão em aberto é como você definiria as permissões de um agente quando uma única tarefa precisa de dados e regras de vários domínios de negócio. Esta é a minha interpretação, não a do autor.

## 📚 Plano de leitura
### Do artigo
- [A Capability-First Approach to Modernizing Retirement Services Technology Platforms](https://www.architectureandgovernance.com/uncategorized/a-capability-first-approach-to-modernizing-retirement-services-technology-platforms) — O original completo, que inclui a Figura 1 e a biografia do autor, nenhum dos dois presente no texto extraído. O artigo não cita outros links.

### Para aprofundar
- Domain-Driven Design, design estratégico (bounded contexts e context mapping) — O artigo se apoia em limites de domínio para organizar as APIs, e esta é a teoria por trás deles.
- Planejamento baseado em capacidades no TOGAF 9.2 (arquitetura de negócio) — Ele situa o trabalho de capacidades dentro do método de arquitetura empresarial de onde vêm os termos do artigo.
- Modelos de maturidade de capacidades, com sua progressão de manual a adaptativo — Eles mostram como pontuar a maturidade de forma consistente entre capacidades.
- Migração strangler fig — A substituição em etapas que o artigo descreve, com sistemas antigos e novos coexistindo.

## Conceitos
- [bounded-context](/pt-br/concepts/bounded-context.md)
- [ai-agent](/pt-br/concepts/ai-agent.md)
- [strangler-fig](/pt-br/concepts/strangler-fig.md)
