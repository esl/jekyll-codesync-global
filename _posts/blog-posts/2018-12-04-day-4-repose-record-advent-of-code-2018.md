
---
title: Day 4: Repose Record - Advent of Code 2018
abstract: I did the Advent of Code 2018 day 4 challenge in Elixir! Parts one and two are as follows:
image: /images/Day 4.png
speaker_id: simon-escobar-benitez
type: article
keywords: Advent of Code, Elixir
date: 2018-12-04
tags: Elixir,Advent of Code
---
I did&nbsp;the Advent of Code 2018&nbsp;<a href="https://adventofcode.com/2018/day/4">day 4 challenge</a>&nbsp;in Elixir! Parts one and two are as follows:

<pre>
<code class="language-elixir">defmodule Day4_1 do
  # 1518-11-01 00:00
  @log_info ~r/\[(\d+)-(\d+)-(\d+) (\d+):(\d+)\] (.*)/
  @guard_entry ~r/Guard #(\d+) begins shift/

  def part1(input) do
    {_, _, guards} =
      input
      |&gt; read_input()
      |&gt; extract_logs()

    {guard_id, _time_asleep} = most_asleep(guards)
    {minute, _size} = get_most_asleep_minutes(guards[guard_id])
    minute * String.to_integer(guard_id)
  end

  def part2(input) do
    {_, _, guards} =
      input
      |&gt; read_input()
      |&gt; extract_logs()

    {guard_id, minute, _size} = most_times_asleep(guards)
    minute * String.to_integer(guard_id)
  end

  defp read_input(input) do
    input
    |&gt; File.read!()
    |&gt; String.split("\n", trim: true)
  end

  def extract_logs(lines) do
    lines
    |&gt; Enum.sort()
    |&gt; Enum.map(fn line -&gt;
      [_year, _month, _day, _hour, minute, message] =
        Regex.run(@log_info, line, capture: :all_but_first)

      case message do
        "falls asleep" -&gt;
          {:sleep, String.to_integer(minute)}

        "wakes up" -&gt;
          {:wake, String.to_integer(minute)}

        _ -&gt;
          [guard_id] = Regex.run(@guard_entry, message, capture: :all_but_first)
          {:guard, guard_id}
      end
    end)
    |&gt; Enum.reduce({nil, nil, %{}}, fn log, {current_guard_id, last_minute, guards} -&gt;
      case log do
        {:guard, guard_id} -&gt;
          {guard_id, nil, Map.put_new(guards, guard_id, [])}

        {:sleep, minute} -&gt;
          {current_guard_id, minute, guards}

        {:wake, minute} -&gt;
          new_range = for min &lt;- last_minute..(minute - 1), do: min
          {current_guard_id, nil, Map.update!(guards, current_guard_id, &amp;(&amp;1 ++ new_range))}
      end
    end)
  end

  defp most_asleep(guards) do
    Enum.reduce(guards, {nil, 0}, fn {guard_id, sleep_range}, {_, current_bigger_time} = acc -&gt;
      time_asleep = length(sleep_range)

      if time_asleep &gt; current_bigger_time do
        {guard_id, time_asleep}
      else
        acc
      end
    end)
  end

  defp most_times_asleep(guards) do
    Enum.reduce(guards, {nil, 0, 0}, fn {guard_id, asleep_range},
                                        {_current_guard, _minute, max_size} = acc -&gt;
      {minute, size} = get_most_asleep_minutes(asleep_range)

      if size &gt; max_size do
        {guard_id, minute, size}
      else
        acc
      end
    end)
  end

  defp get_minutes_of(guard_intervals) do
    {asleep_range, []} =
      guard_intervals
      |&gt; Enum.reduce({[], []}, fn
        {datetime, _}, {range, []} -&gt;
          {range, [datetime.minute]}

        {datetime, _}, {range, [asleep_time]} -&gt;
          new_range = for min &lt;- asleep_time..(datetime.minute - 1), do: min
          {[new_range | range], []}
      end)

    {minute, _size} = get_most_asleep_minutes(asleep_range)
    minute
  end

  defp get_most_asleep_minutes(asleep_range) do
    asleep_range
    |&gt; Enum.group_by(&amp; &amp;1)
    |&gt; Enum.reduce({nil, 0}, fn {minute, occurrences}, {_, times} = acc -&gt;
      size = length(occurrences)

      if size &gt; times do
        {minute, size}
      else
        acc
      end
    end)
  end
end

# :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day4.txt") |&gt; Day4_1.part1()
# :aoc2018 |&gt; :code.priv_dir() |&gt; Path.join("day4.txt") |&gt; Day4_1.part2()</code></pre>

&nbsp;
