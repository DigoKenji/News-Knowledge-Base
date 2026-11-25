---
type: Article Summary
title: 'DoorDash Dispatch: Construindo um sistema de entrega ultrarrápido'
description: A DoorDash reconstruiu o dispatch separando a coordenação da computação pesada e decompondo o problema de otimização, e depois fez a implantação com segurança por meio de teste em sombra e switchbacks.
resource: https://blog.bytebytego.com/p/doordash-dispatch-building-a-lightning
tags:
- backend-systems
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-25T06:47:00-03:00'
sources:
- resource: https://blog.bytebytego.com/p/doordash-dispatch-building-a-lightning
  title: 'DoorDash Dispatch: Building a Lightning-Fast Delivery System'
  author: ByteByteGo
status: stable
source_url: https://blog.bytebytego.com/p/doordash-dispatch-building-a-lightning
source: ByteByteGo
source_type: blog
published: '2026-11-23'
domain: backend-systems
concepts:
- architectural-style
- quality-attributes
- scalability
- distributed-system
- grpc
- control-plane
- latency-throughput
- availability-reliability
- fault-tolerance
- canary-deploy
- training-vs-inference
- reinforcement-learning
- shadow-testing
selected_by: theme
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/423baf74172d8135be36cd9c83fd46e5
lang: pt-br
translation_of: /notes/backend-systems/2026-11-25-doordash-dispatch-building-a-lightning-fast-delivery-system.md
---

## 🎯 Tese central
A DoorDash só conseguiu tomar decisões de dispatch boas e rápidas ao separar a coordenação da computação pesada e ao decompor o problema de otimização em partições. Ela substituiu o motor antigo com segurança, validando primeiro o novo em modo sombra e depois em experimentos switchback com tráfego de produção.

## 🧭 Por que o autor escreveu o texto
O artigo parte de um motor de dispatch monolítico cujo trabalho computacional crescia mais rápido que o volume de entregas. Assim, no pico de demanda, ele precisava cortar caminhos, e suas decisões chegavam tarde demais para serem úteis. O texto explica o redesenho que dividiu o trabalho em camadas e peças menores, e os testes em etapas que permitiram trocar um sistema tão crítico sem quebrar entregas em andamento. Se a tese estiver certa, sistemas de decisão sensíveis à latência precisam de uma mudança arquitetural, porque adicionar hardware sozinho não resolve problemas de otimização acoplados.

## Introdução
> 🔎 Contexto adicional: O artigo abre sem um título. O texto abaixo é a seção inicial, seguida do roteiro que o autor apresenta.

O artigo começa dizendo que a plataforma de dispatch da DoorDash precisa tomar muitas boas decisões relacionadas a entregas enquanto a situação ao redor continua mudando. Para acompanhar a necessidade de inovação, a equipe de engenharia da DoorDash teve de redesenhar o sistema de dispatch. O redesenho tratou de dois problemas principais. O primeiro era como dividir os cálculos em partes menores. O segundo era como executar essas partes em muitas máquinas sem perder as informações necessárias para boas decisões de atribuição.

O roteiro do autor cobre sete pontos:

- O papel de um sistema de dispatch
- Por que o antigo sistema de dispatch da DoorDash tinha dificuldades
- Por que a DoorDash não podia simplesmente adicionar mais servidores
- Separar a coordenação do cálculo pesado
- Quebrar o problema de otimização em partes menores
- Como a DoorDash substituiu com segurança um sistema tão crítico
- As melhorias depois do novo sistema

O artigo inclui um aviso de que se baseia em detalhes públicos compartilhados de várias fontes e convida os leitores a apontar imprecisões. Considere os números abaixo como o resumo do autor sobre informações públicas, não como dados internos da DoorDash.

## O que um sistema de dispatch realmente faz
Quando alguém pede comida, várias coisas precisam acontecer. O restaurante prepara a comida, um entregador vai até o restaurante para buscá-la e depois a leva até o cliente. A DoorDash chama seus entregadores de Dashers. O sistema de dispatch coordena tudo isso decidindo qual Dasher deve atender qual pedido.

Tomar essa decisão exige mais do que saber qual Dasher está mais perto do restaurante. O artigo dá um exemplo: um Dasher a dois minutos de distância pode ter de esperar pelo restaurante, que ainda precisa de 15 minutos para preparar a comida. Enviar esse Dasher imediatamente cria uma longa espera. Um Dasher bem mais distante pode chegar perto do momento em que a comida fica pronta, o que é a escolha melhor. Dois pedidos que seguem em direções parecidas também podem ser combinados em uma única viagem de entrega.

### O dispatch precisa raciocinar sobre espaço e tempo juntos
O espaço abrange a localização dos Dashers, dos restaurantes e dos clientes. O tempo abrange durações de viagem, trânsito, preparo da comida e expectativas de entrega. O ponto do autor é que uma decisão de dispatch precisa levar os dois em conta ao mesmo tempo, já que um Dasher próximo ainda pode ser a escolha errada se o tempo não bater.

### O problema de roteamento de veículos como analogia
O autor compara o dispatch ao problema de roteamento de veículos, que pergunta como veículos devem atender um conjunto de destinos respeitando restrições. A versão da DoorDash é mais difícil porque suas entradas mudam o tempo todo. Pedidos novos chegam constantemente, os Dashers continuam se movendo e as estimativas de preparo da comida mudam enquanto o sistema ainda está calculando.

> 🔎 Contexto adicional: O problema de roteamento de veículos é um problema de otimização bem conhecido sobre planejar rotas para uma frota de veículos que atende um conjunto de destinos sob restrições. O artigo o usa como analogia e não descreve a variante exata que a DoorDash resolve.

### Qualidade da decisão e velocidade da decisão
O autor argumenta que uma decisão de dispatch precisa ser boa o suficiente e também rápida o suficiente para ser útil. Uma atribuição perfeitamente calculada não tem valor se o cálculo demorar demais. O artigo apresenta esse trade-off entre qualidade e velocidade da decisão como ponto de partida de todo o redesenho.

> ❌ **Anti-padrão — Despachar para o Dasher mais próximo:** Escolher o Dasher disponível mais próximo sem considerar quando a comida ficará pronta. **Por que as pessoas fazem isso:** A distância é o sinal mais simples de calcular e de explicar. **Consequência:** O Dasher espera no restaurante, o que cria longos tempos de espera e desperdiça o tempo do entregador. **Correção:** Avaliar espaço e tempo juntos, incluindo o tempo de preparo e a chance de combinar pedidos. *(O artigo apresenta isso como uma abordagem falha por meio do seu exemplo, não como um anti-padrão nomeado.)*

## Por que o sistema antigo teve dificuldades com o crescimento da DoorDash?
O motor de dispatch anterior da DoorDash era um monólito, ou seja, várias responsabilidades principais viviam dentro de um único sistema fortemente acoplado. Ele mantinha o estado da rede de entregas, aplicava regras de negócio e executava cálculos de roteamento caros, tudo no mesmo lugar.

### Monólito e o estado da rede
O autor define estado como informação sobre a situação atual, como os Dashers disponíveis e os pedidos aguardando atribuição. Em um monólito, esse estado, a lógica de negócio e os cálculos pesados ficam emaranhados, de modo que uma mudança em uma área afeta todas as outras. O artigo apresenta esse acoplamento como a raiz das dificuldades.

### Geração de candidatos e otimização
O motor antigo executava seu trabalho em etapas. Ele gerava possíveis atribuições, verificava as rotas propostas e depois otimizava as escolhas. O autor define geração de candidatos como a produção das possibilidades que o sistema pode avaliar. Otimização significa selecionar uma combinação boa o suficiente dessas possibilidades, de acordo com os objetivos e as restrições do sistema.

### Crescimento quadrático no cálculo
A dificuldade era que o trabalho computacional crescia muito mais rápido que o volume de entregas. Os algoritmos importantes do motor original tinham complexidade aproximadamente quadrática, de modo que dobrar o tamanho da entrada podia produzir cerca de quatro vezes mais trabalho computacional. A pressão vinha dos possíveis pares Dasher-pedido: se tanto o número de Dashers quanto o de pedidos crescem, o número de pares possíveis cresce rapidamente. Avaliar rotas e lotes acrescentava trabalho adicional por cima disso.

### Pico de demanda e decisões degradadas
Nos períodos de pico, a demanda podia chegar a quase quatro vezes a capacidade de processamento de dispatch do sistema. O motor então precisava limitar o que considerava ou recorrer a regras de decisão mais simples. Os atrasos também reduziam a utilidade de seus cálculos: um Dasher podia deixar uma coleta promissora escapar, ou a comida já pronta podia ficar esperando enquanto o motor ainda decidia. A conclusão do autor é que o problema ia além de software lento, porque a lentidão da computação afetava diretamente a qualidade das entregas reais e a experiência do usuário.

## Por que a DoorDash não podia simplesmente adicionar mais servidores?
O artigo descreve adicionar servidores como escalabilidade horizontal. Ela funciona bem quando o trabalho pode ser dividido em requisições independentes, como muitas requisições para páginas de produtos não relacionados atendidas por servidores diferentes, em que nenhum servidor precisa coordenar sua resposta com os outros.

As atribuições de dispatch têm mais dependências. Considere dois pedidos de restaurantes próximos que poderiam compartilhar uma viagem de entrega. Se forem enviados aleatoriamente a trabalhadores separados, cada um pode encontrar uma atribuição individual razoável, mas nenhum consegue considerar os dois pedidos juntos. Dois trabalhadores também poderiam, de forma independente, escolher o mesmo Dasher como melhor opção.

Há também um problema de sistema. Mesmo que os cálculos sejam distribuídos, algum componente precisa preparar o trabalho, enviar os dados, reunir os resultados e combiná-los. Se esse componente ficar sobrecarregado, o gargalo apenas terá se deslocado para outro lugar.

Por isso, a DoorDash precisava responder a duas perguntas ao mesmo tempo:

- Como a aplicação pode distribuir a computação por mais máquinas?
- Como o problema de otimização pode ser dividido mantendo relações úteis entre pedidos, rotas e Dashers?

O autor observa que a primeira pergunta exigia uma mudança arquitetural, e a segunda exigia uma mudança na forma de organizar o trabalho.

> ❌ **Anti-padrão — Escalar um otimizador acoplado adicionando servidores idênticos:** Executar mais cópias do motor inteiro e dividir as requisições recebidas entre elas. **Por que as pessoas fazem isso:** A escalabilidade horizontal é a resposta padrão que funciona para requisições web sem estado. **Consequência:** Os trabalhadores tomam decisões conflitantes ou duplicadas, e o componente que divide e junta o trabalho se torna o novo gargalo. **Correção:** Separar as responsabilidades de coordenação do cálculo pesado e decompor o problema para que cada trabalhador tenha uma parte significativa e delimitada. *(O artigo apresenta isso como a razão pela qual a escalabilidade horizontal sozinha não bastava; o rótulo é uma interpretação do argumento dele.)*

## Separar a coordenação do cálculo pesado
A DoorDash reconstruiu a plataforma em torno de duas camadas escaláveis de forma independente. Uma camada é um grupo de componentes com uma responsabilidade específica.

- A **camada de coordenação** executa o processo de dispatch. Ela mantém o estado logístico mais amplo, aplica regras de negócio e organiza o trabalho que precisa ser feito.
- A **camada de computação** é formada por trabalhadores que executam cálculos caros. Esses trabalhadores geram e avaliam possibilidades de roteamento, executam rotinas de otimização e rodam modelos de ML.

Separar as camadas permite que a capacidade de computação cresça sem que cada trabalhador assuma todas as responsabilidades do motor original. O fluxo de execução funciona assim: os trabalhadores operam em paralelo, e uma etapa de reconciliação combina seus resultados em decisões de dispatch coordenadas.

Distribuir a computação introduz um custo de comunicação. Se cada trabalhador recebesse uma cópia completa de todo o estado logístico em cada execução, transmitir e processar esses dados poderia se tornar muito caro.

### Fragmentação localizada de payloads
A DoorDash resolve isso com a fragmentação localizada de payloads. Um payload é o dado enviado com um job, e a fragmentação divide esse dado em porções menores. Cada trabalhador recebe apenas o contexto local necessário para o cálculo que lhe foi atribuído. O coordenador envia esses payloads via gRPC em cada tick de dispatch, que o artigo define como uma rodada de processamento de dispatch.

> ❌ **Anti-padrão — Enviar o estado logístico completo a cada trabalhador:** Enviar o estado inteiro a cada nó de computação em cada execução. **Por que as pessoas fazem isso:** É a forma mais simples de garantir que todo trabalhador tenha o que precisa. **Consequência:** Os custos de transmissão e processamento crescem com o sistema inteiro, corroendo os ganhos do paralelismo. **Correção:** Enviar a cada trabalhador um payload autocontido e localizado. *(O artigo descreve esse custo como o motivo da fragmentação; o rótulo é uma interpretação.)*

### gRPC como transporte
> 🔎 Contexto adicional: gRPC é um framework para comunicação entre serviços. O artigo o usa como canal entre coordenador e trabalhadores e não discute sua configuração.

### Trabalhadores sem estado
Os payloads são autocontidos, o que permite que os trabalhadores fiquem sem estado. Um trabalhador ainda pode manter um modelo treinado ou dados temporários de cálculo. A separação-chave é que a responsabilidade por gerenciar o estado logístico em mudança permanece com o coordenador. Essa divisão significa que trabalhadores podem ser adicionados ou substituídos sem carregar o estado autoritativo da rede.

## Quebrar o problema de otimização em partes menores
Separar as camadas permite a execução paralela, mas não reduz automaticamente o tamanho do problema de otimização. Se cada trabalhador ainda tivesse de avaliar um conjunto enorme e interligado de possibilidades, a pressão computacional original permaneceria. Por isso a DoorDash introduziu a decomposição hierárquica, que significa dividir um problema grande em problemas menores por meio de múltiplas etapas.

O coordenador agrupa o problema de dispatch mais amplo em clusters geográficos localizados e trajetórias de entrega. O objetivo é criar peças gerenciáveis que mantenham as relações locais importantes para o roteamento.

### Pré-agrupamento e refinamento por centroides
A primeira etapa é um pré-agrupamento rápido, que cria grupos iniciais. A segunda é um refinamento baseado em centroides, que ajusta esses grupos usando seus centros. O artigo descreve um centroide como o centro representativo de um grupo de pontos.

O particionamento acontece, portanto, por um agrupamento inicial seguido de refinamento. Cada partição se torna uma unidade de trabalho para um nó de computação, que gera e verifica possibilidades dentro de seu contexto local enquanto outros nós trabalham em outras partições.

Isso traz dois benefícios. Problemas menores reduzem o número de combinações que cada trabalhador precisa considerar, e a execução paralela permite que vários problemas sejam processados ao mesmo tempo.

### Fronteiras geográficas são organizacionais, não reais
O autor enfatiza que a fronteira geográfica é um recurso para organizar o cálculo. Ela não corresponde a uma separação real entre oportunidades de entrega. Pedidos e rotas perto da fronteira ainda podem estar relacionados, então soluções locais podem entrar em conflito.

### Reconciliação global e pontuação
Para lidar com isso, a DoorDash usa uma camada global de reconciliação e pontuação que combina os resultados entre as partições antes que as atribuições finais sejam selecionadas. O autor diz que isso preserva grande parte da qualidade de otimização do problema original, ao mesmo tempo em que permite que a geração cara de candidatos rode em paralelo. Os trabalhadores produzem opções locais úteis, e a reconciliação considera como essas opções podem ser combinadas nas decisões de dispatch mais amplas.

## Como a DoorDash substituiu com segurança um sistema tão crítico
O autor observa que o dispatch afeta diretamente entregas em andamento. Uma configuração defeituosa poderia enviar Dashers a atribuições ruins, aumentar o tempo de espera ou prejudicar o cumprimento dos pedidos. Por isso, a substituição precisava ser validada com cuidado.

### Teste em sombra (shadow testing)
A equipe começou com o teste em sombra. No modo sombra, o trabalho de produção recebido é duplicado: uma cópia vai para o motor existente e outra para a nova plataforma. O motor existente continua controlando as entregas reais. O novo motor também calcula o que faria, mas sua saída é apenas registrada para análise.

Isso permitiu que os desenvolvedores verificassem a nova plataforma em condições reais de produção, incluindo o tráfego de pico. Eles podiam comparar as correspondências propostas, o comportamento relacionado ao tempo de entrega e a resiliência em relação ao sistema existente.

### Análise de regressão comportamental
O artigo chama essa abordagem de análise de regressão comportamental. Regressão, aqui, significa uma mudança que piora um comportamento antes aceitável. A equipe precisava mostrar que a nova arquitetura preservava a qualidade do dispatch e, ao mesmo tempo, melhorava o desempenho.

> ❌ **Anti-padrão — Tratar o teste em sombra como prova final:** Confiar apenas nos resultados do modo sombra para decidir que um novo motor de decisão é seguro. **Por que as pessoas fazem isso:** O modo sombra não traz risco para as entregas e mostra um comportamento realista com entradas reais. **Consequência:** Suas decisões nunca são executadas, então seu efeito real nas entregas permanece não medido. **Correção:** Seguir o teste em sombra com experimentos controlados em produção. *(O artigo afirma esse limite diretamente; o rótulo é uma interpretação.)*

### Experimentos switchback
O teste em sombra tem um limite prático: como as decisões do novo sistema nunca são executadas, ele não consegue mostrar o impacto real dessas decisões. Por isso a DoorDash seguiu o teste em sombra com experimentos switchback em produção. Em um experimento switchback, os dois sistemas se revezam no controle da atividade real de produção sob condições controladas, o que permite comparar seus resultados.

A equipe usou exposição limitada e mecanismos de fallback enquanto verificava a confiabilidade do sistema e os resultados de cumprimento dos pedidos. Ela ampliou a implantação para uma base de usuários maior somente depois de satisfeita.

O artigo resume a progressão: o teste em sombra deu forte confiança no comportamento do novo motor com entradas reais, enquanto os experimentos controlados em produção testaram o que acontecia quando as decisões realmente afetavam as entregas.

## As melhorias
O autor relata ganhos substanciais na capacidade e na responsividade da plataforma depois que o novo sistema de dispatch foi implantado. Os resultados detalhados são:

- **Dez vezes a escala de processamento de dispatch**, possibilitada pela distribuição da computação.
- **Mais de 99.96% das execuções concluídas em menos de 20 segundos**, em comparação com execuções que se estendiam por minutos sob carga intensa no sistema antigo.
- **Mais de 99.99% de disponibilidade durante picos de tráfego do Super Bowl**, apesar de um aumento de 10% semana a semana no volume de pico.

Os três resultados medem propriedades diferentes. A escala de processamento é quanto trabalho o sistema consegue lidar. A latência de execução é quanto tempo leva uma execução. A disponibilidade é se o sistema continua operacional. O autor argumenta que os três importam para o dispatch, porque uma plataforma que eventualmente processa grande volume ainda pode causar problemas se decisões individuais forem lentas demais.

A capacidade extra também permite que o sistema avalie um conjunto maior de possibilidades, em vez de restringir sua busca nos períodos de movimento intenso. O autor conecta essa busca mais ampla a melhores taxas de aceitação dos Dashers, eficiência no cumprimento dos pedidos e ganhos dos Dashers, e relata economias de cumprimento anualizadas na casa dos milhões de dólares. O artigo não traz números para os ganhos de taxa de aceitação ou de ganhos dos Dashers. O benefício prático, na visão do autor, é que as decisões chegavam mais rápido e a plataforma tinha espaço computacional suficiente para considerar atribuições melhores.

## Conclusão
O autor diz que o redesenho também tornou o machine learning viável dentro do dispatch. A nova plataforma tornou factível executar modelos de ML mais exigentes computacionalmente no processo de dispatch.

### Inferência dentro do dispatch
Um modelo treinado executa inferência quando processa uma entrada e produz uma saída, como uma pontuação para uma rota proposta. Se o dispatch precisa de um número enorme dessas avaliações, mesmo um pequeno custo por avaliação se torna significativo. Por isso o posicionamento dos modelos importa.

### Duas camadas de serving de modelos
A DoorDash introduziu duas camadas para atender a necessidades diferentes.

- **L1** serve modelos localmente dentro dos trabalhadores de computação. Cada trabalhador contém o ONNX Runtime, software que executa modelos de ML treinados. Como as avaliações acontecem localmente, elas evitam uma ida e volta de rede até um servidor de modelos separado.
- **L2** serve modelos por meio de infraestrutura remota separada. Ela lida com avaliações de políticas mais complexas, que acontecem com menos frequência. Esses modelos mais pesados rodam em clusters de serving de ML isolados, o que mantém suas demandas computacionais separadas do trabalho de gerenciamento de estado do coordenador.

O princípio é que avaliações frequentes precisam de um caminho de execução muito curto, enquanto avaliações mais pesadas, mas menos frequentes, podem usar capacidade remota dedicada.

> 🔎 Contexto adicional: ONNX Runtime é um motor para executar modelos salvos no formato de intercâmbio ONNX. O artigo o menciona, mas não descreve quais modelos ou formatos a DoorDash usa.

### Aprendizado por reforço multiagente para pesos de objetivos
A plataforma também viabilizou um sistema de aprendizado por reforço multiagente que ajusta as prioridades do otimizador. Aprendizado por reforço é uma abordagem na qual um sistema aprende quais decisões tomar ao receber feedback sobre os resultados. Na aplicação da DoorDash, os agentes ajustam os pesos dos objetivos, que determinam com que força o otimizador favorece diferentes metas, como velocidade de entrega e eficiência de agrupamento.

O ponto final do autor é que a maior contribuição da nova plataforma não foi um ganho de desempenho isolado. A plataforma mudou a trajetória de longo prazo do sistema de dispatch da DoorDash.

## Referências
O artigo tem uma seção de Referências, mas não lista citações. Seus dois parágrafos finais fazem comentários em vez de relatar resultados.

- **Carry-over do switchback:** No dispatch, um Dasher atribuído perto do fim de uma janela do novo motor ainda está nessa viagem quando o motor antigo assume. Cada janela, portanto, herda as atribuições da janela anterior. O comentário pergunta quanto duravam as janelas e se a DoorDash descartou os minutos de borda da comparação.
- **Sequência sombra-depois-switchback:** Para um sistema em que uma decisão lenta também é uma decisão pior, o comentário argumenta que provar o novo motor com tráfego real, antes que ele afete qualquer entrega, é a única forma sensata de substituí-lo.

> 🔎 Contexto adicional: O artigo não atribui esses dois parágrafos finais à DoorDash. Leia-os, portanto, como comentário do autor do artigo, e não como resultados relatados pela DoorDash.

## 🧠 Modelos mentais
### Espaço e tempo juntos
**O modelo:** Uma decisão de dispatch só é boa se levar em conta onde estão os recursos e quando eles estarão prontos. Um recurso próximo ainda pode ser a escolha errada se o tempo não for adequado.
**Como aplicar:** Antes de otimizar uma decisão de alocação, liste as dimensões de tempo, como preparo, viagem e expectativas, ao lado das dimensões espaciais.

### Boa o suficiente e rápida o suficiente
**O modelo:** Uma decisão precisa ser boa o suficiente em qualidade e rápida o suficiente para ser útil. Uma saída perfeita, mas tardia, não tem valor em um sistema em produção.
**Como aplicar:** Defina primeiro um orçamento de latência para cada decisão e então escolha o melhor algoritmo que caiba nele.

### Particionar, resolver localmente, reconciliar globalmente
**O modelo:** Dividir uma otimização grande em partes locais permite que elas rodem em paralelo, mas uma camada global precisa combinar os resultados para que as partes se encaixem.
**Como aplicar:** Defina as fronteiras de partição do cálculo e, em seguida, projete uma etapa de reconciliação que pontue as opções combinadas, e não apenas as locais.

### Teste em sombra primeiro, depois switchback
**O modelo:** O teste em sombra comprova o comportamento com entradas reais sem afetar os resultados. Os experimentos switchback comprovam o efeito sobre os resultados. Cada um cobre uma lacuna que o outro deixa.
**Como aplicar:** Rode o novo sistema em modo sombra para comparar decisões e depois passe a experimentos de produção limitados, reversíveis e com fallback, antes de uma implantação mais ampla.

### Avaliações frequentes ficam locais
**O modelo:** Chamadas de modelo baratas e frequentes devem ficar na mesma máquina que o trabalho que as faz. Chamadas pesadas e raras podem ir para capacidade remota dedicada.
**Como aplicar:** Meça com que frequência cada modelo é chamado e seu custo por chamada, e então posicione-o nas camadas local ou remota de acordo com isso.

## 🔑 Principais conclusões
- Decisões de dispatch devem pesar espaço e tempo juntos, já que um entregador próximo ainda pode ser a escolha errada quando a comida não está pronta.
- O crescimento quadrático do cálculo significa que dobrar a entrada pode quadruplicar aproximadamente o trabalho, e é por isso que o antigo motor monolítico não conseguia acompanhar a demanda de pico.
- Adicionar servidores ajuda apenas quando o trabalho é independente. O dispatch exige coordenação, então a DoorDash separou a camada de coordenação da camada de computação.
- A fragmentação localizada de payloads e os trabalhadores sem estado mantêm sob controle o custo da computação distribuída, enquanto o coordenador é dono do estado logístico em mudança.
- A decomposição hierárquica, com pré-agrupamento e refinamento por centroides, torna o problema gerenciável, e uma camada global de reconciliação mantém as partições consistentes.
- O teste em sombra verifica o comportamento com entradas reais, e os experimentos switchback medem o impacto real antes de uma implantação completa.
- O autor relata dez vezes a escala de processamento, mais de 99.96% das execuções abaixo de 20 segundos e mais de 99.99% de disponibilidade durante picos do Super Bowl, tudo com base em detalhes públicos compartilhados.

## 🏛️ Para Kenji
> Esta é a sua interpretação, não a do autor.

O mesmo padrão se aplica a qualquer serviço de decisão sensível à latência que um arquiteto possa ter sob sua responsabilidade: um otimizador acoplado que precisa responder em segundos, um coordenador central que não pode virar gargalo e um motor central que precisa ser substituído sem interrupção. A sequência de teste em sombra e, depois, rollout switchback limitado com fallback é um plano de migração reutilizável. Uma questão que ela levanta: em um serviço de decisão do seu próprio portfólio, quais dependências entre partições forçariam uma camada global de reconciliação, e como você mediria a qualidade que se perde ao particionar?

## 📚 Plano de leitura
### Do artigo
- [DoorDash Dispatch: Building a Lightning-Fast Delivery System](https://blog.bytebytego.com/p/doordash-dispatch-building-a-lightning) — o artigo completo, para conferir os números e a sequência do redesenho com esta nota.
### Para aprofundar
- Problema de roteamento de veículos e suas heurísticas — para entender a família de otimização à qual o dispatch se parece.
- Experimentos switchback e efeitos de carry-over em experimentação de marketplaces — para julgar se uma comparação de rollout é válida.
- Shadow traffic e dark launching — para planejar uma comparação sem risco de um novo motor com a produção.
- Clusterização baseada em centroides, como k-means — para ver como o pré-agrupamento e o refinamento por centroides funcionam na prática.

## Conceitos
- [architectural-style](/pt-br/concepts/architecture/architectural-style.md)
- [quality-attributes](/pt-br/concepts/architecture/quality-attributes.md)
- [scalability](/pt-br/concepts/backend-systems/scalability.md)
- [distributed-system](/pt-br/concepts/backend-systems/distributed-system.md)
- [grpc](/pt-br/concepts/backend-systems/grpc.md)
- [control-plane](/pt-br/concepts/backend-systems/control-plane.md)
- [latency-throughput](/pt-br/concepts/backend-systems/latency-throughput.md)
- [availability-reliability](/pt-br/concepts/backend-systems/availability-reliability.md)
- [fault-tolerance](/pt-br/concepts/backend-systems/fault-tolerance.md)
- [canary-deploy](/pt-br/concepts/cloud-infra/canary-deploy.md)
- [training-vs-inference](/pt-br/concepts/ai-ml/training-vs-inference.md)
- [reinforcement-learning](/pt-br/concepts/ai-ml/reinforcement-learning.md)
- [shadow-testing](/pt-br/concepts/backend-systems/shadow-testing.md)
