
---
title: Building Blockchain in Elixir
abstract: "Building Blockchain in Elixir
At æternity we chose Elixir to build an alternative implementation of our blockchain protocol. We’ve had a lot of good experiences with this choice, and few bad ones.
"
image: /images/Code Elixir blockchain article image.png
speaker_id: philipp-piwowarsky
type: article
keywords: 
date: 2018-08-03
tags: Elixir,Code Elixir LDN,Blockchain
---
At &aelig;ternity we chose Elixir to build an alternative implementation of our blockchain protocol. We&rsquo;ve had a lot of good experiences with this choice, and few bad ones. During the development process we made a lot of internal style and design decisions, in order to both ensure productivity while working collaboratively on the project and also maintain a high quality product. These decisions enable us to to continuously present a great open-source codebase for everybody.

&nbsp;

## Initial Difficulties

Starting the project from scratch with a new and fairly junior team there were some difficulties. Different developers were used to different code-style guidelines and naming conventions; however as there are few community standards in Elixir, there was no single approach the team could copy. As such, we decided to follow a semantic naming scheme from the beginning to make code as understandable as possible. With the introduction of the Elixir formatter in version 1.6 we chose to use it in the default configuration. Initially, the dynamic typing of the BEAM introduced mistakes in which incorrect structures were passed or data was not pattern matched correctly. We chose to work against this by using precise type definitions and aliases wherever necessary.

&nbsp;

## Great in Elixir and Design

Overall Elixir struck us as a well-suited language to develop as complex an application as our blockchain implementation. Strong concurrency by design is a great basis to work on top of, to build peer to peer applications. Utilising this we can synchronise our network using the encrypted &ldquo;noise protocol&rdquo; and by gossiping or requesting data among nodes worldwide with minimised latency and maximum throughput. The BEAM-provided fault tolerance assists stability; as the network is public for every node to join, it allows our nodes to stay reachable even if attacked. For example, if the component that manages connections to new peers dies unexpectedly due to an overflow in messages from external nodes, it is able to be restarted without affecting the overall state of the node. In various components, such as the connected peers and their status for easy handling or latest blocks, which are frequently accessed, for performance improvements GenServers are used to keep local state for quick access. The fact that Elixir is a modern functional language, with straightforward syntax, made it quite accessible for new developers.<br />
&nbsp;

Other helpful features were the powerful native binary operations, which are especially needed in a blockchain implementation, as it deals with lots of binary data to have structures kept as small as possible. An umbrella project structure helps to introduce lower coupling between modules and a good separation of utilities, to name just one thing. Last but not least Elixir provides a great dependency system, with excellent support for various ways to integrate pre-existing features via Nifs, using existing Erlang code or calling binaries via erlexec.<br />
<br />
For one of the most critical parts of our blockchain implementation - the consensus rule engine - our team engineered a powerful validation interface, covering any kind of transaction that can occur in the network. The `Transaction` module acts as interface for every specialised transaction implementation; it specifies four main callback functions `validate`, `preprocess_check`, `process_chainstate` and `deduct_fee` with type specification that can match various transaction and chainstate datatypes.

Every specific transaction structure uses this as behaviour and implements these functions in the correct way, as needed. In case of our `SpendTx`, the `validate` function checks the validity of structural definitions, for example that the amount sent needs to be greater than 0 and the receiver&rsquo;s address is only allowed to have an exact number of characters. `preprocess_check` will get past the current state of the blockchain and can make additional validations, such as checking if the sending account has sufficient balance to pay for the transaction amount and fee.

If either of these validation steps fails, the transaction is rejected and not processed any further. `process_chainstate` applies changes this transaction introduces on the current state of the blockchain; in the case of the spending transaction it subtracts the amount from the sender&#39;s account and adds it to the receiver&rsquo;s account.

Finally, `deduct_fee` will subtract the transaction fee from the sender&#39;s account. Each specific transaction is enclosed in a wrapping structure that defines its type and calls the implementations function to process the transaction.<br />
<br />
This example shows the semantic correct naming and separation of functions, to enable effortless understanding of the functions, if reading code, without looking up the documentation or parameters passed. Using design patterns and structures like these enables us to have flat architecture and avoid deep nesting of code.

The advantages described above and the patterns we use are just a few of the great functionalities that Elixir enables and which help us to build better code, resulting in a scalable and maintainable blockchain platform. To find out more about &aelig;ternity visit the work-in-progress open source repository of the elixir implementation <a href="https://github.com/aeternity/elixir-node">https://github.com/aeternity/elixir-node</a> or our protocol definition <a href="https://github.com/aeternity/protocol">https://github.com/aeternity/protocol</a>.

&nbsp;
