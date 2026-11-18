---
type: Article Summary
title: 'Apresentação: Escalando sistemas de ranking baseados em LLM para cargas de busca e recomendação críticas em latência'
description: O ranking com LLM na LinkedIn só escala quando tamanho do modelo, comprimento de contexto e eficiência de inferência são otimizados em conjunto.
resource: https://www.infoq.com/presentations/scale-llm-ranking-systems-linkedin/
tags:
- ai-ml
- llm-engineering
- kv-cache
- inference-tradeoffs
- llm-as-a-judge
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://www.infoq.com/presentations/scale-llm-ranking-systems-linkedin/
  title: 'Presentation: Scaling LLM-Based Ranking Systems for Latency-Critical Search & Recommendation Workloads'
  author: InfoQ
status: stable
source_url: https://www.infoq.com/presentations/scale-llm-ranking-systems-linkedin/
source: InfoQ
source_type: blog
published: '2026-11-17'
domain: ai-ml
subdomain: llm-engineering
concepts:
- kv-cache
- inference-tradeoffs
- llm-as-a-judge
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d81809261e766c96542bf
lang: pt-br
translation_of: /notes/ai-ml/2026-11-17-presentation-scaling-llm-based-ranking-systems-for-latency-c.md
---

## 🎯 Tese central
Um LLM usado como ranqueador só cabe em um sistema crítico em latência, com bilhões de membros, se o modelo, o contexto e o caminho de inferência forem reduzidos e ajustados em conjunto, e a menor representação que ainda atinge o padrão de qualidade costuma ser a escolha certa.

## 🧭 Por que o autor escreveu
A busca da LinkedIn está migrando de entradas por palavras-chave e filtros para intenção em linguagem natural, e um LLM consegue interpretar essa intenção muito melhor do que os rankeadores mais antigos. Rodar um LLM nessa escala, com cerca de 1,3 bilhão de membros e metas rígidas de latência P99, quebra as premissas de serviço no estilo chatbot, então a palestra relata o que a LinkedIn mudou em cada camada e quais problemas surgiram apenas em produção. Se a tese se sustentar, o custo e a velocidade do ranking com LLM são definidos pela quantidade de texto que cada candidato contribui e pela eficiência com que a pilha de serving roda, e não apenas pela capacidade do modelo.

## LinkedIn Search
O palestrante, Sundara Raman Ramachandran, é tech lead na LinkedIn com foco em inferência de LLM para ranking e recomendação, e contribuidor de código aberto do SGLang, onde trabalhou na API de scoring usada para ranking com LLM. Ele afirma que a LinkedIn tem cerca de 1,3 bilhão de membros em cerca de 200 países e atende volumes de consulta muito altos para ranking e recomendações. A busca está evoluindo de palavras-chave e filtros específicos para intenção em linguagem natural, por exemplo "encontre vagas de design na indústria do entretenimento" para vagas de emprego, ou "quem na minha rede pode me ajudar a registrar uma patente para o meu negócio" para pessoas. Esse texto bruto é difícil de interpretar com métodos clássicos, e é aí que os LLMs ajudam, mas também torna mais difícil um problema de ranking que já era complexo.

## Arquitetura
O pipeline de ponta a ponta tem três etapas: compreensão da consulta, recuperação e ranking. A palestra foca no ranking, mas as duas primeiras etapas definem o que o ranqueador recebe, então o palestrante as cobre brevemente.

### Compreensão da consulta
Esta camada transforma texto bruto e ambíguo em uma consulta estruturada e normalizada. O palestrante percorre o caso de um membro que é engenheiro de software na Bay Area, com 5 anos de experiência, buscando "encontre vagas de engenheiro de software perto da minha localização". O planejador de consulta identifica os critérios de busca (aqui, cargo e localização) e escolhe como executá-los. O marcador de consulta enriquece a consulta com dados do perfil do membro, como cargo e localização, e com quaisquer filtros que o membro já usa. O reescritor de consulta resolve textos autorreferenciados: "perto da minha localização" é substituído pela localização real do membro, Bay Area. A consulta empacotada e normalizada então segue para a recuperação.

### Recuperação
A recuperação precisa encontrar as K vagas semanticamente mais relevantes entre milhões de anúncios sem pontuar cada uma. A saída da consulta é convertida em um embedding, e uma busca vetorial KNN (k-nearest-neighbour) é executada sobre um índice construído offline a partir dos embeddings de todas as vagas. Esta etapa é ajustada para recall, e não para precisão: passar alguns candidatos a mais para a etapa seguinte é aceitável, mas descartar uma vaga altamente relevante não é.

### Ranking
O ranqueador decide a ordem de exibição dos candidatos recuperados, e a LinkedIn usa um LLM para isso. O prompt entrega ao modelo a consulta de busca, a descrição do perfil do membro e uma vaga, e pergunta se a vaga corresponde à consulta, respondendo sim ou não. A pontuação final vem da probabilidade que o modelo atribui ao token "sim" em relação a "não". Isso é calculado para cada candidato, e a lógica de negócio então decide qual resultado é mostrado primeiro. O palestrante diz que o prompt de produção é essencialmente essa única pergunta de sim ou não.

## Parte zero: ranking com LLM é um problema diferente
O trabalho com LLM generativo tem uma fase de prefill, em que o prompt é processado, seguida de um loop de decode, em que os tokens são gerados um a um por amostragem e predição do próximo token. Sua latência cresce com o número de tokens de saída. O ranking difere em três aspectos:

- Ranquear 50 itens significa montar 50 prompts, um por item, e executá-los como um único prefill em lote.
- Todos os 50 prompts compartilham um prefixo comum longo, formado pela consulta, pelo prompt de sistema e por outro contexto.
- Não há uma fase de decode relevante. O sistema precisa apenas das probabilidades finais dos tokens sim/não, e não de texto gerado.

As restrições são rígidas. O palestrante considera inaceitáveis 10 segundos antes de mostrar resultados, a meta P99 é de menos de 500 milissegundos mais adiante na palestra, e cada requisição precisa pontuar centenas de candidatos com alta concorrência. A abordagem de otimização é organizada em torno de três alavancas: o modelo, o contexto e a inferência. A regra prática do palestrante é que os tokens são a moeda, então o modelo deve ser o menor possível, o contexto o mais curto possível, e a inferência o mais eficiente possível.

### Pontuação apenas com prefill
Como o ranking precisa de um prefill grande e de algumas probabilidades de saída, a maior parte da maquinaria de geração (o loop de decode, a amostragem sobre todo o vocabulário) é trabalho desperdiçado. O restante da palestra se apoia nessa observação, e a API de scoring descrita na seção de inferência existe para eliminar esse desperdício.

## Parte um: otimizações do modelo
O objetivo é um modelo o menor possível, mantendo a qualidade.

### Destilação multi-professor
A equipe partiu de um modelo professor com cerca de 7 bilhões de parâmetros. Ele entende bem o contexto, as políticas de produto e a relevância entre consulta e item, mas nunca foi destinado à produção. Seu papel é fornecer sinais de treinamento de alta qualidade. Seu conhecimento foi destilado em dois modelos professores menores, cada um otimizado para uma métrica diferente: relevância (o quanto o resultado responde à consulta) e engajamento (a probabilidade de o usuário clicar ou ler). Um pequeno modelo de linguagem, chamado de ranqueador SLM, foi então treinado para aprender os objetivos dos dois professores. O palestrante diz que alguma perda de qualidade é esperada e aceitável para o caso de negócio. O modelo passou de um professor de 7 bilhões de parâmetros para um modelo de 0,6 bilhão de parâmetros.

### Poda estruturada
A poda remove partes do modelo, como camadas, camadas MLP ou cabeças de atenção, e então verifica se a perda de qualidade é aceitável. A poda não é gratuita, então o modelo podado precisa ser redestilado, retreinado e ajustado para recuperar a qualidade. A equipe pegou o modelo de 0,6 bilhão, removeu algumas camadas e chegou a cerca de 400 milhões de parâmetros, que consideraram pequeno o suficiente para servir em produção.

## Parte dois: otimizações de contexto
Uma descrição de vaga pode ter cerca de 2.000 tokens. Uma requisição que ranqueia 100 vagas, portanto, carrega cerca de 100 vezes 2.000, ou aproximadamente 200.000 tokens, em uma única passagem. Isso é viável com milhares de membros, mas não com 1,3 bilhão, então a equipe reduziu o contexto.

### Texto do item resumido
Um sumarizador de LLM offline comprime cada descrição de vaga de cerca de 2.000 tokens para cerca de 150 tokens. Isso é uma redução de 16 vezes no tamanho do prompt do item, com menos de 2% de perda de qualidade. Toda a sumarização roda offline. O caminho online apenas busca o resumo armazenado em um banco de dados e o passa ao ranqueador. O palestrante diz que a equipe levou a engenharia de prompt a sério, porque o tamanho do prompt é a moeda.

### Representação do item por embedding único
A equipe foi além e perguntou se um item poderia ser representado por um único embedding em vez de 150 tokens. Um LLM recebe a descrição completa e a comprime em um único vetor denso, de modo que, para fins de computação, o item custa um token em vez de 2.000. Isso também é calculado offline, e o caminho online apenas busca o resultado armazenado.

### Throughput versus qualidade
O palestrante plota a qualidade do ranking contra o throughput, em itens por segundo por GPU:

- Descrição completa vaga (linha de base): melhor qualidade de ranking, cerca de 290 itens por segundo.
- Texto resumido: cerca de 2.200 itens por segundo, próximo de 8 a 9 vezes, com menos de 2% de perda de qualidade.
- Embedding único: cerca de 22.000 itens por segundo, aproximadamente 10 vezes o throughput do texto resumido, e qualidade ligeiramente melhor do que a do texto resumido.

🔎 Contexto adicional: pelos números apresentados, 2.200 dividido por 290 é cerca de 7,6 vezes, um pouco abaixo dos 8 a 9 vezes citados pelo palestrante. O valor de 10 vezes para a etapa de embedding é consistente com 22.000 dividido por 2.200.

A principal conclusão é que a melhor representação não é a mais longa, mas a menor que atende às necessidades de qualidade do caso de uso. O palestrante recomenda tornar o modelo e o contexto enxutos antes de qualquer estimativa de hardware ou planejamento de capacidade.

## Parte três: otimizações de inferência
Motores de código aberto como SGLang, vLLM e TensorRT-LLM já entregam um throughput forte para a maioria das cargas. A equipe não presumiu que isso valesse para a sua própria carga e encontrou gargalos no trabalho da CPU, na troca de mensagens entre processos, no comportamento do runtime e na própria linguagem.

### Tokenização em lote
Cada requisição tem cerca de 50 prompts. O tokenizador os processava um a um, em um loop `for` do Python na CPU, o que travava a GPU enquanto a CPU trabalhava de forma serial. A correção foi a API de lote do tokenizador rápido do Hugging Face, que aceita um lote de prompts e paraleliza o trabalho entre os núcleos. A equipe então moveu o agrupamento para o lado do servidor, para que clientes que não conseguem agrupar não precisem repetir a lógica do lado do cliente. O agrupamento no servidor reúne requisições e executa uma única tokenização em lote.

> ❌ **Anti-padrão — loop de tokenização por item:** tokenizar um lote de prompts um a um na CPU. **Por que as pessoas fazem isso:** é a forma mais direta de escrever o loop. **Consequência:** a CPU é serial e a GPU espera por ela. **Correção:** use a API de lote do tokenizador rápido, ou faça o agrupamento no servidor para que cada cliente não precise disso.

### Manter a integridade do lote entre processos
O tokenizador roda em um processo e o scheduler, que prepara o trabalho e o despacha para a GPU, roda em outro. Os logs mostravam o lote de 50 prompts sendo dividido em lotes menores. O profiling rastreou isso até a comunicação entre processos: os prompts eram enviados uma mensagem ZMQ por vez. 🔎 Contexto adicional: ZMQ (ZeroMQ) é uma biblioteca de mensagens comumente usada para passar mensagens entre processos. A correção foi enviar o lote inteiro em uma única mensagem, para que o lote chegue intacto ao scheduler.

> ❌ **Anti-padrão — uma mensagem por item através de uma fronteira de processo:** enviar cada prompt em sua própria mensagem. **Por que as pessoas fazem isso:** é o padrão quando os itens são tratados individualmente. **Consequência:** o lote é dividido, e a GPU recebe lotes menores do que a requisição precisa. **Correção:** envie o lote em uma única mensagem e mantenha a integridade do lote em cada salto entre processos.

O ponto geral do palestrante é que sistemas em Python construídos sobre multiprocessamento precisam escolher seu protocolo de comunicação entre processos de forma deliberada.

### API de scoring e cópias assíncronas
A geração de texto copia todo o vocabulário de probabilidades da GPU para a CPU e só então amostra. O ranking precisa apenas dos logits de sim/não. A equipe introduziu uma API de scoring dedicada no motor de código aberto que pula esse trabalho e devolve os logits ao final.

Depois disso, a utilização da GPU em produção ainda era ruim. O profiling com PyTorch mostrou lacunas ociosas entre os lançamentos de kernels na GPU. Cada probabilidade de sim/não era copiada da GPU para a CPU uma requisição e um prompt por vez, em uma chamada síncrona e bloqueante, de modo que o scheduler não podia lançar o próximo kernel até que a cópia terminasse. A correção teve duas partes: uma cópia assíncrona e não bloqueante, e uma única cópia em lote em vez de uma por item. Isso permitiu que o trabalho da CPU da requisição anterior se sobrepusesse ao trabalho da GPU da próxima. O palestrante alerta que uma simples chamada `.tolist()` (ele diz "dot to list") tornou a cópia bloqueante.

> ❌ **Anti-padrão — cópia dispositivo-para-host bloqueante por item:** copiar o resultado de cada item da GPU para a CPU de forma síncrona. **Por que as pessoas fazem isso:** uma conversão simples de tensor para lista parece inofensiva. **Consequência:** o scheduler trava e a utilização da GPU cai, com lacunas ociosas entre os lançamentos de kernels. **Correção:** use uma única cópia em lote e não bloqueante, para que o trabalho da CPU e da GPU se sobreponha.

### Compartilhamento de prefixo com máscara de atenção ciente dos itens
O ranking repete a consulta em cada item, então o trabalho sobre a consulta é refeito para cada prompt. A equipe concatenou todos os itens em um único prompt: primeiro a consulta, depois o item 1, um delimitador, o item 2, e assim por diante. Isso pontua n itens em uma única passagem e calcula as entradas key-value da consulta apenas uma vez. O desafio é que o item 2 não pode ser influenciado pelo item 1, ou as pontuações ficam erradas. Por isso, a equipe adicionou uma máscara de atenção personalizada, ciente dos itens, dentro do kernel de atenção (o palestrante diz "flash" no kernel, presumivelmente um kernel no estilo FlashAttention), de modo que cada item veja a consulta e a si mesmo, mas não os outros itens.

🔎 Contexto adicional: o cache key-value (KV) guarda as chaves e valores de atenção dos tokens já processados, para que não precisem ser recalculados; por isso compartilhar as entradas KV da consulta economiza trabalho.

### Picos do coletor de lixo do Python
Depois que o modelo ficou rápido, a produção mostrava picos de latência P99 de cerca de 300 milissegundos a cada poucos segundos, contra uma meta de menos de 500 milissegundos. A equipe descartou os suspeitos habituais, como um aumento de QPS ou um prompt longo. A causa era o coletor de lixo do Python. O servidor de modelo mantém muitos objetos Python de longa duração criados no carregamento do modelo, e o coletor continuava reescaneando todos eles para descobrir o que liberar.

A correção foi iniciar o servidor, executar requisições de aquecimento e então chamar `gc.freeze`. Isso instrui o coletor a excluir todos os objetos que existiam até aquele ponto. Depois disso, apenas os objetos temporários criados no caminho da requisição são escaneados, e o P99 ficou estável.

> ❌ **Anti-padrão — deixar o coletor reescanear objetos de longa duração:** manter os objetos criados no carregamento do modelo no conjunto de varredura do coletor. **Por que as pessoas fazem isso:** é o comportamento padrão, e o custo fica invisível até que a latência seja medida na cauda. **Consequência:** picos periódicos de cerca de 300 milissegundos no P99. **Correção:** aqueça o servidor e então chame `gc.freeze` antes de atender o tráfego.

### O Global Interpreter Lock e o paralelismo em nível de processo
O Global Interpreter Lock (GIL) permite que apenas uma thread execute bytecode Python por vez. O palestrante contrasta isso com Java, C++ e outras linguagens cujas threads podem rodar em paralelo de verdade. O servidor de modelo da LinkedIn coloca o motor SGLang atrás de um processo servicer gRPC. O servicer desserializa a requisição protobuf, cuida da observabilidade (logs e métricas) e despacha o trabalho para um processo worker do SGLang. Em QPS alto, o servicer único virou o gargalo, então a equipe rodou vários processos servicer, cada um em seu próprio núcleo de CPU. O worker do SGLang então virou o gargalo, e a correção foi, de novo, múltiplos processos. Cada worker mantém sua própria cópia do modelo e seu próprio stream CUDA, e os processos dividem o tempo da GPU. Isso funcionou porque o modelo era pequeno o suficiente para que várias cópias coubessem na memória da GPU. O conselho do palestrante é que, em sistemas de produção em Python, o GIL pode ser a única correção que mais melhora o throughput.

## Parte quatro: aprendizados na escala da LinkedIn
### Faça benchmark do que você serve
A equipe otimizou o modelo, o contexto e a inferência, executou um benchmark offline, calculou quantas GPUs seriam necessárias e obteve a aprovação da liderança. Em produção, o tráfego sombra (dark traffic) entregou muito menos do que os números offline. A causa foi o próprio benchmark: o gerador de carga sintético enviava requisições em intervalos igualmente espaçados, enquanto o tráfego real chega em rajadas e pausas, o que aprofunda as filas. O substituto mais próximo do comportamento de produção é gravar e reproduzir o tráfego real. A segunda melhor opção é simular as chegadas com um processo de Poisson, em vez de espaçamento uniforme.

🔎 Contexto adicional: um processo de chegada de Poisson modela requisições que chegam de forma independente a uma taxa média fixa, de modo que aglomerados e intervalos aleatórios ocorrem naturalmente.

> ❌ **Anti-padrão — carga sintética igualmente espaçada:** conduzir um benchmark com requisições uniformemente espaçadas. **Por que as pessoas fazem isso:** é simples de gerar e produz uma taxa média limpa. **Consequência:** o throughput medido superestima o que a produção entrega sob chegadas em rajadas. **Correção:** reproduza tráfego de produção gravado, ou use chegadas com distribuição de Poisson.

### Offline precisa bater com online
Engenheiros de prompt, engenheiros de modelo e a equipe de serving trabalham cada um com seu próprio código, e o sistema de produção usa um motor de código aberto ou um servidor de modelo interno com seus próprios padrões. Pequenos padrões, como se a configuração do tokenizador adiciona tokens de início ou de fim de sequência, podem mudar o prompt inteiro. O palestrante exige paridade absoluta entre offline e online, com divergências tratadas como um portão que bloqueia qualquer lançamento.

### Nenhuma correção isolada
O throughput precisa ser extraído de todos os lugares ao mesmo tempo: um modelo menor, um contexto mais curto e uma inferência mais eficiente. Trabalhar em apenas um deles não leva o sistema para onde ele precisa estar.

### Reúna os atores
Engenheiros de inferência, engenheiros de modelo, engenheiros de prompt e gerentes de produto, que definem a política do produto, devem se reunir regularmente, por exemplo semanalmente, em um único fórum recorrente. O palestrante diz que muitos problemas são identificados ali antes da produção. Sua conclusão é que a primeira otimização é remover as barreiras de comunicação entre esses papéis.

## Perguntas e respostas
**Embedding de token único.** O palestrante usa o LLM como um cross-encoder: o texto completo passa por uma única passagem direta (forward pass), e o embedding é obtido do último token, que deve carregar o contexto de todo o texto. A outra opção que ele menciona é calcular a média dos embeddings de token. O pooling pelo último token funcionou melhor para a equipe. Dentro de um LLM, cada token já é um embedding que alimenta as multiplicações de matrizes, então comprimir 2.000 tokens em um único embedding de item é consistente com a forma como o modelo funciona internamente.

**Poda estruturada.** O método foi tentativa e erro em um ambiente de pesquisa. A equipe pegou o ranqueador aluno de 0,6 bilhão, mascarou certas camadas em experimentos no estilo SFT, começando pelas camadas MLP logo antes da última camada, e mediu a qualidade. A qualidade foi recuperada por meio de pós-treinamento ou de treinamento adicional, com ajuste fino baseado em aprendizado por reforço como opção adicional.

**Qualidade do produto.** A equipe otimiza primeiro para relevância (a pessoa exata buscada deve aparecer em primeiro lugar) e engajamento (cliques e leituras de vagas). Throughput e custo só são tratados depois que os ganhos de produto são confirmados. O ranqueador anterior era um pequeno MLP que calculava algo como uma similaridade de cosseno. Era mais rápido, mas dava respostas piores. O palestrante relata um aumento de 5% a 10% nas métricas de relevância.

🔎 Contexto adicional: a transcrição diz "auction metrics", que muito provavelmente é um erro de transcrição de AUC (área sob a curva ROC). O número do palestrante se refere às métricas de relevância de qualquer forma.

**Segurança e privacidade.** Anúncios de vagas são públicos e não contêm dados pessoais, então não precisam de mascaramento. Para a busca de pessoas, a equipe mascara qualquer informação que não seja já pública no perfil do membro, por exemplo uma data de nascimento, antes que o texto chegue ao modelo, de modo que apenas informações publicamente disponíveis sejam entrada.

**Arquitetura de serving.** A busca de pessoas e a busca de vagas foram lançadas primeiro para usuários premium, e o palestrante espera que a liberação gradual para todos os usuários ocorra até o fim daquele ano. O servidor de modelo é uma única aplicação Python construída em torno de um servidor gRPC em Python que incorpora o SGLang. Não é um conjunto de serviços separados. Esse servidor é o bloco de ranking na arquitetura: ele recebe a consulta e a lista de candidatos e devolve uma pontuação para cada candidato.

## 🧠 Modelos mentais
### Token é a moeda
**O modelo:** cada token no modelo ou no prompt custa computação e latência, então o problema de design é reduzir o modelo, o contexto e o caminho de inferência em conjunto.
**Como aplicar:** antes de planejar o hardware, pergunte quanto cada candidato contribui em tokens, e se isso pode ser calculado offline e armazenado.

### Ranking é pontuação apenas com prefill
**O modelo:** uma requisição de ranking é um lote de prompts que precisam de um prefill e de algumas probabilidades de saída, sem loop de decode.
**Como aplicar:** quando uma pilha de serving orientada à geração é reaproveitada para scoring, remova as etapas de geração de que a carga não precisa, como as cópias de vocabulário completo e a amostragem.

### A menor representação que atinge o padrão
**O modelo:** a melhor representação é a menor que atende à qualidade exigida pelo caso de uso, e não a mais completa.
**Como aplicar:** teste texto completo, resumos e embeddings únicos com a mesma medida de qualidade, e plote qualidade contra throughput antes de escolher.

### Faça benchmark com tráfego de formato produtivo
**O modelo:** um benchmark só é tão bom quanto o seu padrão de chegadas. Carga igualmente espaçada esconde as rajadas que criam filas.
**Como aplicar:** primeiro reproduza tráfego gravado, e use um processo de chegada de Poisson se isso não for possível.

### Portão de paridade
**O modelo:** o pipeline offline e o sistema de serving online precisam produzir prompts idênticos, incluindo os padrões do tokenizador.
**Como aplicar:** torne a paridade offline-online um requisito de lançamento, e teste-a com o tokenizador de produção e os padrões de serving.

## 🔑 Principais conclusões
- O ranking com LLM é pontuação apenas com prefill: 50 candidatos viram 50 prompts em um único lote, e a saída é uma probabilidade de sim/não, então as premissas de serving orientadas à geração não se sustentam.
- Resumir cada vaga de cerca de 2.000 tokens para cerca de 150 tokens custou menos de 2% de qualidade e elevou o throughput de cerca de 290 para cerca de 2.200 itens por segundo por GPU.
- Substituir o texto do item por um único embedding elevou o throughput para cerca de 22.000 itens por segundo por GPU, com qualidade ligeiramente melhor do que a dos resumos.
- Sistemas de serving em Python atingem gargalos em tokenização por item, lotes divididos entre processos, cópias bloqueantes da GPU para a CPU, coleta de lixo e o GIL, e cada um tem uma correção específica.
- Picos de P99 de cerca de 300 milissegundos causados pela coleta de lixo, contra uma meta de P99 de menos de 500 milissegundos, foram eliminados aquecendo o servidor e então chamando `gc.freeze`.
- Benchmarks offline com tráfego sintético igualmente espaçado superestimaram a capacidade, então a equipe recomenda reproduzir tráfego de produção ou usar chegadas de Poisson.
- As configurações offline e online precisam coincidir exatamente, incluindo os padrões do tokenizador, e qualquer divergência deve bloquear o lançamento.

## 🏛️ Para o Kenji
Quando o caminho da requisição inclui um modelo, o custo é aproximadamente o tamanho da entrada multiplicado pelo volume de requisições, então a forma como o contexto é preparado (resumos e embeddings calculados offline) é uma decisão de arquitetura tanto quanto de ML. As lições de benchmark e de paridade se aplicam a qualquer serviço crítico em latência, não apenas ao serving de LLM. Interpretação, não do autor: uma revisão de governança ou de risco que roda um LLM sobre muitos registros poderia enfrentar o mesmo trade-off. A questão que ela levanta é qual medida de qualidade você aceitaria antes de substituir o texto completo por um resumo ou embedding, e quem aprova essa decisão.

## 📚 Plano de leitura
### A partir do artigo
- [apresentações com transcrições](https://www.infoq.com/transcripts/presentations/) — o índice do InfoQ vinculado a partir da transcrição, com outras palestras no mesmo formato.

### Para ir além
- Posts e artigos de engenharia da LinkedIn sobre ranking baseado em LLM — o palestrante diz que os resultados são descritos ali; os títulos não foram citados na palestra.
- A API de scoring do SGLang e seu código aberto — o mecanismo de serving por trás do caminho de scoring descrito aqui.
- O `gc.freeze` do Python e o comportamento do coletor de lixo — a correção para os picos de latência na cauda.
- Processos de chegada de Poisson para testes de carga — a alternativa ao tráfego sintético igualmente espaçado.
- Destilação de conhecimento multi-professor e poda estruturada — os dois métodos usados para reduzir o modelo.
