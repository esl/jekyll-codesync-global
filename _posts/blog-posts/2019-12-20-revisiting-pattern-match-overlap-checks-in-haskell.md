---
title: " Revisiting pattern match overlap checks in Haskell
"
abstract: "How hard can it be to spot missing or overlapping patterns in a Haskell function definition? Surely it’s the least we can expect from a decent compiler? But when you mix in GADTs, pattern guards, view patterns, data families, strict data constructors, and pattern synonyms, matters get surprisingly tricky.

In a 2015 paper “GADTs meet their match” (https://www.microsoft.com/en-us/research/publication/gadts-meet-their-match-pattern-matching-warnings-that-account-for-gadts-guards-and-laziness/) we explored a nice, modular account of pattern-match checking that addresses many of these tricky points. Alas, GHC’s implementation of that paper has proved less than satisfactory: it can be terribly slow, and misses cases that programmers think look obvious. So my colleague Sebastian Graf and I have been radically refactoring the implementation."
speaker_id: simon-peyton-jones
type: video
youtube_id: SWO5OzSxD6Y
keywords: Pattern matching,Haskell,Simon Peyton Jones,Code Mesh LDN,Haskell function,GADTs,pattern guards,view patterns,data families,strict data constructors,pattern synonyms
date: 2019-12-20
tags: Languages,Code Mesh LDN 2019
slides: /uploads/cm19-simon-peyton-jones-revisiting-pattern-match-overlap-checks-in-haskell-1-compressed.pdf
---

