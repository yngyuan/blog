---
title: "Distributed Systems in One Lesson"
date: 2020-05-19T23:21:34-05:00
lastmod: 2020-05-19T23:21:34-05:00
draft: false
keywords: ["distributed system","notes"]
description: "Notes from Tim Berglund's speech on O'Reilly Media."
tags: ["distributed system"]
categories: ["tech", "distributed system"]
author: ""

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: true
autoCollapseToc: false
# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: false
reward: false
mathjax: false

---

<!--more-->

### What a day in a Coffee shop can teach us about distributed storage,  computation,  messaging and consensus.

Notes from Tim Berglund's speech on O'Reilly Media.

## 0 What is a Distributed System?

> A collection of independent computers that appear to its users as one computer.
>
> ​																			Andrew Tanenbaum

### Three Characteristics

- The computers operate concurrently
- The computers fail independently
- The computers do not share a global clock

 A pattern you will see, bad things will get hard when using distributed systems.

## 1 Distributed Storage

### Single-Master Storage

For a small coffee shop, one barista can easily remember all the favourite drinks of customers. 

eg. Tim: Americano

And if Tim decides to change his favourite drink, just tell the barista and it will be changed.

Now business grows and there are more baristas.

### Read Replication

There will be several copies of the customer's info on baristas, and one of them will be main barista, managing others. Now the coffee shop can take more **read traffic**.

- More complexity
- Broken Consistency

### Sharding

When the coffee shop gets too popular. One manager cannot handle. So we use sharding to slice the work to three main baristas to handle more **write traffic**. Names from A-F goes to one main barista, F-N and N-Z  goes to another. And each can operate like the above.

- More complexity
- Limited data model (every query must have one common field to shard on )
- Limited data access patterns

### Consistent Hashing

Let's suppose we knew we are going to be big. And we want to build this business to scale from ground up.

More baristas with token of a range forming a circle. If the customer name hash is greater than one barista's token and smallet than the other, write to the other and so on.

Tim ---Hash---> 9F72: Americano, 

To have more data redundancy we just let 2 or 3 nodes after the nodes have a replicate.

- Consistency

  R+W>N, R is the number of replicas agree on read, W is the number of replicas that successfully take a write. N is the numbr of replicas. We got to decide the R and W to have trade-offs like low latency with eventual consistency. 

When to use this kind of database?

- Scale
- Transactional data (changes a lot)
- Always on

### CAP Theorem

It is proven that a distributed system can only have TWO of the THREE:

- Consistency (always get what you last wrote)
- Availability (when I try, I can read or write)
- Partition Tolerance (allow some nodes to be down)

### Distributed Transacations

#### Acid Transactions

- Atomic (the transaction complete completely or not at all)
- Consistent (When a transaction complete, the database is not broken)
- Isolated (4 levels of isolation)
- Durable (Once a transaction commits, you will be able to see it later)

#### Odering Coffee is not atomic

- Receive order
- Process payment
- Enqueue order
- Make coffee
- Deliver

Different actors do all these. Task 1-3 go to one, 4-5 go to another.

#### Why split the work?

- Parallelization (at least two are working)
- Uneven workloads

#### What could go wrong?

- Payment failure
- Insufficient resources
- Equipment failure
- Worker failure
- Consumer failure

#### Responses to Failure

- Write-off 
- Retry (machine broken, try again)
- Compensating action (when cusumer paid but we cannot deliver)

## 2 Distributed Computation

- Scatter/Gather
- MapReduce
- Hadoop
- Spark
- Storm

### 2.1 MapReduce

It is a computation pattern, a math abstraction, not a program.

- All computation in two functions: Map and Reduce.
- Keep data (mostly) where it is.
- Move computeto data.

(K, V) -->  Mapper()  -->  [(K,V), (K,V), ..., (K, V)] --> Shuffle() --> (K, [V, V, ...V]) --> Reducer()  -->[(K,V), (K,V), ... (K,V)]

#### eg. word count: poems/raven.txt 

turn the poem into a bunch of words. and use the words as a key-value pair, key is the word and value is the count, but we only count 1 and don't remember if we have seen it. Then we shuffle them so that pairs with the same key gets closer to each other. The return of shuffle is like this (tapping: [1,1,1]). And the reducer will do the adding and we have the results.

### 2.2 Hadoop

Hadoop is an implementation of MapReduce.

- MapReduce API
- MapReduce job management
- Distributed Filesystem (HDFS)
- Enormous ecosystem

#### HDFS

- Files and directories
- Metadata managed by a replicated master(name node) in memory
- Files stored in large, immutable, replicated blocks

#### Real-world hadoop

- Cloudera, Hortonworks, MapR
- Nobody writes map, reduce functions
- Hive (SQL-like interface)
- Integration with BI front-ends

#### When to use Hadoop

- Data volume is large
- Data velocity is low
- Latency SLAs are not aggressive

### 2.3 Spark

- Scatter/gather paradigm( similar to MapReduce)
- More general data model (RDDs)
- Moregeneral programming model (transform/action)
- Storage agnostic

#### What's an RDD

RDD stands for resilient distributed datasource.

- Bigger than a computer
- Read from an input source
- Output of a pure function
- Immutable
- Typed
- Ordered
- Lazily evaluated
- Partitioned
- A collection of things

#### Spark API

- Transformation functions turn one RDD into another
- Action functions

Hadoop and Spark are all batch mode processing frameworks.

### 2.4 Storm

- Streaming data processing
- Friendlier programming model than message-passing
- At-least-once processing semantics
- Horizontal scalability, fault tolerance
- Fast answers on massive-scale data

#### Programming Model 

- Stream: a sequence of tuples (probably lots)
- Spout: a source of streams
- Bolt: applies a function an input stream; produces onre or more output streams
- Topology: a graph of spouts and bolts; a "job" that runs indefinitely

Nimbus is a central coordinator of jobs. A supervisor is a node that performs processing. A task is a thread of bolt or sprout execution. A worker is a jvm process where a topology excutes.

#### Stream Grouping

- Assigns tuples to tasks through a consistent-hashing-like mechanism.
- Configurable
  - Shuffle Grouping: random assignment
  - Fields Grouping: mod hash of subset of tuple fields
  - All Grouping: send to every tasks

### 2.5 Lambda Architecture

So far we have problems

- High rate of events
- Must store, must interpret
- The wrong answer, fast
- The right answer, slowly

#### Architecture

- System input is an event system
- Events are immutable
- Batch and stream processing are functional

**Big data store:** we have some event stream, feed up to a long term storage, a durable resting place

- Long term storage
- Batch processing 
- Slow, accurate

**Append-only distributed queue:** And the same data are also going to another event processing framework

-  Temporary queuing
- Stream procesing
- Fast, "wrong"

in the end both goes to a **Fast NoSQL store**.

#### Why call it Lambda?

Because this is funtional transformations of immuatble data.

### 2.6 Synchronization

In distributed systems, "now" is problematic. When the copies of a data don't agree, options are

- Estimate the time: last write wins
  - GPS in every server (we don't do that)
  - Network Time Protocol
- Derive the time logically: vector clocks

#### Network Time Protocol

- Put a very accurate clock on the Internet
- But network latency is variable
- NTP measures and acounts for latency
- Users layers of time servers called *strata*

Facts about NTP

- UDP port 123
- 64-bit time stamp
- Year 2016 problem

/delta = (t_3-t_0) - (t_2-t_1)

more to read: There is no now: problems with simultaneity in distributed systems.

If NTP is not good enough

#### Vector Clocks

- A means of proving sequence
- NOT for telling time
- We are concurrently modifying one value
- Every actor has an ID
- Every actor tracks a sequence number
- Consider Basho's famous example

Trade-offs of vector clocks

- Cannot be wrong (Last Write Wins can) +
- Pushes complexity into the client -

### 2.7 Distributed Consensus: Paxos

- We want agreement between process
- Process are concurrent, asynchronous and failure-prone
- Must meet four requirements
  - Termination: every process decides a value'
  - Validity: If all process propose a value, then all process decide that same value
  - Integrity: if a process decides a value, then that value must have been proposed by another process
  - Agreement: all process must agree on the same value

#### Paxos

- published in 2000
- Determinstic, fault-tolerant consensus protocol
- Used when replicating durable, mutable state
- Guarantees a consistent result

In the most happy case:

**Read**

Client <- Replica nodes 1, 2, 3

If they all agree or most agree, take it.

if all three are different, this is a failed read, no consensus.

**Write**

1. Prepare: Proposer node generates a unique and sortable sequence number with the proposal write and forward to replica nodes.
2. Promise: Now the replica nodes are ACCEPTOR, they only accept if the sequence number is the highest for that key that they have ever seen.They send back the value they are going to write to PROPOSER.
3. Accept Request: Proposer receives quorum, makes progress. Send out to Acceptor and they check one more time.
4. Acceptance: And the write is done.

##### Paxos Uses

- Lightweit transactions on Casandra
  - Insert .. if not exists
- Clustrix transctions
- BigTable's Chubby lock manager
- Master election

#### Other Protocols

- **Raft**: like Paxos, but understandable
- **Blockchain**: like Paxos, but works when people are lying to you



## 3 Messaging

- Means of loosely coupling subsystems
- Messages consumed by subscribers
- Created by one or more producers
- Organized into topics
- Processed by brokers
- Usually persistent over the short term

Messaging Problems:

- What if a topic gets too big for one computer?
- What if one computer is not reliable enough?
- How strongly can we guarantee delivery?

How to solve these?

#### Kafka

- Distributed message queue
- Horizontally scalable, fault-tolerant
- Durable
- Fast
  - 100s MB reads and writes/second
  - Thousands of clients per broker
- Widely deployed
- JVM-based(written in Scala)
- Native Java and Scala APIs (Akka option)
- Binary wire protocol
- Start at three nodes and grow

##### Definations

- Message: an immutable array of bytes
- Topic: a feed of messages
- Producer: a process that publishes messages to a topic
- Consumer: a single-threaded process that subscribes to a topic
- Broker: one of the servers that comprimes a cluster

##### Partitioning

- A scaling strategy
- Done in the producer by a pluggable class and a message key
- Order is maintained in partition only

#### Replication

- Brokers can fail
- Configurable in-sync replica sets
- One leader, n-1 followers
- Only the leader communicates with clients
- When the leader failed, a new leader is elected

##### ZooKeeper in Kafka

- Required part of Kakfa architecture
- Producers use it to find partitions and replication information
- Consumers use it to track current index

#### ZooKeeper

ZooKeeper is a centralized service for maintaining configuration information, naming, providing distributed synchronization, and providing group services.

- Highly availabel
- Manages shared state of an kind
- Transaction control
- Lock management

Fundamentally it is a distributed hierachical key-value store, the data model looks a lot like a file system, just where the files are real small.

##### Architecture

- Written in Java
- Official APIs in C and Java
- In-memory (on-heap)
- Minimum of three nodes for production
- All of the nodes participate

##### Zookeeper Ensemble

ZooKeeper people say ensemble rather than cluster.

That node on the top in that colors is the leader and others are the followers. So let's imagine a client now is performing a read. A client can establish a session with, that is connect to and read from any of those nodes. It can talk to followers. It can talk to the leader. All of those are fine. 

And let's take a look at a write. Again, a write can be done to the master or a write can be done to a follower. If you do a write to a follower, it's not going to succeed until the leader gets it. That follower's gonna say, OK, here, let me help you with that. And it will then propagate the write to the other followers that didn't already have it to make sure that the clusters got the information at once. That's gonna be in memory, committed to those other followers before the write can be acknowledged. Now, the master is gonna look for a majority, a quorum of writes. It's not gonna look for unanimity, so we do have some fault tolerance here.

##### Data model

ZNodes

A ZNode has a few different things. We'll take a look at that list in a minute. But every ZNode has a value, and children, and a few other properties. Now I'm showing these values as strings.

- A byte array (10kmax)
- Timestamped
- Independent, non-inherited ACLs
- Versioned
- Zero or more child ZNodes
- Persistent or session-ephemeral
- Counting ZNodes

Sessions: 

- Connection between the client and a node in the ensemble
- Ephemeral nodes live and die with sessions

Wathces:

- One-time trigger on ZNode values or children
- Asynchronous
- Guaranteed to be seen by the client before the data change

ACLs:

- Apply to individual ZNodes (not children)
- ZNode contains a list of IDs
  - World, auth, digest, ip
- IDs have permissions
  - CREATE, READ, WRITE, DELETE, ADMIN

##### Use Case

- Leader Election
  1. Create a ZNode called leader-election
  2. All candidates create ephemeral sequential children
  3. The child of leader-election with the smallest sequence number is leader
  4. Watch the ZNode with the next smallest sequence number for good news
- Distributed Locks
  1. Setup: create a ZNode called resource
  2. Create ephemeral node called resource/hostname-{i}
  3. Examine children of resource, I hold the lock if my ID is lowest, Otherwise watch the ID preceding mine
  4. Upon update event, I hold the lock
  5. To release the lock, delete my ephemeral node

## Wrap up

### Storage

Relation/Mongo, Cassandra, HDFS

### Computation

Hadoop, Spark, Storm

### Synchronization

NTP, vector locks

### Consensus

Paxos, Zookeeper

### Messaging

Kafka

