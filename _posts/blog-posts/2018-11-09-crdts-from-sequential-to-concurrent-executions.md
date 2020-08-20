---
title: CRDTs: From sequential to concurrent executions
abstract: "When nodes are spread across large geographic distances, being available for local users, and providing short response times, is often at odds with keeping strong consistency across the whole system. Several systems, that target large scale geo-replication, support multi-master operation and transient data divergence, allowing each site to update replicas with no immediate coordination.  From the user application perspective, the system cannot be seen anymore as a single sequential copy, since now operations can be processed concurrently at different locations. Conflict-free Replicated Data Types (CRDTs) can take away a lot of the complexity when migrating from a sequential to a concurrent setting. 

In this talk we will explore a bit of the path in this transition, cover what can be expected, and present a few guiding principles. Concurrent behaviour will be explored for common data types, such as counters, registers, sets and sequences."
speaker_id: carlos-baquero-moreno
type: video
youtube_id: hw4agjz4240
keywords: CRDTs, geo-replication, high availability, concurrent, sequential
date: 2018-11-09
tags: Code Mesh LDN 2018
slides: /images/cm-ldn-18-carlos-baquero-crdts-from-sequential-to-concurrent-executions-compressed.pdf
---

