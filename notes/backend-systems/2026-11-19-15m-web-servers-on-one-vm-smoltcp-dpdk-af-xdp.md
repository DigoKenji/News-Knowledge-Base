---
type: Article Summary
title: 15M web servers on one VM – smoltcp, DPDK, AF_XDP
description: Kernel socket memory and the 16-bit port space, not CPU, cap servers per VM; userland networking removes most of that cost.
resource: https://kapteko.com/15-million-webservers
tags:
- backend-systems
generated:
  by: claude-haiku (Project_News daily run)
  at: '2026-11-19T07:00:00-03:00'
sources:
- resource: https://kapteko.com/15-million-webservers
  title: 15M web servers on one VM – smoltcp, DPDK, AF_XDP
  author: Hacker News (100+ pts)
status: stable
source_url: https://kapteko.com/15-million-webservers
source: Hacker News (100+ pts)
source_type: blog
published: '2026-11-19'
domain: backend-systems
concepts: []
selected_by: discovery
content: open
complete: 'yes'
notion_url: https://app.notion.com/p/41dbaf74172d811a96f8f91ad36de359
---

## 🎯 Core thesis
The kernel's per-socket memory and the 16-bit port space, not CPU, limit how many web servers one VM can run, and moving the TCP/IP stack and scheduling into userland removes most of that cost.

## 🧭 Why the author wrote it
The author opens with a joke: if every one of the 8 billion people had a web server, demand would fall to zero and cybersecurity would be solved. The real experiment is whether one VM with 10GB of RAM and one CPU core can host 15 million web servers, and the article works through each limit it hits. If the thesis is right, RAM per server becomes the design constraint, and the network stack moves into the application.

## Web server

The author chooses to learn from first principles "instead of vibe coding it all". He starts from the few syscalls behind a web server and writes them in C so that every call is explicit. He says any HTTP library ultimately comes down to these calls, with the caveat "well, yes, but actually no", meaning the simplification is only approximate.

> ❌ **Anti-pattern — vibe coding the infrastructure:** Generating networking code from prompts and running it without knowing what the calls do. **Why people do it:** The answers are fluent, and in the author's words it is "too easy to accept its answers without thinking critically". **Consequence:** Limits such as port exhaustion and out-of-memory errors appear only once the code runs at scale. **Fix:** Learn the few syscalls and test each assumption on the real system, as the article does.

### Syscall
A syscall is a function executed by the OS kernel. The server in the article uses five: `listen` asks the kernel to accept connections on a socket, `accept` takes one connection, `recv` reads data from it, `send` writes data to it, and `close` ends it. The author later admits he missed `socket`, which the loop also needs. Every higher-level HTTP library, he argues, is built from these calls.

> 🔎 Added context: a file descriptor is the small integer a process uses to refer to an open socket or file.

### Listening socket
The C listing creates a socket, calls `listen` with a backlog of 128, and then loops on `accept`. For each client it calls `recv`, sends a fixed HTTP 200 reply with a two-byte body, and closes the client socket. The author estimates this loop handles "around 65k RPS".

> 🔎 Added context: the second argument to `listen` is the backlog, the number of pending connections the kernel will queue.

```c
int main(void)
{
    // We use port 0 which makes OS return any available port
    int server = socket(AF_INET, SOCK_STREAM, 0);
    if (listen(server, 128) < 0)
        return 1;
    const char reply[] = "HTTP/1.0 200 OK\r\nContent-Length: 2\r\n\r\nOK";
    for (int client; (client = accept(server, NULL, NULL)) >= 0;) {
        if (recv(client, NULL, 4096, MSG_TRUNC) > 0)
            send(client, reply, sizeof reply - 1, MSG_NOSIGNAL);
        close(client);
    }
}
```

**What this does:**
- `socket(AF_INET, SOCK_STREAM, 0)` creates an IPv4 TCP stream socket, which is the listener.
- `listen(server, 128) < 0` returns exit code 1 if listening fails.
- `reply` is a complete HTTP/1.0 response; `sizeof reply - 1` drops the trailing NUL byte from the length sent.
- `recv(client, NULL, 4096, MSG_TRUNC)` passes no buffer and only checks that data arrived (return value above 0); the request bytes are discarded.
- The listing has no `bind()` call. The comment says port 0 makes the OS choose a port, and the next section shows what that choice costs.

Now the author finishes the article by starting one process per server in a shell loop:

```bash
for i in $(seq 1 15_000_000); do
    echo $i
    ./server &;
done
```

Output as printed in the article:

```text
...
...
28232
program exited with error 1
```

### Ephemeral port range
`listen` failed because the kernel ran out of ports. The server uses `port=0`, so the kernel picks a free port from `ip_local_port_range`, which is small by default:

```bash
sysctl net.ipv4.ip_local_port_range
```

Output: `net.ipv4.ip_local_port_range = 32768    60999`

The author explains why so many ports are skipped. Port 0 was historically for outbound connections: when you call `connect('example.com', 80)`, Linux connects out and then allocates a random local port from `ip_local_port_range` to receive the reply. Ports 1-1024 require root. Ports 1024-32768 are skipped so that dynamically allocated ports do not collide with existing apps, since you do not want `connect('example.com')` to allocate port 3306, the MySQL port. Ports 60999-65535 are excluded for historical reasons dating back to IP Masquerading. Overriding the range to 1-65535 is still short of 15 million. A UDP server and a TCP server can share a port, so the author doubles that to about 128k.

> 🔎 Added context: `sysctl` reads and sets kernel parameters, and the `net.ipv4` prefix refers to IPv4 networking settings.

### Port 0
The author says port 0 is special and that you cannot listen on it, then adds in parentheses that "actually, you can", pointing to a post on the topic (see the reading plan). The point for the experiment is that port 0 asks the kernel to choose a port, which is what exhausts the range.

### Connection tuple and the 65k ceiling
Each listening server is identified by a `(protocol, IP, port)` tuple. The 65k limit comes from the TCP standard, where a port is a `u16`. The author tries `find '*.c' -exec sed -i 's/u16/u32/' \;` to widen the type in the kernel source, and notes it would not help because normal clients still could not connect. Since the tuple includes the IP, using more IP addresses also multiplies the number of listeners. Connecting 450 network cards (15M / 32k = 450) would give 450 unique IPs.

```bash
find '*.c' -exec sed -i 's/u16/u32/' \;
```

> ❌ **Anti-pattern — buying hardware for a software limit:** "whenever you have a software problem, you just throw money at the hardware", adding network cards to solve a port count. **Why people do it:** It is faster than changing the software and feels like a fix. **Consequence:** The article's arithmetic needs 450 cards for ports alone, and the author then finds that kernel memory per socket is the next limit. **Fix:** Use AnyIP, which gives one process many addresses from a single route instead of physical cards.

### AnyIP
AnyIP lets a host listen on a whole subnet. Many VPS and dedicated providers give a whole IPv6 subnet, and one route command makes the entire `/64` local to the machine. That yields 18 quintillion addresses "(plus or minus one)". The same approach works for IPv4, but the author estimates that a subnet with 15M addresses costs around $250 million, so the command must be adjusted for your environment.

A reader's note, as the author relays it, says the network card is the only hardware component that did not get 100 times more expensive in the last year. The author asks whether the problem can be solved in software, and AnyIP is the answer.

```bash
sudo ip -6 route add local 2001:db8:1234:5678::/64 dev lo
```

The server now picks the nth IP from the range:

```c
// Read IP index from CLI, shift base IP by it
int i = read_cli_param(argv[1]);
const uint32_t base_ip = 0x0a400001;   // ex. 10.64.0.1
addr.sin_addr.s_addr = htonl(first_ip + i);   // shift, ex. 10.64.0.2
```

**What this does:**
- The index `i` comes from the first command-line argument, so each process gets a different address.
- `base_ip` is the starting address, `10.64.0.1` in the comment.
- `htonl(first_ip + i)` converts the host-order sum to network byte order before storing it in `sin_addr`.
- The listing names `base_ip` but adds `first_ip`, as printed in the article; the two names are inconsistent.

The full loop with one process per address:

```bash
for i in $(seq 1 15_000_000); do
    echo $i
    ./server $i &;
done
```

Output: `Out of memory`

### One process per server
The author measures one server at about 1.5MB of RAM. Running 15 million of them would need about 20TB, and he calls that "a few bytes more than our 10GB budget".

> 🔎 Added context: 15 million times 1.5MB is 22.5TB, so the article's 20TB is approximate. The gap to the 10GB budget is roughly 2,000 times, which is far more than "a few bytes".

## Runtime

The author notes that he had already written about the overhead of running millions of processes ("Running 10 million Codex CLIs on one server - LFI processes") and about a specialised userland scheduler ("Userland Linux scheduler in Rust"). The answer is one process that runs all guests inside a custom runtime, as V8 can run millions of guests in one process.

### Custom runtime and LFI sandboxing
The runtime uses LFI for sandboxing. LFI gives memory isolation and replaces every syscall with a call to the host handler, so the host can implement I/O however it likes. To run many guests, the runtime loads them all into the main process's memory and makes the CPU switch between them. In the author's words, "we become our own micro OS".

### Memory budget per guest
The author asks how much memory each guest can have: "Ten Great Britains divided by 15 million gives around half of kilo britain. Not much."

> 🔎 Added context: 10GB divided by 15 million is about 667 bytes per guest if GB means 10^9 bytes.

Only the state needed to suspend a guest matters, so the author reviews the accept loop:

```c
for (int client; (client = accept(server, NULL, NULL)) >= 0;) {
    if (recv(client, NULL, 4096, MSG_TRUNC) > 0)
        send(client, reply, sizeof reply - 1, MSG_NOSIGNAL);
    close(client);
}
```

It has two state variables, `server` and `client`, which are both file descriptors. A suspended guest needs its instruction address and those two descriptors:

```rust
struct Context
{
    pc: u64,        // Instruction address to return to
    fd_server: i32, // server file descriptor
    fd_client: i32, // client file descriptor
}
```

**What this does:**
- `pc` is the instruction address where the guest resumes after the scheduler switches back.
- `fd_server` and `fd_client` are the two file descriptors the loop uses.
- The author sizes this at 16 bytes per guest, with the full `u64` and `i32` fields kept on purpose as a safety margin.
- Bit packing could reduce this to about 8 bytes per guest, according to the author.

At 16 bytes per guest, the author says 15 million suspended guests need about 220MB.

> 🔎 Added context: 15 million times 16 bytes is 240 million bytes, slightly more than the 220MB stated in the article.

### Idle and active connections
The author asks an LLM how much a TCP connection takes. It answers that an idle connection takes zero bytes and an active one about 5kb. He builds the budget on that answer: 14 million idle connections at zero bytes, 220MB of scheduling state for 15 million guests, and 1 million active connections at 5kb, which is 5GB. That is about half of the RAM budget, and he says he rarely exceeds his own expectations.

To validate it, he runs 14 million listeners that do nothing:

```c
int main(void) {
    // Run 14M of idle connections - just listen and do nothing
    // SHOULD CONSUMER ZERO MEMORY!
    for (int i = 0; i < 14_000_000; i++) {
        int server = socket(AF_INET, SOCK_STREAM, 0);
        listen(server, 1)
    }
}
```

**What this does:**
- Each loop iteration creates a socket and puts it in the listening state with a backlog of 1.
- The comment states the assumption under test, that idle sockets use no memory.
- The listing is printed without the trailing semicolon after `listen(server, 1)`, as in the article.
- The test is the author's check of the LLM answer, and it fails.

```bash
./run_15m_idle_servers
dmesg | tail -n 1
```

Output: `Out Of Memory`, and the last kernel log line reads `'SHOULD CONSUMER ZERO MEMORY' - what are you, stupid?`. The author notes the kernel still carries leftover comments from Linus.

> ❌ **Anti-pattern — trusting an unverified estimate:** Accepting a generated number, here "idle connection takes zero", without a test. **Why people do it:** The answer sounds authoritative and fits the plan. **Consequence:** The 15-million run runs out of memory, and the budget built on the zero-byte assumption fails. **Fix:** Measure the cost of the smallest unit, one idle listener, before sizing the design.

## Networking in the kernel

The author admits he missed one syscall in the loop: `socket`. This program creates 10 million sockets and runs out of memory in a 10GB VM:

```c
int main(void)
{
    for (int i = 0; i < 10_000_000; ++i) {
        socket(AF_INET, SOCK_STREAM, 0);
    }
}
```

The `socket` call allocates around 3300 bytes whether the socket is active, listening or idle. The author says the kernel was written when RAM was big and cheap.

### Kernel memory per socket
The author asks an AI where the memory goes:

| Struct | sizeof | Description |
|---|---|---|
| struct tcp_sock | 2240B | TCP state, queues, timers, sequence numbers, congestion control, retransmit and delivery bookkeeping |
| struct socket_alloc | 768B | A 128-byte struct socket with an embedded wait queue, plus a 608-byte sockfs inode and padding |
| struct file | 184B | Open-file operations, flags, credentials, reference count, and the pointer back to the socket |
| struct dentry | 192B | The sockfs pseudo-name, inode pointer, and VFS lookup and lifetime bookkeeping |
| Total | 3384B | |

The prose says "around 3300 bytes" and the table total is 3384B. The author concludes that the kernel's use of that memory cannot be reduced much "without rewriting half the kernel". He had hoped to store a `buffer: [u8; 3300]` in its place.

### OSI layers and encapsulation
The author climbs down the OSI model: HTTP at the application layer (L7), TCP at transport (L4), IP at network (L3), Ethernet at data link (L2), and the physical layer (L1). Each layer wraps the data from the layer above it. The article's example packet shows the fields each layer adds:

| Layer | Fields shown in the article |
|---|---|
| TCP (L4) | Src port 50123, Dst port 80, Seq 1001, ACK 7001, Flags ACK |
| IP (L3) | Src IP 192.0.2.10, Dst IP 198.51.100.20, TTL 64, Protocol 6 (TCP) |
| Ethernet (L2) | Dst MAC 02:00:00:00:00:fe, Src MAC 02:00:00:00:00:01, EtherType 0x0800 (IPv4), FCS CRC |

The payload is `GET /`, wrapped at each layer.

> 🔎 Added context: a MAC address is the hardware address of a network interface, used on the local Ethernet segment.

### Stateful and stateless layers
TCP/IP is stateful, but everything below it is stateless. The lower layers do not know or care about the payload, so the author argues that you can send new IP packets or Ethernet frames with handcrafted binary data. You then reimplement TCP and IP yourself. Embedded developers have done this "millions of times". Adam Dunkels's Miniweb combines a custom TCP/IP implementation and an HTTP server that together need only 28 bytes. Less extreme options named in the article are gVisor, which reimplements the whole OS including the network stack, Seastar, a runtime with its own network stack from the ScyllaDB team, lwIP, a popular C TCP/IP implementation, and F-Stack, which uses the FreeBSD TCP/IP stack.

### smoltcp
The author chooses Rust and smoltcp, described as:

> smoltcp is a standalone, event-driven TCP/IP stack that is designed for
> bare-metal, real-time systems. smoltcp does not need heap allocation at all

Adding it is one line, but smoltcp "is essentially a smart parser and can't do I/O". It needs a device underneath it:

```bash
cargo add smoltcp
```

smoltcp itself provides two device options: TUN and TAP (below). DPDK is the third.

```rust
fn main() -> std::io::Result<()> {
    let (mut device, config) = match std::env::args().nth(1).as_deref() {
        Some("tun") => (
            TunTapInterface::new("smoltun", Medium::Ip)?,
            Config::new(HardwareAddress::Ip),
        ),
        Some("tap") => (
            TunTapInterface::new("smoltap", Medium::Ethernet)?,
            Config::new(EthernetAddress([0x02, 0, 0, 0, 0, 2]).into()),
        ),
        _ => panic!("pass tun or tap"),
    };
    let mut iface = Interface::new(config, &mut device, Instant::now());
    iface.update_ip_addrs(|addrs| {
        addrs
            .push(IpCidr::new(IpAddress::v4(192, 0, 2, 2), 24))
            .unwrap();
    });
    let mut sockets = SocketSet::new(Vec::new());
    // Main network loop
    loop {
        let now = Instant::now();
        iface.poll(now, &mut device, &mut sockets);
        wait(device.as_raw_fd(), iface.poll_delay(now, &sockets))?;
    }
}
```

**What this does:**
- The first command-line argument selects the device: `tun` for IP-level, `tap` for Ethernet-level, anything else panics.
- The TAP branch uses a fixed locally administered MAC address, `02:00:00:00:00:02`, because Ethernet frames need one.
- `update_ip_addrs` gives the interface the address 192.0.2.2/24.
- The loop calls `poll` to move packets through smoltcp, then waits on the device's file descriptor until smoltcp's `poll_delay` says to run again.
- The design is event-driven: no thread waits on a socket, and the stack only runs when the device or a timer needs it.

### TUN and TAP
TUN is a virtual network interface provided by the kernel that operates at the IP layer. TAP operates one layer lower, at Ethernet. Both are easy to use with smoltcp, which the article says provides them out of the box.

### DPDK
DPDK lets a program talk to the network card with Ethernet frames, bypassing the kernel. The author gives the cost of a kernel path: a syscall might take about 500ns, and copying data from kernel to userspace might take another 1000ns depending on buffer size. High-frequency trading firms saturating a 400Gbps interface must process up to 600 million packets per second, roughly 1.7ns per packet. With DPDK you write to a memory-mapped buffer to send and read from one to receive, with no syscalls.

DPDK also avoids interrupts. It works with a busy loop that uses an entire CPU core, moving data in both directions:

```text
loop {
    send_from_app_memory_to_network();
    receive_from_network_to_app_memory();
}
```

**What this does:**
- The pseudocode is the author's sketch, not a real API; the real driver code is in the DPDK library.
- Each iteration pushes outgoing data to the card and pulls incoming data from it.
- Because the loop never sleeps, it costs a full core, which is the trade for predictable latency.

> ❌ **Anti-pattern — binding the only NIC to DPDK over SSH:** Enabling the DPDK driver on the network interface you are connected through. **Why people do it:** The article does not say; DPDK is the fastest option, so it is tempting to try on the machine you have. **Consequence:** The driver takes the device away from the OS, so the interface disappears from `ifconfig` and SSH access is lost. **Fix:** The author suggests AF_XDP, which keeps the kernel owning the device so SSH still works, and warns that DPDK needs exclusive access.

### Interrupts and livelock
Each packet arrival fires an interrupt. The CPU stops the app, enters the kernel, processes the interrupt and returns. With a very high packet rate the app is interrupted constantly. The author calls this livelock and says debugging it in production is "its own level of hell". DPDK avoids interrupts entirely, at the cost described above.

### AF_XDP
Linux's AF_XDP subsystem does something similar with less disruption. It bypasses the kernel network stack and uses memory-mapped buffers for zero-copy I/O. The kernel still owns the device, and interrupts still happen. When a packet arrives, the kernel runs a simple eBPF filter that either sends it straight to the userspace buffer or processes it normally, so SSH still works. An alternative mode, `SO_PREFER_BUSY_POLL`, gives a busy loop without interrupts at the cost of a CPU core. DPDK still beats it because it bypasses the kernel completely, but AF_XDP is very close and easier to manage.

> 🔎 Added context: eBPF is a kernel facility for running small, verified programs at hook points such as packet reception.

### Device trait
To support DPDK and AF_XDP in smoltcp, you implement smoltcp's `Device` trait, which takes three methods for full control. Alternatively, the existing crates `rust-dpdk-net` (for DPDK) and `xsk-rs` (for AF_XDP) can do the work.

### Final flow
The author's design, in order:
1. The runtime loads 15M web server guests into its memory.
2. The runtime scheduler starts executing guests one by one.
3. Guest syscalls are intercepted by the runtime and replaced by the smoltcp network stack and its adapters.
4. All 15M guests start listening, each on its own address.
5. The scheduler polls for incoming data from the smoltcp adapters, wakes up a guest, makes it process the data, and switches to the next.

### Benchmark result
The article's benchmark chart is not in the extracted text, so the figures below are the ones stated in prose. The kernel network stack exhausts RAM at around 2.5 million web servers, and requests per second drop as more servers run. With smoltcp, RAM grows more slowly: about 620 bytes per idle server and around 1 kilobyte per active one, reaching 16 million servers. DPDK is the fastest in this benchmark, with AF_XDP just behind.

The author's conclusion is that moving the network stack to userland gives full control of RAM and performance, and that a custom runtime can run guests faster. He cautions that "simply" is a stretch: a proof of concept is easy, but a compatible Linux runtime with 100% POSIX-compatible syscalls is more work, "And another article."

## 🧠 Mental models
### Wrong level of abstraction
**The model:** When a problem cannot be solved at your current layer, the article argues you are probably working at the wrong level of abstraction, and the fix is to descend a layer.
**How to apply it:** When a resource limit resists tuning, list the layers below your code and ask which one owns that resource.

### Each layer wraps the one above
**The model:** Each protocol layer adds its own header around the data from the layer above, so HTTP sits inside TCP, inside IP, inside Ethernet.
**How to apply it:** When designing a bypass, identify which headers your code must produce and which lower layers you can take over.

### Your own micro OS
**The model:** A runtime that holds many guests in one process and switches between them acts as a small operating system, and each suspended guest needs only the state its syscall loop uses.
**How to apply it:** Count the state a loop really needs before choosing between processes, threads and in-process scheduling.

### Busy polling trades a core for predictability
**The model:** A busy loop that polls the device avoids interrupts and livelock, but it occupies an entire CPU core.
**How to apply it:** Before choosing DPDK or AF_XDP busy polling, decide whether the latency gain justifies a dedicated core.

## 🔑 Key takeaways
- Port space caps a single IP at about 64k listeners, so the author scales by adding addresses (AnyIP), not by adding ports.
- Each kernel socket costs about 3384 bytes before any traffic, so roughly 10 million sockets exhaust a 10GB VM.
- An idle connection is not free: the author's test contradicted the LLM answer that idle connections cost zero bytes.
- Per-guest scheduling state of 16 bytes (an instruction address and two file descriptors) makes 15 million suspended guests cost about 220MB in the author's model.
- Userland smoltcp with a custom scheduler reached 16 million servers in the article's benchmark, at about 620 bytes per idle server, against the kernel stack's exhaustion near 2.5 million.
- DPDK is the fastest bypass but takes the NIC away from the OS; AF_XDP keeps the kernel in charge and is close behind.
- A proof of concept is easy to build; a POSIX-compatible runtime is a separate and harder project, according to the author.

## 🏛️ For Kenji
> Interpretation, not the author's: when a scaling limit appears in an architecture review, this article is a reminder to locate it precisely (ports, kernel memory, interrupts or the runtime) before choosing a fix, and to price each fix's operational risk, such as losing management access when a NIC is taken over by DPDK. The question it raises for a platform decision is: if a team proposes a kernel-bypass data path such as DPDK or AF_XDP, what measured latency or memory gain would justify giving up kernel visibility and a fallback for management traffic?

## 📚 Reading plan
### From the article (its References section)
- [How we handle 15M websocket connections on a single server](https://medium.com/beyond-localhost/how-we-handle-15M-websocket-connections-on-a-single-server-8917c3952f59) — the source the author cites for the 15M-connection idea.
- [The port that does not exist](https://acid.vegas/blog/the-port-that-does-not-exist/) — the author's link on why port 0 is special and whether you can listen on it.
- [DPDK documentation](https://doc.dpdk.org/guides/index.html) — the main DPDK documentation site, which the author calls a great overview.
- [AF_XDP overview](https://docs.kernel.org/networking/af_xdp.html) — the kernel documentation for the AF_XDP subsystem.
- [Ten million web servers proof of concept](https://github.com/artemyarulin/AI10M/tree/main/poc/ten-million-webservers) — the author's proof-of-concept code for the experiment.

### To go further
- Running 10 million Codex CLIs on one server - LFI processes — the author's earlier post on why millions of processes add overhead (title only, no link in the article).
- Userland Linux scheduler in Rust — the author's earlier piece on a specialised userland scheduler with minimal RAM per task (title only).
- smoltcp, rust-dpdk-net and xsk-rs — the Rust TCP/IP stack and the DPDK and AF_XDP crates named in the article.
- Miniweb, gVisor, Seastar, lwIP and F-Stack — the alternative network stacks and runtimes the author lists.
