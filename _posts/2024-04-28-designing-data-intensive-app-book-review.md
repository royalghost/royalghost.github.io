---
layout: post
title: "Book Review : Designing Data-Intensive Applications by Martin Kleppmann"
categories: blog
tags: distributed data design book-review
---

**Designing Data-Intensive Applications** book written by Martin Kleppman is one of the most widely prescribed books to read for software engineers and architects. This book nicely fills in the gap to understand the data structures and algorithms those implemented under the hood by popular databases and distributed systems. Hence, this is a great book for those curious minds who seek out to understand _how something works_ and not just _how to make it work_.

The book is quite dense consisting of 12 chapters divided into 3 parts namely Foundations, Distributed Data and Derived Data. It starts with explaining the three most important topics in distributed systems - Reliability, Scalability and Maintainability at the high level. The second chapter is Data Models and Query languages. One of the topics the author has discussed in the second chapter is how to make a decision on when to use SQL versus NoSQL, which is quite common to have a debate for any database design. Two important criterias are mentioned to make this decision, first if the system requires **many to many relationships** and second **data locality** for queries. 

Chapter 2 discusses storage engine, namely log-structured storage engine and page-oriented storage engine. The following is the simples database that use append-only log and details instructions on how to install as well as use is at [1].
```
#!/bin/bash

db_set () {
    echo "$1,$2" >> database
}

db_get () {
    grep "^$1, "database | sed -e "s/^$1,//" | tail -n 1
}
```
SSTables, LSM-trees, LevelDB, Cassandra, HBase, Lucene and others belongs to log-structured category, which are discussed in more details in this chapter.

I think the most important content in this book is Part II. Distributed Data that contains chapters including Replication and Partitioning as well as Transactions, Consistency and Consensus. In chapter Partitioning, the author discussed t**wo main approaches to partitioning, namely Key range partitioning and hash partitioning**. 

Transactions is another interesting chapter if you work more on a relational database because they are well known for safety guarantees provided for concurrency through Atomicity, Consistency, Isolation and Durability (ACID.) The other end of the spectrum is also known as BASE, which stands for Basically Available, Soft state, and Eventual consistency. However, a lot depends upon the specific database implementation so it cannot be generalized. For an e.g. MongoDB is a NoSQL database but is fully ACID compliant [2]. 

Transactions in databases are an abstraction layer that allows an application to pretend that certain concurrency problems and certain kinds of hardware and software faults don’t exist. Without transactions, it becomes very difficult to reason about the effects that complex interacting accesses can have on the database. Broadly categorized, **there are two isolation types, weak and strong**. Read committed, snapshot isolation and serializable for weak isolations. However, weak isolations can lead to various race conditions such as dirty reads, dirty writes, read skew, lost updates, write skew and phantom reads. Weak isolation levels protect against some of those anomalies but to build a more robust system it requires application to handle different scenarios. 

Serializable Transactions protect from most of those issues. **Three approaches to serializable transactions are discussed, namely executing transactions in serial order, two phase locking (2PL) and Serializable snapshot isolation (SSI.)** While the first two have been known for quite some time and the 2PL is one of the most common implementations in RDBMS, SSI was only recently published around 2008 on a paper entitled Serializable Snapshot for database Isolation [3] and PostgreSQL has implemented since it’s version 9.1 [4]

In the last two chapters of part two, the author discussed the challenges with distributed systems followed by examples of algorithms and protocols for building fault tolerant systems. The basic challenge with distributed systems is having multiple nodes and no shared state of any kind between them. Single node is not sufficient to provide a scalable, fault tolerance and low latency. When having multiple nodes, they can only communicate by sending messages among each other over the unreliable network. When sending messages between nodes, packets can be lost, reordered, duplicated or delayed in the network. **A certain kind of consensus is required among nodes for major decisions to be made that are challenged by out of sync clocks, nodes pausing for garbage collection and crashing.**

In the following chapter, the author recommends that the  best way to handle those challenges discussed is to find some general purpose abstractions with useful guarantees and let applications rely on those. This is the same approach as when an application uses transactions to pretend that there are no crashes, no other client is accessing the database and storage devices are reliable. Consensus is one of the most important abstractions for distributed systems to agree on something. **Linearizability, Atomic transaction commit, locks and leases, membership services, uniqueness constraints etc. are different algorithms to use for consensus for a single node.** However, with multiple nodes, an algorithm has to be used to choose a new leader when a current leader node fails. ZooKeeper and etcd are an attempt to provide such algorithms, detects failures and membership service for distributed systems to use. 

> **Kubernetes uses etcd to maintain the state of clusters and configuration** [5]. etcd is a strongly consistent, distributed key-value store that provides a reliable way to store data that needs to be accessed by a distributed system or cluster of machines. It gracefully handles leader elections during network partitions and can tolerate machine failure, even in the leader node. Popular fault-tolerant consensus algorithms are Raft, Paxos, Zab and Viewstamped Replication (VSR.)  Raft is not a Byzantine fault tolerant (BFT) algorithm and the nodes trust the elected leader. Raft achieves consensus via an elected leader. Etcd uses Raft algorithm to achieve distributed consensus. 

The last Part of the book discussed **Derived Data, namely Batch Processing and Stream Processing.** Distributed batch processing engines have a deliberately restricted programming model using callback functions such as mappers and reducers. They are stateless and have no side effects. Partitioning and Fault tolerance are the two main problems that distributed batch processing framework has to solve. MapReduce is extensively discussed as an example of a distributed batch processing framework. Stream Processing is similar to batch processing but done continuously on unbounded streams rather than on a fixed-size input. Message broker and event logs are two types of stream processing. In the last chapter the author discussed the newer approaches to designing data systems and using data integrations techniques. The chapter ended with discussion on ethical aspects of building data intensive applications. 

> I had some experience working in Microsoft Cosmos[6], which is a big data platform inside the company. It provides both storage and compute. It uses Replicated State Library (RSL) as a Paxos implementation to support dynamic replica set reconfiguration, including dynamic sizing and cluster healing. Structured Computations Optimized for Parallel Execution, or SCOPE, was initially a grep-for-select language just like P*SQL, but with C# fragments instead of Perl fragments. SCOPE processes inputs using "extractors" that break a byte stream into records with a well-defined schema.

**Overall, I highly recommend this book for anyone who is interested in understanding the basics of distributed systems**, esp. bridging the gap between theoretical knowledge and their implementations in day to day databases and frameworks we use. When a new version arrives, I anticipate the book to be easier to navigate esp. Chapter II moving to the end of Part I. Vector database [7] is a new type of storage that has recently emerged as a result of the proliferation of machine learning and Large Language Models (LLM.) Database is also starting to leverage machine learned indexes combined with traditional approaches such as using B-Trees to improve performance [8]. 


# References

[1] [https://github.com/royalghost/distributed-systems-examples/tree/main?tab=readme-ov-file#dbsh](https://github.com/royalghost/distributed-systems-examples/tree/main?tab=readme-ov-file#dbsh)

[2] [https://www.mongodb.com/resources/products/capabilities/acid-compliance](https://www.mongodb.com/resources/products/capabilities/acid-compliance) 

[3] [https://dl.acm.org/doi/10.1145/1376616.1376690](https://dl.acm.org/doi/10.1145/1376616.1376690)  

[4] [https://www.postgresql.org/docs/9.1/transaction-iso.html#XACT-SERIALIZABLE](https://www.postgresql.org/docs/9.1/transaction-iso.html#XACT-SERIALIZABLE)

[5] [https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/](https://kubernetes.io/docs/tasks/administer-cluster/configure-upgrade-etcd/) 

[6] [http://vldb.org/pvldb/vol14/p3148-jindal.pdf](http://vldb.org/pvldb/vol14/p3148-jindal.pdf) 

[7] [https://www.cloudflare.com/learning/ai/what-is-vector-database/](https://www.cloudflare.com/learning/ai/what-is-vector-database/) 

[8] [https://alexbeutel.com/papers/mlsys2017_learned_indexes.pdf](https://alexbeutel.com/papers/mlsys2017_learned_indexes.pdf) 