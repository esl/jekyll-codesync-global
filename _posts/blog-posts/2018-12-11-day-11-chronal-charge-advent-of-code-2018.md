
---
title: " Day 11: Chronal Charge - Advent of Code 2018
"
abstract: "I did the Advent of Code 2018 day 11 challenge in Elixir! Parts one and two are as follows:
"
image: /images/Day 11.png
speaker_id: simon-escobar-benitez
type: article
keywords: Advent of Code, Elixir
date: 2018-12-11
tags: Elixir,Advent of Code
---
I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/11">day 11 challenge</a>&nbsp;in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day11 do
  def part1(input) do
    input
    |&gt; read_input()
    |&gt; compute_power_levels()
    |&gt; max_cell()
  end

  def part2(input) do
    input
    |&gt; read_input()
    |&gt; compute_power_levels()
    |&gt; max_cell_dynamic_size()
  end

  defp read_input(input) do
    input
    |&gt; File.read!()
  end

  defp compute_power_levels(input) do
    serial_number = String.to_integer(input)

    for x &lt;- 1..300, y &lt;- 1..300 do
      rack_id = x + 10
      power_level = (rack_id * y + serial_number) * rack_id

      power_level =
        if power_level &gt; 100 do
          power_level
          |&gt; to_string()
          |&gt; String.at(-3)
          |&gt; String.to_integer()
        else
          0
        end

      power_level = power_level - 5
      {{x, y}, power_level}
    end
    |&gt; Enum.into(%{})
  end

  defp max_cell(grid) do
    for size &lt;- 0..2 do
      for x &lt;- 1..(300 - size), y &lt;- 1..(300 - size) do
        power_level =
          for xs &lt;- 0..size, ys &lt;- 0..size do
            Map.get(grid, {x + xs, y + ys}) || 0
          end
          |&gt; Enum.sum()

        {power_level, {x, y}}
      end
    end
    |&gt; List.flatten()
    |&gt; Enum.max_by(fn {power_level, _} -&gt; power_level end)
  end

  defp max_cell_dynamic_size(grid) do
    acc = {{0, 0, 0}, 0}

    Enum.reduce(1..299, acc, fn x, acc -&gt;
      Enum.reduce(1..299, acc, fn y, acc -&gt;
        find_largest(x, y, grid, acc)
      end)
    end)
  end

  defp find_largest(x, y, grid, acc) do
    max_square_size = min(301 - x, 301 - y)
    level = grid[{x, y}]

    {best, _} =
      Enum.reduce(2..max_square_size, {acc, level}, fn square_size,
                                                       {{_coord, prev_level} = prev, level} -&gt;
        level = sum_square(x, y, square_size, grid, level)

        if level &gt; prev_level do
          {{{x, y, square_size}, level}, level}
        else
          {prev, level}
        end
      end)

    best
  end

  defp sum_square(x0, y0, square_size, grid, acc) do
    y = y0 + square_size - 1

    acc =
      Enum.reduce(x0..(x0 + square_size - 2), acc, fn x, acc -&gt;
        acc + grid[{x, y}]
      end)

    x = x0 + square_size - 1

    acc =
      Enum.reduce(y0..(y0 + square_size - 1), acc, fn y, acc -&gt;
        acc + grid[{x, y}]
      end)

    acc
  end
end

# r Day11; :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day11.txt") |&gt; Day11.part1()
# :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day11.txt") |&gt; Day11.part2()
 </code></pre>

<pre>

&nbsp;</pre>
