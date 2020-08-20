
---
title: " Day 8: Memory maneuver - Advent of Code
"
abstract: "I did the Advent of Code 2018 day 8 challenge in Erlang! Parts one and two are as follows:
"
image_url: /uploads//images/Day 8.png
speaker_id: stavros-aronis
type: article
keywords: Advent of Code, Erlang
date: 2018-12-08
tags: Erlang,Advent of Code
---
I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/8">day 8 challenge</a>&nbsp;in Erlang! Parts one and two are as follows:

<pre>
<code class="language-erlang">#!/usr/bin/env escript
-mode(native).

%% https://adventofcode.com/2018/day/8

main(Args) -&gt;
  [Input] = read_input(),
  Sol =
    case Args of
      ["2"] -&gt; value([Input], [1]);
      _ -&gt; sum_metadata([Input])
    end,
  io:format("~p~n", [Sol]).

read_input() -&gt;
  read_input(1).

read_input(0) -&gt; [];
read_input(Nodes) -&gt;
  {ok, [ChildrenCount, MetaCount]} = io:fread("", "~d ~d"),
  Children = read_input(ChildrenCount),
  ReadMeta =
    fun() -&gt;
        {ok, [M]} = io:fread("", "~d"),
        M
    end,
  Meta = [ReadMeta() || _ &lt;- lists:seq(1, MetaCount)],
  Rest = read_input(Nodes - 1),
  [{Children, Meta} | Rest].

sum_metadata([]) -&gt; 0;
sum_metadata([{Children, Meta}|Rest]) -&gt;
  A = sum_metadata(Rest),
  B = sum_metadata(Children),
  C = lists:sum(Meta),
  A + B + C.

value([], Meta) -&gt;
  lists:sum(Meta);
value(Children, Which) -&gt;
  Marked = select(Which, Children),
  lists:sum([value(C, M) || {C, M} &lt;- Marked]).

select(Which, Children) -&gt;
  select(Which, Children, []).

select([], _, Acc) -&gt;
  Acc;
select([N|R], Children, Acc) -&gt;
  try
    C = lists:nth(N, Children),
    select(R, Children, [C|Acc])
  catch
    _:_ -&gt;
      select(R, Children, Acc)
  end.
 </code></pre>

&nbsp;
