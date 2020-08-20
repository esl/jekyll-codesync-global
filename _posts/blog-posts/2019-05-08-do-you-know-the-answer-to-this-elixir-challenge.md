
---
title: Do you know the answer to this Elixir Challenge?
abstract: Elixir has a concept of protocols which provide polymorphism on data types and structures. In our example we pipe the result of generating a MapSet into the to_list/1 function on the Enum module. Enum is a module containing functions that work on data types implementing the Enumerable protocol; examples of these are lists, maps, and ranges.
image: /images/ce-blog.jpg
speaker_id: martin-gausby
type: article
keywords: Elixir
date: 2019-05-08
tags: Code Elixir LDN
---
<ul>
	<li>**Discussion**</li>
</ul>

<pre>
<code class="language-elixir"># What will this return ?
MapSet.new(1..32) |&gt; Enum.to_list()
# Observe what happens if we request 33 elements instead:
MapSet.new(1..33) |&gt; Enum.to_list()
# Let's discuss what happens here;
# What is special about the number 33 and what does it mean ?</code></pre>

<ul>
	<li>**Explanation**</li>
</ul>

A bunch of stuff is happening here. Elixir has a concept of protocols which provide polymorphism on data types and structures. In our example we pipe the result of generating a <code>MapSet</code> into the <code>to_list/1</code> function on the <code>Enum</code> module. <code>Enum</code> is a module containing functions that work on data types implementing the Enumerable protocol; examples of these are lists, maps, and ranges. <code>Enum</code> can do all kinds of neat stuff on these, such as taking a slice of a range:

<pre>
<code class="language-elixir"># Give me first 5 elements after the 10th element in the range 1 to 20
iex&gt; Enum.slice(1..20, 10, 5)
[11, 12, 13, 14, 15]</code></pre>

Intuitively we would think a data structure has an order; for instance a <code>Range</code> is a sequence of numbers increasing (or decreasing) from the start point to the end point of the range. When we ask to turn a <code>Range</code> into a list using <code>Enum.to_list/1</code> it will materialize into a list of numbers, going from first to last. A Range has a natural order, and so does a list&mdash;which is implemented as a linked list; the elements has an order based on the order they were inserted, as the element at the head of the list will point to the next in the list.

But this intuition fails when it comes to the <code>Map</code> data structure. If we ask for the list representation of a <code>Map</code> containing keys consisting of integers we might get fooled into believing that a <code>Map</code> has an reliable order:

<pre>
<code class="language-elixir">iex&gt; Enum.to_list(%{1 =&gt; :a, 2 =&gt; :b, 3 =&gt; :c})
[{1, :a}, {2, :b}, {3, :c}]</code></pre>

Which might seem odd when we try to construct a <code>Map</code> with 33 or more elements and observe what happens when we convert it to a list:

<pre>
<code class="language-elixir">iex&gt; Enum.into(1..33, %{}, fn (x) -&gt; {x, x} end) |&gt; Enum.to_list()
[{11, 11}, {26, 26}, {15, 15} | _output_omitted]</code></pre>

What is special about 33? Why did we get a seemingly random order in our return?

The technical explanation is that the Erlang Virtual Machine stores maps in different data structures depending on the number of elements in the <code>Map</code>. For a <code>Map</code> containing 32 or less elements it is stored in a sorted array&mdash;which explains why we observe an order iterating over the elements using the <code>Enum</code> and <code>Stream</code> functions&mdash;and when the <code>Map</code> contain more than 32 elements the Erlang VM will switch to a data structure called &laquo;hash array mapped trie,&raquo; which provides fast lookups for bigger data sets as well as very fast insertions. When we convert the big map to a list we will get whatever order the internal representation has; which is not intuitive.

For our initial example we convert a <code>MapSet</code> to a list. The <code>MapSet</code> data structure is useful for when we want to store terms in a set. It allows us to ask if a term is present, and perform comparisons between two sets, such as getting the difference or intersection, in a fast and efficient manner. Behind the scenes a <code>MapSet</code> uses a <code>Map</code> to store its values, so it inherit the performance from the <code>Map</code>, but also the unordered nature when the set grows above 32 elements.

To conclude:

<pre>
<code class="language-elixir"># A Map is an unordered data structure, and we should never rely on the order of a Map</code></pre>

While we seemingly observe an order when we convert maps to lists, or iterate over them, we should never rely on the order of a <code>Map</code>. Assuming a reliable order could break our application in a future Erlang/OTP release, as the OTP team could choose to change the internal representation of maps. That said, it is fine to use the functions in the <code>Enum</code> module to iterate over a map, as long as no assumptions on the order of the elements are made.

<ul>
	<li><a href="https://en.wikipedia.org/wiki/Hash_array_mapped_trie" rel="nofollow">https://en.wikipedia.org/wiki/Hash_array_mapped_trie</a></li>
	<li><a href="https://elixir-lang.org/getting-started/protocols.html" rel="nofollow">https://elixir-lang.org/getting-started/protocols.html</a></li>
	<li><a href="https://hexdocs.pm/elixir/Enumerable.html#content" rel="nofollow">https://hexdocs.pm/elixir/Enumerable.html#content</a></li>
	<li><a href="https://hexdocs.pm/elixir/Map.html#content" rel="nofollow">https://hexdocs.pm/elixir/Map.html#content</a></li>
	<li><a href="https://hexdocs.pm/elixir/MapSet.html#content" rel="nofollow">https://hexdocs.pm/elixir/MapSet.html#content</a></li>
</ul>

Did you find this interesting? Want more? Join like-minded individuals at Code Elixir LDN 2019, July 18 in London, UK. Also, for more reading, check out our previous blog post in this series.

<a href="https://codesync.global/conferences/code-elixir-ldn-2019/"><img alt="" src="/uploads/media/default/0001/01/072c1e3e023c11e05b91de4d16a609e5ccaddf9d.jpeg" style="height:200px; width:1000px" /></a>
