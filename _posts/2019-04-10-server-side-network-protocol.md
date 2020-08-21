---
title: " Server-side network protocol
"
abstract: "Erlang's binary pattern matching and finite state machine behaviours make it easy to write network server protocol handlers. At Datometry, we added our own ODBC-driver and corresponding server-side protocol handler to allow our customers to replace ODBC drivers from their previous database vendor. We used Postgres v3 as a starting point. To better understand the protocol, we created a simple server using gen_statem. We'll cover the motivation for building our designing our own network protocol, and the design and implementation details of our prototype server."
speaker1: _speakers/marc-sugiyama.md
type: video
youtube_id: _u_OKKOUbqc
keywords: Network protocol, Server. Finite state machine, Erlang
date: 2019-04-10
tags: Code BEAM SF 2019
slides: /uploads/cb-sf-19-pgv3-server-marc-sugiyama.pdf
---

