---
type: Article Summary
title: Deterministic Simulation Testing
description: Distributed bugs become reproducible when a single-threaded core controls clock, network and disk progress through tick calls.
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
---

## 🎯 Core thesis
Distributed-system bugs hide in interleavings of partial failures and live traffic that live tests cannot reproduce, so the author runs the whole cluster on one thread against simulated clock, network and disk, where the test decides every step and any run can be replayed from a seed.

## 🧭 Why the author wrote it
Standard tests miss interactions between partial failures and in-flight requests, and live chaos tests find bugs that cannot be reproduced on demand. If the thesis holds, failures replay exactly and degraded conditions can be produced on purpose.

## Introduction
Distributed systems are notoriously hard to test. Subtle bugs survive well-architected, heavily tested systems for years, and Jepsen routinely finds consistency violations and split-brain behaviour in established databases. The author argues that the mismatch lies in the method, not the care. The running example is a three-node quorum key-value store (Athens, Byzantium, Cyrene) built in Tickloom, a Java library for deterministic simulation.

## Limitations of Standard Testing Mechanisms
### Unit, integration and end-to-end tests
Unit tests check local logic, integration tests check interactions between components, and end-to-end tests mostly check the happy path. None proves what clients see when part of the system fails while requests are in flight. Injected failures tend to be clean and anticipated, such as stopping a node gracefully.

## Gray Failures
### Gray failures
A gray failure is a partial, degraded fault that the rest of the system does not cleanly detect. The network may drop or delay selected messages, so one node times out while another does not. A JVM garbage-collection pause or CPU throttling can make a node miss a heartbeat and then wake up unaware it was paused. A disk can take 500ms instead of 2ms to sync a Write-Ahead Log, or silently corrupt a few sectors. These failures create the interleavings that break robust systems, and standard tests cannot produce them on demand, so the usual workaround is to inject faults into running systems.

## From Chaos Engineering to Correctness Checking
### Chaos engineering
Chaos engineering asks whether a system stays available under fault. In the three-node store, a node killed during a quorum write should still let the write succeed and the cluster recover (Figures 1 and 2 in the original).

### Availability versus correctness
Availability is not correctness. If a partition isolates Cyrene, Alice's write of "After Dawn" succeeds on Athens and Byzantium. A read that Bob sends to Cyrene's local state returns "Before Dawn". Every request got a response, yet the consistency guarantee broke (Figure 3).

### Jepsen-style history checking
Jepsen runs a live system, injects faults, records what clients observed and checks that history against the system's claimed semantics. Here the history shows a stale read after an acknowledged write:

```text
Alice: invoke set("title", "After Dawn")
Alice: ok     set("title", "After Dawn")
Bob:   invoke get("title")
Bob:   ok     get("title") -> "Before Dawn"   // violation: stale read after acknowledged write
```


## The Nondeterminism Problem

### Unseeded fault schedules
If the fault generator is unseeded, or reseeded from the system time, every run produces a different fault sequence. A seeded pseudo-random generator fixes this for fault decisions.

### Execution nondeterminism
The same fault sequence still yields different executions, because the application runs against wall-clock time, thread scheduling and real network and disk buffers. In the example, Athens needs two of three acknowledgments. If Byzantium's acknowledgment arrives in time the write succeeds, and if it is delayed past the timeout the write fails. Neither outcome is wrong, but a bug on the failing path may never reappear on rerun (Figure 4).

## Sources of Nondeterminism
Four sources change when events become visible to software: the clock, thread scheduling, network timing and disk timing.

### Clock and Time
The same operation can take 40ms in one run and 120ms in the next, and NTP corrections can make wall clocks jump. The same write can finish inside the timeout in one run and cross it in another, so a repeatable test must control what the clock reads (Figure 5).

### Thread Scheduling
The OS decides which thread runs next, and a process paused by GC falls behind its peers. When Alice's write and Bob's read reach the same replica, whichever handler runs first decides whether Bob sees the new value. A repeatable test must decide which work runs next (Figure 6).

### Network Timing
Delivery depends on buffering, congestion, retransmission and routing, so the same replication message can reach a replica before or after Bob's read. Injecting the same partition does not fix this (Figure 7).

### Disk Timing
A write returns once its bytes are in the OS page cache and becomes durable only when an fsync completes or the kernel flushes it. If the coordinator crashes before the flush, "After Dawn" is lost, and if the flush finished, recovery finds it (Figure 8).

🔎 Added context: fsync is the system call that asks the OS to push written data to the storage device. Randomness and iteration order inside the program are also nondeterministic, and the article handles them with coding rules rather than simulation.

## Making Progress Explicit
Real systems advance through hidden progress in the CPU, NIC and disk, which no test controls. Deterministic simulation puts the test in charge: nothing advances unless the test ticks it (Figures 9 and 10). A slowdown becomes a delay of a few ticks, so gray failures become straightforward to test.

## Example Implementation
Tickloom draws on FoundationDB, which pioneered deterministic simulation for databases, and on TigerBeetle, whose single-threaded, tick-driven design was easier to implement in Java. The current version uses callbacks and a custom future, and Java continuations are planned.

## Single-Threaded Execution
Every process, in tests and in production, runs on a single thread. The main loop calls tick() on each component, and every component that needs controlled progress implements Tickable:

```java
public interface Tickable {
    void tick();
}
```

The tick thread must never block. Network and storage calls are asynchronous, there is no Thread.sleep(), production networking uses non-blocking NIO, and background disk work hands results back as events processed during a tick.

### Network interface
Protocol code talks to the network only through a Network interface, so the same code runs against simulated or real sockets. Messages carry source, destination, type, payload and a correlation ID:

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

send() is asynchronous. It enqueues the message and returns, and a reply arrives later as another message. SimulatedNetwork supports partitions, loss and delays in memory, while NioNetwork implements the same contract over TCP with Java NIO.

### Simulated link state
The simulated network models each directed pair of processes as a link and stores the faults applied to it:

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

## How send() Works
A partitioned link, a matching drop rule or a packet-loss roll discards the message. A survivor is placed in a priority queue, stamped with its delivery tick and a sequence number that keeps ordering deterministic.

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

## How tick() Works
Nothing moves until the network is ticked. Each tick advances a virtual counter, applies fault changes and delivers every message now due, in send order.

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

## Routing Messages to Processes
In simulation, all processes share one SimulatedNetwork. A process registers its ProcessId when it is created, and the network calls its onMessage(). A message for an unknown ID is logged and dropped. A process receives its network from outside, so a test can supply the simulated one:

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

The QuorumReplica maps each message type to a handler:

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

onMessage runs the matching handler to completion on the main thread:

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

## Timeouts in Ticks
Time-of-day clocks are not guaranteed to be monotonic, and monotonic clocks can be distorted by process pauses. So timeouts count ticks instead. A timeout of 10 expires after exactly 10 calls to tick():

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

### Request Waiting List
Each in-flight request gets its own tick-based Timeout. Every tick advances the timeouts, and expired requests fail with a TimeoutException:

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

## Testing with a Simulated Cluster
### Quorum write
The replica that receives a set request becomes coordinator. It stamps the value with its clock, sends internal set requests to all three replicas including itself, replies once a majority acknowledges, and fails the write if no majority answers before the timeout.

### Deterministic test
The test isolates Cyrene, delays the Byzantium-to-Athens link by 20 ticks and sets a 10-tick request timeout. Then it ticks the cluster until Alice's write is no longer pending:

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

### Cluster and tickUntil
Cluster wires the processes to the shared simulated network and provides fault helpers. Each tick advances clocks, then networks, then clients, then servers. Tests call tickUntil instead of sleeping:

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

Byzantium's acknowledgment arrives 20 ticks late, after the 10-tick timeout, so the write fails on every run. With a one-tick delay it succeeds on every run, so the outcome depends only on numbers the test chose.

## Seeded Fault Injection
Production bugs from rare interleavings need long random runs. A pseudorandom generator decides when to inject faults, and seeding it per run makes each run repeatable (Figure 15).

### Seeded decisions
SimulationDecisions centralises every random choice. Requests and faults fire when random.nextDouble() falls under a configured probability, and the same object picks fault types, target nodes and healing intervals:

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

### Seeded runner
SimulationRunner builds its Random from a seed. Each tick it may inject faults, tick the cluster and issue one client request, which a single-threaded client buffers if one is already pending:

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

### Fault injection
Random faults isolate a node, partition a pair, delay a directed link or drop messages on a link. Each one is added to the fault schedule with its tick:

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

The same seed gives the same faults and results, so a bug found with seed 42 reappears when seed 42 is rerun. A tick-stamped log of faults and events often reveals the exact interleaving.

## Fast-Forward Execution
In production a tick is a fixed slice of real time, such as 10ms in TigerBeetle. A live test waits out every delay, so thousands of fault combinations add up to days. A simulated tick is one loop iteration, so the run finishes as fast as the CPU allows. Replay reproduces a bug, and fast-forward finds it.

## Correctness Checking
### Client-visible history
As in Jepsen, each client operation is recorded as an :invoke event followed by :ok, :fail or timeout, in EDN:

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

The SimulationRunner passes the history to a ConsistencyChecker, which tests linearizability for a register:

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

If the checker finds a violation, the seed reproduces the exact execution, and the developer can step through the tick sequence that led to it.

### Linearizability
🔎 Added context: linearizability means each operation appears to take effect atomically at one point between its invocation and its response. The article uses it as the correctness criterion for the quorum store.

## Simulating the File System
Crashes lose data that has not reached the device, fsync can fail in subtle ways, and many storage systems bypass the page cache with O_DIRECT. The article simulates storage the way it simulates the network, through an asynchronous FileIO interface.

### FileIO
Every operation returns a future. A completed write() is visible to later reads, but only a completed sync() guarantees durability across a crash:

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
Two byte arrays model the gap. visible holds every written byte, including unsynced ones, and durable holds synced bytes. sync() copies visible to durable, and crash() resets visible to durable, so a write-ahead log can be tested across crashes and restarts (Figures 16 and 17):

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
Operations are queued and return futures. The write copies its data at call time, because the caller may reuse its buffer:

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

tick() runs the operations queued before this tick and completes their futures:

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

## Non-Blocking Futures
Java's CompletableFuture is risky on a single-threaded loop. Blocking calls such as get() or join() freeze the main thread, and async combinators such as thenApplyAsync() run callbacks on pool threads, reintroducing scheduling nondeterminism. TickCompletableFuture has neither, yet still supports callback composition.

> ❌ **Anti-pattern — blocking on a future:** calling get() or join() on a pending future inside a handler. **Why people do it:** it is the simplest way to get a result synchronously. **Consequence:** the tick thread freezes, and the loop that would complete the future never runs. **Fix:** attach a callback and let the tick loop complete it.

> ❌ **Anti-pattern — async combinators in core logic:** thenApplyAsync() or similar. **Why people do it:** it looks like a way to keep work off the caller thread. **Consequence:** replica state changes on a pool thread in a run-dependent order. **Fix:** compose callbacks on the tick thread only.

## Trade-offs
Determinism cannot be added later, so the single-threaded core and asynchronous I/O must be designed in from the start. The constraints apply only on the deterministic side of the seam. Production adapters such as NioNetwork and SingleThreadedFileIO may use threads and sockets if they return results to the tick loop.

## Strict Coding Rules
The core stays deterministic only if the code follows six rules:

> 📋 **Checklist — Strict coding rules**
> - [ ] **No system clock calls:** Time must come from the simulated Clock or tick counter. Nothing like System.currentTimeMillis(), System.nanoTime(), or Instant.now() can be directly used.
> - [ ] **No unseeded randomness:** All random decisions or IDs should come from the provided seeded RandomGenerator or IdGen.
> - [ ] **No threads or standard futures:** The code should not directly use Thread, Executor, or CompletableFuture.
> - [ ] **No direct network or disk I/O:** All I/O must go through the Network and FileIO interfaces.
> - [ ] **No iteration over unordered collections:** Iterating over HashMap, HashSet, or ConcurrentHashMap can visit entries in a different order across runs. Core state uses LinkedHashMap and LinkedHashSet.
> - [ ] **No cross-thread callbacks across the seam:** Background threads in production adapters must never directly invoke methods or complete futures on the deterministic side. They queue completions for the main thread to drain during tick().

> ❌ **Anti-pattern — unordered iteration:** scanning a HashMap of pending requests to expire timeouts. **Why people do it:** HashMap is the default collection. **Consequence:** expiry order varies across runs, so a rare fault path may replay differently. **Fix:** use LinkedHashMap or LinkedHashSet for core state.

> ❌ **Anti-pattern — cross-thread completion:** the disk thread calling future.complete() directly. **Why people do it:** it is the shortest path from I/O completion to the waiting caller. **Consequence:** storage and replica callbacks run on the I/O thread, creating races that simulation never exercises. **Fix:** queue the completion and let the main thread drain it during tick().

Discipline does not scale, because one HashMap iteration or one clock call can silently break replay on a rare path. The author therefore uses architectural fitness functions:

- **Static:** an ArchUnit suite, DeterminismRulesTest, runs on every build and checks that core packages stay behind the seam and obey the first five rules.
- **Runtime:** static analysis cannot tell which thread runs a lambda, so TickCompletableFuture records its owner thread and rejects complete(), fail() or whenComplete() from any other thread:

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

- **Holistic:** the seeded runner and ConsistencyChecker test system-wide invariants, such as linearizability and crash durability, across many fault sequences.

## Production Cost
Most distributed systems are I/O bound, so a single-threaded loop with asynchronous, batched I/O avoids locking and context switching, as in Redis, FoundationDB and TigerBeetle. Handlers must never block or do long CPU work, and scaling means running more processes.

## Ease of Building and Evolving
The simulation foundation takes upfront effort, but the protocol code becomes plain single-threaded logic that is easier to write and evolve. The fault scenario tests then act as a fitness function that makes later changes safer.

## Learning and the Use of LLMs
A deterministic harness gives an LLM a clear interface for exploring systems. The author describes an LLM turning the quorum linearizability diagram in Designing Data-Intensive Applications into an executable Tickloom scenario.

## Acknowledgments
The author thanks Martin Fowler, Rebecca Parsons, Will Wilson and Joran Dirk Greef for reviewing drafts.

## Significant Revisions
17 November 2026: published.

## 🧠 Mental models
### Progress is controlled by the test
**The model:** a run is fixed by the moments the clock, processes, network and disk advance. Nondeterminism is every moment the test did not choose.
**How to apply it:** for a flaky failure, find which of these four was left to the environment and make the test drive it.


### Seed the chaos, log the schedule
**The model:** random faults are reproducible when one seed fixes every random choice, and the logged schedule explains the failure.
**How to apply it:** route all randomness through one seeded object and record the fault schedule with every run.

## 🔑 Key takeaways
- Live chaos testing finds real bugs, but without a seed and a controlled execution order they cannot be reproduced.
- Availability and correctness differ: a quorum store can answer every request and still return a stale value after an acknowledged write.
- In the example, a 10-tick timeout against a 20-tick delay fails the write on every run, so the outcome follows the numbers the test chose.
- Determinism must be designed in: a single-threaded core, asynchronous I/O interfaces and ordered collections are prerequisites, and automated checks enforce them.

## 🏛️ For Kenji
Few architects build a simulator, but the question applies to any replicated service: which clock, network and disk decisions do your tests leave to the environment? This is my interpretation, not the author's.

## 📚 Reading plan
### From the article
- [Jepsen](https://jepsen.io): the consistency-testing suite behind the history-checking approach.
- [Fallacies of Distributed Computing](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing): the assumptions that gray failures break.
- [Gray failures paper](https://www.microsoft.com/en-us/research/wp-content/uploads/2017/06/paper-1.pdf): the source the article cites for gray failures.
- [Tickloom](https://github.com/unmeshjoshi/tickloom): the Java code the article walks through.
- [FoundationDB](https://github.com/apple/foundationdb): the system that pioneered this technique for databases.
- [TigerBeetle](https://github.com/tigerbeetle/tigerbeetle): the tick-driven, single-threaded design the article follows.
- [ArchUnit](https://www.archunit.org/): the tool behind the static determinism rules.
- [Building Evolutionary Architectures](https://learning.oreilly.com/library/view/building-evolutionary-architectures/9781492097532/ch02.html): the source of the fitness-function idea.
- [Designing Data-Intensive Applications](https://dataintensive.net/): the quorum linearizability example the article turns into a test.

### To go further
- USENIX ATC 2020 paper on fsync error scenarios, cited in the article, for storage crash testing.
