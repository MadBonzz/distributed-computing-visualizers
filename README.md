# Distributed Algorithms Failure Lab

Interactive simulations for Cornell CS 5414, Fall 2026. [Open the lab on Vercel](https://distributed-computing-visualizers.vercel.app/) or download [the standalone HTML file](./distributed_failure_lab.html) and open it directly. No installation is required.

Select a simulation and press the green button to run the next protocol step. Click a node or its row on the right to change its inputs, crash or recover it, and inspect what it has received. Click a connection to cut it in both directions. Click again to restore it. In asynchronous protocols, a cut holds messages until their link is restored. In synchronous protocols, a cut can prevent that round’s value from reaching a peer.

A node with queued messages has a **Receive** button. Pick the sender and message to let one participant advance before the others. **Undo** reverses the most recent step or edit. **Restart** returns to the starting configuration for the execution. **Settings** changes the node count and fault bound. The queue, trace, assumption checks and JSON import/export are below the graph.

## Two runs to try

**Ben-Or.** Open the three-node run. Click P0 and P2 and set their initial bits to 1, matching P1. Run phase 1 and read the two A messages each node used. Cut both links to P2, then run phase 2. P0 and P1 decide 1 and their values lock. P2 has only its own B message. Restore P0–P2 and run phase 2 again to let P2 decide. With mixed inputs, set the coin on each node and use **Receive from** to change the senders it can hear. [Open Ben-Or](https://distributed-computing-visualizers.vercel.app/?sim=benor)

**Paxos.** Click P0, enter a ballot and a value, and propose in slot 1. Follow the prepare, promise and accept steps. A majority of acceptances chooses a value before every learner hears it. Add a slot, start a competing run there and switch back at any time. Each slot runs its own prepare and accept phases. The table for every node shows its promise, accepted value and ballot, and learned value in each slot. [Open Paxos](https://distributed-computing-visualizers.vercel.app/?sim=paxos)

## Experiments

| Simulation | Try this on the network |
| --- | --- |
| Synchronous crash consensus | Give one node the smallest input. Under **Crash during a round**, select a single recipient. Run a round, then click that recipient to see exactly who sent it which values. |
| Terminating reliable broadcast | Set a sender and message, schedule a partial send, and see who relays it next round. Switch between basic and early-stopping TRB before starting. |
| Ben-Or | Change a bit, coin or incoming link, then compare the first n − f A and B messages used by each process. |
| Paxos | Try the **Chosen A, two acceptors crash** example. Inspect which promise makes a new proposer adopt A when it wanted B. |
| Two-phase commit | Set one participant’s vote to NO. Alternatively, have everyone vote YES, crash the coordinator and ask peers for a decision from a prepared node. |
| Three-phase commit | After votes arrive, let just one participant receive PRECOMMIT. Crash P0, elect a survivor and follow its state collection and recovery. |
| Chandy–Lamport snapshot | Send tokens, record a balance and receive one transfer or marker at a time. See which incoming channel records the transfer and when the marker closes it. |
| Lamport and vector clocks | Create independent local events at two nodes and compare them. Send a message, deliver it, and compare the send with the receive. |
| Primary–backup replication | Write a value. Let one backup receive it, then crash the primary. Promote the lagging backup and compare log and commit indices. |
| Heartbeat failure detector | Change the timeout and let ticks pass while holding heartbeats. See which live peers are suspected, then deliver one heartbeat to clear that suspicion. |
| Bracha broadcast (supplemental) | Mark the sender Byzantine and send different INIT values to individual peers. Compare the distinct ECHO and READY counts for each value at each receiver. |
| Eager echo broadcast (supplemental) | Choose a single first recipient, send, and crash the sender. Watch the first recipient forward the message and see which peer each later node heard from. |

The Broadcast section contains TRB. Bracha and eager echo appear under **Supplemental demos**. Faults that exceed the selected algorithm’s assumptions can change the outcome. The primary–backup simulation includes unsafe, unfenced promotion so you can observe stale leadership.

[Fall 2026 course schedule](https://www.cs.cornell.edu/courses/cs5414/2026fa/schedule.html) · [Synchronous consensus](https://www.cs.cornell.edu/courses/cs5414/2026fa/notes/Week6.pdf#page=30) · [Basic TRB](https://www.cs.cornell.edu/courses/cs5414/2026fa/notes/Week6.pdf#page=47) · [Early-stopping TRB](https://www.cs.cornell.edu/courses/cs5414/2026fa/notes/Week6.pdf#page=50)
