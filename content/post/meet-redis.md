---
title: "Meet Redis"
date: 2020-07-24T13:30:24-05:00
lastmod: 2020-07-24T13:30:24-05:00
draft: false
keywords: ["redis","jedis"]
description: ""
tags: ["redis"]
categories: ["distributed system"]
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



# NoSQL

- NoSQL = Not only SQL

- Non-relational database

### Why you need it

In web 2.0 era where there are more contents, more Read and Write operations and more dynamic web pages.

- Hign performance
- Huge Storage
- High Scalability & HIgh Availability
- More versitile data types

### Products

MongoDB, Redis, Cassandra, CouchDB, riak etc.

### Types of NoSQL Database

Picture

![soa](/image/nosql.png)

# Redis

Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. 

It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs, geospatial indexes with radius queries and streams. 

## Jedis

A blazingly small and sane redis java client.

```java
// Singleton Pattern
public void demo1(){
	//1. set IP and Ports
  Jedis jedis = new Jedis("IP", PORT);
  //2. save data, key-value
  jedis.set("name","yang");
  //3. get data
  String val = jedis.get("name");
  System.out.println(val);
  //4. release resource
  jedis.close();
}
```



```Java
// Connection Pool
public void demo2(){
  // get connection pool config object
  JedisPoolConfig config = new JedisPoolConfig();
  // set max connection
  config.setMaxTotal(30);
  // set max Idle
  config.setMaxIdle(10);
  
  // get connection pool
  JedisPool jedisPool = new JedisPool(config, "IP", PORT);
  // get core object
  Jedis jedis = null;
  try{
    // get connection from pool
    jedis = jedisPool.getResource();
    jedis.set("name", "yang");
    String val = jedis.get("name");
    System.out.println(val);
  } catch(Exception e){
    e.printStackTrace();
  } finally{
    if(jedis != null){
      jedis.close();
    }
    if(jedisPool != null){
      jedisPool.close();
    }
  }
  
}
```



## Data Types

- String

  - you get what you saved

  - max value 521M
  - get, set, getset, del

- hash

  - a map of String Key, String Value.
  - hset map username jack
  - hget username
  - hmset map2 username rose age 21
  - hgetall
  - hdel
  - hlen
  - hkeys
  - hvals
  - hexists

- list

  - Double-linked list.
  - ArrayList
  - lpush mylist a b c
  - rpush mylist 123
  - lrange mylist 0 5 
  - lpop/rpop mylist
  - llen   
  - rpoplpush mylist1 mylist2

  ![soa](/image/poppush.png)

- set

  - as the name suggests, no duplicate
  - sadd myset a b c
  - srem a
  - smembers myset
  - sismember myset a
  - sdiff myset1 myset2/ sdiffstore
  - sinter myset1 myset2
  - sunion myset1 myset2 

- Sorted Set

  - have a score for sorting
  - because they are sorted, it is easy to find one in the middle
  - used for things like twittor trending.
  - zadd sortedlist 70 zs 80 ls 90 ww
  - zscore sortedlist zs
  - zcard sortedlist
  - zrem sortedlist ww
  - zrange sortedlist 0 -1 (withscores)
  - zreverange sortedlist 0 -1
  - zremrangebyrank sortedlist 0 4
  - zrangebyscore sortedlist 0 100 withscores limit 0 2
  - zcount sortedlist 80 90

## General Commands

- keys my?
  - my2
  - my1
  - my3
- del my1
- exists my2
- get
- rename key1 key2 
- expire key1 1000  // setting expire time
- ttl key1 // time to live
- type key1  

## Characteristics

**There could be up to 16 db in a Redis instance.**

- select 1
- keys *

**Redis transaction**

Redis transactions can execute multiple commands at once, with the following three important guarantees:

- The batch operation is put into the stack buffer before sending the EXEC command.

- After receiving the EXEC command, it enters the transaction execution. Any command in the transaction fails to execute, and the remaining commands are still executed.

- During the transaction execution process, command requests submitted by other clients will not be inserted into the transaction execution command sequence.

A transaction will go through the following three stages from inception to execution:

- Start the Transaction.
- Join the Queue.
- Execute.

*It's important to note that even when a command fails, all the other commands in the queue are processed – Redis will not stop the processing of commands.*

```
redis 127.0.0.1:6379> MULTI // start a transaction block
OK

redis 127.0.0.1:6379> SET book-name "Mastering C++ in 21 days"
QUEUED

redis 127.0.0.1:6379> GET book-name
QUEUED

redis 127.0.0.1:6379> SADD tag "C++" "Programming" "Mastering Series"
QUEUED

redis 127.0.0.1:6379> SMEMBERS tag
QUEUED

redis 127.0.0.1:6379> EXEC
1) OK
2) "Mastering C++ in 21 days"
3) (integer) 3
4) 1) "Mastering Series"
   2) "C++"
   3) "Programming"
```



## Persistence 

Redis provides a different range of persistence options:

- The RDB persistence performs point-in-time snapshots of your dataset at specified intervals.
- The AOF persistence logs **every** write operation received by the server, that will be played again at server startup, reconstructing the original dataset. Commands are logged using the same format as the Redis protocol itself, in an append-only fashion. Redis is able to rewrite the log in the background when it gets too big.
- If you wish, you can disable persistence completely, if you want your data to just exist as long as the server is running.
- It is possible to combine both AOF and RDB in the same instance. Notice that, in this case, when Redis restarts the AOF file will be used to reconstruct the original dataset since it is guaranteed to be the most complete.

### RDB

#### RDB advantages

- RDB is a very compact **single-file** point-in-time representation of your Redis data. RDB files are perfect for **backups**. For instance you may want to archive your RDB files every hour for the latest 24 hours, and to save an RDB snapshot every day for 30 days. This allows you to easily restore different versions of the data set in case of disasters.
- RDB is very good for **disaster recovery**, being a single compact file that can be transferred to far data centers, or onto Amazon S3 (possibly encrypted).
- RDB maximizes Redis **performances** since the only work the Redis parent process needs to do in order to persist is forking a child that will do all the rest. The parent instance will never perform disk I/O or alike.
- RDB allows faster restarts with big datasets compared to AOF.

#### RDB disadvantages

- RDB is NOT good if you need to minimize the chance of data loss in case Redis stops working (for example after a power outage). You can configure different *save points* where an RDB is produced (for instance after at least five minutes and 100 writes against the data set, but you can have multiple save points). However you'll usually create an RDB snapshot every five minutes or more, so in case of Redis stopping working without a correct shutdown for any reason you should be prepared to lose the latest minutes of data.
- RDB needs to fork() often in order to persist on disk using a child process. Fork() can be time consuming if the dataset is big, and may result in Redis to stop serving clients for some millisecond or even for one second if the dataset is very big and the CPU performance not great. AOF also needs to fork() but you can tune how often you want to rewrite your logs without any trade-off on durability.



### AOF

#### AOF advantages

- Using AOF Redis is much more durable: you can have different fsync policies: no fsync at all, fsync every second, fsync at every query. With the default policy of fsync every second write performances are still great (fsync is performed using a background thread and the main thread will try hard to perform writes when no fsync is in progress.) but you can only lose one second worth of writes.
- The AOF log is an append only log, so there are no seeks, nor corruption problems if there is a power outage. Even if the log ends with an half-written command for some reason (disk full or other reasons) the redis-check-aof tool is able to fix it easily.
- Redis is able to automatically rewrite the AOF in background when it gets too big. The rewrite is completely safe as while Redis continues appending to the old file, a completely new one is produced with the minimal set of operations needed to create the current data set, and once this second file is ready Redis switches the two and starts appending to the new one.
- AOF contains a log of all the operations one after the other in an easy to understand and parse format. You can even easily export an AOF file. For instance even if you flushed everything for an error using a [FLUSHALL](https://redis.io/commands/flushall) command, if no rewrite of the log was performed in the meantime you can still save your data set just stopping the server, removing the latest command, and restarting Redis again.

#### AOF disadvantages

- AOF files are usually bigger than the equivalent RDB files for the same dataset.
- AOF can be slower than RDB depending on the exact fsync policy. In general with fsync set to *every second* performance is still very high, and with fsync disabled it should be exactly as fast as RDB even under high load. Still RDB is able to provide more guarantees about the maximum latency even in the case of an huge write load.
- In the past we experienced rare bugs in specific commands (for instance there was one involving blocking commands like [BRPOPLPUSH](https://redis.io/commands/brpoplpush)) causing the AOF produced to not reproduce exactly the same dataset on reloading. These bugs are rare and we have tests in the test suite creating random complex datasets automatically and reloading them to check everything is fine. However, these kind of bugs are almost impossible with RDB persistence. To make this point more clear: the Redis AOF works by incrementally updating an existing state, like MySQL or MongoDB does, while the RDB snapshotting creates everything from scratch again and again, that is conceptually more robust. However - 1) It should be noted that every time the AOF is rewritten by Redis it is recreated from scratch starting from the actual data contained in the data set, making resistance to bugs stronger compared to an always appending AOF file (or one rewritten reading the old AOF instead of reading the data in memory). 2) We have never had a single report from users about an AOF corruption that was detected in the real world.





















