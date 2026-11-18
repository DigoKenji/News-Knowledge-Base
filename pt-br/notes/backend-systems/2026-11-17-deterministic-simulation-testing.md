---
type: Article Summary
title: Teste de Simulação Determinística
description: Bugs distribuídos tornam-se reproduzíveis quando um núcleo single-threaded controla o avanço do relógio, da rede e do disco por meio de chamadas de tick.
resource: https://martinfowler.com/articles/deterministic-simulation-testing.html
tags:
- backend-systems
- system-design
- deterministic-simulation-testing
- linearizability
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-17T21:40:00-03:00'
sources:
- resource: https://martinfowler.com/articles/deterministic-simulation-testing.html
  title: Deterministic Simulation Testing
  author: Martin Fowler
status: stable
source_url: https://martinfowler.com/articles/deterministic-simulation-testing.html
source: Martin Fowler
source_type: blog
published: '2026-11-17'
domain: backend-systems
subdomain: system-design
concepts:
- deterministic-simulation-testing
- linearizability
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41cbaf74172d81b39996c5321faca125
lang: pt-br
translation_of: /notes/backend-systems/2026-11-17-deterministic-simulation-testing.md
---

## 🎯 Tese central
Bugs de sistemas distribuídos se escondem em intercalações de falhas parciais com tráfego real, que testes ao vivo não conseguem reproduzir. Por isso, o autor executa o cluster inteiro em uma única thread, sobre relógio, rede e disco simulados, onde o teste decide cada passo e qualquer execução pode ser repetida a partir de uma seed.

## 🧭 Por que o autor escreveu
Testes padrão deixam passar as interações entre falhas parciais e requisições em trânsito, e testes de caos ao vivo encontram bugs que não podem ser reproduzidos sob demanda. Se a tese se sustentar, as falhas se repetem exatamente e as condições degradadas podem ser produzidas de propósito.

## Introdução
Sistemas distribuídos são notoriamente difíceis de testar. Bugs sutis sobrevivem por anos a sistemas bem arquitetados e intensamente testados, e o Jepsen encontra regularmente violações de consistência e comportamento de split-brain em bancos de dados consolidados. O autor argumenta que o problema está no método, não no cuidado. O exemplo que acompanha o texto é um armazenamento chave-valor com quórum de três nós (Athens, Byzantium e Cyrene), construído na Tickloom, uma biblioteca Java para simulação determinística.

## Limitações dos mecanismos de teste padrão
### Testes unitários, de integração e ponta a ponta
Testes unitários verificam a lógica local, testes de integração verificam a interação entre componentes, e testes ponta a ponta em geral verificam apenas o caminho feliz. Nenhum deles prova o que os clientes veem quando parte do sistema falha com requisições em andamento. As falhas injetadas tendem a ser limpas e previsíveis, como parar um nó de forma graciosa.

## Falhas cinzentas
### Falhas cinzentas (gray failures)
Uma falha cinzenta é um defeito parcial e degradado que o restante do sistema não detecta de forma limpa. A rede pode descartar ou atrasar mensagens selecionadas, de modo que um nó expira por timeout enquanto outro não. Uma pausa de coleta de lixo da JVM ou a limitação de CPU pode fazer um nó perder um heartbeat e depois acordar sem saber que ficou pausado. Um disco pode levar 500 ms em vez de 2 ms para sincronizar um Write-Ahead Log, ou corromper silenciosamente alguns setores. Essas falhas criam as intercalações que quebram sistemas robustos, e testes padrão não conseguem produzi-las sob demanda; por isso, o contorno usual é injetar falhas em sistemas em execução.

## Do chaos engineering à verificação de corretude
### Chaos engineering
Chaos engineering pergunta se um sistema continua disponível sob falha. No armazenamento de três nós, um nó derrubado durante uma escrita com quórum ainda deveria permitir que a escrita tenha sucesso e que o cluster se recupere (Figuras 1 e 2 do original).

### Disponibilidade versus corretude
Disponibilidade não é corretude. Se uma partição isola Cyrene, a escrita de Alice, "After Dawn", tem sucesso em Athens e Byzantium. Uma leitura que Bob envia ao estado local de Cyrene retorna "Before Dawn". Toda requisição recebeu resposta, e mesmo assim a garantia de consistência foi quebrada (Figura 3).

### Verificação de histórico no estilo Jepsen
O Jepsen executa um sistema ao vivo, injeta falhas, registra o que os clientes observaram e verifica esse histórico contra a semântica que o sistema declara. Aqui, o histórico mostra uma leitura desatualizada depois de uma escrita confirmada:

```text
Alice: invoke set("title", "After Dawn")
Alice: ok     set("title", "After Dawn")
Bob:   invoke get("title")
Bob:   ok     get("title") -> "Before Dawn"   // violation: stale read after acknowledged write
```


## O problema do não determinismo

### Agendas de falha não semeadas
Se o gerador de falhas não tiver seed, ou for re-semeado a partir do relógio do sistema, cada execução produzirá uma sequência de falhas diferente. Um gerador pseudoaleatório com seed corrige isso para as decisões de falha.

### Não determinismo da execução
A mesma sequência de falhas ainda produz execuções diferentes, porque a aplicação roda sobre tempo de parede, escalonamento de threads e buffers reais de rede e disco. No exemplo, Athens precisa de dois de três reconhecimentos. Se o reconhecimento de Byzantium chega a tempo, a escrita tem sucesso; se atrasa além do timeout, a escrita falha. Nenhum dos dois resultados está errado, mas um bug no caminho da falha pode nunca reaparecer numa nova execução (Figura 4).

## Fontes de não determinismo
Quatro fontes alteram o momento em que os eventos se tornam visíveis para o software: o relógio, o escalonamento de threads, o tempo de rede e o tempo de disco.

### Relógio e tempo
A mesma operação pode levar 40 ms em uma execução e 120 ms na seguinte, e correções do NTP podem fazer relógios de parede saltarem. A mesma escrita pode terminar dentro do timeout em uma execução e ultrapassá-lo em outra, por isso um teste repetível precisa controlar o que o relógio marca (Figura 5).

### Escalonamento de threads
O sistema operacional decide qual thread roda em seguida, e um processo pausado pela GC fica para trás em relação aos pares. Quando a escrita de Alice e a leitura de Bob chegam à mesma réplica, o handler que roda primeiro decide se Bob vê o novo valor. Um teste repetível precisa decidir qual trabalho roda em seguida (Figura 6).

### Tempo de rede
A entrega depende de buffers, congestionamento, retransmissão e roteamento, então a mesma mensagem de replicação pode chegar a uma réplica antes ou depois da leitura de Bob. Injetar a mesma partição não resolve isso (Figura 7).

### Tempo de disco
Uma escrita retorna assim que seus bytes estão no page cache do SO, e só se torna durável quando um fsync termina ou o kernel os descarrega. Se o coordenador travar antes do flush, "After Dawn" se perde; se o flush já tiver terminado, a recuperação o encontra (Figura 8).

🔎 Contexto adicional: fsync é a chamada de sistema que pede ao SO para enviar os dados escritos ao dispositivo de armazenamento. Aleatoriedade e ordem de iteração dentro do programa também são não determinísticas, e o artigo trata isso com regras de codificação, não com simulação.

## Tornando o progresso explícito
Sistemas reais avançam por meio de progresso oculto na CPU, na NIC e no disco, que nenhum teste controla. A simulação determinística coloca o teste no comando: nada avança a menos que o teste dê um tick (Figuras 9 e 10). Uma lentidão vira um atraso de alguns ticks, e então as falhas cinzentas se tornam simples de testar.

## Implementação de exemplo
A Tickloom se inspira no FoundationDB, que pioneirou a simulação determinística para bancos de dados, e no TigerBeetle, cujo design single-threaded e dirigido por ticks era mais fácil de implementar em Java. A versão atual usa callbacks e um future customizado, e continuações em Java estão planejadas.

## Execução single-threaded
Todo processo, em testes e em produção, roda em uma única thread. O loop principal chama tick() em cada componente, e todo componente que precisa de progresso controlado implementa Tickable:

```java
public interface Tickable {
    void tick();
}
```

A thread de tick nunca pode bloquear. Chamadas de rede e de armazenamento são assíncronas, não existe Thread.sleep(), a rede de produção usa NIO não bloqueante, e o trabalho de disco em background devolve resultados como eventos processados durante um tick.

### Interface de rede
O código de protocolo fala com a rede somente pela interface Network, então o mesmo código roda sobre sockets simulados ou reais. As mensagens carregam origem, destino, tipo, payload e um ID de correlação:

```java
public record Message(
        ProcessId source,
        ProcessId destination,
        PeerType peerType,
        MessageType messageType,
        byte[] payload,
        String correlationId
) 
```

```java
public abstract class Network implements Tickable, AutoCloseable {
    private final ProcessRegistry registry = new ProcessRegistry();
    public void register(Process process) {
        registry.register(process);
    }
    public Set<ProcessId> registeredProcessIds() {
        return registry.processIds();
    }
    protected void deliver(Message message) {
        Optional<Process> process = registry.get(message.destination());
        if (process.isEmpty()) {
            logDropped(message);
            return;
        }
        process.get().onMessage(message);
    }
    @Override
    public abstract void tick();
    public abstract void send(Message message) throws IOException;
    public void bind(ProcessId processId) throws IOException {
        //no-op.. only used by NioNetwork
    }
    @Override
    public void close() throws Exception {
    }
```

send() é assíncrono. Ele enfileira a mensagem e retorna, e a resposta chega depois, como outra mensagem. O SimulatedNetwork suporta partições, perdas e atrasos em memória, enquanto o NioNetwork implementa o mesmo contrato sobre TCP com Java NIO.

### Estado dos links simulados
A rede simulada modela cada par de processos em um sentido como um link e armazena as falhas aplicadas a ele:

```java
class SimulatedNetwork
  private record NetworkLink(ProcessId source, ProcessId destination) {
      boolean isPartitioned(Set<NetworkLink> partitionedLinks) {
          return partitionedLinks.contains(this);
      }
  }
```

```java
class SimulatedNetwork
  private final Set<NetworkLink> partitionedLinks = new HashSet<>();
  private final Map<NetworkLink, Double> linkPacketLoss = new HashMap<>();
  private final Map<NetworkLink, DelayConfig> cloggedUntilTick = new HashMap<>();
  private final Map<NetworkLink, FaultRule> linkFaultRules = new HashMap<>();
```

## Como o send() funciona
Um link particionado, uma regra de descarte que combine ou um sorteio de perda de pacote descarta a mensagem. A sobrevivente entra em uma fila de prioridade, carimbada com seu tick de entrega e com um número de sequência que mantém a ordenação determinística.

```java
class SimulatedNetwork
  @Override
  public void send(Message message) {
      validateMessage(message);
      NetworkLink link = linkFrom(message);
      if (link.isPartitioned(partitionedLinks)) {
          return; // Message dropped due to partition
      }
      if (matchesFaultRule(link, message)) {
          return; // Message dropped due to fault rule
      }
      if (shouldDropPacket(link)) {
          return; // Message lost due to packet loss
      }
      // Use internal counter to calculate delivery tick (TigerBeetle pattern)
      long deliveryTick = calculateDeliveryTick(link, message, currentTick);
      queueForDelivery(message, deliveryTick);
  }
```

```java
private void queueForDelivery(Message message, long deliveryTick) {
    pendingMessages.offer(new QueuedMessage(message, deliveryTick, nextSequenceNumber++));
}
```

## Como o tick() funciona
Nada se move até que a rede receba um tick. Cada tick avança um contador virtual, aplica as mudanças de falha e entrega, na ordem em que foram enviadas, todas as mensagens que já venceram.

```java
class SimulatedNetwork
  @Override
  public void tick() {
      currentTick++; // Increment internal counter (TigerBeetle pattern)
      maybeFlipPartitions();
      maybeClogPaths();
      deliverPendingMessagesFor(currentTick);
  }
```

```java
class SimulatedNetwork
  private void deliverPendingMessagesFor(long tickTime) {
      // Process messages whose delivery time has now arrived
      // PriorityQueue ensures we process in delivery time order
      while (!pendingMessages.isEmpty() &&
              pendingMessages.peek().deliveryTick <= tickTime) {
          QueuedMessage queuedMessage = pendingMessages.poll();
          Message message = queuedMessage.message;
          deliver(message);
          System.out.println("SimNet: delivered " + message + " for tick " + tickTime);
      }
  }
```

## Roteando mensagens para os processos
Na simulação, todos os processos compartilham uma única SimulatedNetwork. Um processo registra seu ProcessId ao ser criado, e a rede chama seu onMessage(). Uma mensagem para um ID desconhecido é registrada no log e descartada. Um processo recebe sua rede de fora, então um teste pode fornecer a simulada:

```java
public abstract class Process implements Tickable, AutoCloseable {
    public final ProcessId id;
    public final Network network;
```

```java
network.register(this);
initialiseMessageHandlers();
```

```java
class Process
  protected interface Handler {
      void handle(Message message);
  }
  protected Map<MessageType, Handler> handlers = new HashMap<>();
  private void initialiseMessageHandlers() {
      this.handlers.putAll(initialiseHandlers());
  }
  protected abstract Map<MessageType, Handler> initialiseHandlers();
  protected Handler getHandler(MessageType messageType) {
      return handlers.get(messageType);
  }
```

A QuorumReplica associa cada tipo de mensagem a um handler:

```java
class QuorumReplica
  @Override
  protected Map<MessageType, Handler> initialiseHandlers() {
      return Map.of(
              QuorumMessageTypes.CLIENT_GET_REQUEST, this::handleClientGetRequest,
              QuorumMessageTypes.CLIENT_SET_REQUEST, this::handleClientSetRequest,
              QuorumMessageTypes.INTERNAL_GET_RESPONSE, this::handleInternalGetResponse,
              QuorumMessageTypes.INTERNAL_SET_RESPONSE, this::handleInternalSetResponse,
              QuorumMessageTypes.INTERNAL_GET_REQUEST, this::handleInternalGetRequest,
              QuorumMessageTypes.INTERNAL_SET_REQUEST, this::handleInternalSetRequest);
  }
```

onMessage executa o handler correspondente até o fim, na thread principal:

```java
class Process
  public final void onMessage(Message message) {
      if (state == ProcessState.STOPPED) {
          return;
      }
      onMessageReceived(message);
      MessageType messageType = message.messageType();
      Handler handler = getHandler(messageType);
      if (handler == null) {
          System.err.println("No handler found for message " + messageType);
          return;
      }
      
      // Check if process is running before handling message
      if (state != ProcessState.RUNNING) {
          System.err.println(id + ": Received message " + messageType + " but process not initialized yet");
          handleUninitializedMessage(message);
          return;
      }
      
      handler.handle(message);
  }
```

## Timeouts em ticks
Relógios de hora do dia não são garantidamente monotônicos, e relógios monotônicos podem ser distorcidos por pausas de processo. Por isso, os timeouts contam ticks. Um timeout de 10 expira exatamente após 10 chamadas a tick():

```java
public final class Timeout {
    
    private final String name;
    private final long durationTicks;
    private long ticks = 0;
    private boolean ticking = false;
    private long startTick = 0;
    
    /**
     * Creates a new timeout with the specified duration.
     * 
     * @param name human-readable name for debugging
     * @param durationTicks number of ticks before timeout fires
     */
    public Timeout(String name, long durationTicks) {
        this.name = name;
        this.durationTicks = durationTicks;
    }
    
    public void start() {
        ticking = true;
        startTick = ticks;
    }
    
    public void tick() {
        ticks++;
    }
    
    public boolean isExpired() {
        if (!ticking) {
            return false;
        }
        return (ticks - startTick) >= durationTicks;
    }
```

### Lista de requisições em espera
Cada requisição em andamento recebe seu próprio Timeout baseado em ticks. Cada tick avança os timeouts, e as requisições expiradas falham com uma TimeoutException:

```java
class RequestWaitingList
  public void add(Key key, RequestCallback<Response> callback) {
      //create Timeout with configured timeoutTicks.
      Timeout timeout = new Timeout("request-" + key, timeoutTicks);
      timeout.start();
      pendingRequests.put(key, new CallbackDetails(callback, timeout));
  }
```

```java
class RequestWaitingList
  public void tick() {
      // First, tick all timeouts
      pendingRequests.values().forEach(callbackDetails -> 
          callbackDetails.getTimeout().tick());
      
      List<Key> expiredRequestKeys = getExpiredRequestKeys();
      if (expiredRequestKeys.isEmpty()) {
          return;
      }
      timeoutExpiredRequests(expiredRequestKeys); 
  }
  private void timeoutExpiredRequests(List<Key> expiredRequestKeys) {
       expiredRequestKeys.forEach(expiredRequestKey -> {
          CallbackDetails callbackDetails = pendingRequests.remove(expiredRequestKey);
          if (callbackDetails != null) {
              callbackDetails.getRequestCallback().onError(
                  new TimeoutException("Request expired after " + timeoutTicks + " ticks")
              );
          }
      });
  }
  
  private List<Key> getExpiredRequestKeys() {
      return pendingRequests.entrySet().stream()
              .filter(entry -> entry.getValue().isExpired())
              .map(Map.Entry::getKey)
              .collect(Collectors.toList());
  }
```

## Testando com um cluster simulado
### Escrita com quórum
A réplica que recebe uma requisição de set torna-se coordenadora. Ela carimba o valor com seu relógio, envia requisições internas de set às três réplicas (incluindo ela mesma), responde assim que a maioria confirma e falha a escrita se nenhuma maioria responder antes do timeout.

### Teste determinístico
O teste isola Cyrene, atrasa o link de Byzantium para Athens em 20 ticks e define um timeout de requisição de 10 ticks. Em seguida, dá ticks no cluster até que a escrita de Alice não esteja mais pendente:

```java
class QuorumWriteTimeoutTest
  @Test
  void writeTimesOutWhenAcknowledgmentIsDelayed() throws IOException {
      try (Cluster cluster = new Cluster()
              .withProcessIds(List.of(ATHENS, BYZANTIUM, CYRENE))
              .withRequestTimeoutTicks(10)
              .useSimulatedNetwork()
              .build(QuorumReplica::new)) {
          cluster.start();
          cluster.isolateProcess(CYRENE);
          cluster.setNetworkDelay(BYZANTIUM, ATHENS, 20);
          QuorumReplicaClient alice =
                  cluster.newClientConnectedTo(ALICE, ATHENS, QuorumReplicaClient::new);
          TickCompletableFuture<SetResponse> write =
                  alice.set("title".getBytes(), "After Dawn".getBytes());
          cluster.tickUntil(() -> !write.isPending());
          assertTrue(write.isFailed());
      }
  }
```

### Cluster e tickUntil
O Cluster conecta os processos à rede simulada compartilhada e fornece auxiliares para falhas. Cada tick avança os relógios, depois as redes, depois os clientes e por fim os servidores. Os testes chamam tickUntil em vez de dormir:

```java
class Cluster
  public void tick() {
      advanceClocks();
      tickNetworks();
      tickClients();
      tickServers();
  }
  private void advanceClocks() {
      processClocks.values().forEach(clock -> clock.advance(1));
  }
  private void tickServers() {
      serverNodes.forEach(Node::tick);
  }
  private void tickClients() {
      clientNodes.forEach(ClientNode::tick);
  }
  private void tickNetworks() {
      networks.forEach(Network::tick);
  }
```

```java
class Cluster
  public void tickUntil(BooleanSupplier p) {
      int tickCount = 0;
      while (!p.getAsBoolean()) {
          if (tickCount > DEFAULT_TIMEOUT_TICKS) {
              fail("Timeout waiting for condition to be met.");
          }
          tick();
          tickCount++;
      }
  }
```

O reconhecimento de Byzantium chega 20 ticks atrasado, depois do timeout de 10 ticks, então a escrita falha em todas as execuções. Com um atraso de um tick, ela tem sucesso em todas as execuções; logo, o resultado depende apenas dos números que o teste escolheu.

## Injeção de falhas com seed
Bugs de produção causados por intercalações raras exigem execuções aleatórias longas. Um gerador pseudoaleatório decide quando injetar falhas, e semeá-lo por execução torna cada execução repetível (Figura 15).

### Decisões com seed
SimulationDecisions centraliza todas as escolhas aleatórias. Requisições e falhas disparam quando random.nextDouble() fica abaixo de uma probabilidade configurada, e o mesmo objeto escolhe os tipos de falha, os nós-alvo e os intervalos de cura:

```java
class SimulationDecisions
  
      public static final double DEFAULT_REQUEST_PROBABILITY = 0.4;
      public static final double DEFAULT_FAULT_PROBABILITY = 0.08;
      public static final int DEFAULT_HEAL_INTERVAL_TICKS = 40;
  
      private final Random random;
      private final double requestProbability;
      private final double faultProbability;
      private final int healIntervalTicks;
  
      public SimulationDecisions(Random random) {
          this(random, DEFAULT_REQUEST_PROBABILITY, DEFAULT_FAULT_PROBABILITY, DEFAULT_HEAL_INTERVAL_TICKS);
      }
  
      
      public boolean shouldIssueClientRequest() {
          return random.nextDouble() <= requestProbability;
      }
  
      public boolean shouldInjectFault() {
          return random.nextDouble() <= faultProbability;
      }
  
      public boolean shouldHealNetwork(long currentTick) {
          return currentTick % healIntervalTicks == 0;
      }
  
      public FaultType chooseFaultType() {
          FaultType[] types = FaultType.values();
          return types[random.nextInt(types.length)];
      }
```

### Runner com seed
SimulationRunner constrói seu Random a partir de uma seed. A cada tick, ele pode injetar falhas, dar tick no cluster e emitir uma requisição de cliente, que um cliente single-threaded armazena em buffer se já houver uma pendente:

```java
class SimulationRunner
  protected final SimulationDecisions simulationDecisions;
  public SimulationRunner(long randomSeed, int clusterSize, int numClients,
                          ProcessFactory processFactory,
                          Cluster.ClientFactory<QuorumReplicaClient> clientFactory) throws IOException {
      Random random = new Random(randomSeed);
      this.simulationDecisions = new SimulationDecisions(random);
      this.cluster = Cluster.createSimulated(clusterSize, processFactory, random);
      this.clients = createClients(numClients, clientFactory);
  }
  private void runClusterFor(long runForTicks) {
      for (int i = 0; i < runForTicks; i++) {
          currentTick++;
          injectRandomFaults();
          cluster.tick();
          maybeIssueClientRequest();
      }
  }
  private void maybeIssueClientRequest() {
      if (simulationDecisions.shouldIssueClientRequest()) {
          // A client is single-threaded and issues one request at a time.
          // If a request is pending, the request is buffered and issued later.
          SingleRequestIssuer<ClusterClient> client = simulationDecisions.pick(clients);
          client.issueRequest();
      }
  }
```

### Injeção de falhas
Falhas aleatórias isolam um nó, particionam um par, atrasam um link em um sentido ou descartam mensagens em um link. Cada uma é adicionada à agenda de falhas com o seu tick:

```java
class FaultInjectingSimulationRunner
  @Override
  protected void injectRandomFaults() {
      if (simulationDecisions.shouldHealNetwork(currentTick())) {
          cluster().resetNetworkConditions();
          faultSchedule.add("tick=" + currentTick() + " heal-all");
          return;
      }
      if (!simulationDecisions.shouldInjectFault()) {
          return;
      }
      SimulationDecisions.FaultType fault = simulationDecisions.chooseFaultType();
      switch (fault) {
          case ISOLATE_NODE   -> isolateRandomNode();
          case PARTITION_PAIR -> partitionRandomPair();
          case DELAY_LINK     -> delayRandomDirectedLink();
          case DROP_LINK      -> dropRandomDirectedLink();
      }
  }
```

A mesma seed produz as mesmas falhas e os mesmos resultados, então um bug encontrado com a seed 42 reaparece quando a seed 42 é executada de novo. Um log de falhas e eventos com ticks costuma revelar a intercalação exata.

## Execução com avanço rápido
Em produção, um tick é uma fatia fixa de tempo real, como 10 ms no TigerBeetle. Um teste ao vivo espera cada atraso, então milhares de combinações de falhas somam dias. Um tick simulado é uma iteração de loop, então a execução termina tão rápido quanto a CPU permitir. O replay reproduz um bug, e o avanço rápido o encontra.

## Verificação de corretude
### Histórico visível para o cliente
Como no Jepsen, cada operação do cliente é registrada como um evento :invoke seguido de :ok, :fail ou timeout, em EDN:

```text
[
  {:process 0, :type :invoke, :f :write, :value ["Key-1" "Value-1"]}
  {:process 0, :type :ok,     :f :write, :value ["Key-1" "Value-1"]}
  {:process 1, :type :invoke, :f :read,  :value ["Key-1" nil]}
  {:process 1, :type :ok,     :f :read,  :value ["Key-1" "Value-1"]}
  {:process 2, :type :invoke, :f :write, :value ["Key-2" "Value-2"]}
  {:process 2, :type :fail,   :f :write, :value ["Key-2" "Value-2"]}
]
```

O SimulationRunner passa o histórico a um ConsistencyChecker, que testa a linearizabilidade de um registrador:

```java
class FaultInjectingSimulationRunnerTest
  @Test
  void faultInjectedHistoryShouldBeCheckable() throws IOException {
      long seed = 123L;
      long runForTicks = 500;
      var runner = new FaultInjectingQuorumKVScenarioRunner(seed);
      History history = runner.runAndGetHistory(runForTicks);
      assertFalse(runner.faultSchedule().isEmpty(), "Expected at least one injected fault");
      assertTrue(ConsistencyChecker.check(history, LINEARIZABILITY, REGISTER));
  }
```

Se o verificador encontrar uma violação, a seed reproduz a execução exata, e o desenvolvedor pode percorrer a sequência de ticks que levou até ela.

### Linearizabilidade
🔎 Contexto adicional: linearizabilidade significa que cada operação parece produzir efeito de forma atômica em um único ponto entre sua invocação e sua resposta. O artigo a usa como critério de corretude para o armazenamento com quórum.

## Simulando o sistema de arquivos
Falhas derrubam dados que ainda não chegaram ao dispositivo, o fsync pode falhar de maneiras sutis, e muitos sistemas de armazenamento contornam o page cache com O_DIRECT. O artigo simula o armazenamento da mesma forma que simula a rede, por meio de uma interface FileIO assíncrona.

### FileIO
Cada operação retorna um future. Uma write() concluída fica visível para leituras posteriores, mas somente um sync() concluído garante durabilidade através de uma falha:

```java
public interface FileIO {
    TickCompletableFuture<ByteBuffer> read(long offset, int length);
    TickCompletableFuture<Integer> write(ByteBuffer buffer, long offset);
    TickCompletableFuture<Void> sync();
    TickCompletableFuture<Long> size();
    TickCompletableFuture<Void> truncate(long newSize);
    TickCompletableFuture<Void> close();
}
```

### SimulatedFile
Dois arrays de bytes modelam a diferença. visible guarda todos os bytes escritos, inclusive os não sincronizados, e durable guarda os bytes sincronizados. sync() copia visible para durable, e crash() redefine visible com o conteúdo de durable, de modo que um write-ahead log pode ser testado através de falhas e reinícios (Figuras 16 e 17):

```java
final class SimulatedFile {
    private byte[] visible = new byte[0];
    private byte[] durable = new byte[0];
    long size() {
        return visible.length;
    }
    byte[] read(long offset, int length) {
        if (offset >= visible.length) {
            return new byte[0];
        }
        int from = (int) offset;
        return Arrays.copyOfRange(visible, from, Math.min(visible.length, from + length));
    }
    void write(long offset, byte[] data) {
        long end = offset + data.length;
        if (end > visible.length) {
            visible = Arrays.copyOf(visible, (int) end);
        }
        System.arraycopy(data, 0, visible, (int) offset, data.length);
    }
    void truncate(long newSize) {
        if (newSize < visible.length) {
            visible = Arrays.copyOf(visible, (int) newSize);
        }
    }
    void sync() {
        durable = visible.clone();
    }
    /** On restart, what is on disk is what becomes visible. */
    void crash() {
        visible = durable.clone();
    }
}
```

### SimulatedFileIO
As operações são enfileiradas e retornam futures. A escrita copia seus dados no momento da chamada, porque quem chama pode reutilizar o buffer:

```java
class SimulatedFileIO
  @Override
  public TickCompletableFuture<ByteBuffer> read(long offset, int length) {
      return submit(() -> ByteBuffer.wrap(file.read(offset, length)));
  }
  @Override
  public TickCompletableFuture<Integer> write(ByteBuffer buffer, long offset) {
      // Copy now: the write takes effect later, and the caller may reuse its buffer.
      byte[] data = new byte[buffer.remaining()];
      buffer.duplicate().get(data);
      return submit(() -> {
          file.write(offset, data);
          return data.length;
      });
  }
```

tick() executa as operações enfileiradas antes deste tick e completa seus futures:

```java
class SimulatedFileIO
  @Override
  public void tick() {
      // Only the calls queued before this tick: a call made from a callback completes on the next tick.
      for (int due = queue.size(); due > 0; due--) {
          queue.poll().run();
      }
  }
```

## Futures não bloqueantes
O CompletableFuture do Java é arriscado em um loop single-threaded. Chamadas bloqueantes como get() ou join() congelam a thread principal, e combinadores assíncronos como thenApplyAsync() executam callbacks em threads de pool, reintroduzindo o não determinismo de escalonamento. O TickCompletableFuture não tem nenhum dos dois problemas, mas ainda oferece composição de callbacks.

> ❌ **Anti-pattern — bloquear em um future:** chamar get() ou join() em um future pendente dentro de um handler. **Por que as pessoas fazem isso:** é a forma mais simples de obter um resultado de forma síncrona. **Consequência:** a thread de tick congela, e o loop que completaria o future nunca roda. **Correção:** anexar um callback e deixar o loop de tick completá-lo.

> ❌ **Anti-pattern — combinadores assíncronos na lógica central:** thenApplyAsync() ou similares. **Por que as pessoas fazem isso:** parece uma forma de tirar trabalho da thread que chama. **Consequência:** o estado da réplica muda em uma thread de pool, em uma ordem que depende da execução. **Correção:** compor callbacks apenas na thread de tick.

## Trade-offs
Determinismo não pode ser adicionado depois, então o núcleo single-threaded e a I/O assíncrona precisam ser projetados desde o início. As restrições valem apenas do lado determinístico da costura. Adaptadores de produção, como NioNetwork e SingleThreadedFileIO, podem usar threads e sockets desde que devolvam os resultados ao loop de tick.

## Regras estritas de codificação
O núcleo permanece determinístico somente se o código seguir seis regras:

> 📋 **Checklist — Regras estritas de codificação**
> - [ ] **Sem chamadas ao relógio do sistema:** o tempo deve vir do Clock simulado ou do contador de ticks. Nada como System.currentTimeMillis(), System.nanoTime() ou Instant.now() pode ser usado diretamente.
> - [ ] **Sem aleatoriedade sem seed:** todas as decisões aleatórias ou IDs devem vir do RandomGenerator com seed ou do IdGen fornecido.
> - [ ] **Sem threads ou futures padrão:** o código não deve usar Thread, Executor ou CompletableFuture diretamente.
> - [ ] **Sem I/O direta de rede ou disco:** toda I/O deve passar pelas interfaces Network e FileIO.
> - [ ] **Sem iteração sobre coleções não ordenadas:** iterar sobre HashMap, HashSet ou ConcurrentHashMap pode visitar entradas em ordens diferentes entre execuções. O estado central usa LinkedHashMap e LinkedHashSet.
> - [ ] **Sem callbacks entre threads através da costura:** threads de background nos adaptadores de produção nunca devem invocar métodos ou completar futures do lado determinístico. Elas enfileiram as conclusões para que a thread principal as drene durante tick().

> ❌ **Anti-pattern — iteração não ordenada:** varrer um HashMap de requisições pendentes para expirar timeouts. **Por que as pessoas fazem isso:** HashMap é a coleção padrão. **Consequência:** a ordem de expiração varia entre execuções, então um caminho raro de falha pode ser reproduzido de forma diferente. **Correção:** usar LinkedHashMap ou LinkedHashSet para o estado central.

> ❌ **Anti-pattern — completação entre threads:** a thread de disco chamando future.complete() diretamente. **Por que as pessoas fazem isso:** é o caminho mais curto da conclusão de I/O até o chamador que espera. **Consequência:** callbacks de armazenamento e de réplica rodam na thread de I/O, criando condições de corrida que a simulação nunca exercita. **Correção:** enfileirar a conclusão e deixar a thread principal drená-la durante tick().

A disciplina não escala, porque uma única iteração sobre HashMap ou uma única chamada ao relógio pode quebrar silenciosamente o replay em um caminho raro. Por isso, o autor usa funções de fitness arquiteturais:

- **Estática:** uma suíte ArchUnit, DeterminismRulesTest, roda em cada build e verifica se os pacotes centrais permanecem atrás da costura e obedecem às cinco primeiras regras.
- **Em tempo de execução:** análise estática não consegue dizer qual thread executa uma lambda, então o TickCompletableFuture registra sua thread dona e rejeita complete(), fail() ou whenComplete() vindos de qualquer outra thread:

```java
class TickCompletableFuture   private final Thread ownerThread = Thread.currentThread();
  private void checkOnOwnerThread() {
      if (Thread.currentThread() != ownerThread) {
          throw new IllegalStateException(
                  "TickCompletableFuture must be accessed on its owner (tick) thread ["
                          + ownerThread.getName() + "], but was called from ["
                          + Thread.currentThread().getName() + "]");
      }
  }
  public void complete(T result) {
      checkOnOwnerThread();
      if (state != State.PENDING) {
          throw new IllegalStateException("Future is already resolved");
      }
      this.state = State.COMPLETED;
      this.result = result;
      if (callback != null) {
          callback.accept(result, null);
          callback = null;
      }
  }
```

- **Holística:** o runner com seed e o ConsistencyChecker testam invariantes de sistema inteiro, como linearizabilidade e durabilidade após falhas, em muitas sequências de falha.

## Custo de produção
A maioria dos sistemas distribuídos é limitada por I/O, então um loop single-threaded com I/O assíncrona e em lote evita locks e troca de contexto, como no Redis, no FoundationDB e no TigerBeetle. Handlers nunca podem bloquear nem fazer trabalho de CPU longo, e escalar significa rodar mais processos.

## Facilidade de construção e evolução
A fundação de simulação exige esforço inicial, mas o código de protocolo se torna lógica single-threaded simples, mais fácil de escrever e evoluir. Os testes de cenários de falha, então, funcionam como função de fitness que torna mudanças futuras mais seguras.

## Aprendizado e uso de LLMs
Um harness determinístico oferece a um LLM uma interface clara para explorar sistemas. O autor descreve um LLM transformando o diagrama de linearizabilidade com quórum de Designing Data-Intensive Applications em um cenário executável na Tickloom.

## Agradecimentos
O autor agradece a Martin Fowler, Rebecca Parsons, Will Wilson e Joran Dirk Greef por revisarem os rascunhos.

## Revisões relevantes
17 de novembro de 2026: publicado.

## 🧠 Modelos mentais
### O progresso é controlado pelo teste
**O modelo:** uma execução é fixada pelos momentos em que o relógio, os processos, a rede e o disco avançam. O não determinismo é todo momento que o teste não escolheu.
**Como aplicar:** diante de uma falha intermitente, descubra qual dessas quatro fontes foi deixada para o ambiente e faça o teste conduzi-la.


### Semear o caos, registrar a agenda
**O modelo:** falhas aleatórias são reproduzíveis quando uma única seed fixa todas as escolhas aleatórias, e a agenda de falhas registrada explica a falha.
**Como aplicar:** encaminhe toda a aleatoriedade por um único objeto com seed e registre a agenda de falhas em cada execução.

## 🔑 Principais conclusões
- Testes caóticos ao vivo encontram bugs reais, mas, sem uma seed e uma ordem de execução controlada, eles não podem ser reproduzidos.
- Disponibilidade e corretude são coisas diferentes: um armazenamento com quórum pode responder a todas as requisições e ainda devolver um valor desatualizado depois de uma escrita confirmada.
- No exemplo, um timeout de 10 ticks contra um atraso de 20 ticks faz a escrita falhar em todas as execuções, então o resultado segue os números que o teste escolheu.
- O determinismo precisa ser projetado desde o início: núcleo single-threaded, interfaces de I/O assíncronas e coleções ordenadas são pré-requisitos, e verificações automatizadas os impõem.

## 🏛️ Para o Kenji
Poucos arquitetos constroem um simulador, mas a pergunta vale para qualquer serviço replicado: quais decisões de relógio, rede e disco os seus testes deixam para o ambiente? Esta é a minha interpretação, não a do autor.

## 📚 Plano de leitura
### A partir do artigo
- [Jepsen](https://jepsen.io): a suíte de testes de consistência por trás da abordagem de verificação de histórico.
- [Fallacies of Distributed Computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing): as premissas que as falhas cinzentas quebram.
- [Gray failures paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2017/06/paper-1.pdf): a fonte que o artigo cita sobre falhas cinzentas.
- [Tickloom](https://github.com/unmeshjoshi/tickloom): o código Java que o artigo percorre.
- [FoundationDB](https://github.com/apple/foundationdb): o sistema que pioneirou essa técnica para bancos de dados.
- [TigerBeetle](https://github.com/tigerbeetle/tigerbeetle): o design single-threaded e dirigido por ticks que o artigo segue.
- [ArchUnit](https://www.archunit.org/): a ferramenta por trás das regras estáticas de determinismo.
- [Building Evolutionary Architectures](https://learning.oreilly.com/library/view/building-evolutionary-architectures/9781492097532/ch02.html): a origem da ideia de função de fitness.
- [Designing Data-Intensive Applications](https://dataintensive.net/): o exemplo de linearizabilidade com quórum que o artigo transforma em teste.

### Para ir além
- Artigo do USENIX ATC 2020 sobre cenários de erro de fsync, citado no artigo, para testes de falha de armazenamento.
