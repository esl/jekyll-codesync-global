
---
title: " Day 2: Inventory Management System - Advent of Code 2018
"
abstract: "I did the Advent of Code 2018 day 2 challenge in Elixir! Parts one and two are as follows:
"
image_url: /uploads//images/Day 2.png
speaker_id: simon-escobar-benitez
type: article
keywords: Advent of Code, Elixir
date: 2018-12-02
tags: Elixir,Advent of Code
---
I did&nbsp;the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/2">day 2 challenge</a>&nbsp;in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day2 do
  def checksum(input) do
    {two, three} =
      input
      |&gt; read_input()
      |&gt; Enum.reduce({0, 0}, fn line, {appears_two_times, appears_three_times} -&gt;
        {two_times, three_times, _} =
          line
          |&gt; String.graphemes()
          |&gt; Enum.group_by(&amp; &amp;1)
          |&gt; get_checksum_values()

        {appears_two_times + two_times, appears_three_times + three_times}
      end)

    two * three
  end

  def differ(input) do
    lines = input |&gt; read_input()

    # length(diff) == 4 # [eq: "_", del: "_", ins: "_", eq: "_"]
    for line1 &lt;- lines,
        line2 &lt;- lines,
        line1 != line2,
        diff = String.myers_difference(line1, line2),
        length(diff) == 4 do
      diff
      |&gt; Keyword.get_values(:eq)
      |&gt; Enum.join()
    end
    |&gt; Enum.uniq()
  end

  defp read_input(input) do
    input
    |&gt; File.read!()
    |&gt; String.split("\n", trim: true)
  end

  defp get_checksum_values(groups) do
    Enum.reduce(groups, {0, 0, MapSet.new()}, fn {_key, chars},
                                                 {two_times, three_times, seen} = acc -&gt;
      value = length(chars)

      if MapSet.member?(seen, value) do
        acc
      else
        new_seen = MapSet.put(seen, value)

        case value do
          2 -&gt; {two_times + 1, three_times, new_seen}
          3 -&gt; {two_times, three_times + 1, new_seen}
          _ -&gt; {two_times, three_times, new_seen}
        end
      end
    end)
  end
end

:aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day2.txt") |&gt; Day2.checksum()
:aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day2.txt") |&gt; Day2.differ()</code></pre>

<pre>

&nbsp;</pre>
