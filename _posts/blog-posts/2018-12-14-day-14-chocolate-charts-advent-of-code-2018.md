
---
title: " Day 14: Chocolate Charts - Advent of Code 2018
"
abstract: "I did the Advent of Code 2018 day 14 challenge in Erlang! Parts one and two are as follows:
"
image_url: /uploads//images/Day 14.png
speaker_id: stavros-aronis
type: article
keywords: Advent of Code, Erlang
date: 2018-12-14
tags: Advent of Code,Erlang
---
I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/14">day 14 challenge</a>&nbsp;in Erlang! Parts one and two are as follows:

<pre>
<code class="language-erlang">#!/usr/bin/env escript
-mode(native).

%% https://adventofcode.com/2018/day/14

-define(INPUT, 635041).

main(Args) -&gt;
  Sol =
    case Args of
      ["2"] -&gt; seek([6,3,5,0,4,1]);
      _ -&gt;
        {_, P} = score(?INPUT),
        P
    end,
  io:format("~p~n", [Sol]).

seek(Target) -&gt;
  seek(Target, 1000).

seek(Target, Range) -&gt;
  io:format("~p~n", [Range]),
  {M, _} = score(Range),
  io:format("Score done~n"),
  case find(Target, M) of
    {true, I} -&gt; I;
    false -&gt; seek(Target, Range * 10)
  end.

find(Target, M) -&gt;
  Rs = [D || {_, D} &lt;- lists:sort(maps:to_list(M))],
  io:format("Seq Done"),
  find(0, Rs, Target).

find(_, [], _) -&gt;
  false;
find(I, [_|RRs] = Rs, Ds) -&gt;
  case lists:prefix(Ds, Rs) of
    true -&gt;
      {true, I};
    false -&gt;
      find(I + 1, RRs, Ds)
  end.

score(N) -&gt;
  score(0, 1, #{0 =&gt; 3, 1 =&gt; 7}, 2, N).

score(_, _, Map, M, N) when M &gt; N + 11 -&gt;
  Fold =
    fun(X, A) -&gt;
        A * 10 + maps:get(X, Map)
    end,
  {Map, lists:foldl(Fold, 0, lists:seq(N, N + 9))};
score(E1, E2, Map, M, N) -&gt;
  %io:format("~p~n", [[D || {_, D} &lt;- lists:sort(maps:to_list(Map))]]),
  S1 = maps:get(E1, Map),
  S2 = maps:get(E2, Map),
  New = S1 + S2,
  %io:format("~p~n", [{E1, E2, S1, S2, New}]),
  {NewMap, NewM} =
    case New &gt; 9 of
      true -&gt;
        {Map#{M =&gt; New div 10, M + 1 =&gt; New rem 10}, M + 2};
      false -&gt;
        {Map#{M =&gt; New}, M + 1}
    end,
  {NewE1, NewE2} =
    {(E1 + S1 + 1) rem NewM, (E2 + S2 + 1) rem NewM},
  score(NewE1, NewE2, NewMap, NewM, N).
 </code></pre>

&nbsp;
