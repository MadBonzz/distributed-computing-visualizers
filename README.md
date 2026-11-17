# Distributed Algorithms — Failure Lab

An offline, interactive browser workbench for exploring adversarial executions of distributed algorithms from Cornell CS 5414 (Fall 2026), plus a supplemental Byzantine reliable-broadcast algorithm.

## Run

Open `distributed_failure_lab.html` in Chrome, Firefox, or Edge. No server, packages, login, or internet are needed after download. Everything executes locally inside this one HTML file.

## Start with the edge-case library

1. Choose a protocol from the left sidebar.
2. Click **Edge cases** to load a state designed to expose a particular correctness or liveness condition.
3. Change `n` and `f` (click **Apply / reset**), edit a node by selecting it, and/or crash/recover nodes.
4. In asynchronous protocols, pick individual messages and choose **Deliver**, **Drop**, **Duplicate**, **Edit**, or **↑/↓** to reorder. **Edit** lets you inject a Byzantine-like corrupted or forged message.
5. Use **Deliver next**, **Deliver 20**, or **Random 20** when you want the system to progress.
6. In synchronous protocols, instead use **Next round** and configure nodes to crash after sending to chosen recipients.
7. Watch the network diagram, durable state, event trace, and property checks. Use **Undo** and **Export state** to branch experiments. **Import state** loads saved JSON runs.

## Included workbenches

| Algorithm | What you can explore |
| --- | --- |
| Single-decree Paxos | Multiple proposer ballots; promise/accept/accepted; durable accepted ballots and values; quorum intersection; majority failures; leader preemption and recovery |
| Ben-Or randomized consensus | Initial binary inputs; first n−f A/B arrivals; adversarial scheduling; crash faults; coin override; probabilistic termination |
| Synchronous crash consensus | f+1 rounds; min-value flooding; adversarial dangerous chains; mid-round partial sends and crashes |
| Terminating reliable broadcast (TRB) | Distinguished sender; relaying; sender crash after partial transmission; delivery of value or SF at f+1 |
| Bracha Byzantine reliable broadcast | Byzantine equivocation; authenticated sender identities; INIT/ECHO/READY; n > 3f; corrupt ECHO/READY messages; Byzantine sender non-termination |
| Crash-fault eager echo broadcast | Relaying a message after its sender crashes; comparison to TRB |
| Chandy–Lamport snapshot | FIFO channels; markers; local state, in-transit messages and conservation of transferred tokens |
| Lamport and vector clocks | Local/send/receive events; interleaving; scalar versus vector timestamps |
| Two-phase commit (2PC) | YES/NO votes; durable decision log; uncertainty; coordinator crash; cooperative termination and blocking |
| Three-phase commit (3PC) | PRECOMMIT/ACK, coordinator failure, replacement election, collection of participant states, recovery decisions |
| Heartbeat failure detector | False suspicions from delayed messages; threshold selection; actual crash detection |
| Primary–backup (illustrative) | Replicated write acknowledgement; crash and unsafe promotion of a stale backup; lost-update risk without fencing |

## Suggested high-value experiments

**Paxos — previously chosen value after crash**: Open Paxos → Edge cases → *Chosen A, two acceptors crash*. The acceptors P0, P1, P2 persist ballot 5 and value A (a majority of five). P0 and P1 are crashed. Select P3, set ballot 9 and desired value B, then start PREPARE. Deliver the promises and watch P3 adopt A, even though its own desired value is B.

**Paxos — not chosen yet**: Load *Partially accepted, not chosen* and see how a new proposer can choose B if no majority accepted A.

**2PC blocking**: Load *Everyone YES, coordinator crashes*. Ask peers for a decision. All survivors remain PREPARED because no one knows whether the coordinator logged COMMIT or ABORT.

**3PC leader change**: Load *Crash after partial PRECOMMIT*. P1 is committable; P2 and P3 are uncertain. Elect P1, then deliver STATE_REQ/STATE replies, PRECOMMIT and ACK to finish the transaction.

**Chandy–Lamport channel cut**: Load *Message in transit*. P1 sent four tokens before P0 took a snapshot, but P0 records its local state first. Deliver P1→P0 TRANSFER before P1→P0 MARKER. The transfer appears in the channel-state record instead of P0's recorded local balance.

**Bracha**: Load *Byzantine source equivocates*. P0 sends A to two peers and B to another, while also injecting ECHO(A). Even a Byzantine origin cannot cause honest recipients to deliver conflicting values, given n > 3f and reliable authenticated links.

## Model boundaries — important

This is a **single-run executable teaching simulator**, not an exhaustive model checker or a formal proof engine. It permits manually seeded states that may be unreachable under a correct implementation. The monitors identify conflicting visible decisions, bad fault bounds, and other obvious violations, but passing them on one trace is not a correctness proof.

- Chandy–Lamport assumes reliable FIFO channels and no process crashes.
- The synchronous consensus and TRB examples assume synchronous rounds, reliable correct-process links, and at most f crash failures. Crash-in-round partial sends are possible; arbitrary corruption and partitions are outside their correctness model.
- Single-decree Paxos requires unique ballots and durable acceptor promises/accepted values; eventual progress additionally requires a stable successful proposer and a reachable majority.
- Ben-Or in this version requires f < n/2, fair delivery and independent fair coins for almost-sure termination. Manually overriding coins is useful for counterexamples but changes the probabilistic assumption.
- 2PC's uncertainty can genuinely block after all participants vote YES. Durable log state is retained through crashes.
- 3PC's state-collection recovery is a **simplified crash-only teaching variant** requiring accurate knowledge of operational processes and dependable timely communication. Arbitrary network partitions can make 3PC unsafe.
- Bracha requires n > 3f, authenticated message sender identities, reliable delivery between correct nodes, and at most f Byzantine/crashed processes. It is a reliable broadcast protocol, not Byzantine agreement/total order consensus.
- The eager echo and primary–backup workbenches illustrate mechanisms and failure modes; they are not complete production-grade protocols.
- Manually changing arbitrary messages or raw states can violate each algorithm's model assumptions. That is deliberate for studying what breaks.

## Source lectures

Fall 2026 schedule: https://www.cs.cornell.edu/courses/cs5414/2026fa/schedule.html

The app links from each algorithm to its related lecture PDF. Bracha's Byzantine reliable broadcast is supplemental: https://algorithm.tamnd.com/bracha-reliable-broadcast/
