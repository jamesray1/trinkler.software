---
layout: post
title: "Introduction"
author: "R. Trinkler"
toc: false
---

## Introduction

This is a short introduction blog post that touches briefly on the history of blockchain and provides an overview of what readers can expect in the blog posts to follow.

## From Bitcoin

Bitcoin has given us a first glimpse of what a distributed (p2p) and open-source financial system could look like. It was designed to be a _p2p electronic cash system_ - a way to exchange and store value that _does not require trust in any third-party_. This [not requiring trust in any third-party was important to Bitcoins Founder Satoshi Nakamoto](http://satoshi.nakamotoinstitute.org/posts/p2pfoundation/1/) and should be seen as an indispensable component for any kind of blockchain.

_Bitcoin_ was a means of recording ownership of its native token. Conceptually similar projects followed such as _Litecoin_ or _Dogecoin._

_Namecoin_ was one of the earlier projects to realise that blockchain technology can be used for more than just the recording of token ownership. It can also be used to record arbitrary information such as the ownership of domain names. However many of these new projects failed to achieve critical mass.

Their biggest problem was their _isolation_, meaning that they were unable to talk to each other or benefit from each other in a way that upheld the trust-free nature blockchains were meant to have.

What remained was the clear understanding that blockchains were meant to play a bigger role than simply the store of ownership (although store of ownership remains a major usecase for blockchains).

## To Ethereum

To achieve this, the ways _transactions_, or user generated blockchain-external information, could modify the _state_, or blockchain data layer, were generalised such that they could execute a distributed Turing complete virtual machine. This means transactions were now not just a means of transacting ownership but in fact they were more of a usage right to a _shared general purpose computer_.

Now things like multiple tokens on one chain or a domain name registry could be easily created on the same blockchain in a way where they were connected with each other. The entire implementation of Namecoin could be reduced to a couple of lines of code! Furthermore, to register a name you could use an already existing token on Ethereum. Due to the fact that they resided on the same blockchain, they were able to talk to each other and benefit from each other. Achieving critical mass was now much simpler as network participants were able to access many different blockchain applications using the same underlying software.

The first blockchain to achieve Turing completeness was Ethereum.

The same way we have seen the idea of Bitcoin be replicated, modified and often improved upon multiple times, we see the same fate for Ethereum.

We see conceptually similar projects such as _Neo_, _Cardano_ (improves formal correctness) or _Rchain_ (improves performance) and projects similar to Bitcoin with _Bitcoin Cash_ among others. We also see projects where the difference is not of a technical nature but of a philosophical nature - such as _Ethereum_ and _Ethereum Classic._

While these types of blockchains solved the problem of isolation locally, meaning they were able to talk and benefit from each other as long as the application resided on the same blockchain, they were not able to solve the problem of isolation globally.

_As blockchains they were unable to talk and benefit from each other._

However as demand for these kinds of blockchains grew we saw another problem arise: _scalablity_. In the first generation of blockchains a user was able to _opt out_ of downloading a record of ownership of one blockchain. For example a Bitcoin user was not forced to download and validate the Dogecoin blockchain. This was not the case with Ethereum-like blockchains. For example on Ethereum a Cryptokitties fan has to forcibly download and validate everything that happens in the asset management world with Melon and vice versa. We're talking about a shared general purpose computer after all - any (write-) usage of this computer, no matter for what purpose, has to be carried out by the entire network as well.

Cries for scalability grew louder.

## To what's next - Web3

There have been multiple attempts to solve or mitigate the scalability problems. These include:

- Various methods of pruning of proof helpers such as historical transactions and blocks,
- Splitting up of the blockchain state such as sharding,
- Parallelisation of transaction execution,
- More performant crypto, networking, virtual machines and consensus algorithms,
- Off-chain computing such as lightning networks, state-channels.

While these are innovative and promising ideas - all of them are different _types of optimization_. They're not fit to solve the problems of _isolation_ and _scalablity_.

### Interconnectivity

Ethereum solved the problem of local interconnectivity for applications.
Networks like _Polkadot_ will solve the problem of global interconnectivity. That is to say, it allows entire blockchains to exchange information in a way that does not require trust in any third party.

_Don't let people tell you otherwise, what we need to solve scalability is:_

- **Domainchains**: Splitting up of blockchains into types of domain specific shards which are easy to use for non-technical people and have their own individual execution environment.
- **Interconnectivity**: Connect those shards into a framework that allows them to exchange information. I.e. remove isolation.

_To fullfil the potential blockchain technology has to offer we additionally need:_
- **Distributed hosting**: To access the blockchain client application and its frontend web application in a way that is trust free.

Those three things will allow us for the first time in human history to build _Autonomous Applications_.

### Usability

To solve scalability we need to solve usability first. In the context of blockchain design; Usability can be seen as a superset of scalability. What we see happening next is the _specialisation_ of blockchains. This essentially means _domain_ specific blockchains that fullfil only one purpose, where users can easily _opt-out_ of certain domainchains in which they are not interested anymore and change to others - all in the context of interconnectivity. We will see that Blockchains which are specialised enough have a state, or data layer, that is small enough that a user can download it in less than a minute in the context of simply opening up a webpage in browsers. We will take a closer look at this concept in a future post about _usability._

## Blog posts to follow

A collection of blog posts around web3 native topics such as
- `{block-, domain-, web-}` chains
- interconnectivity
- distributed hosting

and their implications on society.

Topics to follow include
1. DIY-Blockchain part 1: Overview
2. DIY-Blockchain part 2: State
3. DIY-Blockchain part 3: Validity
4. DIY-Blockchain part 4: Canonicality
5. Rise of the domainchains
6. The importance of usability
7. A history of Keynesian economics (The corruption of the Regulator)
8. A history of Austrian economics (The disastrous implications of having no Regulator)
9. A new law is emerging (Autonomous Applications)
10. _Tools for a catallactic Society_

## Acknowledgment

Many of the terms and ideas in featured blog posts originate from the [Ethereum yellow paper](http://gavwood.com/paper.pdf), [Polkadot paper](https://github.com/w3f/polkadot-white-paper/blob/master/PolkaDotPaper.pdf),  [Polkadot specs](https://github.com/w3f/polkadot-spec/blob/master/spec.md), [Nimiq whitepaper](https://medium.com/nimiq-network/nimiq-a-peer-to-peer-payment-protocol-native-to-the-web-ffd324bb084) and their corresponding implementations.

For a more extensive list of resources, see the [Alexandria](https://github.com/Trinkler/alexandria) collection.

Further sources of inspiration include various discussions with individuals as well as with the entities [Melonport AG](https://melonport.com/), [Parity Technologies](https://www.parity.io/), [Web3 Foundation](https://web3.foundation/) and many more.

All writing produced that is inspired by these ideas is reflective of my own opinions, and therefore all errors in the text are also my own. The mentioning of ideas or developments within the blockchain ecosystem or the usage of said ideas as examples within the text is not intended to be suggestive of endorsement. All references made herein seek the purpose of further educating the reader.


## Contribute

Seen a mistake? We ❤ [issues](https://github.com/Trinkler/website/issues/new) and [pull requests](https://github.com/Trinkler/website/fork)!

Engage in the discussion - join us on [Discord](https://discord.gg/Te7sWv3).

Thanks for reading!
