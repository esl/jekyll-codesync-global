---
title: gen_persistence: persist the state of your processes
abstract: Often we want to persist the state of our Erlang processes so that it can be recovered after a crash, started/restarted on demand (via a supervisor or manually) or migrated in the cluster via a storage provider. To solve it I have created a  new small opensource library.

With gen_perstistence  all events coming to your modules are stored to the disk and replayed on the restart but it’s also possible to store a snapshot of the state to reduce the recovery time. An implementation of gen_statem and gen_server with persistence is also provided.
speaker_id: benoit-chesneau
type: video
youtube_id: g-2qYXMexg8
keywords: Gen_persistence,Erlang,P2P,Benoit Chesneau
date: 2019-09-06
tags: Code BEAM STO 2019
slides: /images/cb-sto-19-gen-persistence-persist-the-state-of-your-processes-benoit-chesneau-compressed.pdf
---

