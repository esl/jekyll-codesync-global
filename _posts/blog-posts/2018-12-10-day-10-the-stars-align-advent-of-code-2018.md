
---
title: Day 10: The Stars Align - Advent of Code  2018
abstract: "I did the Advent of Code 2018 day 10 challenge in Elixir! Parts one and two are as follows:
"
image: /images/Advent-of-BEAM-10-18.jpg
speaker_id: simon-escobar-benitez
type: article
keywords: 
date: 2018-12-10
tags: Elixir,Advent of Code
---
I did the Advent of Code 2018 <a href="https://adventofcode.com/2018/day/10">day 10 challenge</a> in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day10 do
  def part1(input) do
    {grid, _times} =
      input
      |&gt; read_input()
      |&gt; parse_input()
      |&gt; iter()

    draw(grid)
  end

  def part2(input) do
    {_grid, times} =
      input
      |&gt; read_input()
      |&gt; parse_input()
      |&gt; iter()

    times
  end

  defp read_input(input) do
    input
    |&gt; File.read!()
    |&gt; String.split("\n", trim: true)
  end

  defp parse_input(input) do
    Enum.map(input, fn line -&gt;
      [x, y, vx, vy] =
        Regex.scan(~r/(-?\d+)/, line, capture: :all_but_first)
        |&gt; List.flatten()
        |&gt; Enum.map(&amp;String.to_integer/1)

      {{x, y}, {vx, vy}}
    end)
  end

  defp iter(grid) do
    distance =
      grid
      |&gt; layout()
      |&gt; distance()

    iter(grid, distance, 0)
  end

  defp iter(grid, previous_distance, times) do
    new_grid =
      Enum.map(grid, fn {{x, y}, {vx, vy} = v} -&gt;
        {{x + vx, y + vy}, v}
      end)

    distance =
      new_grid
      |&gt; layout()
      |&gt; distance()

    if distance &lt;= previous_distance do
      iter(new_grid, distance, times + 1)
    else
      {grid, times}
    end
  end

  defp layout(grid) do
    Enum.reduce(grid, {0, 0, 0, 0}, fn {{x, _}, _}, {min_x, max_x, min_y, max_y} -&gt;
      {
        Enum.min([min_x, x]),
        Enum.max([max_x, x]),
        Enum.min([min_y, x]),
        Enum.max([max_y, x])
      }
    end)
  end

  defp distance({min_x, max_x, min_y, max_y}) do
    abs(max_x - min_x) + abs(max_y - min_y)
  end

  defp draw(grid) do
    grid =
      grid
      |&gt; Enum.map(&amp;{elem(&amp;1, 0), 0})
      |&gt; Enum.into(%{})

    {min_x, max_x, min_y, max_y} = layout(grid)

    for y &lt;- min_y..max_y do
      for x &lt;- min_x..max_x do
        if Map.get(grid, {x, y}) do
          IO.write("#")
        else
          IO.write(".")
        end
      end

      IO.write("\n")
    end

    grid
  end
end

# r Day10; :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day10.txt") |&gt; Day10.part1()
# r Day10; :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day10.txt") |&gt; Day10.part2()</code></pre>

&nbsp;
