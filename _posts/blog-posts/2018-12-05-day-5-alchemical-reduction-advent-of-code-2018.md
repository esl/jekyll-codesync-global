
---
title: Day 5: Alchemical Reduction - Advent of Code 2018
abstract: I did the Advent of Code 2018 day 5 challenge in Elixir! Parts one and two are as follows:
image: /images/Day 5.png
speaker_id: simon-escobar-benitez
type: article
keywords: Advent of Code, Elixir
date: 2018-12-05
tags: Elixir,Advent of Code
---
I did&nbsp;the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/5">day 5 challenge</a>&nbsp;in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day5 do
  def part1(input) do
    input
    |&gt; read_input()
    |&gt; String.codepoints()
    |&gt; reduce_polymer()
  end

  def part2(input) do
    chars =
      input
      |&gt; read_input()
      |&gt; String.codepoints()

    unit_types =
      chars
      |&gt; Enum.map(&amp;String.downcase/1)
      |&gt; Enum.uniq()

    Enum.reduce(unit_types, 1_000_000, fn unit_type, min_size -&gt;
      without_unit =
        Enum.reject(chars, fn char -&gt;
          String.downcase(char) == unit_type
        end)

      min(min_size, reduce_polymer(without_unit))
    end)
  end

  defp read_input(input) do
    input
    |&gt; File.read!()
    |&gt; String.trim()
  end

  defp reduce_polymer(chars) do
    Enum.reduce(chars, [], fn
      char, [] -&gt;
        [char]

      char, [last_seen | rest] = acc -&gt;
        result =
          if char != last_seen &amp;&amp;
               (char == String.upcase(last_seen) || char == String.downcase(last_seen)) do
            rest
          else
            [char | acc]
          end

        result
    end)
    |&gt; length()
  end
end

# :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day5.txt") |&gt; Day5.part1()
# :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day5.txt") |&gt; Day5.part2()

# input = "dabAcCaCBAcCcaDA"
# r Day5; Day5.part2(input)</code></pre>

&nbsp;
