
---
title: From a web application to a distributed system
abstract: There is currently a lot of interest in how these problems are solved in the BEAM environment (using Actor model) and how some common patterns like Supervisor or GenServer are used in other languages or frameworks, Akka for example.
image: /images/sto-e.jpg
speaker_id: gianluca-padovani
type: article
keywords: 
date: 2018-05-10
tags: Erlang,Elixir
---
This article is an introduction to Gianluca Padovani&rsquo;s talk at Code BEAM STO, on how to develop a web application that can scale to a distributed system.

### Why this talk at Code BEAM STO?

Gianluca compares conference presentations to software and bananas, they rot if we do not take care of them. His Stockholm talk will update his Milan talk, as well as the code-base that both of them refer to. Gianluca plans to eventually create a workshop about this topic once complete, that can help new developers familiarise themselves with these concepts.

### In the Beginning...

In the beginning, web development was very &ldquo;simple,&rdquo; there was a web server, a bunch of scripts and a database. Every single http request was translated into SQL queries that loaded or wrote data in a DB, the result was often the creation of a HTML page and nothing more. If we were in a &ldquo;complex environment&rdquo; we had some background tasks powered often by something similar to cron. Our DB was the truth and everything was in a single place, everything had a beginning (the arrival of the http request) and an end (the delivery of the http response).

This procedure developed a development attitude where we created all the related objects (we were probably adopting an OOP language) at the beginning of our work and we destroyed everything at the end. We were &ldquo; not allowed&rdquo; to keep data and object in memory because everything started and finished with the life of the http request.

However, over time, the web became a platform to develop &ldquo;applications&rdquo; where the state cannot be kept in a single database and in a single process (you may hear a little voice whispering&nbsp; &ldquo;microservices&rdquo; &hellip;). Operations now span the length of a single http request and we now need the ability to communicate with a lot of different systems and keep track of their states.

There is currently a lot of interest in how these problems are solved in the BEAM environment (using Actor model) and how some common patterns like Supervisor or GenServer are used in other languages or frameworks, Akka for example. Gianluca has been developing software since the last millennium and has seen a lot of patterns and development models rise and fall. There&rsquo;s a saying in Italy &ldquo;paese che vai usanza che trovi&rdquo; (when in Rome do as the Romans do) and this holds true with programming too, there are patterns that the newbies need to learn. For those who are coming to the BEAM ecosystem through Elixir they are indeed lucky, because they will find they are able to take advantages of Erlang and the BEAM virtual machine&rsquo;s patterns, which are already &lsquo;battle hardened&rsquo; from over 20 years of development history.

### About Gianluca Padovani&rsquo;s Code BEAM STO talk

How can we create a web application that takes advantage of the characteristics of the BEAM using the resources of a single node and eventually scale this to a multi-node application?

Gianluca Padovani will be giving the talk &lsquo;From a web application to a distributed system&rsquo; at Code BEAM STO. In this talk, he will be creating a simple e-commerce system that will expose some classic distributed system problems that will be resolved using patterns from the BEAM.

If you want to have a heads-up before the talk, you can dig into the code that the talk will be&nbsp; based on, here is the **<a href="https://github.com/gpad/les">repo</a>** and here is <a href="https://www.slideshare.net/gpadovani/from-a-web-application-to-a-distributed-system">**the link**</a> of the talk that Gianluca did in Milan. You can also follow him on **<a href="https://medium.com/@GPad?source=post_header_lockup">Medium</a>**.
