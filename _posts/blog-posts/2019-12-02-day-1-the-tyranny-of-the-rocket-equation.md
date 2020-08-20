
---
title: " Day 1: The Tyranny of the Rocket Equation
"
abstract: "Day 1: The Tyranny of the Rocket Equation
"
image_url: /uploads//images/DAY 01.png
speaker_id: stavros-aronis
type: article
keywords: Advent of Code, Erlang
date: 2019-12-02
tags: Erlang,Advent of Code
---
I did&nbsp;the Advent of Code 2019&nbsp;<a href="https://adventofcode.com/2019/day/1">day 1&nbsp;challenge</a>&nbsp;in Erlang!&nbsp;

&nbsp;

<pre>
<code class="language-erlang">#!/usr/bin/env escript
-mode(native).

%% https://adventofcode.com/2019/day/1

main(Args) -&gt;
  Nums = lists:append(read_list("~d")),
  Sol =
    case Args of
      ["2"] -&gt; fuel_proper(Nums);
      _ -&gt; fuel_mod(Nums)
    end,
  io:format("~w~n", [Sol]).

read_list(Pat) -&gt;
  read_list(Pat, []).

read_list(Pat, Acc) -&gt;
  case io:fread("", Pat) of
    {ok, Res} -&gt; read_list(Pat, [Res|Acc]);
    eof -&gt; lists:reverse(Acc)
  end.

fuel_mod(Nums) -&gt;
  lists:sum([(Mod div 3) - 2 || Mod &lt;- Nums]).

fuel_proper(Nums) -&gt;
  lists:sum([mod_proper(Mod) || Mod &lt;- Nums]).

mod_proper(Mod) -&gt;
  mod_proper(Mod, 0).

mod_proper(Lift, Fuel) -&gt;
  NF = (Lift div 3) - 2,
  case NF &lt; 0 of
    true -&gt; Fuel;
    false -&gt; mod_proper(NF, Fuel + NF)
  end.
</code></pre>

&nbsp;

&nbsp;

<pre>

&nbsp;</pre>
