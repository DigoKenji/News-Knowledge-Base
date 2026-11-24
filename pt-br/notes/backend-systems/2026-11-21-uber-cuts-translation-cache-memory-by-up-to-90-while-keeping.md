---
type: Article Summary
title: Uber Reduz a Memória do Cache de Tradução em até 90% Mantendo a Latência P99 Abaixo de 1 MS
description: A Uber reduziu em mais de 90% a memória inicial do seu cache de tradução em Go, mantendo chaves e valores acessados com frequência na RAM e transferindo o restante para o disco.
resource: https://www.infoq.com/news/2026/11/uber-go-translation-cache
tags:
- backend-systems
- hybrid-memory-disk-cache
- gc-pressure
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-21T07:00:00-03:00'
sources:
- resource: https://www.infoq.com/news/2026/11/uber-go-translation-cache
  title: Uber Cuts Translation Cache Memory by up to 90% While Keeping P99 Latency Below 1 MS
  author: Leela Kumili
status: stable
source_url: https://www.infoq.com/news/2026/11/uber-go-translation-cache
source: InfoQ
source_type: blog
published: '2026-11-20'
domain: backend-systems
concepts:
- hybrid-memory-disk-cache
- gc-pressure
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41fbaf74172d818d9407eb1b0a1f01ac
lang: pt-br
translation_of: /notes/backend-systems/2026-11-21-uber-cuts-translation-cache-memory-by-up-to-90-while-keeping.md
---

## 🎯 Tese central
A Uber cortou em mais de 90% a pegada inicial de memória do seu cache de tradução em Go, mantendo na memória as chaves de tradução e os valores acessados com frequência e armazenando os demais valores em disco, enquanto manteve a latência p99 de cache miss abaixo de 1 ms.

## 🧭 Por que o autor escreveu
Os serviços em Go da Uber gastavam uma grande parcela da CPU com coleta de lixo, e datasets de tradução com milhões de chaves podiam levar alguns serviços à falta de memória durante os refreshes do cache. Se a tese se sustentar, um cache híbrido de memória e disco pode manter a API Get() e Put() existente e reduzir a memória na escala da Uber.

## Abertura: o problema e o redesenho
A Uber redesenhou seu cache de tradução baseado em Go. O artigo informa que o sistema atende milhões de traduções por segundo com taxa de acerto acima de 99.9%. O autor cita a Uber Engineering: "Na escala da Uber, até pequenas ineficiências no gerenciamento de memória se somam."

## Perfilamento: para onde foram a memória e a CPU
A análise de memória dos serviços em Go da Uber encontrou dois problemas. A coleta de lixo consumia mais de 20% da utilização de CPU em alguns serviços, e as traduções respondiam por cerca de 70% dos objetos alocados.

### Sobrecarga da coleta de lixo (GC)
A coleta de lixo é o trabalho do runtime que encontra e libera memória do heap que não está mais em uso. Seu custo cresce com o número de objetos que o heap contém e com os ponteiros entre eles, então um cache construído com milhões de objetos pequenos gasta CPU na coleta em vez de atender consultas. Os números do artigo mostram o efeito: as traduções eram cerca de 70% dos objetos alocados, e o GC consumia mais de 20% da CPU em alguns serviços. A lição é que a quantidade de objetos dirige o custo, não apenas o número de bytes.
> 🔎 Contexto adicional: O artigo não descreve os detalhes internos do coletor do Go; a descrição acima é o comportamento geral do runtime do Go.

> ❌ **Antipadrão — vários objetos por entrada:** cada entrada de tradução era construída a partir de vários objetos e ponteiros. **Por que as pessoas fazem isso:** O artigo não diz; grafos de objetos por registro são um padrão comum. **Consequência:** A sobrecarga do GC cresce com o número de entradas. **Correção:** Armazenar as entradas como valores simples em um map plano indexado por string.

## Primeira correção: de sync.Map para um map simples
### sync.Map versus map com RWMutex
O sync.Map do Go é um tipo de map seguro para concorrência. A carga de trabalho era intensiva em leitura, com atualizações pouco frequentes e sem exclusões individuais. A Uber o substituiu por um map[string]string convencional protegido por um RWMutex, que permite que muitos leitores prossigam juntos enquanto um escritor tem acesso exclusivo. Isso reduziu as alocações de objetos e cortou a CPU em mais de 30% no serviço afetado. O artigo não traz um benchmark do próprio sync.Map; o ganho informado é a mudança de CPU após a troca.

## Picos de refresh e opções rejeitadas
Os datasets de tradução podiam ter de dois a três milhões de chaves, mas cada serviço normalmente lia apenas um pequeno subconjunto. Os refreshes eram o próximo problema. O serviço construía um cache substituto em segundo plano e depois o trocava atomicamente, então as duas cópias ficavam na memória ao mesmo tempo, e alguns serviços ficavam sem memória durante os refreshes.

### Refresh por construir e depois trocar
O padrão constrói uma estrutura substituta completa e depois direciona os leitores para ela em uma única etapa. A troca é atômica, mas durante a construção as cópias antiga e nova ficam residentes. O artigo relata o resultado diretamente: alguns serviços ficaram sem memória no refresh.

> ❌ **Antipadrão — cópia sombra completa no refresh:** todo o cache substituto é construído antes da troca. **Por que as pessoas fazem isso:** O artigo descreve a troca como atômica, mas não dá o motivo. **Consequência:** O pico de memória fica aproximadamente o dobro durante o refresh, e alguns serviços ficaram sem memória. **Correção:** Manter apenas as chaves na memória e paginar os valores a partir do disco, como fez a Uber.

Uber também considerou um cache externo e a geração de subconjuntos de tradução específicos por serviço. Ambos aumentavam custos operacionais ou de manutenção, então a equipe os rejeitou.

## Design híbrido: chaves na memória, valores no disco
A equipe adotou um design de armazenamento híbrido que mantém a API Get() e Put() existente. Todas as chaves ficam na memória para buscas rápidas, e os valores de tradução vivem em arquivos apoiados em disco. O artigo inclui um diagrama do cache, que não está no texto extraído.

### Armazenamento híbrido em memória e disco
As buscas nunca acessam o disco pela chave, então um miss é o único caminho que lê um valor do disco. Como o índice de chaves é pequeno em comparação com os valores, o uso de memória continua baixo mesmo quando o dataset cresce. Valores acessados com frequência são mantidos na memória por uma política LRU. A troca é uma leitura ocasional de disco para um valor frio em troca de um heap muito menor. O artigo informa que os misses apoiados em disco mantêm a latência p99 abaixo de 100 microssegundos.

### Sharding
O cache usa 256 shards, cada um com seu próprio mutex de leitura e escrita, suas próprias entradas em memória e seu próprio arquivo em disco. Um lock é, portanto, compartilhado apenas dentro de um shard, não em todo o cache. O artigo não informa nenhuma medição de contenção.

### Chaves hash de seis bytes e eviction LRU
Chaves hash de seis bytes reduzem a sobrecarga de metadados, e o sistema volta ao formato de chave original para lidar com colisões. Uma política LRU global baseada em tempo gerencia as entradas em memória.
> 🔎 Contexto adicional: O LRU despeja as entradas que ficaram mais tempo sem acesso, para que o conjunto quente permaneça na RAM.

### Pré-carga de aquecimento após o refresh
Durante um refresh, o sistema usa o histórico de acessos para pré-carregar as traduções acessadas nos 15 minutos anteriores. Isso evita um cache frio logo após a troca.

## Resultados informados
Em um exemplo, um dataset de tradução que usava cerca de 800 MB de memória precisou de cerca de 40 MB para metadados após o redesenho. O limite configurado era de 50.000 entradas na memória, e o uso real de memória tipicamente ficava entre 80 MB e 120 MB. Em outra carga de trabalho, cerca de 30.000 de mais de quatro milhões de entradas estavam na memória, e a taxa de acerto se manteve acima de 99.9%. O artigo também informa que a memória caiu de cerca de 2.7 GB para 750 MB. Isso é um corte de cerca de 72%, abaixo do destaque de 90%, e o artigo não diz qual carga de trabalho ele mede nem como os 90% são calculados.

## Lições e recomendações
A equipe recomenda minimizar referências entre objetos para reduzir a sobrecarga do GC, e medir com que frequência os valores em cache são acessados. Quando apenas uma pequena parcela dos dados em cache está ativa, uma abordagem híbrida mantém os valores quentes na memória e move o restante para o disco.

## 🧠 Modelos mentais
### Chaves na memória, valores no disco
**O modelo:** Os dados de índice são pequenos e necessários em cada busca, enquanto os dados de valor são grandes e em sua maioria frios, então só os valores quentes precisam de RAM.
**Como aplicar:** Meça que fração dos valores em cache é realmente lida. Se for pequena, mantenha o índice na memória e leia os valores do disco.

### Menos objetos, menos GC
**O modelo:** O custo do GC acompanha o número de objetos e referências do heap, não apenas os bytes armazenados.
**Como aplicar:** Armazene as entradas como valores simples em maps planos, e conte as alocações em um perfil antes de ajustar qualquer outra coisa.

### Pequenas ineficiências em escala
**O modelo:** Um custo pequeno demais para ser notado em um processo se torna grande quando multiplicado por uma frota, como sugere a citação da Uber Engineering.
**Como aplicar:** Perfile primeiro o caminho quente nos serviços com a maior pegada.

## 🔑 Principais conclusões
- A perfilagem mostrou o GC acima de 20% da CPU em alguns serviços, com as traduções em cerca de 70% dos objetos alocados.
- Trocar sync.Map por map[string]string sob um RWMutex cortou a CPU em mais de 30% no serviço afetado.
- Os refreshes de construir-e-trocar mantinham as duas cópias na memória, e alguns serviços ficaram sem memória durante os refreshes.
- O design híbrido mantém todas as chaves e os valores acessados com frequência na memória, em 256 shards, com o restante em arquivos apoiados em disco.
- Um dataset caiu de cerca de 800 MB para cerca de 40 MB de metadados, com memória real de 80 a 120 MB com limite de 50.000 entradas.
- Os refreshes pré-carregam as traduções acessadas nos 15 minutos anteriores, o que evita um cache frio após a troca.
- Uma carga de trabalho informada caiu de cerca de 2.7 GB para 750 MB, um corte de cerca de 72% em vez do destaque de 90%.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para a arquitetura de sistemas, layout de memória e custo de GC são decisões de capacidade, então pertencem à revisão de design junto com a escolha de armazenamento. O design híbrido também troca uma leitura ocasional de disco em um miss frio por menor uso de memória, o que é uma decisão sobre o orçamento de latência. A pergunta que ele levanta: quais dos nossos datasets com muitas leituras têm um conjunto quente pequeno que justificaria chaves na memória e valores no disco?

## 📚 Plano de leitura
### Do artigo
- Post do blog da Uber (citado duas vezes como fonte do design do cache e dos números de memória) — o texto original, com o diagrama e os detalhes. O texto extraído não tem URL para ele, então nenhuma é dada aqui.
### Para aprofundar
- sync.Map do Go e seus trade-offs em relação a um map simples com mutex — para entender quando a primeira correção se aplica.
- Pacing do coletor de lixo do Go e custo de alocação — para entender como a quantidade de objetos dirige a CPU gasta com GC.
- Políticas de eviction LRU, baseadas em tempo versus baseadas em tamanho — para escolher a política das entradas em memória.
- Aquecimento de cache após trocas atômicas — para evitar caches frios no refresh.
- Caches híbridos de memória e disco — designs relacionados que mantêm um índice na RAM.

## Conceitos
- [hybrid-memory-disk-cache](/pt-br/concepts/backend-systems/hybrid-memory-disk-cache.md)
- [gc-pressure](/pt-br/concepts/backend-systems/gc-pressure.md)
