---
type: Article Summary
title: 'Apresentação: Modernizando o varejo em escala: arquitetando uma plataforma da Nuvem‑à‑Borda para uma empresa global'
description: A transformação da Tesco mostra que mudanças em grandes empresas exigem, em conjunto, mudança de talentos, processos e plataforma, entregues em etapas escalonadas.
resource: https://www.infoq.com/presentations/tesco-enterprise-tech-transformation
tags:
- architecture
- enterprise-architecture
- ai-agent
- model-context-protocol
- bounded-context
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-22T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/presentations/tesco-enterprise-tech-transformation
  title: 'Presentation: Modernising Retail at Scale: Architecting a Cloud‑to‑Edge Platform for a Global Enterprise'
  author: Jayesh Bhayani
status: stable
source_url: https://www.infoq.com/presentations/tesco-enterprise-tech-transformation
source: InfoQ
source_type: blog
published: '2026-11-20'
domain: architecture
subdomain: enterprise-architecture
concepts:
- ai-agent
- model-context-protocol
- bounded-context
selected_by: architecture-lane
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/420baf74172d815d89a2ff94819bc62a
foundation_candidates:
- system of record
- build versus buy
- workload placement
lang: pt-br
translation_of: /notes/architecture/2026-11-22-presentation-modernising-retail-at-scale-architecting-a-clou.md
---

## 🎯 Tese central
A transformação tecnológica em grandes empresas só funciona quando a mudança de sistemas anda junto com a mudança de talentos, processos e cultura, e é entregue em pequenos passos escalonados, ligados à demanda real do negócio.

## 🧭 Por que o autor escreveu
Jayesh Bhayani, que passou dez anos na Tesco, parte do estado que encontrou: uma tecnologia fragmentada, em silos e dependente de fornecedores, que não conseguia entregar varejo omnichannel em escala. Ele compartilha o que foi necessário para modernizar um varejista desse tamanho e defende que as lições se aplicam a qualquer negócio com estrutura física, canais digitais e operações em tempo real. Se a tese estiver certa, uma nova plataforma sozinha não entregará o resultado, porque o modelo operacional precisa mudar junto com ela.

## Abertura (antes do primeiro título)
> 🔎 Contexto adicionado: a transcrição tem uma introdução sem título antes do primeiro cabeçalho. Ela é mantida aqui como uma parte própria.

Bhayani conta que entrou na Tesco depois de cerca de 25 anos no Vale do Silício. Ao ver o site pela primeira vez, achou-o "bastante básico" e pensou que a rede de supermercados tinha pouca tecnologia, mas encontrou um grande conjunto de sistemas em silos que precisava de transformação em escala. Ele apresenta a palestra como um relato compactado de dez anos de aprendizado, cobrindo a plataforma, a mudança cultural por trás dela e para onde a empresa está indo.

## Contexto: a enorme escala do nosso negócio
### Escala operacional
A Tesco opera cerca de 5,000 lojas de tamanhos variados em cinco países, depois de vender seus negócios na Tailândia e na Malásia. Tem 20 centros de distribuição, 25 milhões de clientes (quase um terço da população do Reino Unido) e 350,000 colaboradores, dos quais cerca de 15,000 trabalham na sede e cerca de 6,000 na tecnologia. Processa cerca de 50 milhões de cestas, 1.3 milhão de pedidos online por semana, 50 milhões de caixas pela cadeia de suprimentos e 4.5 bilhões de chamadas por dia, com cobertura de entrega em domicílio de 98% no Reino Unido. Decisões de tecnologia pesam muito nesse tamanho.

### Ambiente de missão crítica
Quase 50,000 dispositivos de ponto de venda (POS), chamados de caixas no Reino Unido, rodam nas lojas, junto com milhares de dispositivos portáteis na mesma plataforma. Falhas, portanto, afetam o cliente de imediato. O autor usa isso para explicar por que a transformação aqui é mais difícil do que em uma startup ou em uma empresa de médio porte.

## Por que isso importa além do varejo?
### Eficiência e eficácia
Eficiência é fazer as coisas bem e rápido. Eficácia é fazer as coisas certas. O autor ressalta que as duas são diferentes: uma equipe pode ser rápida em tudo e ainda assim perseguir as metas erradas. Ele afirma que a lição vale para qualquer setor com estrutura física, operações digitais e demanda em tempo real.

> ❌ **Antipadrão — Eficiência sem eficácia:** Fazer muitas coisas rapidamente sem verificar se são as coisas certas. **Por que as pessoas fazem isso:** não citado no artigo. **Consequência:** a organização está "perseguindo metas erradas". **Correção:** verifique a meta primeiro e só então busque velocidade: "fazer as coisas certas e fazê-las da maneira certa".

## O ponto de partida - O grande PORQUÊ
### Stacks em silos e identidade fragmentada
A tecnologia existia, mas estava separada em silos pela estrutura histórica do negócio. Lojas e e-commerce rodavam sistemas diferentes, e o próprio e-commerce tinha várias stacks para supermercado, marketplace, roupas (F&F) e vinhos, incluindo Microsoft .NET e Oracle. O cliente tinha um login diferente para cada uma, sem visão única do cliente, plataforma de dados, gestão de produtos ou fonte única de verdade para estoque ou preço.

### Parque monolítico, em batch e on-premise
Os sistemas rodavam como monólitos em data centers privados, porque a nuvem pública ainda não era usada. Obter um servidor podia levar cerca de dois meses. O software de POS das lojas tinha cerca de 20 anos, era orientado a batch e difícil de conectar. Os engenheiros levavam disquetes às lojas para implantar o software.

### Software de fornecedor customizado
A escala da Tesco lhe permitia exigir mudanças sob medida dos fornecedores. Isso a prendeu por anos a versões fortemente customizadas, algumas com pelo menos uma década e impossíveis de atualizar. A terceirização pesada para consultorias agravou o problema, que o autor descreve como cultura e qualidade ruins.

> ❌ **Antipadrão — Customizar produtos de fornecedores para o seu formato:** Exigir mudanças sob medida porque a sua escala permite. **Por que as pessoas fazem isso:** o autor diz que isso "parece bom no início". **Consequência:** anos de dependência de um produto customizado, e sistemas que não podem ser atualizados. **Correção:** a resposta do artigo, mais adiante, é decidir entre construir ou comprar pela diferenciação e manter as plataformas compradas flexíveis (veja as seções sobre framework e nuvem).

## Imperativo estratégico para a mudança
### Omnichannel e coerência global
As expectativas dos clientes mudavam rápido, e a forma antiga de fazer negócios não funcionava. O negócio queria mudar enquanto a tecnologia estava parada. A Tesco precisava de experiência omnichannel, coerência global entre as estruturas físicas e digitais, ciclos de mudança rápidos, mais confiabilidade, segurança, competência e capacidade. A resposta foi migrar de stacks fragmentadas para plataformas unificadas que atendem todos os canais e mercados, construídas como uma "plataforma de varejo" multi-tenant (o autor enfatiza que ela é mais do que uma plataforma técnica).

### Tempo de espera para mudanças
Uma mudança pequena no software de POS levava cerca de seis meses do fornecedor. Depois, a regressão quebrava outras três coisas, e o fornecedor cobrava de novo para corrigi-las. Qualquer coisa que chegasse aos caixas levava de seis a nove meses.

> ❌ **Antipadrão — Aceitar os ciclos de mudança do fornecedor:** Tratar um ciclo de lançamento de seis a nove meses como normal. **Por que as pessoas fazem isso:** o artigo não diz; o modelo do fornecedor era a única opção. **Consequência:** regressões, cobranças repetidas por correções de bugs e nenhuma mudança no nível da loja por meses. **Correção:** a resposta do artigo são plataformas que a empresa controla, o que depois permitiu que as equipes entrassem em produção em semanas.

## Onde estamos agora?
### Cobertura da plataforma e confiança
Todo o negócio do Reino Unido e da República da Irlanda já roda na plataforma, e os negócios da Europa Central estão sendo incluídos. A confiança em tecnologia é maior ali, e a estratégia de tecnologia está integrada à estratégia do negócio. O autor contrasta isso com a mentalidade de serviço anterior ("só precisamos de três coisas, você pode construir?"), que criava os silos. A tecnologia agora é "parte integrante de todo o ecossistema de negócios", e não uma unidade em um canto.

## Ecossistema da transformação
### Transformação de talentos
A maior parte do desenvolvimento era terceirizada, com mentalidade de serviço. A Tesco redefiniu carreiras, cargos e descrições de funções, e decidiu internalizar o trabalho. Começou no Reino Unido e na Índia, depois acrescentou Hungria e Polônia para escalar e encontrar talentos. Os frameworks de carreira foram redefinidos em produto, engenharia e programas, e o recrutamento também mudou.

### Produto, não programa
A gestão de produtos não existia no início. Gerentes de programa e engenheiros de serviço conduziam o trabalho, e o contato favorito de um executivo de negócio na tecnologia "resolvia" tudo por meio da terceirização. A mudança foi para uma empresa ágil e orientada a produto, que constrói produtos, e não soluções pontuais. O autor observa que os gerentes de programa continuam existindo, mas fazem um trabalho diferente.

### Organização que espelha o negócio
A Tesco tem quatro grandes unidades de negócio: cadeia de suprimentos e distribuição; comercial, que decide o sortimento de cerca de 40,000 a 100,000 SKUs, preço, promoção, layout das lojas e corredores; canais, ou seja, online e lojas; e cliente, que cobre a fidelidade. A tecnologia foi reorganizada para espelhar essa estrutura, porque "você não pode comandar o seu próprio navio em uma grande empresa". O terceiro pilar é a transformação de processos, já que processos inadequados derrubam a transformação de talentos e de sistemas.

> ❌ **Antipadrão — Comandar o próprio navio:** Tecnologia organizada como uma unidade separada que afirma ser diferente do resto da empresa. **Por que as pessoas fazem isso:** não citado no artigo. **Consequência:** o autor diz que essa transformação não é sustentável. **Correção:** alinhar a organização aos domínios e à estrutura do negócio.

## Transformação de sistemas: framework estratégico
### Construir e comprar
A Tesco constrói o que é diferencial central e compra o que não é. A plataforma de varejo é um diferencial, incluindo o software de POS de cada caixa, os coletores portáteis de escaneie-e-compre e as câmeras. As funções de apoio, como sistemas de pessoas e financeiros, são compradas. Os produtos comprados ainda precisam se integrar à plataforma.

### Limites de domínio e guardrails
O controle centralizado de mudanças e os comitês de arquitetura, onde projetos e código ficam em fila para revisão, não combinam mais com o ritmo da entrega, e o autor diz que combinarão ainda menos com uma força de trabalho agêntica. A Tesco descentralizou com limites claros de domínio, padrões compartilhados e "liberdade dentro dos guardrails".

> ❌ **Antipadrão — Filas de revisão centrais:** Cada projeto e mudança de código espera por um comitê. **Por que as pessoas fazem isso:** não declarado no artigo. **Consequência:** o autor sugere que as filas não acompanham a entrega. **Correção:** defina limites de domínio e padrões, e depois deixe as equipes decidirem como fazer, dentro desses guardrails.

### Excelência técnica e recuperação de perda total
A excelência técnica é proativa. Ela vai além de bugs e aplicação de patches do sistema operacional, e pergunta como a empresa evita repetir os mesmos problemas. A recuperação de perda total é um problema diferente da recuperação de desastres (DR), que o autor considera básica. Líderes de negócio precisam de educação sobre isso, e o tema exige tempo e foco dedicados. O resultado são entregas mais rápidas, com responsabilidade clara e resiliência de longo prazo.

> ❌ **Antipadrão — Deixar a resiliência ao acaso:** Presumir que a resiliência acontecerá porque os engenheiros cuidarão disso nas horas vagas. **Por que as pessoas fazem isso:** o autor diz que até a equipe de tecnologia presume isso. **Consequência:** sem tempo nem foco dedicados, a perda total fica sem planejamento. **Correção:** planeje a recuperação de perda total como uma capacidade financiada dentro da excelência técnica.

## A plataforma de varejo e de tecnologia
### Princípios e diretrizes da plataforma
As diretrizes vão além dos clientes. A experiência do colaborador importa para os sistemas operacionais das lojas, e a experiência do fornecedor importa porque os fornecedores avaliaram mal a Tesco. Outras diretrizes são reuso entre mercados e unidades de negócio, operação offline, risco cibernético e conformidade embutidos, e custo como funcionalidade. Os princípios são: serviços componíveis; design por domínio; caminhos de dados claros e qualidade de dados; segurança em camadas, do perímetro ao núcleo; multi-tenancy por padrão, com uma implantação atendendo vários países e negócios; construção de capacidades de produto em vez de soluções pontuais; e abstrações.

### Operação offline na borda
Milhares de dispositivos de borda rodam nas lojas, onde a rede pode falhar. Substituir a rede custaria centenas de milhões, e as margens do varejo de alimentos ficam em cerca de 4% (3% a 4.5%). Os mesmos microservices rodam na nuvem e na borda, com abstrações que mantêm a lógica de negócio e os dados consistentes. Se a rede falhar, a loja ainda identifica clientes de fidelidade, aceita pagamentos com cartão e por aproximação, e trata atualizações de preço na gôndola (shelf edge). O design falha a favor do cliente.

### Custo como funcionalidade
O gasto com nuvem pública era de GBP 50 milhões a 60 milhões por ano quatro ou cinco anos antes, e crescia exponencialmente. Para um negócio de GBP 50 bilhões com margem de cerca de 4%, algo em torno de GBP 2 bilhões, uma conta de nuvem de GBP 100 milhões é um grande impacto no P&L. O autor diz que a tecnologia deve manter o custo como funcionalidade, e não como restrição, e deve saber que valor ele compra.

### Abstrações
Adicionar um item à cesta chama pelo menos 40 microservices: token de identidade, ID do produto, UUID do cliente, personalização, preço, descrição e mais. Sem abstrações, um caminho do celular até o data center levaria cerca de 40 idas e voltas na rede, o que o autor chama de extremamente lento. As abstrações escondem a complexidade e, nessa escala, mantêm os ambientes digital e físico coesos sem se tornarem um gargalo.

> ❌ **Antipadrão — Expor cada serviço ao cliente:** Os clientes chamam cada microservice diretamente pela rede. **Por que as pessoas fazem isso:** não declarado no artigo. **Consequência:** cerca de 40 idas e voltas na rede para uma única inclusão na cesta, o que o autor chama de "extremamente lento". **Correção:** coloque abstrações entre os clientes e os serviços.

### Domínios e o desvio pela nuvem
A plataforma de varejo tem mais de 60 domínios e centenas de microservices na nuvem privada e pública. No início, a Tesco tinha apenas servidores em data center, sem elasticidade. Depois, deu cartões de crédito aos desenvolvedores para que se cadastrassem na nuvem pública às pressas, o que trouxe custo alto e valor incerto, seguido de repatriação e nova migração. A lição é construir primeiro as capacidades centralizadas, o que leva mais tempo, mas evita uma migração cara.

> ❌ **Antipadrão — Adoção de nuvem com cartão de crédito:** Deixar as equipes adotarem a nuvem pública de forma ad hoc para avançar rápido. **Por que as pessoas fazem isso:** velocidade e necessidade de elasticidade. **Consequência:** custo alto, valor incerto, depois repatriação e nova migração. **Correção:** construa primeiro as capacidades centrais, para que a migração seja planejada uma única vez.

### Posicionamento de cargas de trabalho
Serviços que precisam de elasticidade e escala real vão para a nuvem pública, e o autor prefere PaaS portátil. Sistemas grandes, como a tecnologia de cadeia de suprimentos e de centros de distribuição, rodam na nuvem privada, que também oferece a mesma experiência para desenvolvedores. A presença é cerca de 80% privada e 20% pública, mas o custo é o inverso: cerca de 80% na nuvem pública. Uma configuração pública equivalente custa de 8 a 10 vezes mais. PaaS proprietário prende você ao fornecedor, e o autor alerta que as taxas de licença sobem exponencialmente. Ele também observa que um desenvolvedor pode gastar quase GBP 500 por dia em desenvolvimento de código agêntico sem clareza sobre o valor criado.

> ❌ **Antipadrão — Fácil de entrar, difícil de justificar:** Usar a nuvem pública sem saber que valor ela cria. **Por que as pessoas fazem isso:** a nuvem pública é "fácil de começar". **Consequência:** os custos crescem exponencialmente, e o autor cita GBP 500 por dia de gasto com desenvolvimento agêntico. **Correção:** escolha PaaS portátil e posicione cada carga de trabalho de acordo com valor e flexibilidade.

### Uma equipe é dona de cada plataforma
Uma equipe é dona da plataforma de borda, da plataforma de nuvem do varejo e, durante a migração, da plataforma legada. O autor diz que dividir um problema entre uma equipe que constrói novos serviços e outra que mantém os sistemas antigos "nunca funciona", e a motivação cai rápido.

> ❌ **Antipadrão — Propriedade dividida entre o antigo e o novo:** Uma equipe constrói novos serviços enquanto outra mantém os sistemas legados. **Por que as pessoas fazem isso:** não declarado no artigo. **Consequência:** na experiência do autor, isso nunca funciona. **Correção:** mantenha uma equipe responsável pelos dois lados durante a migração.

### Serviços compartilhados
No início, os engenheiros gostavam de operar Cassandra, Kafka e protocolos por conta própria. À medida que as cargas de produção cresciam, a Tesco não tinha equipe separada de QA ou suporte, então os engenheiros eram totalmente responsáveis pela produção. A Tesco então investiu em serviços compartilhados, para que os engenheiros não precisassem mais operar a própria infraestrutura.

## Além da IA nas operações - Comércio agêntico
### IA em previsão e logística
A Tesco usa IA há muito tempo. A previsão é o problema de suprimento mais difícil: se as previsões erram, as prateleiras esvaziam ou a comida é desperdiçada. O exemplo do autor são 100 kg de tomates. As previsões dependem dos prazos de entrega, que variam de quatro entregas por dia para tomates até seis meses para jeans, e da validade dos produtos. A distribuição usa o layout do estoque, a roteirização de caminhões e o preenchimento de cargas em milhares de milhas por dia. Personalização e precificação também são orientadas por IA.

### Comércio agêntico é uma decisão de negócio
Os clientes passam cada vez mais tempo no ChatGPT e no Gemini e podem comprar mantimentos por meio de agentes. O autor argumenta que não participar traz o risco de marginalização, mas participar também traz o risco de perder o primeiro contato com o cliente. Por isso, é "uma decisão de negócio", e não apenas uma decisão de tecnologia.

### Dados legíveis por máquina e gateways de agentes
A otimização para buscas precisa mudar. Os rastreadores para agentes precisam de dados legíveis por máquina, o que o autor chama de AEO (entendimento agêntico). Frameworks de identidade, consentimento e política precisam definir o que os agentes podem fazer em um site ou por meio de APIs. Gateways para agentes, com sub-agentes e protocolos agent-to-agent, ficarão ao lado dos canais existentes, e o MCP ajudará os agentes a descobrir serviços e dados. Protocolos de comércio como UCP e ACP estão surgindo, mas o autor diz que ainda não estão prontos para escala de produção. As regras do Reino Unido não permitem que agentes façam pagamentos em nome dos clientes, e as regras variam entre países.

> 🔎 Contexto adicionado: o artigo se refere a um diagrama de arquitetura que não está na transcrição, então esta descrição se baseia apenas no texto falado.

### Do sistema de registro ao sistema de delegação
A plataforma muda de um sistema de registro para um sistema de delegação, porque agora os agentes trabalham com os dados e serviços da empresa e tomam decisões em nome dos clientes. APIs de experiência já existentes, como adicionar item à cesta, podem precisar de endpoints extras para agentes, mas continuam rodando na mesma plataforma de varejo.

## O que poderia ter sido melhor?
### Uma plataforma técnica central, construída cedo
A Tesco migrou cedo para a nuvem pública, e cada equipe construiu seu próprio CI/CD, observabilidade e outras ferramentas. Cada equipe afirmava ter o melhor pipeline, mas o autor queria um único pipeline de CI/CD, e não 50. A gestão do portfólio chegou tarde. Com cerca de 6,000 profissionais de tecnologia e centenas de produtos, a Tesco agora usa um modelo de maturidade de produtos para posicionar cada produto em uma curva de maturidade.

> ❌ **Antipadrão — Um pipeline por equipe:** Cada equipe constrói seu próprio CI/CD e observabilidade. **Por que as pessoas fazem isso:** cada equipe acredita que o próprio pipeline é o melhor. **Consequência:** esforço duplicado em muitos pipelines. **Correção:** construa cedo uma plataforma técnica central.

### Estratégia de localização
A engenharia pode estar em vários locais. Gestão de produtos, engenharia e trabalho de programas não podem estar em lugares diferentes, ou as equipes não serão autossuficientes. O autor diz que pensar nisso cedo evita reimplementação e migração caras.

## Dicas para qualquer transformação em grandes empresas
### Mude processos, não apenas sistemas
A transformação de sistemas não basta. Os processos de negócio também precisam mudar, porque se desenvolvem organicamente, como os sistemas. O autor pergunta por que quatro países deveriam ter processos operacionais diferentes, e recomenda simplificar as operações.

### Construa capacidades centrais cedo para ganhar vazão
Capacidades centrais como CI/CD e observabilidade devem ser construídas cedo. Com a plataforma pronta, a Tesco agora consegue entrar em produção em duas ou três semanas, quando o mesmo trabalho poderia levar seis meses. Sem essas capacidades, a empresa teria apenas esforço duplicado a cada nova mudança.

### Evoluir, evitar tecnologia por tecnologia e comunicar valor
A plataforma era organizada horizontalmente. Com as plataformas maduras, a Tesco está se tornando mais vertical, formando parcerias entre domínios de negócio e tecnologia. O valor precisa ser comunicado cedo, porque é difícil de explicar fora da tecnologia e até dentro dela. O autor alerta contra "tecnologia por tecnologia".

### Proteja a propriedade intelectual (IP) e evite dependência de fornecedor
A Tesco tem cuidado com o vazamento de propriedade intelectual ao usar desenvolvimento de código agêntico, e pede aos fornecedores de SaaS garantias de compartilhamento de dados e de proteção de IP para manter seu diferencial. O autor termina dizendo que a tecnologia não para de mudar, do blockchain à nuvem pública e aos agentes, e que, sem a cultura, o talento e a confiança certos na empresa, a transformação "não vai funcionar".

## 🧠 Modelos mentais
### Eficiente versus eficaz
**O modelo:** Eficiência é fazer as coisas bem e rápido. Eficácia é fazer as coisas certas. Velocidade nas metas erradas continua sendo desperdício.
**Como aplicar:** Antes de medir a velocidade de entrega, verifique se a meta está certa.

### Liberdade dentro dos guardrails
**O modelo:** As decisões são descentralizadas dentro de limites de domínio claros, padrões compartilhados e guardrails, em vez de passarem por filas em comitês centrais.
**Como aplicar:** Escreva os guardrails primeiro e depois deixe as equipes de domínio escolherem o como.

### Construa o que diferencia, compre o resto
**O modelo:** Construa os diferenciais centrais, aqui a plataforma de varejo e seu software de POS. Compre as funções de apoio e integre tudo o que for comprado à plataforma.
**Como aplicar:** Para cada capacidade, pergunte se ela diferencia o negócio. Se não diferencia, compre e integre.

### Falhe a favor do cliente
**O modelo:** Quando a rede falha, as operações da loja degradam em direção ao cliente. A fidelidade continua sendo identificada e os pagamentos continuam sendo aceitos.
**Como aplicar:** Decida antecipadamente quais operações devem continuar offline e como cada uma deve falhar.

## 🔑 Principais conclusões
- A mudança de sistemas não bastou na Tesco. Talento, processos e organização também precisaram mudar, o que, segundo o autor, levou cerca de dez anos.
- Uma plataforma de varejo única agora roda 100% dos negócios do Reino Unido e da República da Irlanda, está se estendendo à Europa Central e é multi-tenant por padrão.
- Construa os diferenciais centrais, como o POS, e compre as funções de apoio, integrando todo produto comprado à plataforma.
- Cerca de 80% da presença da Tesco está na nuvem privada, mas cerca de 80% do custo está na nuvem pública. Uma configuração pública equivalente custa de 8 a 10 vezes mais, por isso PaaS portátil e posicionamento deliberado das cargas de trabalho importam.
- As lojas continuam operando quando a rede falha: ainda identificam clientes de fidelidade e aceitam pagamentos, e uma equipe é dona das plataformas de borda e de nuvem.
- O comércio agêntico é uma decisão de negócio. Ele exige dados legíveis por máquina, políticas de consentimento e gateways de agentes, e move a plataforma de sistema de registro para sistema de delegação.
- Construir cedo um CI/CD e uma observabilidade centrais é o que permite à Tesco entrar em produção em duas ou três semanas, quando antes levava cerca de seis meses.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios TOGAF | Negócio (capacidades, unidades de negócio, estrutura organizacional, modelo de produto); Aplicação (mais de 60 domínios, microservices, abstrações); Tecnologia (nuvem privada e pública, dispositivos de borda, POS); Dados (de forma leve: caminhos de dados, qualidade de dados, visão única do cliente) |
| Escopo | Empresa: uma plataforma de varejo única em cinco países e quatro unidades de negócio. O subdomínio é enterprise-architecture, e o artigo também desce a detalhes em nível de solução, como o caminho da chamada de inclusão na cesta |
| Stakeholders e preocupações (concerns) | Líderes de negócio (confiança, demanda, valor); clientes (experiência); colaboradores nas lojas (sistemas operacionais); fornecedores (experiência); engenheiros (carga de suporte à produção); finanças (custo e P&L); reguladores (regras de pagamento por agentes); segurança (risco cibernético, vazamento de IP) |
| Frameworks e notações | Nenhum citado (nem TOGAF, ArchiMate, BPMN ou similares). Protocolos citados: MCP, ACP, UCP, agent-to-agent |

### Decisões e trade-offs
- **Construir versus comprar:** Construa os diferenciais centrais, compre as funções de apoio e integre os produtos comprados. Custo: esforço de integração e dependência de fornecedores para as partes compradas.
- **Centralizar versus descentralizar:** Descentralize dentro de limites de domínio e guardrails, em vez de comitês centrais. Custo: disciplina em padrões e guardrails.
- **Posicionamento em nuvem pública versus privada:** Coloque serviços elásticos e orientados por escala na nuvem pública, e sistemas grandes na nuvem privada. Custo: a nuvem pública custa cerca de 8 a 10 vezes mais em uma configuração equivalente, e os custos sobem exponencialmente se não forem gerenciados.
- **PaaS portátil versus proprietário:** Use apenas PaaS portátil, para manter a opção de mudar. Custo: o artigo admite que isso é "não perfeito" e abre mão de parte da conveniência dos serviços gerenciados.
- **Modo de falha offline:** Falhe a favor do cliente na borda. Custo: o artigo não informa o custo; ele está implícito na necessidade de dados consistentes quando a rede volta.
- **Mudança escalonada versus big-bang:** Sequencie a mudança em pequenos passos, porque um negócio de GBP 50 bilhões não pode parar. Custo: mudança geral mais lenta e uma jornada longa e irregular.
- **Propriedade de equipe entre legado e novo:** Uma equipe é dona das plataformas antiga e nova durante a migração. Custo: a mesma equipe carrega as duas cargas.
- **Serviços compartilhados versus infraestrutura de autoatendimento:** Centralize serviços compartilhados para que os engenheiros deixem de operar seu próprio Cassandra e Kafka. Custo: investimento em uma equipe de plataforma compartilhada.
- **CI/CD central versus por equipe:** Construa um único pipeline central. O autor diz que a equipe deveria tê-lo construído antes. Custo: investimento inicial antes que as equipes de produto precisem dele.
- **Camada de abstração versus chamadas diretas aos serviços:** Adicione abstrações para que os clientes evitem cerca de 40 idas e voltas por inclusão na cesta. Custo: uma camada extra para projetar e operar.
- **Participar do comércio agêntico:** Participe, mas com gateways e políticas. Custo: o primeiro contato com o cliente pode migrar para outra empresa, e UCP e ACP não estão prontos para produção.

### Fundamentos em que se apoia
- [Arquitetura Empresarial (EA)](/pt-br/foundations/enterprise-architecture.md): o artigo trata o grupo inteiro como uma única arquitetura, mas a usa para orientar um programa de mudança de vários anos, o que a definição cobre.
- [Arquitetura de Negócio](/pt-br/foundations/business-architecture.md): o autor mapeia as equipes de tecnologia para as quatro unidades de negócio, que é a visão de estrutura de negócio descrita na definição.
- [Capacidade de negócio](/pt-br/foundations/business-capability.md): o artigo usa "capacidade" para produtos de plataforma reutilizáveis, como em "construa capacidades de produto, não soluções". Isso estende a definição na direção de unidades de entrega, então o termo é usado de forma diferente aqui.
- [Domínio de arquitetura](/pt-br/foundations/architecture-domain.md): o artigo cobre todos os quatro domínios TOGAF, com Dados de forma leve.
- [Arquitetura de Aplicações](/pt-br/foundations/application-architecture.md): os mais de 60 domínios e microservices formam a estrutura de aplicações.
- [Arquitetura de Tecnologia](/pt-br/foundations/technology-architecture.md): a nuvem privada, a nuvem pública e os serviços de borda formam a estrutura de tecnologia.
- [Empresa](/pt-br/foundations/enterprise.md): a Tesco é uma única empresa em cinco países e quatro unidades de negócio.

## 🏛️ Para o Kenji
Sua interpretação, não a do autor: isso importa para qualquer arquiteto corporativo que precise decidir o quanto construir versus comprar e onde colocar as cargas de trabalho, mantendo o custo visível. Também tem relação com o lado de arquitetura do MBA, porque o artigo liga as escolhas tecnológicas ao P&L e à estrutura do negócio. Uma pergunta que ele levanta: para uma plataforma em que você trabalha, quais capacidades realmente diferenciam o negócio, e como você mostraria isso a um CFO cético?

## 📚 Plano de leitura
### Do artigo
- [Apresentação no InfoQ: Modernising Retail at Scale](https://www.infoq.com/presentations/tesco-enterprise-tech-transformation) — a palestra original e sua transcrição completa; leia para a sessão de perguntas e respostas e para quaisquer slides não reproduzidos aqui.
### Para aprofundar
- Domínios de arquitetura do TOGAF 9.2 (Negócio, Dados, Aplicação, Tecnologia) — para situar os exemplos do artigo no modelo padrão.
- Modelos de maturidade de gestão de produtos e de portfólio em grandes organizações de TI — o artigo usa uma curva de maturidade para decisões de portfólio, mas não a explica.
- Protocolos de comércio agêntico e MCP — o artigo é cauteloso com UCP e ACP, então vale verificar o estado atual por conta própria.

## Conceitos
- [Agente de IA](/pt-br/concepts/ai-agent.md)
- [Model Context Protocol (MCP)](/pt-br/concepts/model-context-protocol.md)
- [Bounded context](/pt-br/concepts/bounded-context.md)
