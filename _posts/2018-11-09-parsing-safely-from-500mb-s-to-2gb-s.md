---
title: " Parsing safely, from 500MB/S to 2GB/s
"
abstract: "To handle low level data, we now have a few safe languages, and good parsing libraries, to make sure untrusted data will never overstep its bounds. Unfortunately, when we need performance, we will too often resort to handwritten state machines, generally in C, and maybe a little assembly while we're at it.

Thanks to one of the most annoying formats to parse (HTTP), we will see how we can write a naive parser in Rust, and transform it to beat state of the art handwritten C parsers while keeping it as readable and safe as the original one."
speaker1: _speakers/geoffroy-couprie.md
type: video
youtube_id: 7VNsmlCAmHU
keywords: Parser, Combinators, Rust,
date: 2018-11-09
tags: Code Mesh LDN 2018
slides: /uploads/cm-ldn-18-geoffroy-couprie-parsing-safely-from-500mb-s-to-2gb-s-compressed.pdf
---

