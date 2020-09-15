---
title: " Day 12: Subterranean Sustainability - Advent of Code 2018
"
abstract: "I did the Advent of Code 2018 day 12 challenge in Erlang! Parts one and two are as follows:
"
image_url: /uploads/images/Day_12.png
speakers:
- _speakers/stavros-aronis.md
type: article
keywords: Advent of Code, Erlang
date: 2018-12-12
tags: Advent of Code,Erlang
---

I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/12">day 12 challenge</a>&nbsp;in Erlang! Parts one and two are as follows:

<pre>
<code class="language-erlang">#!/usr/bin/env escript
-mode(native).

%% https://adventofcode.com/2018/day/12

main(Args) -&gt;
  {ok, [Initial]} = io:fread("", "initial state: ~s"),
  {ok, []} = io:fread("", ""),
  Evolve = make_evolve(read_list("~c~c~c~c~c =&gt; ~c")),
  Sol =
    case Args of
      ["2"] -&gt;  sum_state(state_after(50 * 1000, {0, Initial}, Evolve));
      _ -&gt; sum_state(state_after(20, {0, Initial}, Evolve))
    end,
  io:format("~p~n", [Sol]).

read_list(Pat) -&gt;
  read_list(Pat, []).

read_list(Pat, Acc) -&gt;
  case io:fread("", Pat) of
    {ok, Res} -&gt; read_list(Pat, [Res|Acc]);
    eof -&gt; lists:reverse(Acc)
  end.

make_evolve(List) -&gt;
  Fold =
    fun([[A],[B],[C],[D],[E],[F]], Map) -&gt;
        Map#{[A,B,C,D,E] =&gt; F}
    end,
  lists:foldl(Fold, #{}, List).

state_after(0, State, _Evolve) -&gt;
  State;
state_after(N, State, Evolve) -&gt;
  %io:format("~p~n", [State]),
  NewState = evolve(State, Evolve),
  state_after(N - 1, NewState, Evolve).

evolve({F, Line}, Evolve) -&gt;
  evolve("...." ++ Line ++ "....", Evolve, F-2, []).

evolve([A,B,C,D,E|Rest], Evolve, F, Acc) -&gt;
  NC = maps:get([A,B,C,D,E], Evolve, $.),
  {NF, NAcc} =
    case Acc =:= [] andalso NC =:= $. of
      true -&gt; {F + 1, []};
      false -&gt; {F, [NC|Acc]}
    end,
  evolve([B,C,D,E|Rest], Evolve, NF, NAcc);
evolve(_, _, F, Acc) -&gt;
  Pred = fun(C) -&gt; C =:= $. end,
  {F, lists:reverse(lists:dropwhile(Pred, Acc))}.

sum_state({F, List}) -&gt;
  sum_state(F, List, 0).

sum_state(_, [], Sum) -&gt; Sum;
sum_state(I, [C|Rest], Sum) -&gt;
  NSum =
    case C =:= $# of
      true -&gt; Sum + I;
      false -&gt; Sum
    end,
  sum_state(I+1, Rest, NSum).
 </code></pre>

&nbsp;
