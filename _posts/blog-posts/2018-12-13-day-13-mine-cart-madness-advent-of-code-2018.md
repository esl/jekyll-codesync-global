
---
title: Day 13: Mine Cart Madness - Advent of Code 2018
abstract: "I did the Advent of Code 2018 day 13 challenge in Erlang! Parts one and two are as follows:
"
image: /images/Day 13.png
speaker_id: stavros-aronis
type: article
keywords: Advent of Code, Erlang
date: 2018-12-13
tags: Erlang,Advent of Code
---
I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/13">day 13 challenge</a>&nbsp;in Erlang! Parts one and two are as follows:

<pre>
<code class="language-erlang">#!/usr/bin/env escript
-mode(native).

%% https://adventofcode.com/2018/day/13

main(Args) -&gt;
  Lines = read_lines(),
  Map = make_map(Lines),
  Sol =
    case Args of
      ["2"] -&gt; final_cart(Map);
      _ -&gt; first_collision(Map)
    end,
  io:format("~p~n", [Sol]).

read_lines() -&gt;
  read_lines([]).

read_lines(Acc) -&gt;
  case io:get_line("") of
    eof -&gt; lists:reverse(Acc);
    Line -&gt; read_lines([Line|Acc])
  end.

make_map(Lines) -&gt;
  make_map(Lines, 0, 0, #{}, []).

make_map([], Y, X, Map, Carts) -&gt;
  {Carts, Map, Y - 1, X - 2};
make_map([Line|Rest], Y, MX, MapIn, CartsIn) -&gt;
  Fold =
    fun(S, {X, Map, Carts}) -&gt;
        NewMap =
          case S of
            H when H =:= $-; H =:= $&lt;; H =:= $&gt; -&gt; Map#{{Y, X} =&gt; $-};
            V when V =:= $|; V =:= $^; V =:= $v -&gt; Map#{{Y, X} =&gt; $|};
            $+ -&gt; Map#{{Y, X} =&gt; S};
            $/ -&gt; Map#{{Y, X} =&gt; S};
            $\\ -&gt; Map#{{Y, X} =&gt; S};
            _ -&gt; Map
          end,
        NewCarts =
          case S =:= $&gt; orelse S =:= $&lt; orelse S =:= $v orelse S =:= $^ of
            true -&gt; orddict:store({Y, X}, {S, 1}, Carts);
            false -&gt; Carts
          end,
        {X + 1, NewMap, NewCarts}
    end,
  {X, NewMap, NewCarts} = lists:foldl(Fold, {0, MapIn, CartsIn}, Line),
  make_map(Rest, Y + 1, max(MX, X), NewMap, NewCarts).

%% print({Carts, Map, MY, MX}) -&gt;
%%   Foreach =
%%     fun(Y) -&gt;
%%         For =
%%           fun(X) -&gt;
%%               case orddict:find({Y, X}, Carts) of
%%                 {ok, {V, _}} -&gt;
%%                   io:format("~c", [V]);
%%                 error -&gt;
%%                   io:format("~c", [maps:get({Y,X}, Map, $ )])
%%               end
%%           end,
%%           lists:foreach(For, lists:seq(0, MX)),
%%           io:format("~n")
%%     end,
%%   lists:foreach(Foreach, lists:seq(0, MY)).

first_collision(Map) -&gt;
  %% print(Map),
  %% io:format("~n"),
  case step(Map) of
    {[X|_], _NewMap} -&gt; X;
    {[], NewMap} -&gt; first_collision(NewMap)
  end.

step({Carts, Map, MY, MX}) -&gt;
  {Crash, NewCarts} = step(Carts, Map, [], []),
  {Crash, {NewCarts, Map, MY, MX}}.

step([], _Map, NewCarts, Crashed) -&gt;
  {lists:reverse(Crashed), NewCarts};
step([{{Y,X}, {Dir, C}}|Rest], Map, NCarts, Crashed) -&gt;
  {NY, NX} =
    case Dir of
      $&gt; -&gt; {Y, X + 1};
      $&lt; -&gt; {Y, X - 1};
      $^ -&gt; {Y - 1, X};
      $v -&gt; {Y + 1, X}
    end,
  case {orddict:find({NY, NX}, Rest), orddict:find({NY, NX}, NCarts)} of
    {error, error} -&gt;
      Track = maps:get({NY, NX}, Map),
      ND =
        case Track of
          $/ -&gt;
            case Dir of
              $&gt; -&gt; $^;
              $&lt; -&gt; $v;
              $^ -&gt; $&gt;;
              $v -&gt; $&lt;
            end;
          $\\ -&gt;
            case Dir of
              $&gt; -&gt; $v;
              $&lt; -&gt; $^;
              $^ -&gt; $&lt;;
              $v -&gt; $&gt;
            end;
          $+ -&gt;
            case C of
              1 -&gt; left(Dir);
              2 -&gt; straight(Dir);
              3 -&gt; right(Dir)
            end;
          _ -&gt; Dir
        end,
      NC =
        case Track of
          $+ -&gt; (C rem 3) + 1;
          _ -&gt; C
        end,
      NewNCarts = orddict:store({NY, NX}, {ND, NC}, NCarts),
      step(Rest, Map, NewNCarts, Crashed);
    _ -&gt;
      NewRest = orddict:erase({NY, NX}, Rest),
      NewNCarts = orddict:erase({NY, NX}, NCarts),
      step(NewRest, Map, NewNCarts, [{NX, NY}|Crashed])
  end.

straight(S) -&gt; S.

left($&gt;) -&gt; $^;
left($&lt;) -&gt; $v;
left($^) -&gt; $&lt;;
left($v) -&gt; $&gt;.

right($&gt;) -&gt; $v;
right($&lt;) -&gt; $^;
right($^) -&gt; $&gt;;
right($v) -&gt; $&lt;.

final_cart(Map) -&gt;
  %% print(Map),
  %% io:format("~n"),
  %% timer:sleep(500),
  case step(Map) of
    {_, {[{{Y,X},_}], _, _, _}} -&gt; {X,Y};
    {_, NewMap} -&gt; final_cart(NewMap)
  end.
 </code></pre>

&nbsp;
