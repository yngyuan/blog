---
title: "Interview Basic Knowledge"
date: 2020-07-09T12:40:18-05:00
lastmod: 2020-07-09T12:40:18-05:00
draft: true
keywords: ["Interview", "Java"]
description: ""
tags: ["Interview", "Java"]
categories: ["Interview"]
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



This is a review of everything about technical interview for SDE. Here is an overview of the article.

## Overview

- Computer Science Basics
  - Operating Systems
  - Network
  - Database
- Programming Basics
  - Data Types
- Java Specific
  - Collections
  - Garbage Collector
  - Memory
  - Exception Handling
- Coding Practice
  - Recursion
  - Loop
  - Boundary
  - Data Structue
  - Tree Traversal

- Object Oriented Programming
  - Class and Object
  - Interface and Implementation
  - Inheritance and Encapsulation
  - Generics
  - Immutable Types

- Design Patterns
  - Singleton
  - Inheritance to Composition
- Advanced Knowledge
  - Parallel Computing
  - Multi-thread
  - Resource management
  - Socket
  - Async IO
  - Infrastructure Development

- Interview Process & Tips



## 1 Computer Science Basics

### 1.1 Operating Systems

#### Process vs Thread

Assume CPU is a factory that is always running. However the power is limited. So each time there could only be one house that is up and running, aka one CPU can only run one task. Process is like one of those houses. It represents the single task a CPU could do. At any given moment, CPU is running one process, while other processes are not running.

Inside the house there could be many workers collaborating on a task. Thread is like the worker. So there could be multiple threads in one process. The space in a house is shared by the workers, aka the memory of a process is shared among threads. And when a thread is using some memory, the other threads must wait. Kind of like a restroom. So to prevent others from walking in when you are in the restroom, we use a lock, Mutual exclusion, aka Mutex. When others see this, they wait till it opens.

For some rooms they can hold many people. We use Semaphore. It's like there are N keys outside the room. One take a key off the wall and enters. Others wait when there is no key left.



#### Storage and Addressing

Hard drive

Memory

Cache

Register



**Addressing Space**

32bit --> 4GB

64bit --> ~10^19 Bytes



#### Process Communication

- File

- Signal (eg.  Kill -siginal number )

  - -1 HUP(hang up)

  - -2 INT(interrupt)
  - .......

- Message Queue

- Pipes/Named Pipes

- Shared Memory

- Synchronization mechanism (Semaphore, Mutex)

- Socket(Can be used for communication between different machines)



### 1.2 Network

Look throught the lens of history, not theory.

Alice Bob

Not Reliable

- lost packet
- 

Not Safe



Sliding Window in TCP





### 1.3 Database













