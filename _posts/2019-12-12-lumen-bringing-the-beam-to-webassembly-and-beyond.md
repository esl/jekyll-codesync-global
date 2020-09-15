---
title: " Lumen - Bringing the BEAM to WebAssembly and Beyond
"
abstract: "Lumen is a new compiler and runtime for BEAM languages (currently Erlang and Elixir) that supports targeting environments that were previously unsupported or infeasible for the BEAM virtual machine, e.g. WebAssembly, bare metal embedded hardware and more. To best support these new targets, Lumen takes an alternative implementation approach compared to the BEAM - rather than being constructed as a compiler that produces bytecode which is then executed by a virtual machine, Lumen is instead an ahead-of-time compiler that produces native code in the form of a standalone executable. It builds on the capabilities and ecosystem provided by the Rust and LLVM toolchains, and makes it possible to apply BEAM languages to domains that were previously inaccessible."
speakers:
- _speakers/paul-schoenfelder.md
type: video
youtube_id: PBY8MQ09D9w
keywords: WebAssembly,Compilers,BEAM VM,Erlang,Elixir,Paul Schoenfelder,Code Mesh LDN
date: 2019-12-12
tags: Infrastructure,Code Mesh LDN 2019
slides: /uploads/cm19-paul-schoenfelder-lumen-compressed.pdf
---
