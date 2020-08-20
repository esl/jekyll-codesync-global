
---
title: " Day 3: No matter how you slice it - Advent of Code 2018
"
abstract: "I did the Advent of Code 2018 day 3 challenge in Elixir! Parts one and two are as follows:
"
image_url: /uploads//images/Day 3.png
speaker_id: simon-escobar-benitez
type: article
keywords: Advent of Code, Elixir
date: 2018-12-03
tags: Elixir,Advent of Code
---
I did&nbsp;the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/3">day 3 challenge</a>&nbsp;in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day3 do
  @grid_info ~r/#(\d+) @ (\d+),(\d+): (\d+)x(\d+)/

  def part1(input) do
    {overlaps, _} =
      input
      |&gt; read_input()
      |&gt; extract_grid()
      |&gt; build_map()
      |&gt; Enum.reduce({MapSet.new(), MapSet.new()}, fn {x, y, _id}, {overlap, no_overlap} -&gt;
        if MapSet.member?(no_overlap, {x, y}) do
          {MapSet.put(overlap, {x, y}), no_overlap}
        else
          {overlap, MapSet.put(no_overlap, {x, y})}
        end
      end)

    MapSet.size(overlaps)
  end

  def part2(input) do
    map =
      input
      |&gt; read_input()
      |&gt; extract_grid()
      |&gt; build_keyed_map()

    {id, _} =
      for {id1, coordinates1} &lt;- map,
          {id2, coordinates2} &lt;- map,
          id1 != id2 do
        overlaps = MapSet.intersection(coordinates1, coordinates2)

        if MapSet.size(overlaps) != 0 do
          {id1, id2}
        else
          {id1, nil}
        end
      end
      |&gt; Enum.group_by(fn {key, _} -&gt; key end, fn {_, value} -&gt; value end)
      |&gt; Enum.find(fn {_key, overlaps} -&gt;
        Enum.all?(overlaps, &amp;Kernel.==(&amp;1, nil))
      end)

    id
  end

  defp read_input(input) do
    input
    |&gt; File.read!()
    |&gt; String.split("\n", trim: true)
  end

  defp extract_grid(lines) do
    Enum.map(lines, fn line -&gt;
      [id, x, y, width, height] = Regex.run(@grid_info, line, capture: :all_but_first)

      [
        id,
        String.to_integer(x),
        String.to_integer(y),
        String.to_integer(width),
        String.to_integer(height)
      ]
    end)
  end

  defp build_map(grid) do
    Enum.flat_map(grid, fn [id, x, y, width, height] -&gt;
      for x &lt;- x..(x + width - 1),
          y &lt;- y..(y + height - 1) do
        {x, y, id}
      end
    end)
  end

  defp build_keyed_map(grid) do
    Enum.reduce(grid, %{}, fn [id, x, y, width, height], acc -&gt;
      coordinates =
        for x &lt;- x..(x + width - 1),
            y &lt;- y..(y + height - 1) do
          {x, y}
        end

      Map.put_new(acc, id, MapSet.new(coordinates))
    end)
  end
end

# :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day3.txt") |&gt; Day3.part1()
# :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day3.txt") |&gt; Day3.part2()
 </code></pre>

&nbsp;
