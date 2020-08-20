---
title: " Why time is evil in distributed systems
"
abstract: "Building distributed systems is hard, even using languages such as Erlang that support them well.  There are many problems that have to be solved: partial failure, nondeterminism, observable delays, event ordering, global state, distributed consistency, performance (latency and throughput), and so on.  Progress has been made in solving these problems, but often in isolation and without realizing how the solutions are related.  In this talk I go to the heart of the matter and explain why almost all of these hard problems are avatars of one problem, namely real-world time.  I explain how to avoid time when building distributed systems.  There are some cases when time cannot be avoided, even in principle, and I will explain those as well.  In both situations, using and avoiding time, I will try to present general solutions at a correct level of abstraction.  All ideas will be illustrated by examples of real distributed systems from my experience (edge computing, consistent replication, etc.).  I hope that this talk will give you a fresh outlook on distributed systems and help you design better ones."
speaker_id: peter-van-roy
type: video
youtube_id: NBJ5SiwCNmU
keywords: Distributed systems, Erlang, consistency performance, global state, Peter van Roy
date: 2019-05-28
tags: Code BEAM STO 2019
slides: /images/cb-sto-19-why-time-is-evil-in-distributed-systems-and-what-to-do-about-it-peter-van-roy-compressed.pdf
---

