---
type: Article Summary
title: Telecom sob pressão de custos
description: Operadoras de telecom entregam mais dados por menos receita, então o retorno da IA está nas operações de rede e exige governança primeiro.
resource: https://mitsloanreview.com.br/telecom-sob-pressao-de-custos/
tags:
- architecture
- business-architecture
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-20T07:00:00-03:00'
sources:
- resource: https://mitsloanreview.com.br/telecom-sob-pressao-de-custos/
  title: Telecom sob pressão de custos
  author: MIT Sloan Management Review Brasil
status: stable
source_url: https://mitsloanreview.com.br/telecom-sob-pressao-de-custos/
source: MIT Sloan Management Review Brasil
source_type: blog
published: '2026-11-16'
domain: architecture
subdomain: business-architecture
concepts: []
selected_by: architecture-lane (PC run)
content: pc-run
complete: 'yes'
notion_url: https://app.notion.com/p/41ebaf74172d81fe948dd7f8c07737f9
lang: pt-br
translation_of: /notes/architecture/2026-11-20-telecom-sob-pressao-de-custos.md
---

## 🎯 Tese central
As operadoras de telecom entregam hoje mais dados por menos receita por unidade, então o retorno real da IA está nas operações de rede, e ele depende de governança e observabilidade antes que agentes possam entregá-lo.

## 🧭 Por que o autor escreveu
O artigo parte de um aperto: no 2º trimestre de 2026 o preço médio por gigabyte caiu 13,4% em relação ao ano anterior, enquanto o uso de dados por usuário subiu 16,8%, e o capex está em contração no mundo todo. Se a tese se sustenta, o investimento em IA deve se justificar por resultados operacionais, como tempo de reparo, escalonamentos e volume de alertas, e não principalmente por chatbots de atendimento ao cliente. A assinatura indica que a peça é conteúdo patrocinado ou em coautoria com a e-Core, e sua principal voz é um executivo da e-Core; por isso, os números de fonte fornecedora devem ser lidos com essa ressalva.

## Abertura: um preço por gigabyte em queda
O original não tem título para os primeiros quatro parágrafos; esta seção os cobre.

### Preço por gigabyte versus volume de dados
Segundo o Painel Econômico-Financeiro da Anatel, o preço médio por gigabyte na telefonia móvel caiu de R$ 6,19 para R$ 5,36 entre o 2º trimestre de 2025 e o 2º trimestre de 2026, uma queda de 13,4%. No mesmo período, o uso médio de dados por usuário subiu de 5,82 GB para 6,80 GB, alta de 16,8%. O ponto do autor é que as operadoras transportam mais tráfego enquanto ganham menos por unidade transportada.

> 🔎 Contexto adicional: preço multiplicado pelo volume dá um gasto aproximado por usuário. Pela minha conta, R$ 6,19 × 5,82 GB dá cerca de R$ 36,0 e R$ 5,36 × 6,80 GB dá cerca de R$ 36,5, então o gasto total por usuário é aproximadamente estável (cerca de +1%). A afirmação do artigo de que as operadoras "recebem menos" se refere ao preço unitário, não à receita total por cliente. Este é um cálculo meu, não do autor.

### A receita cresce enquanto o investimento fica parado
A Conexis Brasil Digital informa receita bruta de R$ 338 bilhões em 2025 para as telecomunicações brasileiras, acima da inflação pelo terceiro ano seguido. O investimento chegou a R$ 36,3 bilhões e, ajustado pela inflação, ficou praticamente estável, com +0,2% em relação ao período anterior. O setor ganha mais enquanto seus gastos com rede quase não se movem.

### Contenção de custos global
A Omdia informa que a receita global de conectividade cresceu 4% em 2025, chegando a US$ 1,3 trilhão, enquanto o capex, o investimento das operadoras, caiu 2%, para US$ 303 bilhões. A Dell'Oro Group projeta outra queda de 2% no capex global de telecom em 2026 e crescimento médio de apenas 1% ao ano até 2030. O autor apresenta isso como um ciclo de contenção de custos em todo o setor, e não como uma peculiaridade brasileira.

### Legado e commoditização
Douglas Pereira, engenheiro de telecom com experiência na Vivo, na Ericsson e na TIM, e hoje head de vendas de transformação empresarial para a América Latina na e-Core, argumenta que o histórico de muitas aquisições das operadoras faz com que o legado e a cultura pesem sobre a inovação, e que eficiência e time to market continuam difíceis. Ele acrescenta que "a telecom tradicional está se tornando uma commodity", então o desafio é monetizar a base existente com produtos digitais, usando upsell e cross-sell em uma presença fixa e móvel já disponível em todo o Brasil.

## A IA deixa de ser projeto e vira infraestrutura
### Previsões de investimento em IA
A IDC prevê que as operadoras de telecom invistam US$ 617 milhões em IA na América Latina em 2026. O Brasil responde por quase metade desse total, e os gastos devem crescer 37% ao ano entre 2024 e 2029.

### Infraestrutura e redes baseadas em intenção
A IDC prevê que a IA se torne a infraestrutura central das telecomunicações, levando as redes de estáticas para dinâmicas, nas quais objetivos de negócio se transformam em comportamento de rede sem intervenção humana. O artigo chama isso de modelo baseado em intenção.

> 🔎 Contexto adicional: em redes baseadas em intenção, uma operadora declara um objetivo, como um nível de serviço para uma região, e o sistema o traduz em configuração. Os engenheiros definem a intenção em vez de alterar cada equipamento manualmente.

### Sinal de adoção
Na Telecom DX Survey 2025, 67% das operadoras brasileiras citaram a IA entre as tecnologias mais importantes para sua transformação digital.

### O gargalo não é a tecnologia
Pereira diz que IA e agentes estão na agenda de todos os executivos, mas muitos líderes ainda não sabem como aplicá-las, como medir o ROI nem como comprovar o investimento. A restrição que o artigo identifica é de capacidade organizacional e de medição.

## Onde está o ganho?
### O atendimento ao cliente é onde a IA está hoje
A Anatel registrou 1.354.791 reclamações em 2025, 6,9% a mais que no ano anterior, interrompendo uma sequência de quedas. Cobrança e cancelamento cresceram mais, enquanto as reclamações sobre qualidade técnica e reparos caíram 9,3%.

### O custo oculto dos contatos não resolvidos
Pereira descreve um cliente que tenta o telefone, um chatbot, o WhatsApp, o e-mail e a ouvidoria sem obter resposta. Do lado da empresa, um cliente gerou cinco tentativas de reclamação, ou seja, um único cliente sobrecarrega todos os canais de atendimento. A lição implícita do artigo é que uma empresa que só fica sabendo das falhas pelos clientes paga por elas várias vezes.

> ❌ **Anti-pattern — detectar falhas por meio de reclamações de clientes:** as operadoras esperam que os clientes relatem interrupções, que então contatam vários canais, e vizinhos ligam para a operadora dizendo que o problema é grave. **Por que as pessoas fazem isso:** o artigo não diz; os dados de reclamação são o sinal mais fácil de obter. **Consequência:** um custo oculto que o autor chama de "gigantesco", espalhado entre canais e contatos repetidos. **Correção:** detectar desvios na própria rede, usando monitoramento e agentes de IA, e abrir chamados antes que os clientes liguem.

### Onde a IA deve atuar: a rede
O relatório da GSMA "A Economia Móvel na América Latina 2026" (maio de 2026) constata que 85% das implantações de IA das operadoras regionais já estão em operação, mas 78% se concentram no atendimento ao cliente e apenas 8% nas redes.

Exemplo de Pereira: a IA pode avaliar várias características ao mesmo tempo, sem esperar dez reclamações da mesma região. Com governança, observabilidade e agentes de IA que analisam desvios, o sistema produz um diagnóstico para as equipes de campo e abre chamados. Depois de uma tempestade, por exemplo, o sistema cruza os dados de vento, raios e equipamentos offline da região para descobrir o que foi danificado, de modo que uma equipe preparada chega com o que é necessário.

### Redes em camadas e ruído de alertas
As redes se sobrepõem em camadas (fixa, 2G, 3G, 4G, 5G e Wi-Fi), e uma falha em uma camada se propaga. À medida que a demanda cresce, as ferramentas de monitoramento geram mais alertas, e grande parte deles vira ruído que atrasa a resposta ao que importa.

> 🔎 Contexto adicional: MTTR (mean time to repair, tempo médio de reparo) é o tempo médio entre a detecção de uma falha e sua restauração. O artigo o usa como principal métrica do valor operacional da IA.

### Como o ganho é medido
As estimativas da IBM sugerem que uma estratégia bem desenhada de IA generativa para operações de rede poderia reduzir o MTTR em 30% a 40%. Ela também poderia reduzir, em proporção semelhante, o tempo que técnicos do centro de operações gastam respondendo a perguntas de campo, e cortar em 20% a 30% os escalonamentos para níveis superiores de suporte. São projeções para uma estratégia bem desenhada, e não resultados medidos no objeto do artigo.

### Um caso de fornecedor: menos alertas, detecção mais rápida
Um cliente da e-Core que recebia cerca de 100.000 alertas por mês passou a tratar 27.000 no período, e seu tempo de detecção de incidentes caiu 57%. O autor conclui que as equipes podem focar nas decisões que exigem julgamento sem aumentar o quadro de pessoal na mesma proporção do tráfego, o que importa em um setor sem folga de custos. Trata-se de um único caso relatado pelo fornecedor.

### Governança e visibilidade primeiro
Pereira conclui que o ponto de partida é a governança e a visibilidade. A IA só funciona quando o negócio e seu destino são compreendidos, com processos definidos, bases e ativos mapeados, segurança, governança clara e escalonamento proativo, tudo voltado ao negócio e ao fortalecimento das equipes. Ele encerra dizendo que telecom é um mercado muito dinâmico, e que todos precisam da velocidade e da flexibilidade que a tecnologia bem aplicada proporciona.

## 🧠 Modelos mentais
### Pressão de commodity
**O modelo:** Quando o produto transportado vira commodity, o valor deixa de estar na capacidade em si e passa para serviços construídos sobre a base existente, como produtos digitais vendidos por upsell e cross-sell.
**Como aplicar:** Em qualquer negócio do tipo utilidade, pergunte se a receita ainda cresce com o volume ou apenas com serviços adicionados à mesma presença.

### Governança antes dos agentes
**O modelo:** Agentes de IA agregam valor apenas sobre governança, observabilidade, processos definidos e ativos de dados mapeados.
**Como aplicar:** Antes de aprovar um agente de IA, verifique se suas entradas, decisões e resultados são observáveis e se o processo sobre o qual ele atua está definido.

### Correlacione, não conte
**O modelo:** Um alerta é evidência apenas em contexto. Cruzar clima, status de equipamentos e dados regionais transforma o volume de alertas em um diagnóstico.
**Como aplicar:** Meça os sistemas de alerta pelas decisões que eles viabilizam, e não pelo número de alertas que produzem.

## 🔑 Principais conclusões
- O preço médio por gigabyte caiu 13,4% em relação ao ano anterior no 2º trimestre de 2026, enquanto o uso de dados por usuário subiu 16,8%.
- A receita das telecomunicações brasileiras cresceu acima da inflação em 2025, enquanto o investimento ficou praticamente estável, com +0,2% em termos reais.
- Entre as implantações de IA das operadoras regionais, 78% se concentram no atendimento ao cliente e 8% nas redes, um equilíbrio que o autor sugere ser subutilizado para as redes.
- As reclamações sobre cobrança e cancelamento cresceram em 2025, enquanto as reclamações técnicas e de reparo caíram 9,3%.
- O autor julga a IA de rede pelo tempo de reparo, tempo de técnicos, escalonamentos e volume de alertas, citando faixas da IBM.
- Segundo Pereira, governança, observabilidade e dados mapeados vêm antes dos agentes de IA.
- O caso da e-Core (de 100.000 para 27.000 alertas; tempo de detecção 57% menor) é relatado pelo fornecedor.

## 🏗️ Lente de arquitetura
| Aspecto | Neste artigo |
|---|---|
| Domínios TOGAF | Negócio (pressão de custos, monetização da base, operações de atendimento ao cliente); Dados (ativos de dados mapeados, dados de clima, equipamentos e alertas cruzados); Aplicação (agentes de IA, ferramentas de monitoramento, chatbots, abertura de chamados); Tecnologia (redes fixas e móveis em camadas, de 2G a 5G e Wi-Fi, controle de rede baseado em intenção) |
| Escopo | Segmento ou capacidade: operações de rede e atendimento ao cliente dentro de uma operadora, com números de mercado do setor usados como contexto. Não é um programa de toda a empresa nem uma solução única. |
| Stakeholders e preocupações (concerns) | Executivos (ROI, comprovação do investimento, custo); centro de operações de rede e equipes de campo (ruído de alertas, tempo de reparo); clientes (cobrança, cancelamento, contatos repetidos); a reguladora Anatel (tendências de reclamações); equipe e cultura (legado de aquisições) |
| Frameworks e notações | Nenhum citado. O artigo cita fontes (Anatel, Conexis, Omdia, Dell'Oro, IDC, GSMA, IBM) e um modelo de rede "baseado em intenção", mas nenhum framework de arquitetura. |

### Decisões e trade-offs
- **Onde aplicar a IA primeiro:** No atendimento ao cliente, onde está a maioria das implantações, ou nas operações de rede, que o autor recomenda. O atendimento ao cliente é o uso atual e mensurável; as operações de rede exigem maturidade de dados e monitoramento para ser críveis, e seu ROI é mais difícil de comprovar.
- **Governança e visibilidade antes dos agentes:** O autor recomenda. O custo é um início mais lento, com investimento em processos, bases e mapeamento de ativos antes de resultados visíveis.
- **Monetizar a base existente com serviços digitais:** Recomendado em vez de depender do volume de conectividade. O custo é a capacidade de upsell e cross-sell que a operadora precisa construir, e que o legado e a cultura tornam mais lenta.
- **Avançar para redes baseadas em intenção:** Apresentado como o rumo para o qual a IDC vê a IA levando as telecomunicações. O artigo não discute os custos nem os riscos de um comportamento de rede autônomo.

### Fundamentos em que se apoia
- [Domínio de arquitetura](/pt-br/foundations/architecture-domain.md) — as quatro áreas que o artigo toca correspondem aos quatro domínios primários do TOGAF, embora o artigo não use o TOGAF.
- [Arquitetura de Negócio](/pt-br/foundations/business-architecture.md) — custo, monetização e operações de atendimento ao cliente se encaixam aqui, mas o artigo não usa o termo.
- [Capacidade de negócio](/pt-br/foundations/business-capability.md) — a "capilaridade" (alcance da rede) do artigo é tratada como um ativo, não como uma capacidade, então a ligação é frouxa.
- [Arquitetura de Tecnologia](/pt-br/foundations/technology-architecture.md) — as redes em camadas (fixa, 2G a 5G, Wi-Fi) são os componentes tecnológicos cuja interação propaga falhas.
- [Arquitetura de Dados](/pt-br/foundations/data-architecture.md) — bases mapeadas e dados cruzados são a pré-condição do artigo para a IA.
- [Arquitetura de Aplicações](/pt-br/foundations/application-architecture.md) — agentes de IA, chatbots e ferramentas de monitoramento são as aplicações; o artigo não descreve como elas se agrupam em capacidades.

## 🏛️ Para o Kenji
Este é um modelo útil para construir um business case de IA em um provedor de serviços: o valor aparece no tempo de reparo, nos escalonamentos e no volume de alertas, e a base de dados é uma pré-condição. Verifique a fonte primeiro, porque o conteúdo é em coautoria com um fornecedor, então os números da e-Core e da IBM são de origem fornecedora. Esta é a sua interpretação, não a do autor: como você comprovaria o ROI de uma plataforma de correlação de alertas quando o MTTR de referência e os volumes de alertas não são medidos de forma consistente, e qual métrica um CFO aceitaria?

## 📚 Plano de leitura
### Do artigo
O artigo não cita links. As fontes que ele menciona estão listadas abaixo.
### Para aprofundar
- Anatel, Painel Econômico-Financeiro — fonte dos números de preço por gigabyte e de reclamações; confira você mesmo o trimestre mais recente.
- GSMA, "A Economia Móvel na América Latina 2026" (maio de 2026) — fonte da divisão de implantações de 85%, 78% e 8%.
- IBM, IA generativa em operações de rede — fonte das faixas de MTTR, de tempo de técnicos e de escalonamento.
- Redes baseadas em intenção (intent-based networking) — o modelo-alvo por trás da visão da IDC sobre a IA como infraestrutura de telecom.
- Correlação de alertas e AIOps — a técnica por trás do ponto do artigo de que o volume de alertas é ruído sem contexto.
