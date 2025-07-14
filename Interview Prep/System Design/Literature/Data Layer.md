# Replication

TODO

# Partitioning

Challenges:
* **Skew** - unbalanced load across partitions. A partition with disproportionately high load is called a _hot spot_.

## Partitioning methods

**Key range**
* Advantages:
    * Range queries
* Disadvantages:
    * Hot spots in some scenarios
        * E.g. data from a network of sensors, where the key is the timestamp of the measurement. We write data from the sensors to the database as the measurements happen, so all the writes end up going to the same partition (the one for today), so that partition can be overloaded with writes while others sit idle.

**Key hash**
* Advantages:
    * Keys are uniformly (fairly) distributed accross partitions
* Disadvantages:
    * Range queries

**Key lists** - A partition is assigned a list of keys.
* Advantages:
    * More control
        * E.g. popular celebrities/events on social media
* Disadvantages:
    * More housekeeping
    * Not every scenario is fit for this

**Composite partitioning** - Combination of the above partitioning methods. E.g. range partitioning + hash partitioning.
* Advantages:
    * Completeness - Doesn't optimize for a single use case only.
* Disadvantages:
    * TODO: Read about this

[Horizontal vs vertical vs functional partitioning](https://docs.microsoft.com/en-us/azure/architecture/best-practices/data-partitioning)

TODO: What Cassandra did (it seems that DynamoDB also does that)

TODO: Consistent hashing

## Secondary Indexes and Partitioning

**Local indexes** - Each partition has its own secondary indexes.
* Advantages:
    * No overhead
* Disadvantages:
    * Reads require accessing all partitions.

**Global indexes** - Secondary indexes for all partitions are stored independently of partition. Index storage is also partitioned (using a partitioning method of choice).
* Advantages:
    * Reads only access the required partitions.
* Disadvantages:
    * Writes are slower and more complicated, since several partitions of the secondary index need to be updated.
    * Secondary index update is not atomic with the document update. It requires a distributed transaction.

## Rebalancing Partitions

**Fixed number of partitions** - Create many more partitions than there are nodes, and assign several partitions to each node.
* _Grow_ -> Add new node and steal a few partitions from every existing node until partitions are fairly distributed once again.
* _Shrink_ -> Remove the node and fairly distribute the node's partitions to other nodes.
* Drawbacks:
    * Doesn't work very well with _key range_ partitioning.
    * Need to determine the optimal amount of partitions ahead of time.

**Dynamic partitioning**
* _Grow_
    * Split the partiion into two partitions (now a single node contains two partitions).
    * Transfer one of the two partitions to a new node
* _Shrink_
    * Merge two partitions
    * Transfer one of them to the other one's node
    * Remove the empty node

**Number of partitions proportional to number of nodes** - Have a fixed number (`N`) of partitions per node.
* _Grow_: When a new node joins the cluster, it:
    * randomly chooses `N` existing partitions to split
    * takes ownership of the newly created partitions (halves)
* Characteristics:
    * Only works with key hash partitioning.
    * Corresponds most closely to the original definition of _consistent hashing_, defined by Karger.

## Request Routing

On a high level, there are a few different approaches to this problem:
* Client contacts any node (e.g. round-robin); node handles or forwards the request to the appropriate node.
* Send all requests from clients to a routing tier first (load balancer).
* Clients be aware of the partitioning and they connect directly to the nodes.


# Transactions

**ACID**:
* **Atomicity**: All or nothing (abortability, rollback). Usually achieved using _write-ahead log_.
* ~~**Consistency**: Responsibility of the application, not the database. Letter _C_ doesn't really belong in ACID.~~
* **Isolation**: Concurrently running transactions shouldn't interfere with each other.
* **Durability** Once a transaction has committed successfully, the written data will not be forgotten. This is achieved by ensuring that the data is written to the hard drive (usually using _write-ahead log_).

## Isolation

* **Serializability**
    * **Theoretically "perfect"** solution, recommended by researchers.
    * In practice, it has a **performance cost** which most databases were not willing to pay.
* **Weak isolation**
    * Loosened isolation. Protects against some concurrency issues, but not all.
    * Examples
        * Read commited
        * Snapshot isolation
        * ...

### Serializability

Most databases are providing serializability using one of the 3 techniques:
* Actual serializability
    * Literally executing transactions sequentially one by one
    * Has to be single threaded, which limits the usability.
* Two-Phase Locking (2PL)
* Serializable Snapshot Isolation (SSI)

# Consistency in Replication

Consistency in replicated systems is a granular concept and it cannot be treated as a binary value (consistent / not consistent). CAP theorem might be missleading because it oversimplifies the concept of consistency (it assumes linearizability). Most of the modern storage systems allow you to fine-tune consistency level according to your needs.

When thinking about consistency, we should consider different **guarantees** that our use case might require, and then figure out which consistency level satisfies them.

Consistency guarantees:
* _Read your writes_ (concurrent write-read)
    * Violation: Write to the database and refresh the page, this time reading from a replica that still hasn’t received the write.
* _Monotonic reads_ (concurrent write-read-read)
    * Violation: Read some new data from a replica, and then send another read that ends up on a different replica still hasn’t received the write
* _Consistent prefix_ (concurrent writes)
    * Violation: Reading messages/comments... in incorrect order.
* _Unique constraint_ (concurrent writes)
    * Violation: Concurrently creating an item
* _Leader election_ (concurrent writes)
    * Violation: Two replicas may end up thinking they are leaders.
* _Bank account withdrawal_ (concurrent writes)

Readings:
* [Replicated Data Consistency Explained Through Baseball](https://www.microsoft.com/en-us/research/wp-content/uploads/2011/10/ConsistencyAndBaseballReport.pdf)
* [Please stop calling databases CP or AP](https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html)

Consistency levels:

* **Strict consistency** (**linearizability**) - A write needs to be propagated synchronously to all replicas.
    * This comes with a **cost in availability or performance**. If we try to achieve it in:
        - Single-leader
            - Make all read requests go to the leader: performance penalty, loss of fault tolerance
            - Synchronous communication with all the followers: performance penalty, loss of fault-tolerance
        - Multi-leader: Not possible - the idea behind multi-leader replication is that leaders communicate asynchronously, which contradicts strong consistency.
        - Leaderless: Not possible - even quorums have some edge cases.

* **Sequential consistency** - Writes by different replicas have to be seen in the same order by all replicas (irregardless of wall-clock time). I.e. you can read the stale value as long as the overall sequence of write performed by all processes are the same. This imposes a **total order** of operations.

* **Causal consistency** - Only writes that are potentially causally related must be seen by all replicas in the same order. Concurrent writes may be seen in a different order on different replicas. This imposes a **partial order** of operations.
    * Two operations are causally related if one happens before the other.
    * _Happens before_ [definition]: An operation A happens before another operation B if B knows about A, or depends on A, or builds upon A in some way (e.g. FB comments thread).
    * Two operations are concurrent if neither happens before the other.

* **Weak (eventual) consistency** - this is a broad term. For me, it is any consistency model where that does not satisfy any of the above models.

**CAP theorem**: In case of network interruption/partition [P]
* If your application requires linearizability, and some replicas are disconnected from the other
replicas due to a network problem, then some replicas cannot process requests while they are
disconnected: they must either wait until the network problem is fixed, or return an error (either
way, they become unavailable).
* If your application does not require linearizability, then it can be written in a way that each
replica can process requests independently, even if it is disconnected from other replicas (e.g.,
multi-leader). In this case, the application can remain available in the face of a network
problem, but its behavior is not strongly consistent (linearizable).

Note: When it comes to strict consistency, it is not only about CAP theorem. Strict consistency is also **slow** (as we mentioned). Many distributed database that do not provide strict consistency chose to do so because of performance, not availability.

In many cases, **systems that appear to require linearizability in fact only really require causal consistency**, which can be implemented more efficiently. 
The good news is, **a system can be causally consistent without incurring the performance hit of making it linearizable** (in particular, the CAP theorem does not apply). In fact, **causal consistency is the strongest possible consistency model that does not slow down due to network delays, and remains available in the face of network failures**. 

## Implementing Causal Consistency

**Version vectors**
* Each item in the storage is assigned a version number which gets incremented on every write. Upon each write, the version number is returned to the client, and the clients sends its version number with each write.
* When the server receives a write, if the current item's version number is lower or the same as the client's version number, it can safely overwrite the data (since it knows that the client is already aware of them, i.e. it is not concurrent). If the current item's version number is higher than the client's version number, the conflict needs to be resolved (on the server, or left to the client to decide).
* When having multiple replicas, we need a version number per replica - a version vector.
* **Tradeoffs**:
    * Needs a mechanism for conflict resolution

Note: Any mechanism that achieves sequential consistency also achieves causal consistency.

How MongoDB implements causal consistency:
* [paper](https://dl.acm.org/doi/pdf/10.1145/3299869.3314049)
* [documentation](https://www.mongodb.com/docs/manual/core/read-isolation-consistency-recency/#causal-consistency)

**Lamport timestamps**
* Each operation in the system gets assigned a sequence number, which is a pair of (_counter_, _node ID_)
* Every node and every client keeps track of the maximum counter value it has seen so far, and includes that value on every request/response.
* Provides total ordering of operations
    * the timestamp with the greater counter value happened after
    * if the counter values are the same, the one with the greater node ID happened after
* When a node receives a request with a value grater that its own, it increases its own counter value to that value
* When a node receives a request with a value lower than (or equal to) its own, the conflict needs to be resolved (on the server, or left to the client to decide)
* **Tradeoffs**:
    * Needs a mechanism for conflict resolution
    * Unique constraint violation still possible. E.g. creating two users with the same email
        * Lamport timestamps could only determine the winner after the creation, when it is too late

## Implementing Sequential Consistency

**Total order broadcast** is a theoretical protocol for exchanging messages between nodes. Informally, it requires that two safety properties always be satisfied:
* Reliability
    * No messages are lost: if a message is delivered to one node, it is delivered to all nodes.
* Ordering
    * Messages are delivered to every node in the same order.

An important aspect of total order broadcast is that **the order is fixed at the time the messages are delivered**. A node is not allowed to retroactively insert a message into an earlier position in the order if subsequent messages have already been delivered. This fact makes total order boradcast stronger than timestamp ordering.

Total order broadcast is exactly what you need for database replication: if every message represents a write to the database, and every replica processes the same writes in the same order, then the replicas will remain consistent with each other, aside from any temporary replication lag (sequential consistency).

It can be proved that **total order broadcast is equivalent to consensus**. That is, if you can solve one, you can transform it into a solution for the other.

## Consensus

_Consensus_ - get several nodes to agree on something.

There is a number of situations in which it is important for nodes to agree. E.g:
* _Sequential consistency_: If we can make all the nodes agree which operation happened before another, we can achieve sequential consistency.
* _Atomic commit_: If we want to maintain transaction atomicity (A in ACID), we have to get all nodes to agree on the outcome of the transaction - either they all roll back or they all commit.
* _Leader election_: The leadership position might be contested if some nodes can't communicate with each other due to a network fault. In this case, consensus is important to avoid split brain.

A consensus algorithm must satisfy the following properties:
* Uniform agreement
    * No two nodes end up having a different decision
* Integrity
    * No node decides twice. No change of mind.
* Validity
    * The final decision has to be proposed by some node.
* Termination
    * Every node that does not crash eventually decides a value. (essential for fault-tolerance)
    * Note: If _all_ nodes chrash at once, then it is not possible for any algorithm to decide anything. Thus, the termination property is subject to the assumption that fewer than half of the nodes are crashed or unreachable.

```
THE IMPOSSIBILITY OF CONSENSUS
You may have heard about the FLP result — named after the authors Fischer, Lynch, and Paterson—which proves that there is no algorithm that is always able to reach consensus if there is a risk that a node may crash. In a distributed system, we must assume that nodes may crash, so reliable consensus is impossible. Yet, here we are, discussing algorithms for achieving consensus. What is going on here?
The answer is that the FLP result is proved in a very restrictive model that assumes a deterministic algorithm that cannot use any clocks or timeouts. If the algorithm is allowed to use timeouts, or some other way of identifying suspected crashed nodes (even if the suspicion is sometimes wrong), then consensus becomes solvable. Even just allowing the algorithm to use random numbers is sufficient to get around the impossibility result.
Thus, although the FLP result about the impossibility of consensus is of great theoretical importance, distributed systems can usually achieve consensus in practice.
```

### Consensus algorithms

**Two-Phase Commit (2PC)** [BAD]:
1. Application reads and writes data on multiple database nodes (participants).
2. Coordinator sends a prepare request to each of the nodes, asking them whether they are able to commit (_phase 1_)
    * If all participants reply "yes", indicating they are ready to commit, then the coordinator sends out a commit request, and the commit actually takes place (_phase 2_)
    * If any of the participants replies "no", the coordinator sends an abort request to all nodes in _phase 2_.

Why is 2PC bad?
* _What if commit/rollback requests fail?_ If a participant crashes between phase 1 and phase 2, there is no going back. The system becomes unavailable until the participant recovers.
* _What if coordinator crashes?_ Each node usually holds a row-level lock after phase 1, which makes the system unavailable.

Better consensus algorithms: **Zab**, **Raft**, **Paxos**, **VSR**

Total order broadcast requires messages to be delivered exactly once, in the same order, to all nodes. If you think about it, this is equivalent to performing several rounds of consensus: in each round, nodes propose the message that they want to send next, and then decide on the next message to be delivered in the total order. _Zab_, _Raft_ and _VSR_ implement total order broadcast directly (because that is more efficient than doing repeated rounds of one-value-at-a-time consensus). In case of _Paxos_, this optimization is known as _Multi-Paxos_

Most commercial databases use consensus only for transactions and leader election.
