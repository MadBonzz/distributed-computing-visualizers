# Distributed Algorithms Failure Lab

Interactive simulations for Cornell CS 5414, Fall 2026. [Open the lab](https://distributed-computing-visualizers.vercel.app/) or download [the standalone HTML file](./distributed_failure_lab.html) and open it in your browser.

## Try synchronous consensus

The lab opens with four processes and a crash bound of one. Click **Run round 1** to exchange inputs. A **+** in the table marks a value learned in that round. Click **Run round 2** to see each survivor decide the minimum value it knows.

Choose **Dangerous chain across rounds** in the **Example** menu. P0 starts with 0 and the other processes start with 1. In round 1, P0 sends only to P1 and crashes. In round 2, P1 sends only to P2 and crashes. Check P3’s known values before running round 3. **Undo step** takes you back one action so you can change the execution.

## Change an execution

- Select a process in the table to edit its input, state, or failure. **Edit setup & failures** also contains process counts, the fault bound, and protocol-specific actions.
- In synchronous consensus and TRB, one click runs one round. Open **Round history and network** to inspect which messages were sent.
- In message-driven simulations, each guided step processes a group of currently deliverable messages of one type, ballot, or round. Open **Network and message scheduling** between steps to deliver one message, reorder replies, or inject faults. The guided and manual controls act on the same execution.
- Open **Execution trace and property checks** to inspect events or save and load an execution as JSON. An assumption check applies to the execution being inspected. Manual state edits can create configurations outside the protocol’s fault model.

## Simulations

| Simulation | Experiment |
| --- | --- |
| Synchronous crash consensus | Change inputs and arrange partial sends to see why a hidden minimum can take f + 1 rounds to propagate. |
| Terminating reliable broadcast | Crash the sender before or during its send. Compare immediate delivery of the value, relaying in the following round, and delivery of SF. Switch between basic and early-stopping TRB in setup. |
| Paxos | Load **Chosen A, two acceptors crash**. P3 wants B at ballot 9. Inspect how P2’s persisted acceptance of A affects P3’s choice after its promises arrive. |
| Ben-Or consensus | Change binary inputs, the first n − f replies, or the coin outcomes at individual processes. The guided action advances the current A or B messages. |
| Two-phase commit | Have a participant vote NO, or crash the coordinator after participants log YES. Ask peers whether anyone has a final decision. |
| Three-phase commit | Deliver PRECOMMIT to one participant, crash the coordinator, then elect a survivor to collect states. The recovery model assumes reliable communication and knowledge of which processes survived. |
| Chandy–Lamport snapshot | Deliver a transfer after the receiver records its balance and before the incoming channel closes. Inspect the recorded in-transit tokens. |
| Lamport and vector clocks | Compare local, send, and receive events at different processes. |
| Primary–backup replication | Follow a write and its acknowledgments. The stale-backup promotion example illustrates an unfenced failover. |
| Heartbeat failure detector | Compare a crashed peer with a healthy peer whose heartbeats arrive late. This is an illustrative timeout-based detector. |

**Supplemental demos** contains Bracha Byzantine reliable broadcast and eager echo broadcast. These were added for comparison. The course broadcast section contains TRB.

[Fall 2026 course schedule](https://www.cs.cornell.edu/courses/cs5414/2026fa/schedule.html) · [Synchronous consensus, slide 30](https://www.cs.cornell.edu/courses/cs5414/2026fa/notes/Week6.pdf#page=30) · [Basic TRB, slide 47](https://www.cs.cornell.edu/courses/cs5414/2026fa/notes/Week6.pdf#page=47) · [Early-stopping TRB, slide 50](https://www.cs.cornell.edu/courses/cs5414/2026fa/notes/Week6.pdf#page=50)
