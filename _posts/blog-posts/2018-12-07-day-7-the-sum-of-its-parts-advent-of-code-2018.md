
---
title: " Day 7: The Sum of Its Parts - Advent of Code 2018
"
abstract: "I did the Advent of Code 2018 day 7 challenge in Elixir! Parts one and two are as follows:
"
image_url: /uploads/images/Day_7.png
speaker1: _speakers/simon-escobar-benitez.md
type: article
keywords: Advent of Code, Elixir
date: 2018-12-07
tags: Elixir,Advent of Code
---
I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/7">day 7 challenge</a>&nbsp;in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day7 do
  @instruction_info ~r/Step (\w+) must be finished before step (\w+) can begin/
  def part1(input) do
    input
    |&gt; read_input()
    |&gt; parse_input()
    |&gt; discover_dependencies([])
  end

  defp read_input(input) do
    input
    |&gt; File.read!()
    |&gt; String.split("\n", trim: true)
  end

  defp parse_input(lines) do
    Enum.reduce(lines, %{}, fn line, acc -&gt;
      [step, dependency] = Regex.run(@instruction_info, line, capture: :all_but_first)

      acc
      |&gt; Map.put_new(step, [])
      |&gt; Map.update(dependency, [step], fn dependencies -&gt;
        Enum.sort([step | dependencies])
      end)
    end)
    |&gt; Enum.to_list()
  end

  defp discover_dependencies(deps_tree, acc) when map_size(deps_tree) == 0do
    acc |&gt; Enum.reverse() |&gt; Enum.join()
  end

  defp discover_dependencies(deps_tree, acc) do
    {step, []} = Enum.find(deps_tree, fn {_, deps} -&gt; Enum.empty?(deps) end)

    new_dependencies =
      Enum.reduce(deps_tree, %{}, fn
        {^step, _}, acc -&gt; Map.delete(acc, step)
        {other_step, deps}, acc -&gt; Map.put_new(acc, other_step, List.delete(deps, step))
      end)

    discover_dependencies(new_dependencies, [step | acc])
  end
end

# r Day7; :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day7.txt") |&gt; Day7.part1()
# :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day7.txt") |&gt; Day7.part2()

input = "Step D must be finished before step E can begin.
Step F must be finished before step E can begin.
Step C must be finished before step A can begin.
Step B must be finished before step E can begin.
Step A must be finished before step D can begin.
Step C must be finished before step F can begin.
Step A must be finished before step B can begin."

# r Day7; Day7.part1(input)
 </code></pre>

<pre>

&nbsp;</pre>
