
---
title: Day 18: Settlers of The North Pole - Advent of Code 2018
abstract: I did the Advent of Code 2018 day 18 challenge in Erlang! Parts one and two are as follows:
image: /images/Day 18.png
speaker_id: stavros-aronis
type: article
keywords: Advent of Code, Erlang
date: 2018-12-18
tags: Erlang,Advent of Code
---
I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/18">day 18 challenge</a>&nbsp;in Erlang! Parts one and two are as follows:

<pre>
<code class="language-erlang">#!/usr/bin/env escript
-mode(native).

%% https://adventofcode.com/2018/day/18

main(Args) -&gt;
  Lines = read_lines(),
  Map = make_map(Lines),
  Sol =
    case Args of
      ["2"] -&gt; score(evolve(1000000000, Map));
      _ -&gt; score(evolve(10, Map))
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
  make_map(Lines, 0, 0, #{}).

make_map([], Y, X, Map) -&gt;
  {Map, Y - 1, X - 2};
make_map([Line|Rest], Y, MX, MapIn) -&gt;
  Fold =
    fun(S, {X, Map}) -&gt;
        NewMap =
          case S of
            $\n -&gt; Map;
            _ -&gt; Map#{{Y, X} =&gt; S}
          end,
        {X + 1, NewMap}
    end,
  {X, NewMap} =
    lists:foldl(Fold, {0, MapIn}, Line),
  make_map(Rest, Y + 1, max(MX, X), NewMap).

%% print({Map, MaxX, MaxY}) -&gt;
%%   Foreach =
%%     fun(Y) -&gt;
%%         For =
%%           fun(X) -&gt;
%%               C =
%%                 case {X, Y} of
%%                   _ -&gt; maps:get({Y, X}, Map, $.)
%%                 end,
%%               io:format("~c", [C])
%%           end,
%%         lists:foreach(For, lists:seq(0, MaxX)),
%%         io:format("~n")
%%     end,
%%   lists:foreach(Foreach, lists:seq(0, MaxY)).

evolve(N, Map) -&gt;
  evolve(N, {erlang:phash2(Map), Map}, #{}).

evolve(0, {_, Map}, _) -&gt;
  Map;
evolve(N, {Hash, Map}, Memo) -&gt;
  {NewMap, NewMemo} =
    case maps:get(Hash, Memo, no) of
      no -&gt;
        NM = evolve(Map),
        NHash = erlang:phash2(NM),
        NMe = Memo#{Hash =&gt; {NHash, NM}},
        {{NHash, NM}, NMe};
      O -&gt;
        {O, Memo}
    end,
  evolve(N - 1, NewMap, NewMemo).

evolve({Map, MaxX, MaxY}) -&gt;
  RFold =
    fun(Y, RAcc) -&gt;
        Fold =
          fun(X, Acc) -&gt;
              W = {Y, X},
              C = maps:get(W, Map),
              {O, T, Ya} = count_neighs(W, {MaxY, MaxX}, Map),
              case {C, O, T, Ya} of
                {$., _, N, _} when N &gt;= 3 -&gt; Acc#{W =&gt; $|};
                {$|, _, _, N} when N &gt;= 3 -&gt; Acc#{W =&gt; $#};
                {$#, _, T, Ya} when Ya == 0; T == 0 -&gt; Acc#{W =&gt; $.};
                _ -&gt; Acc
              end
          end,
        lists:foldl(Fold, RAcc, lists:seq(0, MaxX))
    end,
  {lists:foldl(RFold, Map, lists:seq(0, MaxY)), MaxX, MaxY}.

count_neighs({Y, X}, {MaxY, MaxX}, Map) -&gt;
  Ns =
    [{YY, XX}
     || YY &lt;- lists:seq(Y - 1, Y + 1),
        XX &lt;- lists:seq(X - 1, X + 1),
        YY &gt;= 0,
        XX &gt;= 0,
        YY =&lt; MaxY,
        XX =&lt; MaxX,
        {YY, XX} =/= {Y, X}],
  Fold =
    fun(W, {O, T, Ya}) -&gt;
        case maps:get(W, Map) of
          $. -&gt; {O + 1, T, Ya};
          $| -&gt; {O, T + 1, Ya};
          $# -&gt; {O, T, Ya + 1}
        end
    end,
  lists:foldl(Fold, {0, 0, 0}, Ns).

score({Map, _, _}) -&gt;
  Fold =
    fun(_, V, {T, Y}) -&gt;
        case V of
          $| -&gt; {T + 1, Y};
          $# -&gt; {T, Y + 1};
          _ -&gt; {T, Y}
        end
    end,
  {Y, T} = maps:fold(Fold, {0, 0}, Map),
  Y * T.
 </code></pre>

&nbsp;
