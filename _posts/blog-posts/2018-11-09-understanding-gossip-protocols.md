---
title: Understanding gossip protocols
abstract: In large Distributed Systems knowing the state of the whole system is a difficult task which becomes harder as we increment the number of nodes. There are too many nodes to communicate with and many algorithms that solve the problem tend to grow linearly with the number of nodes. The underlying network is a problem too, we can’t rely on hardware solutions as they wouldn’t be available in the cloud (e.g. Multicast). It’s also really complex to maintain an updated graph of nodes and even to store the graph itself, in large systems.

Many distributed systems nowadays rely on Gossip protocols to share the state of the system among the nodes because they avoid these problems.

A Gossip protocol is a communication protocol, a way of multicasting messages inspired by epidemics, human gossip, and social networks.
speaker_id: felix-lopez
type: video
youtube_id: QQ2n1UX3Qwg
keywords: gossip, protocols, distributed systems, nodes,
date: 2018-11-09
tags: Code Mesh LDN 2018
slides: /images/cm-ldn-18-felix-lopez-luis-gossip-compressed-1.pdf
---

