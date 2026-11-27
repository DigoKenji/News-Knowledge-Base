---
type: Article Summary
title: 'EP234: Docker x Kubernetes'
description: O Docker empacota aplicações e o Kubernetes as orquestra em várias máquinas; o artigo também explica os mecanismos internos do Redis, trade-offs entre modelos, marca d'água em texto e nove padrões de sistemas distribuídos.
resource: https://blog.bytebytego.com/p/ep234-docker-vs-kubernetes
tags:
- cloud-infra
- virtualization-containers
- container-orchestration
- consistency-models
- replication-partitioning
- consensus
- message-queue
- saga-pattern
- distributed-system
- fine-tuning
- inference-tradeoffs
- large-language-model
- circuit-breaker
- text-watermarking
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-27T06:47:00-03:00'
sources:
- resource: https://blog.bytebytego.com/p/ep234-docker-vs-kubernetes
  title: 'EP234: Docker vs Kubernetes'
  author: ByteByteGo
status: stable
source_url: https://blog.bytebytego.com/p/ep234-docker-vs-kubernetes
source: ByteByteGo
source_type: blog
published: '2026-11-21'
domain: cloud-infra
concepts:
- virtualization-containers
- container-orchestration
- consistency-models
- replication-partitioning
- consensus
- message-queue
- saga-pattern
- distributed-system
- fine-tuning
- inference-tradeoffs
- large-language-model
- circuit-breaker
- text-watermarking
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/425baf74172d81fb9796e6d110bf3f08
lang: pt-br
translation_of: /notes/cloud-infra/2026-11-27-ep234-docker-vs-kubernetes.md
---

## 🎯 Tese central
O Docker e o Kubernetes atuam em camadas diferentes: o Docker empacota uma aplicação em um contêiner portátil e a executa, normalmente em uma única máquina, enquanto o Kubernetes gerencia muitos contêineres em muitas máquinas.

## 🧭 Por que o autor escreveu
O artigo é uma edição curta de newsletter que explica, uma seção breve por vez, como funcionam vários sistemas comuns de infraestrutura e de IA. O título enquadra a comparação entre Docker e Kubernetes, que costuma ser mencionada na mesma frase, então a primeira pergunta é onde uma ferramenta termina e a outra começa. Se a tese estiver certa, escolher entre elas vira uma questão de escopo: um único host pede Docker e Compose, enquanto necessidades de produção como escalonamento e autocorreção entre máquinas pedem Kubernetes.

## Docker x Kubernetes

### Imagem de contêiner e contêiner (Docker)
O Docker é uma ferramenta para empacotar uma aplicação, incluindo seu código, bibliotecas, ambientes de execução e configuração, em uma unidade portátil chamada imagem de contêiner. Um contêiner é iniciado a partir dessa imagem e pode rodar em um notebook, em um servidor ou na nuvem. A imagem é o artefato empacotado e o contêiner é a instância em execução criada a partir dela. O objetivo de empacotar tudo junto é a portabilidade: a mesma unidade pode ser implantada em cada um desses lugares.

> 🔎 Contexto adicionado: Um contêiner compartilha o kernel do sistema operacional do host, em vez de inicializar um sistema operacional completo próprio, e essa é a principal diferença em relação a uma máquina virtual. Trata-se de contexto geral, não de uma afirmação do artigo.

### Docker Compose
O Docker normalmente roda em uma única máquina. O artigo cita o Docker Compose como a forma de gerenciar vários contêineres nesse único host. Essa é a fronteira do escopo do Docker: o Compose coordena contêineres em um host, não uma frota de máquinas.

### Kubernetes
O Kubernetes é uma plataforma de orquestração que gerencia a implantação e a manutenção de aplicações em contêineres, especialmente entre várias máquinas. O artigo lista o que ele faz: inicia e para contêineres, cria mais contêineres ou reduz o número deles conforme a necessidade, distribui o tráfego de rede e aplica atualizações da aplicação. Segundo o artigo, o Kubernetes é útil quando uma aplicação em produção precisa de escalonamento automático, autocorreção, alta disponibilidade e implantação em múltiplas máquinas. Uma aplicação pequena em uma única máquina pode não precisar desses recursos.

## Como o Redis funciona por dentro
O ponto de partida do artigo é que o Redis é rápido porque mantém tudo na RAM e usa estruturas de dados eficientes em memória. Em seguida, o texto acompanha o que acontece com um único comando.

### Dados mantidos na RAM
O Redis guarda seus dados em memória, e cada etapa seguinte opera sobre esse estado em memória. A velocidade vem de evitar o disco no caminho normal de leitura e escrita. O artigo dá isso como o motivo da velocidade do Redis e não a quantifica.

### Envio do comando e decodificação (RESP)
A aplicação envia um comando do Redis ao servidor. Se a replicação estiver configurada, os comandos que alteram dados normalmente são enviados ao servidor primário. O Redis decodifica a requisição, em geral codificada no Redis Serialization Protocol (RESP), e a separa em nome do comando, chave ou chaves, valores e demais argumentos.

### Análise, validação e autorização
O Redis analisa e valida a requisição. Ele verifica se o comando existe e se os argumentos estão formatados corretamente, e verifica se o cliente está autenticado e autorizado. Se a requisição for inválida, um erro é retornado e nada é executado.

### Busca na tabela de comandos e execução
O Redis procura o comando em sua tabela interna de comandos, que aponta para o código que o executa. Os comandos são executados sequencialmente, um após o outro.

### Atualização das chaves na memória e expiração
O Redis localiza as chaves relevantes em seus dicionários em memória e executa a operação solicitada. Se uma chave tiver um tempo de vida (TTL), o Redis guarda separadamente o timestamp de expiração, e as chaves expiradas são removidas por expiração preguiçosa (lazy) ou ativa.

> 🔎 Contexto adicionado: A expiração preguiçosa verifica a chave quando ela é acessada, enquanto a expiração ativa remove chaves expiradas em segundo plano. Ambas são comportamentos padrão do Redis, não afirmações detalhadas do artigo.

### Persistência e replicação
Os comandos que alteram dados na RAM podem precisar propagar essas escritas para sistemas de apoio. O artigo descreve o AOF como um mecanismo de persistência baseado em registro (log) das escritas, e o RDB como um mecanismo de persistência baseado em snapshots. Ele também menciona a replicação, que copia as escritas para outros servidores Redis.

> 🔎 Contexto adicionado: No Redis, o RDB é um formato de arquivo de snapshot, e a replicação é um mecanismo separado que transmite as escritas para servidores réplica. A frase do artigo combina os dois de forma imprecisa.

### Política de remoção (eviction)
Quando o Redis atinge o limite de memória configurado, ele aplica sua política de eviction. O artigo não nomeia as políticas.

> 🔎 Contexto adicionado: Uma política de eviction decide quais chaves remover para liberar memória quando o limite é atingido, por exemplo, as chaves menos usadas recentemente.

### Resposta
Por fim, o Redis monta a resposta no formato RESP e a devolve ao cliente.

## Modelos abertos x modelos fechados: 7 trade-offs em 1 diagrama
Para todo projeto de IA existem duas opções: baixar os pesos de um modelo aberto e executá-lo você mesmo, ou chamar a API de um fornecedor. O diagrama do artigo não está no texto extraído, então os sete trade-offs abaixo vêm das descrições escritas.

### Fine-tuning
Pesos abertos permitem fazer fine-tuning nas suas próprias GPUs e com os seus dados. Modelos fechados não dão acesso aos pesos, então o usuário não consegue ajustar diretamente os pesos do modelo.

> 🔎 Contexto adicionado: Fine-tuning significa treinar novamente um modelo pré-treinado com um conjunto de dados menor e específico para uma tarefa.

### Versionamento
Com pesos abertos, você mantém o artefato. Pode fixar qualquer versão e executá-la pelo tempo que quiser. Modelos fechados servem snapshots datados e fixados, mas o fornecedor os aposenta no próprio cronograma. A diferença prática está em quem controla por quanto tempo uma versão continua disponível.

### Transparência
Pesos abertos são inspecionáveis. Um modelo fechado é uma caixa-preta com uma API.

### Implantação
Pesos abertos podem ser implantados em diversos tipos de hardware. Com modelos fechados, todos os dispositivos se conectam a um único endpoint.

### Licenciamento
As licenças de pesos abertos são personalizadas, e seus termos variam de modelo para modelo. Modelos fechados rodam sob termos de serviço comerciais definidos pelo fornecedor.

### Self-hosting
Pesos abertos são self-hosted por padrão, então a stack é sua. Modelos fechados são API-first, embora alguns fornecedores vendam implantações on-premises ou em VPC privada.

### Custo
Self-hosting só compensa em alto volume, depois de somar o custo operacional. APIs são mais baratas em baixo volume e não têm custo inicial. O artigo não traz um ponto de equilíbrio (break-even).

## Como funciona a marca d'água de texto do Claude?
O autor diz que a Anthropic compartilhou recentemente a intenção de aplicar marca d'água em textos, para que o texto gerado por IA possa ser identificado. O autor afirma que o post se baseia na própria compreensão de como isso funciona, então esta seção não é uma descrição oficial.

### Geração palavra por palavra
Os LLMs produzem texto palavra por palavra. A cada passo, geram probabilidades para a próxima palavra provável. Normalmente, um gerador de números aleatórios escolhe um dos bons candidatos.

> 🔎 Contexto adicionado: Na prática, os modelos trabalham com tokens, que são pedaços de subpalavras, então a palavra usada pelo artigo é um termo informal. A etapa de amostragem funciona da mesma forma com tokens.

### Marca d'água por restrição da escolha
Em vez de amostrar livremente entre os candidatos, uma função com chave recebe uma chave secreta mais as últimas poucas palavras e decide quais candidatos são válidos para escolha. Isso se repete em toda a resposta. Os pontos em que existem várias escolhas plausíveis são onde o sinal da marca d'água fica registrado.

### Detecção pela taxa de correspondência
Para detectar uma marca d'água, o verificador pega cada palavra candidata do texto e pergunta se ela é uma escolha válida, dada a chave secreta e as poucas palavras anteriores. Uma palavra válida conta como correspondência. No texto inteiro, o texto com marca d'água corresponde com muito mais frequência, então a taxa geral de correspondência pode servir como uma pontuação de geração por IA. Portanto, a detecção depende da mesma chave secreta.

O autor acrescenta uma visão pessoal: ele se frustra com falsos negativos das técnicas de detecção de texto por IA, especialmente em textos técnicos. Ele pergunta se a detecção é útil ou se vai criar mais problemas.

## 9 padrões de sistemas distribuídos que você deveria conhecer
O artigo dá uma definição curta para cada um dos nove padrões. A lista está completa: nove itens, um por padrão.

### Replicação
Replicação cria cópias exatas dos seus dados e as armazena em servidores diferentes. O artigo dá apenas essa definição.

> 🔎 Contexto adicionado: Réplicas costumam ser mantidas por disponibilidade e para escalar leituras. O padrão de quórum abaixo depende de haver várias réplicas.

### Sharding
Sharding divide horizontalmente um banco de dados grande, de modo que linhas diferentes fiquem em máquinas separadas. Ao contrário da replicação, que copia os mesmos dados, o sharding divide os dados entre as máquinas.

### Consistent hashing
Consistent hashing distribui dados entre máquinas ou servidores usando um anel circular virtual.

> 🔎 Contexto adicionado: Cada chave recebe um hash que a posiciona em um ponto do anel, e é atribuída a um servidor pela posição no anel. Assim, adicionar ou remover um servidor move apenas parte das chaves.

### PubSub
PubSub é um padrão de mensageria assíncrona que desacopla os criadores de serviços dos seus consumidores. O criador publica e não precisa saber quem consome.

### Circuit breaker
Um circuit breaker impede que uma aplicação execute repetidamente uma operação que provavelmente vai falhar.

> ❌ **Anti-padrão — chamadas repetidas a uma operação que está falhando (implícito no artigo):** a aplicação continua executando uma operação que provavelmente vai falhar. **Por que as pessoas fazem isso:** o artigo não diz. **Consequência:** o artigo sugere que a aplicação continua atingindo uma operação com falha. **Correção:** parar de chamá-la enquanto as falhas persistirem, que é o que o circuit breaker faz.

### Retry com backoff
Retry com backoff lida com falhas momentâneas de rede ou timeouts breves de serviço. É um padrão de resiliência.

> 🔎 Contexto adicionado: Backoff significa esperar progressivamente mais tempo entre as tentativas, para que as novas tentativas não se acumulem todas de uma vez sobre um serviço já em dificuldade.

### Eleição de líder
A eleição de líder designa um único nó mestre para gerenciar ações e manter o estado do cluster. O artigo diz que isso impede que dois nós achem que estão no comando.

> 🔎 Contexto adicionado: Quando dois nós acreditam ser o líder, diz-se que o cluster tem split-brain.

### Leitura/escrita com quórum
Leitura/escrita com quórum garante que réplicas suficientes concordem em cada leitura e em cada escrita, de modo que os conjuntos se sobreponham. O artigo diz que isso é usado em bancos de dados distribuídos para garantir informações atualizadas.

> 🔎 Contexto adicionado: Se o número de réplicas numa escrita somado ao número numa leitura exceder o total de réplicas, todo conjunto de leitura compartilha pelo menos uma réplica com todo conjunto de escrita. A garantia real depende de como cada banco de dados configura esses números.

### Saga
Uma saga gerencia uma transação distribuída por meio de uma sequência de transações locais em múltiplos microservices. Se uma etapa falhar, cada etapa anterior é desfeita por sua própria ação compensatória.

## 🧠 Modelos mentais
### Imagem versus contêiner
**O modelo:** A imagem é o artefato empacotado e portátil. Um contêiner é uma instância em execução criada a partir dela.
**Como aplicar:** Trate as imagens como aquilo que você versiona e entrega, e os contêineres como aquilo que você inicia, para e substitui.

### Sobreposição de quórum
**O modelo:** Leituras e escritas tocam réplicas suficientes para que seus conjuntos necessariamente se sobreponham.
**Como aplicar:** Ao escolher o número de réplicas, verifique se os tamanhos de leitura e escrita garantem sobreposição, e não apenas se parecem grandes.

### Artefato fixado versus snapshot do fornecedor
**O modelo:** Com pesos abertos, você guarda o artefato e decide sua duração. Com modelos fechados, você depende do cronograma de aposentadoria do fornecedor.
**Como aplicar:** Ao escolher um modelo, pergunte quem controla por quanto tempo esta versão exata continua disponível.

## 🔑 Principais conclusões
- O Docker empacota uma aplicação e suas dependências em uma imagem de contêiner que roda em um notebook, em um servidor ou na nuvem.
- O Docker normalmente roda em uma máquina, e o Docker Compose gerencia vários contêineres nesse host.
- O Kubernetes é voltado às necessidades de produção entre máquinas: escalonamento automático, autocorreção, alta disponibilidade e atualizações contínuas (rolling updates).
- O Redis é rápido porque mantém os dados na RAM, e cada comando é validado, procurado em uma tabela de comandos e executado sequencialmente.
- Self-hosting de pesos abertos só compensa em alto volume, depois de contabilizado o custo operacional. APIs são mais baratas em baixo volume, sem custo inicial.
- O artigo descreve a marca d'água como uma escolha de palavras válidas definida por uma chave secreta, detectada por uma taxa de correspondência ao longo do texto.
- Os nove padrões resolvem problemas diferentes: replicação e sharding distribuem dados, circuit breaker e retry lidam com falhas, eleição de líder e quórum coordenam o acordo, e saga desfaz transações parciais.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. A comparação entre Docker e Kubernetes funciona como uma fronteira de decisão: quando um serviço precisa de autocorreção e implantação em várias máquinas, o custo da camada de orquestração fica mais fácil de justificar. A comparação entre modelos abertos e fechados se aplica às cargas de trabalho de IA na sua arquitetura, onde a pergunta é em que volume o custo operacional do self-hosting deixa de compensar. Uma segunda pergunta é quem seria o dono da versão do modelo se um fornecedor a aposentasse.

## 📚 Plano de leitura
### Do artigo
- Nenhum. O texto extraído do artigo não contém links.

### Para aprofundar
- Opções de persistência do Redis (snapshots RDB e logs AOF): para ver os trade-offs por trás da etapa de persistência.
- Autocorreção e escalonamento no Kubernetes: para ver como os recursos de orquestração listados pelo artigo são implementados.
- Consistent hashing em anel: para entender como as chaves se movem quando servidores são adicionados ou removidos.
- Leituras e escritas com quórum em bancos replicados: para verificar a condição de sobreposição com as configurações de um banco específico.

## Conceitos
- [virtualization-containers](/pt-br/concepts/cloud-infra/virtualization-containers.md)
- [container-orchestration](/pt-br/concepts/cloud-infra/container-orchestration.md)
- [consistency-models](/pt-br/concepts/backend-systems/consistency-models.md)
- [replication-partitioning](/pt-br/concepts/backend-systems/replication-partitioning.md)
- [consensus](/pt-br/concepts/backend-systems/consensus.md)
- [message-queue](/pt-br/concepts/backend-systems/message-queue.md)
- [saga-pattern](/pt-br/concepts/backend-systems/saga-pattern.md)
- [distributed-system](/pt-br/concepts/backend-systems/distributed-system.md)
- [fine-tuning](/pt-br/concepts/ai-ml/fine-tuning.md)
- [inference-tradeoffs](/pt-br/concepts/ai-ml/inference-tradeoffs.md)
- [large-language-model](/pt-br/concepts/ai-ml/large-language-model.md)
- [circuit-breaker](/pt-br/concepts/backend-systems/circuit-breaker.md)
- [text-watermarking](/pt-br/concepts/security/text-watermarking.md)
