---
title: "Distributed Systems in One Lesson"
date: 2020-05-19T23:21:34-05:00
lastmod: 2020-05-19T23:21:34-05:00
draft: true
keywords: []
description: ""
tags: []
categories: []
author: ""

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: false
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

### 1 MapReduce

It is a computation pattern, a math abstraction, not a program.

- All computation in two functions: Map and Reduce.
- Keep data (mostly) where it is.
- Move computeto data.

(K, V) -->  Mapper()  -->  [(K,V), (K,V), ..., (K, V)] --> Shuffle() --> (K, [V, V, ...V]) --> Reducer()  -->[(K,V), (K,V), ... (K,V)]

#### eg. word count: poems/raven.txt 

turn the poem into a bunch of words. and use the words as a key-value pair, key is the word and value is the count, but we only count 1 and don't remember if we have seen it. Then we shuffle them so that pairs with the same key gets closer to each other. The return of shuffle is like this (tapping: [1,1,1]). And the reducer will do the adding and we have the results.

### 2 Hadoop

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

### 3 Spark

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

### Storm

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

















