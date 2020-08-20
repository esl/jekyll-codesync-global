
---
title: Day 9: Marble Mania - Advent of Code 2018
abstract: I did the Advent of Code 2018 day 9 challenge in Erlang! Parts one and two are as follows:
image: /images/Day 9.png
speaker_id: stavros-aronis
type: article
keywords: Advent of Code, Erlang
date: 2018-12-09
tags: Erlang,Advent of Code
---
I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/9">day 9 challenge</a>&nbsp;in Erlang! Parts one and two are as follows:

<pre>
<code class="language-erlang">#!/usr/bin/env escript
-mode(native).

%% https://adventofcode.com/2018/day/9

main(Args) -&gt;
  {ok, [N, M]} = io:fread("", "~d ~d"),
  Sol =
    case Args of
      ["2"] -&gt; high_score(N, M * 100);
      _ -&gt; high_score(N, M)
    end,
  io:format("~p~n", [Sol]).

high_score(N, M) -&gt;
  high_score({[0], []}, 1, #{}, N, M).

high_score(_Marbles, Turn, Scores, _N, Last) when Turn &gt; Last -&gt;
  FindWinner =
    fun(K, V, {_, MaxV} = Max) -&gt;
        case V &gt; MaxV of
          true -&gt; {K, V};
          false -&gt; Max
        end
    end,
  {_Winner, HighScore} = maps:fold(FindWinner, {0, 0}, Scores),
  HighScore;
high_score(Marbles, Turn, Scores, N, Last) -&gt;
  case Turn rem 23 =:= 0 of
    false -&gt;
      NewMarbles = add_rotate_2(Turn, Marbles),
      high_score(NewMarbles, Turn + 1, Scores, N, Last);
    true -&gt;
      {H, NewMarbles} = rem_rotate_neg7(Marbles),
      Points = Turn + H,
      Update = fun(V) -&gt; V + Points end,
      Winner = Turn rem N,
      NewScores = maps:update_with(Winner, Update, Points, Scores),
      high_score(NewMarbles, Turn + 1, NewScores, N, Last)
  end.

add_rotate_2(New, {[0], []}) -&gt; {[New, 0], []};
add_rotate_2(New, {Front, Back}) -&gt;
  case Front of
    [H1, H2|T] -&gt;
      {[New|T], [H2, H1|Back]};
    [H1] -&gt;
      [H2|T] = lists:reverse(Back),
      {[New|T], [H2, H1]};
    [] -&gt;
      [H1, H2|T] = lists:reverse(Back),
      {[New|T], [H2, H1]}
  end.

rem_rotate_neg7({Front, Back}) -&gt;
  case Back of
    [HN1, HN2, HN3, HN4, HN5, HN6, HN7|R] -&gt;
      {HN7, {[HN6, HN5, HN4, HN3, HN2, HN1|Front], R}};
    _ -&gt;
      Rev = Back ++ lists:reverse(Front),
      rem_rotate_neg7({[], Rev})
  end.
 </code></pre>

&nbsp;
