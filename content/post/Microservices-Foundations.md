---
title: "Microservices Foundations"
date: 2020-07-18T22:09:50-05:00
lastmod: 2020-07-18T22:09:50-05:00
draft: false
keywords: ["Microservice"]
description: ""
tags: ["Microservice"]
categories: ["Microservice"]
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

## Before Microservices 

### 1. N-tier monolithic architecture

- 3 Layer: Presentation, Business Process, Data Access(Facade, Processing, I/O)

- Code coupling, Require considerable time to build, test and deploy.
- Scaling out a monolith takes deploying the entire package, even only one set of functions are overload.

### 2. Service-oriented architecture (SOA)

- Decompose applications into smaller modules
- Web server tech is based on SOAP(*Simple Object Access Protocol*), everything is either ok(200) or fault(500). However there are recoverable errors that can occur.
- New level of coupling between internal and external elements.

### 3. Microservices

System Decomposition: breaking  a system into smaller units that make it easier to solve a series of system problems.

- Protocol Aware Heterogeneous Interoperability: All communivations over ReST

- Polygot development support

Trade-offs

- Higher Complexity.
- Distribution tax: increase in network communications between the individual services. Higher Latency. 
- Risk congestion casing catastrophic latency.
- Reduction of reliability.( If one microservice is sick, it could affect all.)

## Core Concepts of Microservices

### 1. Servicese

- Watch for too fine grained.

- Don't be afraid to refactor.

### 2. Communications

All communication between individual services in a microservices architecture is over HTTP using ReST based services.

Protocol Aware Heterogeneous Interoperability

- Services are bound to a protocol(in this case HTTP) and execute communication over that protocol in a way that works in a mixed, or heterogeneous environment. 

### 3. Distribution and Scale

- Able to distribute customer service globally and business service locally.

- Elastic Scalability. Scale the busy services.
- Dangers of latency and gridlock

### 4. Bounded Context

Understanding how to properly decompose an application for a microservices implementation is not an easy task. This design pattern Domin-Driven Design can help you.

 The core concept is :

-  investigate your working system.
-  determine the domains. 
- break services up accordingly.

Part of the goal in building on your bounded contexts is to reduce your cross-domain calls where appropriate.

Strong contracts and well-defined boundaries allow for self-discovery.

### 5. Database and Transactions

In the data layer, you have to take more into account than simply the bounded context of your domain because now you have to deal with data transactions.

Transactional Boundaries

- Cannot eliminate transactions completely.
- No distributed transactions.
- New ways of thinking area needed.

Recommended pattern of building your data domains. 

- tart with the services instead of the database. By starting with the services and having them all connect to the monolithic database, you'll start to see if your domains are well-defined. 
- You can see the traffic flows across the network
- leverage that knowledge to start modeling your data itself. 

The overall objective is to minimize the cross-domain calls where possible, enforce your needed transaction boundaries, and then you can start decomposing your databases into smaller instances. 

#### No ACID, only BASE

Traditional systems aimed for transactions that were **ACID** compliant. 

- Atomic: Either succeeds completely or fails completely.
- Consistent:  guarantees all of the constraints or data model rules will be enforced.
- Isolated: visibility rules are well-defined, such that no other transaction can read data that is not in the correct state.
- Durable: once guaranteed to be in the datastore until modified.

 In a microservices architecture, we strive for **BASE** instead of **ACID**. We eventually achieve the end state in all nodes of the distributed datastore.

- **B**asic **A**vailability
- **S**oft-state
- **E**ventual consistency

### 6. API layer

**A pure proxy.**

If you find your API layer is doing transformations or executing logic, you're doing it wrong.

 In a pure microservices architecture, an API layer is nothing more than an aggregated proxy of all of your service offerings. The API layer is used to shield the outside world or your clients from knowing the structure, organization, or even what exact service is exposing a specific operation which is actually very useful. The API layer provides a standardized proxy interface that will expose whatever service endpoints and API operations we configure it to expose.

Consider our use cases around scaling up our system under load or scaling down under a lull.

- In this scaling model the API layer isolates the client from needing to know the direct IP address and port of the service it's calling.
- another use case and that is isolation from change.

## Advanced Concepts of Microservices

### 1. Asynchronous Communication

Asynchronous communications are not easy, and you need to put sufficient work into ensuring that your communications reach their destination and are processed in accordance with predetermined tolerances. 

#### Event-driven microservices

![event](/image/tra.png)

-  In this model, the services put a message into an asynchronous message broker or a temporary data store and then drive events from this new state. The downstream event processors will process the data and eventually cause the data to be stored in its final data store. Conversions will then occur through either distributed data patterns or subsequent event processing.

#### Stream Data Platforms

![stream](/image/pro.png)

- In a stream data platform, events are written to a central message broker. These events then trigger listener operations that take action on that data if it applies to them. These events can trigger operations that format the data, cause other downstream events, or various other activities. These stream data platforms, in my experience, can be highly useful in large distributed systems because often events trigger multiple operations and not just one.

### 2. Tracing and logging

- Plan for unified logging strategies across your entire platform.
- As the system size grows, you may find yourself moving from traditional file-system based logging to log aggregators. These log aggregators make uniformity that much more critical, so that you can coalesce them and scan them more easily.
- Tracing is based on the concept of creating a unique token, called a trace, and using that trace in all internal logging events for that call stack. By embedding this value in all of the logging and timing output for all of the services involved. Each service uses the trace, and then passes it downstream to all of the service calls it makes. Each of those, in turn, do the same. 
- There are several different strategies for moving the trace ID from service to service, but the important part is that the trace ID exists in all of the log messaging, for all services throughout the system for a given call stack. By leveraging this strategy, you can aggregate a set of log messages and timings when looking at call metrics and behavior.

### 3. Continuous Delivery

to build out a CICD, or continuous integration and continuous delivery pipeline.

- A CICD pipeline starts with the most basic aspects of building your committed code base in an automated fashion. Building your code can be as simple as executing a script that manages the build cycle itself to as complex as a large distributed model of containerization and sandbox build operations. The build step compiles and, often, executes unit tests to ensure that the code is ready for further testing and deployment. Once the build step has completed, we often add a step of automated deployment to a non-production environment. This automated deployment step moves the compiled artifact to a runtime that often mimics production, but does not take production traffic. In this environment, we often see integration and system testing occurring, as well as security penetration testing. These operations should provide a clear indication of safety around moving the code to production. 

Continuous delivery is required to achieve agility in a microservices architecture.

### 4. Hybrid architectures

#### Hierarchical Service Architecture

![hie](/image/hie.png)

- Many thought leaders are opposed to this model
- Prevents circular dependencies
- Models an n-tier architecture via services instead of modules
  - n the n-tier hierarchy model, you may define a few different classes of services. One common class is the Data Service that exposes data domain-specific logic completely to the outside world. 
  - Another common class is the Business Process Services that define high-level business processes that are well-defined.
  -  You may also create Gateway Services that build abstractions to external dependencies. And 
  - finally, you may define Edge Services that expose your data and business processes to the outside world.

#### Service-Based Architecture

![soa](/image/soa.png)

- Single underlying database
- Leverages services to handle decomposition
- Gains some agility without modifications to datastore

## Making Architecture Choices

### 1. Design considerations

- Continuous intergration and continuous delivery pipelines.
- Logging and tracing.
- Consider Leveraging domain-driven design.
- Service design to control Latency
  - Consider non-blocking code
  - Standardize your stack when possible
  - Design your systems to be asynchronous first

### 2. Managing trade-offs

#### Benefits of Paying the Distribution Tax

- Distributability
- Well-defined service boundaries
- Scalability

#### Issues of Complexity

- Scalability and deployments
- Large number of moving parts

#### Polygot Development Practices

- Many pros and cons
- Use it as a tool

### 3. Edge services

- API layer should be used only as a proxy layer.

#### Edge Services

- Outbound edge services
  - Expose your client's specific needs to the outside world.
  - eg. Not every client that consumes your service needs the same data payload. Mobile scenarios tend to thrive on smaller payloads of data, you manage your own edge services specific for clients and simply proxy them.
- Inbound/Translation edge sercies
  - Abstract you from third-party dependencies.
  - Eg. Your company most likely leverages a third party send system to handle outgoing email communications. When you consume these services, you can either call the APIs directly or build an edge service that you own to interact with the third party. If you build the abstraction layer or edge service, you control not only the vendor API changes, but also provide yourself a much easier path for vendor replacement if the need arises.

#### Key benifits of edge sercies

- Manage code transformations similar to other code in your system.
- Provide a consistent interface even if underlying services change

### 4. DevOps culture

- Devops aims to bring the conversation between operations and development into the same sphere.

- Devops aims to leverage automation and embed the work into the development function.

Most of the issues from microservices can be seen as operational issues. The distribution tax in a microservices architecture is one that must be closely monitored to ensure lag in the system doesn't have major impacts. There are several architectural mitigations we have discussed, but regardless of the mitigations, monitoring the system remains the most important aspect. A platform of continuous monitoring and automated responses become a necessity for operations.

We want to maximize the positives like scaling, agility, and the ability to globally distribute our system. To do that, we have to automate. We also need to mitigate the latency and complexity of deployments as well as general operations. Once again, to do that, we have to automate. 



## Reference

Notes from Frank Moley's course *Microservices Foundations* on LinkedIn Learning.

