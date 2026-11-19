---
type: Article Summary
title: LinkedIn reduz de 750 para 20 as falhas diárias de fetch do shuffle no Spark com Apache Celeborn
description: Mover o armazenamento do shuffle para workers dedicados do Celeborn reduziu de 750 para 20 as falhas diárias de fetch do shuffle no Spark da LinkedIn.
resource: https://www.infoq.com/news/2026/11/linkedin-celeborn-remote-shuffle
tags:
- backend-systems
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/linkedin-celeborn-remote-shuffle
  title: LinkedIn Cuts Daily Spark Shuffle Fetch Failures from 750 to 20 with Apache Celeborn
  author: InfoQ
status: stable
source_url: https://www.infoq.com/news/2026/11/linkedin-celeborn-remote-shuffle
source: InfoQ
source_type: blog
published: '2026-11-18'
domain: backend-systems
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d81539f9cee297624f5f3
lang: pt-br
translation_of: /notes/backend-systems/2026-11-19-linkedin-cuts-daily-spark-shuffle-fetch-failures-from-750-to.md
---

## 🎯 Tese central
Mover o armazenamento do shuffle para fora do cluster de computação do Spark e para um serviço dedicado do Apache Celeborn reduziu as falhas diárias de fetch do shuffle da LinkedIn de cerca de 750 para 20, ao mesmo tempo que diminuiu o custo de computação e melhorou os tempos de execução em comparação com a implementação anterior, o Magnet.

## 🧭 Por que o autor escreveu
A LinkedIn executa mais de 300.000 jobs do Spark por dia e está migrando grandes backfills para uma arquitetura Flink unificada, o que aumenta a carga de shuffle. O artigo argumenta que o armazenamento do shuffle deve escalar de forma independente da computação e que isso resolve os problemas de confiabilidade, custo e tempo de execução que vinham com o design anterior.

## Escala e o problema do shuffle
A plataforma de dados offline da LinkedIn executa mais de 300.000 jobs do Spark por dia em milhares de nós de produção, consumindo cerca de 200 petabyte-hora de computação. À medida que a empresa avança rumo a uma arquitetura Flink unificada para processamento em tempo real e histórico, backfills em grande escala acrescentam mais cargas de shuffle. O shuffle move dados intermediários entre as etapas de processamento e pode sobrecarregar redes, discos e os tempos de execução das aplicações.

### Shuffle
O shuffle é a etapa em que os dados produzidos por uma etapa de um job distribuído são redistribuídos para que a etapa seguinte leia a parte de que precisa. No Spark, as tarefas produtoras gravam sua saída e as tarefas consumidoras a buscam pela rede, de modo que os dados passam pelo armazenamento em toda execução. A preocupação do artigo é que esses dados intermediários são grandes, de vida curta e ficam nas mesmas máquinas que executam a computação.
> 🔎 Contexto adicionado: um shuffle é a etapa em que a saída de cada tarefa map é redistribuída para que cada reducer leia a partição de que precisa.

## Magnet: o shuffle push-based anterior
A LinkedIn já havia melhorado as leituras do shuffle com o Magnet, sua implementação de shuffle push-based, que contribuiu para o Apache Spark 3.2. O Magnet agrupa os blocos do shuffle por partição de reducer em arquivos maiores, o que reduz as leituras aleatórias em disco. A limitação estava na localização: o armazenamento do shuffle continuava dentro do cluster de computação, de modo que os External Shuffle Services disputavam recursos com as cargas de computação.

### Magnet (shuffle push-based)
O shuffle push-based muda o padrão de leitura, e não a localização dos dados. Em vez de muitos blocos pequenos que cada reducer precisa localizar e ler, os blocos são combinados por partição de reducer em arquivos maiores. Menos leituras, e maiores, significam menos I/O aleatório em disco, que é o ganho que o Magnet entregou. O artigo mostra que esse ganho sozinho não resolveu a disputa por recursos causada por executar o shuffle dentro do cluster de computação.

> ❌ **Anti-padrão — armazenamento do shuffle dentro do cluster de computação:** External Shuffle Services rodam nos mesmos nós da computação e compartilham CPU, memória, I/O de disco e rede com ela. **Por que as pessoas fazem isso:** o artigo não diz; era a configuração em que o Magnet rodava. **Consequência:** a carga do shuffle e a da computação disputam os mesmos recursos, e os dados do shuffle ficam atrelados ao ciclo de vida dos executors. **Correção:** mover o armazenamento do shuffle para workers dedicados que escalam por conta própria, como faz o Celeborn.

## Apache Celeborn: armazenamento do shuffle separado da computação
Com o Apache Celeborn, as tarefas map enviam seus dados intermediários para workers dedicados. Os workers agregam e armazenam cada partição de forma independente dos executors de computação, e os reducers buscam os dados diretamente desses workers. O Celeborn roda como um serviço de shuffle separado, com seus próprios workers e um control plane replicado, diferentemente do Magnet, que dependia de serviços de merger dentro do cluster Spark. O artigo afirma que essa separação reduz a disputa de recursos entre computação e shuffle.

### Apache Celeborn
O Celeborn é um serviço remoto de shuffle de código aberto para mecanismos de big data, e o artigo o usa com Spark e Flink. Sua ideia principal é que os dados do shuffle são mantidos por um serviço, e não pelo executor que os produziu. O armazenamento pode, portanto, crescer junto com o volume de dados do shuffle, e um executor pode terminar ou falhar sem perder dados de que outras etapas ainda precisam. Isso importa mais na escala da LinkedIn, onde o volume do shuffle e a demanda de computação crescem em ritmos diferentes.

### Envio para workers dedicados
Nesse design, a etapa de push tira os dados do executor assim que eles são produzidos. Os workers fazem a fusão por partição, de modo que os reducers têm um caminho de leitura mais limpo. Como o armazenamento é separado, os workers podem ser dimensionados, atualizados e substituídos sem tocar na frota de computação. O artigo credita essa separação com a redução da disputa, e não a um mecanismo específico.

O artigo cita Aravind Patnam, engenheiro de software sênior (staff) da LinkedIn:
> "Operar isso de forma confiável em milhares de nós e centenas de milhares de aplicações diárias exigiu uma engenharia substancial além do projeto upstream."

O artigo também mostra uma figura com a legenda "Arquitetura do Celeborn na LinkedIn", que não é reproduzida no texto extraído.

## Control plane confiável
A implantação da LinkedIn usa um control plane baseado em Raft, com cinco nós distribuídos entre zonas de disponibilidade. O Apache Ratis fornece a replicação de metadados, a eleição de líder e o failover. A LinkedIn acrescentou replicação com consciência de rede e integrou o Celeborn à sua plataforma de saúde de hosts, de modo que workers próximos de manutenção são despriorizados e workers degradados são excluídos. O artigo credita essas mudanças com uma redução de 20 vezes nas falhas de tarefas durante disrupções de infraestrutura, em comparação com o Magnet.

### Control plane baseado em Raft
O control plane guarda os metadados que dizem aos reducers onde estão seus dados, então perdê-lo travaria os jobs. Um design baseado em Raft mantém esses metadados replicados em vários nós e elege um líder para coordenar as mudanças. Os cinco nós aqui estão distribuídos entre zonas de disponibilidade, então a falha de uma única zona não remove o quórum. O Apache Ratis é a biblioteca que fornece a replicação, a eleição e o comportamento de failover.
> 🔎 Contexto adicionado: Raft é um algoritmo de consenso que mantém um log replicado em vários nós e elege um líder para ordenar as escritas. O Apache Ratis é uma implementação Java de código aberto do Raft.

### Replicação com consciência de rede e saúde de hosts
A replicação com consciência de rede posiciona as cópias dos dados do shuffle levando em conta a topologia da rede. A integração com a plataforma de saúde de hosts permite que o Celeborn evite workers que estão prestes a cair para manutenção ou que estão degradados. O artigo descreve isso como uma forma de proteger os jobs durante disrupções de infraestrutura, e é daí que vem a redução de 20 vezes nas falhas de tarefas.

## Otimizações do data plane
A LinkedIn então ajustou o data plane. Moveu o armazenamento do shuffle para SSDs NVMe, o que reduziu a duração total do shuffle em 40%. Trocou a compressão LZ4 pela Zstandard, o que reduziu em cerca de 35% a frota de workers de shuffle necessária. Os workers receberam interfaces de rede de 100 Gbps, e mudanças na JVM e no data plane tornaram a seleção de slots seis vezes mais rápida. No conjunto da frota, o custo do shuffle por tarefa caiu quase 90%, e a fatia do tempo de execução dos executors gasta esperando pelo shuffle caiu cerca de 10-15% em relação ao pico.

### SSDs NVMe
SSDs NVMe têm latência muito menor e throughput maior que SSDs SATA tradicionais, o que combina com os muitos pequenos reads e writes aleatórios que o shuffle gera. Mover o armazenamento do shuffle para eles reduziu em 40% o tempo do shuffle nesta implantação. O artigo não informa o hardware de base, então o ganho vale apenas para a configuração da LinkedIn.

### Zstandard no lugar de LZ4
Compressão troca CPU por bytes movidos e armazenados. A Zstandard normalmente comprime mais do que a LZ4, a um custo maior de CPU, então a troca só compensa quando armazenamento ou rede são o gargalo. Aqui, a troca reduziu a frota de workers em cerca de 35%, o que o artigo apresenta como a economia. O artigo não informa o custo de CPU da mudança.

### Dimensionamento de workers e seleção de slots
Os workers foram provisionados com interfaces de rede de 100 Gbps, então a capacidade de rede não é o limite para buscar dados do shuffle. A seleção de slots, a etapa que atribui trabalho aos workers, ficou seis vezes mais rápida graças a otimizações na JVM e no data plane. Essas mudanças somam-se às mudanças de hardware e, juntas, reduziram o custo de shuffle por tarefa em quase 90%.

## Convergência para uma plataforma Flink unificada
A plataforma Flink convergida processa centenas de milhões de eventos por hora em backfills históricos, concluindo cargas de vários dias em menos de dez horas. Milhares de pipelines agora usam computações Flink unificadas para processamento nearline e correções históricas, substituindo implementações Spark paralelas. A LinkedIn está explorando cache e atualizações upstream mais rápidas, e continua contribuindo para o Celeborn.

### Computações Flink unificadas
Uma computação unificada usa um único caminho de código tanto para dados nearline (recentes) quanto históricos, de modo que a equipe não mantém mais uma versão Spark e uma versão Flink da mesma lógica. A evidência do artigo é a escala: milhares de pipelines migrados, e backfills de vários dias agora terminam em menos de dez horas. O artigo credita o trabalho com o shuffle por tornar essa migração possível, mas não mede essa dependência diretamente.

## 🧠 Modelos mentais
### Armazenamento do shuffle que sobrevive à sua computação
**O modelo:** Dados intermediários do shuffle deveriam ser um serviço próprio, e não um subproduto que nasce e morre com o executor que os escreveu. O argumento do artigo é que isso permite que o armazenamento escale independentemente da computação e reduz a dependência do ciclo de vida dos executors.
**Como aplicar:** Antes de colocar um armazenamento intermediário com estado nos nós de computação, pergunte se o I/O, as falhas e o escalonamento dele precisam acompanhar a computação ou podem ser separados.

## 🔑 Principais conclusões
- O Celeborn reduziu de cerca de 750 para 20 as falhas diárias de fetch do shuffle da LinkedIn, em comparação com o design do Magnet.
- O custo de computação caiu 10% e os tempos de execução das aplicações melhoraram 14%, ambos medidos em relação ao Magnet.
- Workers de shuffle dedicados, separados dos executors, reduzem a disputa de recursos entre computação e shuffle.
- Um control plane Raft de cinco nós distribuído entre zonas de disponibilidade, com Apache Ratis, reduziu em 20 vezes as falhas de tarefas durante disrupções de infraestrutura em comparação com o Magnet.
- SSDs NVMe reduziram em 40% a duração do shuffle, e a Zstandard no lugar da LZ4 reduziu a frota de workers em cerca de 35%.
- O custo do shuffle por tarefa caiu quase 90%, e o tempo dos executors esperando pelo shuffle caiu cerca de 10-15% em relação ao pico.
- Backfills unificados em Flink concluem cargas de vários dias em menos de dez horas e substituíram milhares de pipelines Spark paralelos.

## 🏛️ Para o Kenji
*Esta é a sua interpretação, não a do autor.* Para um arquiteto de plataformas de dados, o artigo é um caso concreto de tratar o shuffle como um serviço com dono próprio, com domínio de falhas e curva de escalonamento próprios, o que é uma decisão de arquitetura de dados tanto quanto de engenharia. A pergunta que ele levanta é que custo operacional, em posse do control plane e em atualizações, uma equipe assume ao tirar uma camada com estado do cluster de computação, e se a redução de falhas justificaria isso em uma plataforma menor.

## 📚 Plano de leitura
### Do artigo
- [Artigo do InfoQ](https://www.infoq.com/news/2026/11/linkedin-celeborn-remote-shuffle) — o artigo-fonte, para o texto original e as legendas das figuras.
- Post do blog da LinkedIn (citado duas vezes no artigo; a URL não está no texto extraído, então não é linkado aqui) — a fonte primária dos detalhes da implantação.

### Para ir além
- Documentação do projeto Apache Celeborn — como os workers, o control plane e o shuffle push-based são configurados.
- Shuffle push-based do Apache Spark (Magnet, Spark 3.2) — o design que o Celeborn substitui na LinkedIn.
- Consenso Raft e Apache Ratis — o modelo de replicação e eleição de líder por trás do control plane.
- Batch e streaming unificados no Apache Flink — o modelo de processamento por trás da plataforma convergida de backfill da LinkedIn.
