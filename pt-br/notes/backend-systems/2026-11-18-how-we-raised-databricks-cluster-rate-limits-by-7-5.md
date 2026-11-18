---
type: Article Summary
title: Como aumentamos os limites de taxa dos clusters do Databricks em 7,5×
description: Limites fixos de taxa de requisições não protegem trabalhos de longa duração; controle quantas operações rodam conforme a saúde do sistema muda.
resource: https://www.databricks.com/blog/how-we-raised-databricks-cluster-rate-limits-75
tags:
- backend-systems
- system-design
- retry-storm
- adaptive-concurrency-limit
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-18T07:30:00-03:00'
sources:
- resource: https://www.databricks.com/blog/how-we-raised-databricks-cluster-rate-limits-75
  title: How we raised Databricks cluster rate limits by 7.5×
  author: Databricks
status: stable
source_url: https://www.databricks.com/blog/how-we-raised-databricks-cluster-rate-limits-75
source: Databricks
source_type: blog
published: '2026-11-18'
domain: backend-systems
subdomain: system-design
concepts:
- retry-storm
- adaptive-concurrency-limit
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d8196bb0dc80812916152
lang: pt-br
translation_of: /notes/backend-systems/2026-11-18-how-we-raised-databricks-cluster-rate-limits-by-7-5.md
---

## 🎯 Tese central
Para trabalhos de longa duração, um limite fixo de taxa de requisições não acompanha uma capacidade que muda com o tempo. Por isso, o serviço deve decidir quanto trabalho iniciar com base no número de operações ativas e na saúde de suas dependências, mantendo uma taxa fixa apenas como uma barreira externa de proteção.

## 🧭 Por que o autor escreveu o texto
O Databricks protegia a criação de clusters com limites fixos de taxa de requisições, difíceis de ajustar: eles rejeitavam clientes em períodos saudáveis e continuavam aceitando trabalho durante lentidões, então os engenheiros os ajustavam manualmente. O autor defende que contar o trabalho ativo e adaptar-se à saúde do sistema permite ao serviço elevar seus padrões com segurança, e o post relata os resultados da implantação.

## Introdução: uma cozinha com uma regra fixa
O artigo abre com um resumo. Limites de concorrência adaptativos acompanham os fluxos de criação de clusters de longa duração e ajustam quanto trabalho roda conforme a saúde do sistema muda. Filas limitadas e escalonamento justo absorvem picos e dividem a capacidade, enquanto um teto fixo de taxa de requisições da API permanece como barreira voltada ao cliente. No escopo relatado, o Databricks elevou os padrões de taxa de requisições para criação de clusters em 7,5× e removeu mais de cem substituições específicas de clientes.

Os autores trabalham na equipe de Compute Platform, que transforma infraestrutura de nuvem em clusters prontos para as cargas de trabalho dos clientes. Isso significa provisionar máquinas virtuais e coordenar a criação de clusters na AWS, Azure e Google Cloud. O objetivo deles é manter esses sistemas disponíveis e, ao mesmo tempo, permitir que os clientes usem o máximo de capacidade que for seguro oferecer.

> 🔎 Contexto adicionado: a seção de abertura não tem título no texto original, então recebe aqui um título baseado no seu conteúdo.

### Limite fixo de taxa de requisições
Historicamente, a plataforma era protegida por limites fixos de taxa de requisições, testados com diferentes padrões de carga e cenários de falha. Escalar significava adicionar recursos e ajustar os limites por cliente. O artigo diz que limites conservadores rejeitavam requisições mesmo com capacidade ociosa, mas continuavam aceitando requisições quando serviços ou dependências, como os provedores de nuvem, ficavam lentos.

> ❌ **Anti-padrão — Reescrever limites manualmente durante incidentes:** Reduzir os limites à mão sempre que for preciso aliviar a carga, e depois aumentá-los de novo após a recuperação. **Por que as pessoas fazem isso:** Reduzir um limite é uma forma direta de aliviar a carga durante um incidente. **Consequência:** Cada incidente exige que uma pessoa defina o número duas vezes, e o número fica sempre desatualizado para o próximo padrão de carga. **Correção:** Deixe o serviço ajustar a admissão com base no trabalho ativo e em sinais de saúde, para que ninguém precise reescrever o limite.

### Analogia da cozinha: concorrência como refeições em preparo
O artigo compara a plataforma de computação a uma cozinha que busca atender o máximo de pedidos que seus cozinheiros e equipamentos conseguirem. Um gerente fixa uma regra: aceitar no máximo dez novos pedidos por minuto, aconteça o que acontecer na chapa. Mesmo com capacidade de cozinha sobrando, a regra recusa pedidos. Quando um forno quebra, a cozinha continua aceitando pedidos mais rápido do que os cozinheiros e equipamentos restantes conseguem concluir, e o gerente precisa reescrever o cartaz o tempo todo.

Uma cozinha melhor mantém uma fila curta e limita quantas refeições são preparadas ao mesmo tempo. Se um forno quebra, menos refeições começam e, conforme a capacidade volta, mais podem começar. Os pedidos podem esperar por pouco tempo, mas só enquanto ainda houver tempo para servi-los. A conclusão é que, para operações de longa duração, uma taxa fixa de requisições é um modelo ruim de capacidade. O objetivo é deixar os clientes operarem a pleno vapor quando a capacidade permitir e, depois, reduzir o ritmo o suficiente para manter o trabalho útil andando durante a degradação.

### Fila curta como amortecedor
Na analogia, uma fila curta guarda os pedidos que ainda não podem começar. A fila só é útil enquanto cada pedido ainda puder ser atendido a tempo. As seções seguintes tornam isso preciso, com limites para o tamanho da fila e para o tempo de espera.

## Contexto
Uma chamada de API pode disparar muito mais trabalho do que o próprio handler executa. Na criação de clusters, o handler valida a requisição, registra um item de trabalho e retorna. Em seguida, um fluxo de trabalho em segundo plano coordena serviços, atualiza o estado e provisiona recursos, como máquinas virtuais na nuvem. Essas operações são concluídas de forma assíncrona, e sua duração depende da saúde e da capacidade das dependências. A chamada de API pode levar milissegundos, enquanto o fluxo de trabalho em si pode levar minutos.

Um limitador de taxa de requisições no ponto de entrada da API limita as requisições por janela de tempo. O artigo observa que isso é útil para aplicar cotas, prevenir abusos e proteger os recursos usados para tratar chamadas de API.

### Fluxo de trabalho assíncrono
O handler e o fluxo de trabalho são separados. O handler termina rapidamente ao registrar um item de trabalho, e o trabalho real continua em segundo plano por minutos. Um limite colocado apenas no handler, portanto, enxerga a parte barata da operação e não a parte cara.

### Limitador de taxa de requisições
Um limitador de taxa de requisições limita quantas requisições são aceitas em uma janela de tempo. É uma boa ferramenta para cotas e proteção contra abusos, mas o artigo argumenta que é um modelo ruim de capacidade para trabalho que sobrevive à requisição.

## O problema
Um limite fixo de taxa de requisições é mais fácil de ajustar quando a capacidade disponível e o trabalho por trás de cada requisição permanecem razoavelmente previsíveis. A criação de clusters dificulta as duas coisas. Uma requisição pode provisionar algumas VMs e outra, muitas mais, e a saúde do provedor de nuvem muda o tempo que o provisionamento leva. Durante uma lentidão compartilhada, muitos fluxos de trabalho demoram mais ao mesmo tempo, então a mesma taxa de chegada deixa muito mais trabalho em andamento, enquanto a capacidade do sistema de concluí-lo cai.

A figura do artigo deixa isso concreto. À esquerda, as chegadas se mantêm fixas em uma operação por unidade de tempo. Aumentar a duração de duas para seis unidades de tempo triplica o trabalho em andamento. À direita, os processadores ficam lentos ou indisponíveis, o que reduz a capacidade disponível para tratar esse trabalho.

Quando uma dependência se degrada, mais chamadas falham ou expiram. Fluxos de trabalho de longa duração repetem essas chamadas para se recuperar de falhas temporárias. Somadas em muitos fluxos ativos, essas retentativas aumentam a pressão sobre a dependência em dificuldade, o que causa mais falhas e, por sua vez, mais retentativas.

Essas retentativas internas vêm de trabalho que já foi aceito, então o limitador de taxa da API nunca as vê. Elas podem manter a sobrecarga mesmo depois que o tráfego de entrada volta ao normal. Limitar os fluxos ativos ajuda a conter essa pressão e dá ao trabalho inacabado a chance de escoar.

Nenhuma das direções de ajuste resolve o descompasso. Uma taxa menor cria folga ao rejeitar mais trabalho em períodos saudáveis, e uma lentidão grande o suficiente ainda pode esgotar essa folga. Uma taxa maior aproveita melhor a capacidade saudável, mas aceita mais trabalho durante a degradação. Como diz o artigo, um único número precisa cobrir os dois tipos de dia.

> ❌ **Anti-padrão — Uma taxa fixa para períodos saudáveis e degradados:** Escolher um único número de requisições por janela e tratá-lo como a capacidade do serviço. **Por que as pessoas fazem isso:** O artigo observa que uma taxa fixa é mais fácil de ajustar quando a capacidade e o trabalho por requisição são previsíveis. **Consequência:** Um número baixo rejeita trabalho em períodos saudáveis, e um número alto aceita demais durante uma lentidão. **Correção:** Limite o número de fluxos de trabalho ativos, que pode acompanhar a capacidade conforme ela muda, em vez de ajustar um único número para os dois cenários.

### Trabalho em andamento
Trabalho em andamento é a quantidade de trabalho aceito que ainda não terminou. Ele cresce com a taxa de chegada multiplicada pelo tempo que cada operação leva, então uma lentidão pode multiplicá-lo mesmo quando as chegadas são constantes. A triplicação da figura, de duas para seis unidades de tempo, mostra esse efeito. O trabalho em andamento é o que um limite de concorrência controla, e por isso é um sinal melhor do que as chegadas para trabalho de longa duração.

### Tempestade de retentativas
Uma tempestade de retentativas é um ciclo em que uma dependência degradada causa chamadas falhas, as falhas disparam retentativas em muitos fluxos ativos, e essas retentativas somam carga que causa mais falhas. As retentativas vêm de trabalho já aceito, então um limite de admissão na API não as enxerga. O remédio proposto pelo artigo é limitar os fluxos de trabalho ativos para que o trabalho inacabado possa escoar.

## Terminologia
O artigo diz que "limitação de taxa" às vezes é usada de forma ampla, incluindo limites sobre trabalho concorrente. Ele distingue os dois mecanismos pelo que cada um conta.

### Operação
Uma operação é o fluxo de trabalho completo, incluindo o trabalho em segundo plano que continua depois que a API retorna.

### Limite de taxa de requisições
Um limite de taxa de requisições restringe as requisições aceitas por unidade de tempo, como 10 requisições por segundo, com uma possível margem para rajadas.

### Concorrência
Concorrência é quantas operações estão ativas ao mesmo tempo, incluindo as que aguardam dependências.

### Limite de concorrência
Um limite de concorrência restringe as operações ativas, como 100 de cada vez. Cada operação ocupa um slot até terminar.

### Backlog
Backlog é o trabalho aceito que espera para começar. Ele ainda não obteve um slot ativo.

O artigo observa que qualquer um dos dois tipos de limite pode ser fixo ou adaptativo. Sua comparação é entre limites fixos de taxa de requisições e limites de concorrência adaptativos para proteger trabalhos de longa duração.

## Solução
A cozinha precisa acompanhar as refeições ainda em preparo e a capacidade disponível para concluí-las. No Databricks classic compute, o equivalente é o conjunto de fluxos de criação de clusters que ainda estão provisionando a infraestrutura. A abordagem é dar a cada fluxo de trabalho ativo um slot e ajustar o número de slots conforme as condições mudam.

### Slot
Um slot é a unidade de admissão de um fluxo de provisionamento ativo. Considere uma requisição CreateCluster para um cluster com um driver e vários nós de trabalho em VMs. Depois que a requisição é aceita, seu fluxo de provisionamento aguarda um slot antes de começar. A plataforma acompanha se esse trabalho está na fila, em processamento ativo ou concluído.

Se todos os slots estiverem ocupados, a requisição fica na fila. Um fluxo ativo mantém seu slot enquanto provisiona VMs e aguarda dependências. Quando o fluxo termina, seu slot fica disponível para outro lançamento de cluster. Um cluster lançado com sucesso pode continuar executando cargas de trabalho dos clientes por horas sem ocupar um slot de provisionamento.

O slot precisa cobrir a operação de provisionamento inteira, incluindo o trabalho em segundo plano depois que a API retorna. Acompanhar um slot não exige manter uma thread. O controlador conta os fluxos de provisionamento ativos. Clusters em execução, VMs alocadas e vCPUs são medidas separadas. Um fluxo pode provisionar muitas VMs, então um slot não implica um custo fixo de recursos.

### Limite de concorrência adaptativo
Um limite de concorrência adaptativo é um teto sobre as operações ativas que muda conforme as condições. Sinais úteis para a criação de clusters incluem quanto tempo o provisionamento leva e se os pools de threads críticos estão saturados. Quando esses sinais indicam congestionamento, o controlador reduz o número de fluxos de provisionamento simultâneos. Conforme as condições se recuperam, ele permite mais, com cautela.

> 🔎 Contexto adicionado: RPC (remote procedure call) é uma requisição de um serviço interno para outro, então "rejeições por limite de taxa de RPC" significa requisições recusadas na fronteira entre serviços.

### Adaptar-se à capacidade disponível
Agora suponha que um provedor de nuvem leve mais tempo para provisionar VMs. Os fluxos ativos mantêm seus slots por mais tempo, então novos lançamentos de cluster começam com menos frequência, mesmo com o limite de slots fixo, e os clientes veem esperas maiores no lançamento conforme as requisições aceitas entram na fila. O controle adaptativo também altera o próprio limite de slots.

O artigo faz uma separação clara: a pressão de capacidade muda quando o trabalho aceito começa, enquanto as rejeições por limite de taxa de RPC ficam reservadas para taxas de requisições de entrada excepcionalmente altas. Reduzir o limite impede que novas operações comecem até que o trabalho ativo caia abaixo dele, mas as operações existentes ainda precisam de tempo para terminar. Esse tempo de resposta importa quando os fluxos duram minutos.

As retentativas também precisam de backoff e de um limite de tentativas, para que os fluxos ativos não sobrecarreguem continuamente uma dependência em recuperação. A conclusão do artigo é que a limitação de concorrência não consegue reparar uma dependência indisponível, mas pode limitar a pressão extra enquanto essa dependência se recupera.

### Retentativas com backoff e limite de tentativas
Backoff espaça as retentativas, e um limite de tentativas restringe quantas vezes um fluxo tenta novamente. Juntos, eles impedem que fluxos ativos adicionem carga a uma dependência que está tentando se recuperar. Retentativas são úteis para falhas temporárias, e é por isso que o artigo as mantém, mas as limita em vez de removê-las.

> ❌ **Anti-padrão — Tentar novamente sem backoff nem limite de tentativas:** Fluxos de longa duração repetem chamadas falhas a dependências imediatamente e sem limite. **Por que as pessoas fazem isso:** Retentativas são como os fluxos se recuperam de falhas temporárias, como o artigo observa. **Consequência:** Somadas em muitos fluxos ativos, as retentativas mantêm uma dependência em recuperação sob pressão, o que sustenta a sobrecarga. **Correção:** Adicione backoff e um limite de tentativas, e combine-os com um limite de concorrência.

## O diabo está nos detalhes
O laço de controle responde quanto trabalho iniciar. Construir um serviço útil também significa decidir quem espera, por quanto tempo espera e o que o cliente é informado.

### Explicar por que uma requisição está sendo limitada
"Requisições demais" pode esconder duas situações muito diferentes. Na primeira, um cliente envia seu tráfego habitual enquanto uma dependência compartilhada fica mais lenta. A capacidade caiu, e pedir ao cliente que corrija o padrão de tráfego atribui erradamente um problema do serviço ao cliente. Na segunda, um cliente envia uma rajada incomumente grande enquanto o serviço continua saudável. Outros clientes disputam os mesmos recursos, e limitar essa rajada protege a parcela justa deles.

Para distinguir esses casos, a plataforma acompanha separadamente a saúde do sistema, a demanda de entrada de cada tenant e a decisão de escalonamento. Um grande número de operações em andamento, por si só, não prova que um cliente enviou demais, porque essas operações podem estar presas atrás de uma dependência lenta. Uma rajada não precisa ser limitada por justiça se houver capacidade ociosa e a política permitir.

A distinção pertence às métricas e às explicações dadas aos clientes. Ela diz aos operadores se devem investigar uma capacidade perdida ou uma carga de trabalho incomum, e diz aos clientes se mudar seu comportamento vai ajudar. Falhas causadas pela degradação do serviço devem ser contadas nas métricas de confiabilidade do serviço, mesmo quando aparecem como limitação. Para requisições aceitas que esperam na fila, o impacto é medido na latência de lançamento do cluster.

> ❌ **Anti-padrão — Culpar o cliente por limitações causadas por degradação:** Devolver uma resposta de "requisições demais" a um cliente cujo tráfego habitual está sendo retido por uma dependência compartilhada lenta. **Por que as pessoas fazem isso:** Um grande número de operações em andamento parece um cliente enviando demais. **Consequência:** O cliente muda um comportamento que não vai ajudar, e o problema real do serviço fica escondido. **Correção:** Acompanhe separadamente a saúde do sistema, a demanda por tenant e a decisão de escalonamento, e explique a limitação pela sua causa.

### Enfileirar enquanto a requisição ainda tem tempo de dar certo
Para uma operação que leva minutos, uma espera curta costuma ser preferível a um erro imediato seguido de uma retentativa. Uma fila pode absorver uma rajada e dar tempo ao sistema para se recuperar. Isso só funciona se a espera ainda couber no prazo da requisição.

A orientação do artigo é limitar o tamanho da fila e o tempo de espera de cada requisição, tornar o progresso visível e rejeitar ou expirar o trabalho quando ele não puder mais terminar de forma útil. Uma fila não transforma sobrecarga sustentada em capacidade. Uma fila sem limite pode transformar uma interrupção breve em horas de atraso.

> ❌ **Anti-padrão — Fila sem limite:** Aceitar trabalho em uma fila sem limite de tamanho nem de tempo de espera. **Por que as pessoas fazem isso:** Esperar evita o erro imediato que, de outra forma, dispararia uma retentativa. **Consequência:** Uma interrupção breve vira horas de atraso, porque o backlog nunca expira. **Correção:** Limite o tamanho da fila e o tempo de espera de cada requisição, torne o progresso visível e expire o trabalho que não pode mais terminar a tempo.

### Manter a fila e a limpeza baratas
Uma requisição na fila deve ser um registro pequeno, e não um worker em execução ocupando recursos escassos. Um backlog maior não deve tornar cada operação ativa mais cara por causa de varreduras repetidas ou da disputa por threads e conexões.

A limpeza merece a mesma atenção. Se um backlog grande expirar de uma vez, o serviço precisa descartá-lo dentro de um prazo previsível. A recomendação do artigo é planejar para o pior backlog, manter a expiração e a rejeição baratas e evitar preparação cara para trabalho cujo prazo já passou. Caso contrário, a fila pode manter o serviço ocupado muito depois de a congestão original ter terminado.

### Registro de requisição na fila
Uma requisição na fila é um registro pequeno que aguarda um slot. Mantê-lo pequeno significa que o backlog custa pouco para ser mantido, então um backlog grande não desacelera as operações ativas. Esta é a propriedade de fila barata que o artigo exige.

### Decidir o que "justo" significa para o produto
Suponha que um serviço precise atrasar cem requisições. É melhor que cem clientes vejam cada um uma requisição lenta, ou que um único cliente veja cem requisições lentas? A métrica agregada pode parecer a mesma, mas a experiência do cliente não será.

Um escalonador faz essa escolha, esteja ela especificada ou não. A recomendação do artigo é definir qual trabalho deve ir primeiro, qual parcela cada tenant deve receber sob disputa e como o trabalho de menor prioridade evita esperar para sempre. Os tenants podem usar capacidade ociosa quando a política permitir, enquanto o acesso justo é preservado quando outros tenants precisam da sua parcela.

### Política de escalonamento e parcela justa
A política de escalonamento decide a ordem em que o trabalho na fila começa. A parcela justa define a porção de capacidade de cada tenant sob disputa. Ambas precisam ser definidas explicitamente, ou o escalonador aplica uma política implícita que ninguém escolheu.

## Estado atual e trabalhos futuros
As ideias foram aplicadas a um sistema integrado de gestão de carga para a criação de clusters no Databricks classic compute. Uma requisição curta à API inicia um fluxo de trabalho muito mais longo, que coordena várias dependências, incluindo provedores de nuvem pública, para provisionar recursos de computação. O sistema combina requisições na fila, limites de concorrência adaptativos e escalonamento justo. Ele usa o trabalho já em andamento e a saúde do sistema para decidir quando as operações enfileiradas podem começar.

A equipe manteve o limite de taxa de requisições da API como última linha de defesa contra volumes de tráfego excepcionalmente altos. Ele oferece uma fronteira de taxa que pode ser comunicada aos clientes. Os limites de concorrência adaptativos ficam dentro do serviço e governam quando o trabalho aceito começa. O artigo é explícito ao dizer que uma resposta de sucesso da API não garante um lançamento imediato do cluster nem sua conclusão em um tempo fixo.

### Gestão de carga integrada
O sistema integrado é a combinação descrita pelo artigo: fila, limite adaptativo e escalonador justo, operando a partir do trabalho em andamento e da saúde do sistema. A fronteira da API fica fora desse sistema e não o controla.

### Resultados da implantação
A análise inicial da implantação relatou três resultados. Os limites padrão de taxa de requisições para criação de clusters ficaram 7,5× maiores. Houve mais de 99,99% menos erros de limite de taxa na criação de clusters no escopo relatado. Mais de cem substituições de limite de taxa específicas de clientes no escopo foram removidas.

O artigo credita ao controle de concorrência adaptativo a possibilidade do aumento de 7,5×, porque ele protege os fluxos de longa duração conforme as condições mudam. Desde o aumento, poucos clientes atingiram os limites mais altos de taxa para criação de clusters, e nenhuma nova substituição manual foi adicionada para esses limites. Os clientes têm mais espaço para escalar, com menos necessidade de exceções ajustadas individualmente.

### Questões de design em aberto
O artigo aponta duas direções para trabalhos futuros. Quando as operações têm custos de recurso muito diferentes, contar cada uma como um slot pode ser grosseiro demais, e slots ponderados ou limites separados são extensões possíveis. Para fluxos de trabalho com vários gargalos, a questão é onde aplicar o controle para que uma dependência lenta não segure trabalho não relacionado.

Durante uma lentidão do provedor de nuvem, fluxos que já estão provisionando VMs podem continuar repetindo chamadas mesmo depois que os novos lançamentos ficam mais lentos. A recomendação do artigo é combinar limites de concorrência com backoff nas retentativas e um limite de tentativas, para reduzir essa pressão durante a recuperação.

### Slots ponderados
Slots ponderados dariam às operações caras mais de um slot, para que uma única requisição grande de cluster conte pelo seu custo real. O artigo apresenta isso como uma extensão possível, e não como algo que a implantação fez.

## Perguntas frequentes
### Qual é a diferença entre limites de taxa de requisições e de concorrência?
Um limite de taxa de requisições restringe quantas requisições são aceitas por unidade de tempo, enquanto um limite de concorrência restringe quantas operações estão ativas ao mesmo tempo. Cada operação ativa ocupa um slot até terminar, incluindo o tempo gasto aguardando dependências. Os dois limites protegem coisas diferentes, por isso o artigo os trata como mecanismos separados.

### Por que um slot de concorrência precisa cobrir o fluxo inteiro de criação de cluster?
Um fluxo CreateCluster mantém seu slot enquanto provisiona VMs, incluindo o tempo de espera por dependências depois que a API retorna. Quando o provisionamento termina, o slot é liberado, mesmo que o cluster possa continuar executando cargas de trabalho dos clientes. O controlador conta os fluxos de provisionamento ativos, não todos os clusters em execução nem as VMs e vCPUs alocadas, e acompanhar um slot não exige manter uma thread.

### Como os limites de concorrência adaptativos reagem à degradação do sistema?
Quando um provedor de nuvem fica mais lento, os fluxos de provisionamento mantêm seus slots por mais tempo, e as requisições aceitas esperam mais para ser lançadas. O controlador também reduz o limite de slots quando sinais de congestionamento, como a latência de provisionamento ou a saturação do pool de threads, indicam pressão. Conforme as condições se recuperam, ele permite mais trabalho concorrente, com cautela.

### Por que o Databricks ainda mantém um limite fixo de taxa de requisições na API?
O limite fixo de taxa de requisições protege contra volumes de tráfego de entrada excepcionalmente altos e oferece uma fronteira que pode ser comunicada aos clientes. Os limites de concorrência adaptativos funcionam dentro do serviço, então a pressão de capacidade enfileira as requisições aceitas e atrasa o provisionamento. Uma resposta de sucesso da API ainda não garante um lançamento ou uma conclusão imediata do cluster em um tempo fixo.

### Quais resultados o Databricks relatou após adotar limites de concorrência adaptativos?
A análise inicial da implantação relatou limites padrão de taxa de requisições para criação de clusters 7,5× maiores, mais de 99,99% menos erros de limite de taxa na criação de clusters no escopo relatado, e a remoção de mais de cem substituições de limite de taxa específicas de clientes no escopo. Desde o aumento, poucos clientes atingiram os limites mais altos, e nenhuma nova substituição manual foi adicionada para esses limites. O artigo diz que o tema complementar das FAQs é a aplicação de cotas de API em escala.

## 🧠 Modelos mentais
### Cozinha com fila curta
**O modelo:** A capacidade são os cozinheiros e os equipamentos, então a cozinha admite pedidos apenas na velocidade em que podem ser concluídos e enfileira o restante por pouco tempo. **Como aplicar:** Quando uma dependência fica mais lenta, admita menos itens novos em vez de reescrever uma regra fixa.

### Trabalho em andamento, não taxa de chegada
**O modelo:** Com uma taxa de chegada fixa, o trabalho no sistema cresce conforme a duração de cada operação; triplicar a duração de duas para seis unidades de tempo triplica o trabalho em andamento. **Como aplicar:** Meça as operações ativas e a duração delas junto com a taxa de requisições, e não só a taxa de requisições.

### Um slot cobre a operação inteira
**O modelo:** Um slot é mantido até o fim do provisionamento em segundo plano, e não apenas pelos poucos milissegundos do handler. **Como aplicar:** Defina o tempo de vida de um slot para que corresponda ao trabalho que ele protege, e não considere a parte da frente, barata, como a operação inteira.

## 🔑 Principais conclusões
- Um limite fixo de taxa de requisições não consegue acompanhar a capacidade de trabalhos de longa duração, porque um único número precisa cobrir períodos saudáveis e degradados.
- Contar operações ativas com slots controla quanto trabalho começa, e cada slot é mantido durante o provisionamento em segundo plano.
- Retentativas de trabalho já aceito contornam o limitador da API, por isso limitar os fluxos ativos é o que contém uma tempestade de retentativas.
- Limites adaptativos diminuem diante de sinais de congestionamento, como a latência de provisionamento e a saturação do pool de threads, e voltam a crescer com cautela conforme as condições se recuperam.
- Filas limitadas, com prazos por requisição e expiração barata, absorvem uma interrupção breve sem criar horas de atraso.
- A limitação deve ser explicada pela sua causa: a limitação causada por degradação é um problema do serviço, não mau comportamento do cliente.
- O limite fixo da API permanece como barreira final, e a implantação relatada mostrou padrões 7,5× maiores com mais de 99,99% menos erros de limite de taxa no escopo.

## 🏛️ Para o Kenji
Esta é a sua interpretação, não a do autor. Para um Staff Architect, a ideia transferível é a separação entre controle de admissão na fronteira que você promete aos clientes e controle interno sobre quanto trabalho aceito roda ao mesmo tempo. A pergunta que ela levanta: qual dos seus serviços tem um limite fixo que foi ajustado em dias saudáveis, e o que esse limite faz durante uma queda de dependência?

## 📚 Plano de leitura
### A partir do artigo
- High Performance Ratelimiting at Databricks (sem URL no texto extraído) — a peça complementar que o artigo cita para aplicar cotas de API em escala, o problema complementar a este.

### Para aprofundar
- Retentativas com backoff exponencial e jitter — como espaçar as retentativas para que não se sincronizem em uma tempestade de retentativas.
- Limitação de concorrência adaptativa com controladores do tipo AIMD — a família de regras de feedback que aumentam o limite devagar e o cortam diante de congestionamento.
- Lei de Little — a relação entre taxa de chegada, tempo no sistema e trabalho em andamento, que é a aritmética por trás da figura do artigo.
- Descarte de carga com filas limitadas — como rejeitar ou expirar trabalho antes que ele desperdice capacidade escassa.

## Conceitos
[Tempestade de retentativas](/pt-br/concepts/retry-storm.md) · [Limite de concorrência adaptativo](/pt-br/concepts/adaptive-concurrency-limit.md)
