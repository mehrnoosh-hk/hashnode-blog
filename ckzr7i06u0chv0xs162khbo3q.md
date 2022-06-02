## Data Consistency

Data consistency have different meanings in different contexts:

- ACID in the context of transactions 
A in "ACID" means Atomicity which means if a transaction creates some updates to a database, either all are committed or aborted. A transaction transforms the database from one consistent state to another.
- C means Consistent means satisfying application-specific invariants. Like each class with students enrolled should have at least one lecturer. **Preserving invariants relies on Atomicity.**
- Read after write consistency.
- Replication: Replicas should be consistent with other replicas.
- consistency model, in the same state? When? Read operations return the same results.

**In distributed systems**:
We might have a transaction that involves more than one node.
Either all nodes must commit, or all most abort.
If one node crashes in the middle of transactions, then all other nodes abort.

**Atomic commit vs Consensus **

What if the coordinator crashes?
Coordinator writes to disk.
Read decision from disk.
If there is no decision on the disk, then aborts.
What if the coordinator sends a commit to some nodes but not all of them before it crashes?
Problem: what if the coordinator crashes after "prepare message" and before broadcasting decision, other nodes don't know how the coordinator has decided? All nodes stock because they can not abort after sending ok to "prepare message". 
Entire algorithms blocked until coordinator recovers.
There is a fault-tolerant algorithm.
