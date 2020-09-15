---
title: " Day 6: Chronal Coordinates - Advent of Code 2018
"
abstract: "I did the Advent of Code 2018 day 6 challenge in Elixir! Parts one and two are as follows:
"
image_url: /uploads/images/Day_6.png
speakers:
- _speakers/simon-escobar-benitez.md
type: article
keywords: Advent of Code, Elixir
date: 2018-12-06
tags: Elixir,Advent of Code
---

I did the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/6">day 6 challenge</a>&nbsp;in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day6 do
  def part1(input) do
    {_infinites, finites} =
      input
      |&gt; read_input()
      |&gt; create_grid()
      |&gt; group_by_infinites()

    {_coordinates, layout} =
      finites
      |&gt; Enum.group_by(fn {x, y, _dist} -&gt; {x, y} end)
      |&gt; Enum.max_by(fn {_, v} -&gt; length(v) end)

    length(layout)
  end

  def part2(input) do
    {max_x, max_y, coordinates} =
      input
      |&gt; read_input()

    for x &lt;- 0..max_x,
        y &lt;- 0..max_y do
      coordinates
      |&gt; Enum.reduce(0, fn {point_x, point_y}, acc -&gt;
        distance = abs(x - point_x) + abs(y - point_y)
        distance + acc
      end)
    end
    |&gt; Enum.filter(&amp;(&amp;1 &lt; 10_000))
    |&gt; Enum.count()
  end

  defp read_input(input) do
    {max_x, max_y, coordinates} =
      input
      |&gt; File.read!()
      |&gt; String.trim()
      |&gt; String.split("\n", trim: true)
      |&gt; Enum.reduce({-1, -1, []}, fn line, {max_x, max_y, coordinates} -&gt;
        [x, y] = String.split(line, ", ")
        x = String.to_integer(x)
        y = String.to_integer(y)
        coordinate = {x, y}
        max_x = if x &gt; max_x, do: x, else: max_x
        max_y = if y &gt; max_y, do: y, else: max_y
        {max_x, max_y, [coordinate | coordinates]}
      end)

    {max_x, max_y, Enum.reverse(coordinates)}
  end

  defp create_grid({max_x, max_y, coordinates}) do
    for x &lt;- 0..max_x,
        y &lt;- 0..max_y do
      [{px, py, dist1}, {_, _, dist2} | _] =
        Enum.map(coordinates, fn {point_x, point_y} -&gt;
          distance = abs(x - point_x) + abs(y - point_y)
          {point_x, point_y, distance}
        end)
        |&gt; Enum.sort_by(fn {_, _, distance} -&gt; distance end)

      if dist1 == dist2 do
        nil
      else
        cond do
          x == 0 -&gt;
            {:infinite, px, py}

          x == max_x -&gt;
            {:infinite, px, py}

          y == 0 -&gt;
            {:infinite, px, py}

          y == max_y -&gt;
            {:infinite, px, py}

          true -&gt;
            {px, py, dist1}
        end
      end
    end
  end

  defp group_by_infinites(grid) do
    Enum.reduce(grid, {MapSet.new(), []}, fn
      nil, acc -&gt;
        acc

      {:infinite, px, py}, {infinites, finites} -&gt;
        {MapSet.put(infinites, {px, py}), finites}

      {px, py, dist}, {infinites, finites} -&gt;
        if MapSet.member?(infinites, {px, py}) do
          {infinites, finites}
        else
          {infinites, [{px, py, dist} | finites]}
        end
    end)
  end
end

# r Day6; :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day6.txt") |&gt; Day6.part1()
# r Day6; :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day6.txt") |&gt; Day6.part2()
 </code></pre>

&nbsp;
