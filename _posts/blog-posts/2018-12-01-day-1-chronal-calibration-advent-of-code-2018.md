
---
title: " Day 1: Chronal Calibration - Advent of Code 2018
"
abstract: "Advent of Code 2018 - Day 1 solution in Elixir! #AdventOfBEAM
"
image: /images/Day 1.png
speaker_id: simon-escobar-benitez
type: article
keywords: Advent of Code
date: 2018-12-01
tags: Elixir,Advent of Code
---
I did&nbsp;the Advent of Code 2018 <a href="https://adventofcode.com/2018/day/1">day 1 challenge</a>&nbsp;in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day1 do
  def frequency(input) do
    input
    |&gt; read_input()
    |&gt; Enum.reduce(0, fn line, acc -&gt;
      String.to_integer(line) + acc
    end)
  end

  def repeated_frequency(input) do
      input
      |&gt; read_input()
      |&gt; Stream.cycle()
      |&gt; Enum.reduce_while({0, MapSet.new([0])}, fn line, {acc, seen} -&gt;
        value = String.to_integer(line) + acc
        if MapSet.member?(seen, value) do
          {:halt, value}
        else
          {:cont, {value, MapSet.put(seen, value)}}
        end
      end)
  end

  defp read_input(input) do
    input
    |&gt; File.read!()
    |&gt; String.split("\n", trim: true)
  end
end

:aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day1.txt") |&gt; Day1.frequency()
:aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day1.txt") |&gt; Day1.repeated_frequency()</code></pre>

&nbsp;

&nbsp;
