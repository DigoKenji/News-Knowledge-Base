---
type: Article Summary
title: 'Projetar uma vez, implantar em escala: construindo um ecossistema de ML orientado por templates'
description: Padronizar o design de modelos em templates compartilhados permite ajustar técnicas de ML uma única vez e distribuí-las por toda a frota de anúncios da Meta.
resource: https://engineering.fb.com/2026/11/18/ml-applications/design-once-deploy-at-scale-template-driven-ml-ecosystem
tags:
- ai-ml
- multi-agent-orchestration
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://engineering.fb.com/2026/11/18/ml-applications/design-once-deploy-at-scale-template-driven-ml-ecosystem
  title: 'Design Once, Deploy at Scale: Building a Template-Driven ML Ecosystem'
  author: Meta Engineering
status: stable
source_url: https://engineering.fb.com/2026/11/18/ml-applications/design-once-deploy-at-scale-template-driven-ml-ecosystem
source: Meta Engineering
source_type: blog
published: '2026-11-18'
domain: ai-ml
concepts:
- multi-agent-orchestration
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d81a889b1f48773564391
lang: pt-br
translation_of: /notes/ai-ml/2026-11-19-design-once-deploy-at-scale-building-a-template-driven-ml-ec.md
---

## 🎯 Tese central
Padronizar o design de modelos em um pequeno conjunto de templates reutilizáveis e combináveis transforma a iteração de modelo a modelo em um problema compartilhado de sistemas, de modo que técnicas comprovadas podem ser ajustadas uma única vez e distribuídas por uma grande frota de modelos.

## 🧭 Por que o autor escreveu o texto
Na escala de ranking de anúncios da Meta, o gargalo deixou de ser inventar técnicas melhores e passou a ser propagá-las para cada modelo em que se aplicam, e adaptar cada uma à mão não escala. Se a tese estiver certa, o template compartilhado se torna a superfície onde a inovação aterrissa, de modo que os ganhos chegam a muito mais modelos com menor custo de engenharia.

## Introdução: o problema da fragmentação
> A fonte não tem título sobre esta abertura, por isso este título foi acrescentado para estruturar o texto.

O artigo parte do gargalo. Em uma grande frota de modelos em produção, a parte difícil é distribuir técnicas de forma confiável e eficiente por um espaço de modelos fragmentado. Os números-resumo da Meta, todos dos últimos dois anos, são: redução de 92% no tempo de engenharia por modelo, mais de 5000 combinações técnica-modelo distribuídas, ganho médio de 0,63% na qualidade offline com capacidade de serving neutra, e 0,86% de ganho online cumulativo, cobrindo 70% dos modelos em quantidade. Os sistemas de ranking de anúncios da Meta processam bilhões de requisições diárias em muitos produtos e objetivos de otimização, servidos por um grande número de modelos em produção.

### Fragmentação
Fragmentação significa que um mesmo sistema se divide em muitas variantes divergentes, com arquiteturas, features, configurações de treino, restrições de serving e lógica específica de plataforma diferentes. O artigo diz que essa diversidade deu flexibilidade às equipes, mas criou três problemas sistêmicos: alto custo de engenharia para distribuir novas ideias amplamente, alto custo de confiabilidade causado por uma explosão de casos de borda, e baixa previsibilidade ao estimar o impacto de uma mudança em todo o ecossistema. Uma nova arquitetura pode levar anos para chegar à frota inteira, e um insight de ajuste em um modelo pode dizer pouco sobre o próximo. Com o tempo, propagar se torna mais difícil do que a própria inovação.

### Lei de escala desfavorável para a engenharia
Se cada ideia precisa ser avaliada e adaptada modelo a modelo, o trabalho de engenharia cresce linearmente com o número de modelos. Isso é administrável com poucos modelos, mas vira um gargalo sério conforme a frota cresce. O artigo apresenta isso como uma lei de escala desfavorável para a própria engenharia.

> 🔎 Contexto adicionado: "Lei de escala" aqui descreve como o esforço de engenharia cresce com o número de modelos, e não uma lei de escala de redes neurais.

### AutoML e automação
AutoML e sistemas de otimização funcionam melhor em um espaço estruturado. Quando cada modelo tem um formato diferente, as técnicas de maior potencial alcançam apenas um subconjunto dos modelos. O ponto do artigo é que a automação é limitada pela fragmentação, e não que a automação esteja errada.

> 🔎 Contexto adicionado: AutoML é aprendizado de máquina automatizado, sistemas que buscam entre designs de modelo ou configurações sem ajuste manual para cada caso.

> ❌ **Anti-padrão — automação sobreposta à fragmentação:** acrescentar mais uma camada de automação sobre uma configuração complexa, modelo a modelo. **Por que as pessoas fazem isso:** o artigo não declara um motivo; automatizar a complexidade existente é a primeira reação óbvia. **Consequência:** as técnicas mais promissoras alcançam apenas os modelos cujo formato se encaixa na automação. **Correção:** reduza primeiro a complexidade padronizando o espaço de design, e só então automatize.

### Standard Model Templates (SMT)
A resposta foi reduzir a complexidade antes de acrescentar automação. A equipe introduziu os Standard Model Templates (SMT), uma abordagem que padroniza o design de modelos em um conjunto curado de templates reutilizáveis. A Figura 1, como consta no artigo:

> Figura 1: Antes do SMT, cada técnica era ajustada à mão em cada modelo, e as combinações a testar cresciam exponencialmente. Com o SMT, a otimização multimodelo ajusta as técnicas uma única vez em modelos representativos, e as que generalizam entram na próxima versão do template.

## Uma fundação orientada por templates
### Template
Um template é uma configuração de modelo opinativa e mínima, construída a partir de componentes compartilhados: escolhas de arquitetura, lógica de features, configurações de treino e configurações de serving. Modelos semelhantes são agrupados em torno de um número menor de templates padrão. A lógica específica de cada modelo continua existindo onde é necessária, mas as partes comuns ficam explícitas e compartilhadas. O artigo diz que usa um pequeno conjunto de templates que cobre os principais padrões de ranking e recuperação em diferentes tamanhos de modelo. Esses pacotes cobrem seleção de features, interação entre features, paralelismo de modelo, configuração de treino e serving com consciência de hardware.

> 🔎 Contexto adicionado: Paralelismo de modelo significa dividir a computação de um mesmo modelo entre vários aceleradores de hardware.

> ❌ **Anti-padrão — design independente por modelo:** manter cada modelo em produção como um design amplamente independente. **Por que as pessoas fazem isso:** o artigo diz que a diversidade deu flexibilidade às equipes, para que cada modelo possa ser moldado às suas próprias necessidades. **Consequência:** distribuir e validar cada ideia cresce com o número de modelos, e a depuração acontece em um espaço de configuração sem limites. **Correção:** agrupe modelos semelhantes em torno de templates padrão compartilhados e mantenha a lógica específica de cada modelo apenas onde for necessária.

### Iteração no nível do template
Em vez de avaliar uma nova ideia de forma independente em cada modelo, a equipe a avalia em um conjunto bem menor de templates representativos, e então a propaga para os grupos de modelos que os usam. O artigo chama isso de transição da iteração por modelo para a iteração por template. Seguem três vantagens:

- **Complexidade de propagação reduzida:** uma implementação compartilhada atende a muitos modelos downstream, o que reduz o custo de distribuir novas ideias em escala.
- **Confiabilidade melhorada:** menos configurações únicas tornam viável construir validações mais robustas, melhores salvaguardas e fluxos de depuração mais transferíveis. Instabilidades de treino ou regressões de serving são mais fáceis de raciocinar dentro de uma estrutura de template compartilhada.
- **Previsibilidade melhorada:** quando os modelos de um grupo compartilham um template, o desempenho nos modelos representativos se torna um sinal melhor para o restante do grupo, o que torna o planejamento em nível de ecossistema menos dependente de suposições.

Com o tempo, a abordagem se tornou a base de um sistema de desenvolvimento de ML mais amplo, e não apenas um esforço de padronização.

A Figura 2, como consta no artigo:

> Figura 2: O ciclo de desenvolvimento do SMT (à esquerda) e a arquitetura em camadas dentro de cada template (à direita). As técnicas são ajustadas uma única vez em modelos representativos, adicionadas a uma nova versão do template e distribuídas para cada modelo construído sobre ele.

## SMTs: da padronização à transferência escalável de técnicas
A transição para os SMTs permitiu otimizar o ecossistema inteiro como uma unidade coesa. Em vez de gerenciar uma frota de modelos independentes, a equipe trata o cenário como uma superfície compartilhada de inovação.

### Transferência de design holístico
Em vez de propagar técnicas individuais uma a uma, a equipe transfere designs de modelo holísticos e comprovados, de modelos de origem sólidos para famílias de modelos-alvo, por meio da camada de template compartilhada. O artigo argumenta que muitos ganhos vêm das interações entre escolhas arquiteturais, decisões de escala e estratégias de treino, e não de componentes isolados aplicados de forma independente.

> ❌ **Anti-padrão — propagar técnicas uma a uma:** aplicar componentes isolados a cada modelo de forma independente. **Por que as pessoas fazem isso:** o artigo descreve isso como a abordagem anterior e não declara um motivo. **Consequência:** ganhos que vêm de interações entre arquitetura, escala e treino são perdidos. **Correção:** transfira designs completos e comprovados no nível de template, a partir de modelos de origem.

### Otimização baseada em leis de escala
Quando a estrutura do modelo é padronizada, as decisões de escala se tornam reutilizáveis. Em vez de redescobrir os trade-offs de custo-desempenho para cada modelo, a equipe os aprende no espaço de templates e os aplica de forma mais ampla. O artigo diz que isso torna a alocação de recursos mais fundamentada e menos ad hoc.

> 🔎 Contexto adicionado: Uma lei de escala descreve como a qualidade de um modelo muda de forma previsível com recursos como tamanho ou computação. O artigo não define o termo.

### Resultados e a premissa da diversidade
Ao longo de dois anos, a abordagem entregou:

- uma redução de 92% no tempo de engenharia por modelo;
- mais de 5000 combinações técnica-modelo distribuídas pela frota;
- uma melhoria média de 0,63% na qualidade offline do modelo com capacidade de serving neutra;
- um ganho online cumulativo de 0,86%, cobrindo 70% dos modelos em quantidade.

O artigo também questiona a suposição comum de que objetivos de otimização diversos exigem designs de modelo altamente diversificados. Sua afirmação é que um template compartilhado bem projetado pode generalizar muito além do esperado.

## Otimização multimodelo
### Otimização multimodelo
A otimização multimodelo torna a iteração de templates rigorosa. Ela avalia técnicas candidatas em modelos representativos e busca configurações que melhorem o desempenho agregado, limitando regressões.

### Taxa de regressão e consistência
Uma técnica com ganho médio forte, mas com regressões frequentes, é difícil de distribuir por uma grande frota. O artigo diz que a consistência importa tanto quanto o pico de melhoria para a propagação em escala de produção, então a decisão pondera tanto o desempenho agregado quanto a taxa de regressão.

> ❌ **Anti-padrão — distribuir apenas com base no ganho médio:** escolher técnicas pelo ganho médio e ignorar com que frequência elas regridem. **Por que as pessoas fazem isso:** o artigo não declara um motivo; o ganho médio é o número de manchete mais simples. **Consequência:** uma técnica que ajuda em média, mas regride em muitos modelos, é difícil de lançar em toda a frota. **Correção:** avalie os candidatos tanto pelo ganho agregado quanto pela taxa de regressão antes da integração.

### Validação em modelos reservados
Depois da otimização, as técnicas candidatas são validadas em modelos reservados, para verificar se os ganhos se transferem além do conjunto representativo. As que passam entram na próxima versão do template. Ao longo de dois anos, a equipe distribuiu 80 novas técnicas de ML por esse fluxo. O artigo observa que todo modelo que adota um template maduro herda esse histórico de melhorias generalizadas, em vez de começar de uma baseline isolada.

## Por que o SMT importa para sistemas de ML agentivos
### Construção de modelos com agentes
O artigo descreve uma visão comum: um único agente de ponta a ponta explora um grande espaço de design usando ferramentas, pipelines de treino e resultados de avaliação. Na prática, o espaço de busca é livre demais. Sem prioridades fortes, o agente gasta esforço em regiões improdutivas, se recupera de falhas evitáveis ou propõe mudanças difíceis de explicar e ainda mais difíceis de lançar.

> ❌ **Anti-padrão — busca livre de agente ponta a ponta:** deixar um único agente buscar em um espaço de design sem restrições. **Por que as pessoas fazem isso:** o artigo apresenta isso como a abordagem frequentemente imaginada e não declara um motivo. **Consequência:** esforço desperdiçado em regiões improdutivas e mudanças difíceis de explicar ou de lançar. **Correção:** dê estrutura aos agentes, um espaço delimitado e uma decomposição em subproblemas especializados.

### Espaço de busca delimitado e piso de qualidade
Os SMTs codificam padrões de design comprovadamente bons. Isso dá aos agentes um ponto de partida estruturado, um espaço de busca delimitado e um piso de qualidade. Em vez de tratar a construção de modelos como uma busca livre, o trabalho pode ser decomposto em subproblemas especializados.

### Construção de modelos com múltiplos agentes
Na Meta, os SMTs são a base de uma abordagem multiagente: os templates fornecem a estrutura, e os agentes fornecem exploração adaptativa e automação. No nível do template, os agentes ajudam a construir, avaliar e melhorar os templates. Eles propõem ideias de otimização com base em técnicas comprovadas, lançam experimentos em modelos representativos, diagnosticam falhas e iteram ao longo de várias rodadas. Isso ajuda a gerenciar os ciclos de vários dias que o desenvolvimento de templates exige. Os agentes também podem assumir trabalhos mais difíceis, como migrar modelos para novas plataformas de hardware. Uma técnica validada dessa forma chega a todo modelo construído a partir daquele template, a uma fração do custo de computação necessário para adaptar cada modelo separadamente.

## Lições aprendidas e o caminho à frente para os SMTs
### Padronização opinativa
A padronização só funciona se for opinativa o bastante para remover diversidade desnecessária, preservando a flexibilidade limitada que importa. O artigo alerta que um sistema de templates que tente acomodar todos os casos de borda históricos acabará recriando a fragmentação que deveria resolver.

> ❌ **Anti-padrão — acomodar todos os casos de borda históricos:** construir templates que absorvem todas as variantes existentes. **Por que as pessoas fazem isso:** o artigo não declara um motivo. **Consequência:** o sistema de templates recria a fragmentação que deveria remover. **Correção:** mantenha os templates opinativos e preserve apenas a flexibilidade que importa.

### Estrutura possibilita automação
Os SMTs funcionam porque a automação fica mais poderosa à medida que o sistema subjacente se torna mais estruturado. O desenvolvimento orientado por templates tornou o ecossistema mais fácil de raciocinar para sistemas de otimização, fluxos de escala e agentes, e não apenas mais fácil de operar para humanos.

### Caminho à frente
O artigo lista o trabalho restante: melhorar a cobertura dos templates, fortalecer a otimização baseada em leis de escala e avançar rumo a fluxos de trabalho agentivos totalmente de ponta a ponta. Isso inclui construção agentiva de templates, geração de modelos, avaliação online e otimização contínua a partir de sinais online. Os fluxos também precisam se tornar mais confiáveis e mais explicáveis.

A conclusão do artigo é que projetar uma vez e implantar em escala é "uma escolha arquitetural sobre como fazer um grande ecossistema de ML aprender mais rápido", e não apenas uma tática de eficiência.

## Leia o paper
O artigo aponta para o paper "Design Once, Deploy at Scale: Template-Driven ML Development for Large Model Ecosystems" para ver todos os detalhes. Nenhuma URL aparece no texto extraído, então nenhuma é indicada aqui.

## Agradecimentos
O artigo credita 28 pessoas pelo nome. Os nomes estão no original e não afetam o argumento.

## 🧠 Modelos mentais
### Padronize a superfície onde a inovação aterrissa
**O modelo:** a inovação escala quando a superfície compartilhada onde ela aterrissa é padronizada. A afirmação arquitetural do artigo é que projetar uma vez e implantar em escala é uma escolha sobre estrutura.
**Como aplicar:** antes de escalar uma técnica, identifique a unidade compartilhada onde ela será implantada, como um template ou um grupo de componentes, e padronize essa unidade primeiro.

### Avalie nos representativos, depois valide nos modelos reservados
**O modelo:** quando os modelos de um grupo compartilham um template, o desempenho nos modelos representativos é um forte sinal para o restante do grupo.
**Como aplicar:** escolha modelos representativos por grupo, meça tanto o ganho médio quanto a taxa de regressão, e então confirme nos modelos reservados antes da implantação.

### Consistência vence o ganho de pico para a propagação
**O modelo:** uma técnica precisa ser consistente para se propagar. Ganhos médios fortes com regressões frequentes são difíceis de distribuir por uma grande frota.
**Como aplicar:** condicione as implantações à taxa de regressão, além do ganho agregado.

### Busca delimitada para agentes
**O modelo:** agentes funcionam melhor em um espaço delimitado e conhecido, que estabelece um piso de qualidade, do que em uma busca livre.
**Como aplicar:** dê ao agente um template como ponto de partida, e divida a tarefa em subproblemas especializados em vez de uma busca aberta única.

## 🔑 Principais conclusões
- Adaptar cada ideia modelo a modelo faz o esforço de engenharia crescer linearmente com o número de modelos; a resposta do artigo é avaliar as ideias em alguns templates representativos.
- A abordagem SMT da Meta reduziu em 92% o tempo de engenharia por modelo ao longo de dois anos e distribuiu mais de 5000 combinações técnica-modelo.
- Os resultados relatados são um ganho médio de 0,63% na qualidade offline com capacidade de serving neutra e um ganho online cumulativo de 0,86%, cobrindo 70% dos modelos em quantidade.
- A otimização multimodelo pondera a taxa de regressão junto com o ganho médio, e os candidatos precisam passar pela validação em modelos reservados antes de entrar na próxima versão do template; 80 novas técnicas foram distribuídas dessa forma.
- Transferir designs completos no nível de template captura ganhos das interações entre escolhas de arquitetura, escala e treino que componentes isolados deixam passar.
- Templates dão aos agentes um espaço de busca delimitado e um piso de qualidade; o artigo diz que fluxos multiagente então cuidam de experimentos, diagnóstico de falhas e migrações de hardware no nível do template.
- A padronização precisa continuar opinativa; um sistema de templates que acomoda todos os casos de borda históricos recria a fragmentação.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para um arquiteto que projeta plataformas corporativas e de IA, o princípio transferível é padronizar a superfície compartilhada antes de automatizá-la, de forma semelhante a arquiteturas de referência e catálogos de componentes governados. Dada a sua experiência em governança de IA, a pergunta que isso levanta é quais controles de governança deveriam se acoplar a um template compartilhado, de modo que um único design aprovado cubra cada modelo construído a partir dele, e que evidência de regressão você exigiria antes que uma mudança de template chegue a todos os modelos downstream.

## 📚 Plano de leitura
### A partir do artigo
- Nenhuma URL aparece no texto extraído do artigo, então nenhum link é indicado. O paper "Design Once, Deploy at Scale: Template-Driven ML Development for Large Model Ecosystems" é citado apenas pelo título; leia-o para o método completo.
- O post "How Meta Uses AI Agents to Scale ML Techniques Across Ads Ranking Models" é citado apenas pelo título; leia-o para mais detalhes sobre o fluxo com agentes.

### Para ir além
- AutoML e neural architecture search: contexto sobre a automação de escolhas de design de modelos, que o artigo diz funcionar melhor em um espaço estruturado.
- Leis de escala para modelos neurais: como a alocação de recursos pode ser planejada em vez de definida ad hoc.
- Design de sistemas multiagente: como decompor tarefas e delimitar as ações dos agentes.
- Testes de regressão para lançamentos de modelos de ML: medir regressões por segmento além das métricas agregadas.
- Transferência de métricas offline para online: por que ganhos offline podem ou não prever ganhos online.

## 🔗 Conceitos
- [multi-agent-orchestration](/pt-br/concepts/ai-ml/multi-agent-orchestration.md)
