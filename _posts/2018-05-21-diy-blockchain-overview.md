---
layout: post
title: "DIY Blockchain"
author: "R. Trinkler"
toc: false
---

## Overview

This is a four part series on _do-it-yourself_ blockchains. The idea is that by understanding the design decisions of blockchains well enough it becomes simpler to design your very own specialised blockchain or _domainchain_.

As we have seen in the introduction blog post, as a technology space we are moving into an era wherein we can remove isolation not just within a blockchain itself but amongst entire blockchains. This can be achieved by blockchains connecting to a _multichain_ framework. The removal of isolation will lead to increased competition amongst blockchains. Thus in order to stay relevant blockchains will have to specialise, otherwise - due to the interconnectivity - it will be too easy for network participants to switch to a similar but improved (more specialised) version of this blockchain. I.e. the less specialised blockchain will lose network participants.

_In other words the network effect we've seen for singular blockchains will start to become less relevant and the network effects of these singular blockchains will instead take a passage upwards to add to the network effect of the multichain framework they are connected to._

These multichain frameworks solve the last two major problems in the blockchain space: _isolation_ and _usability_. Thus they may very well be ([as I mentioned in 2016](https://youtu.be/rfRufqN8S9c?t=14m20s)) the _last big inventions of this space_.

So lets begin:

## Introduction

A _blockchain_ is a [transactional singleton machine with a shared-state](https://ethereum.github.io/yellowpaper/paper.pdf). That is to say it is a virtual computer with a shared data storage as well as an emulated and shared CPU called a virtual machine. The shared data storage as well as the shared virtual machine are the same for each participant running this virtual computer - they are canonical.

A _domainchain_, sometimes also referred to as a _parachain_ or _zone_, is a domain-specific blockchain. An example for a domainchain is [Melonchain](https://trinkler.sh/projects/).

A _webchain_ is a blockchain that is executable in the browser. An example of a webchain is [Nimiq](https://nimiq.com).

Not every blockchain is a webchain or domainchain. But all webchains and domainchains are blockchains.

Generally speaking we can split our blockchain into three parts: __State, Validity and Canonicality.__ In the following three sections we will talk about these parts, their comparisons to a traditional computer, their main goals, and the topics that they encompass. We will follow up on this in the following three blog posts.

## State

_**Traditional (Data)**: A technology consisting of computer components and recording media that are used to retain digital data. **Modern (State)**: Shared data, usually stored in one or multiple mappings._

Shared data is data that is distributed amongst a network of participants, where distributed data can always be _validated_ locally without the need for trust. Furthermore, the distribution happens in a byzantine tolerant way. To elaborate, there is a mechanism in place with which network participants can come to consensus over which data to distribute and validate (canonicality).

The storage or **State** has two main goals
- Provide an efficient way to query information (retrieval)
- Lower disk storage footprint (storage)

Note `read` and `write` operations to the state are inherently different in blockchain networks. `Read` can be done very efficiently as the database to read from (the state) is already on the local machine of the user. `Write` operations on the other hand are very inefficient as each modification to the database needs to be propagated through the network and then canonicalised. In this part we will focus on `read/retrival` operations and on how to keep storage footprints low. In the canonicality part we will look into different networking protocols and canonicality algorithms which can make `write` operations more efficient.

Topics include:
- Transaction linking vs State
- Definitions
    - State
    - Object
- Retrieval
    - Patrica Merkle Tree
    - Orthogonal Range Tree
- Storage
    - Partitioning (Sharding)
    - Off-chain computing/storage (Plasma)
    - Maintenance Fees

## Validity

_**Traditional (CPU)**: The central processing unit (CPU) of a computer is what manipulates data by performing computations. **Modern (VM)**: In our case the CPU is virtual (software rather than hardware) and emulated using the hardware CPU. This is called a Virtual Machine (VM)._

The VM is the execution environment that can modify the state. Modifications happen according to a ruleset defined in a **state transition function**. By applying the state transition function from the very first block, the _Genesis Block_, to the latest Block, the _Head_, we can _validate_ the integrity and accuracy of our local copy of the blockchain state.

The **Validity** has as its main goals:
- Allowing for efficient/light state transition proofs (performance).
- Minimizing the amount of proof helpers (maximizing state pruning)

Topics include:
- Definitions
    - Transaction
    - Block
    - State Transition Function
- Runtime (VM)
    - Turing completeness
    - Compilers
- Validity proofs
    - Full node
    - Light client
    - Canonical Hash Tree
    - Nano client
    - NiPoPoW

## Canonicality

_**Modern (One truth):** A new concept, native to blockchain technology. Establishes one truth among many possiblities._

Once we have our state and a proof of validity, we need to come to consensus as to which one of the possibly multiple proofs of validity we should use as a network.

The **Canonicality** has as its main goals:
- Coming to consensus on which validity proof to use (avoiding network splits and solving transaction ordering problems)
- Minimizing networking requirements to come to consensus (performance)

Topics include:
- Byzantine generals problem
- Transaction ordering problem
- Definition
    - Consensus
- Consensus Algorithm
    - Proof of Work
    - Proof of Stake
    - Proof of Authority
    - Proof of Spacetime
    - Proof of Reputation
    - Proof of Computing Work
- Exchange of information (Networking)
    - Bootstrapping
    - Ethereum Wire Protocol
    - Libp2p
    - WebRTC

## Conclusion

This provides a broad overview of what it entails to build your own blockchain. In three follow up posts we will dig into the details and specifics of what those three things: State, Validity and Canonicality really mean and how they can be tweaked and adjusted to build not just a generic blockchain but your very own domainchain and/or webchain.

## Acknowledgment

Sources of inspiration include various discussions with individuals as well as with the entities [Melonport AG](https://melonport.com/), [Parity Technologies](https://www.parity.io/), [Web3 Foundation](https://web3.foundation/) and many more.

For an extensive list of resources, see the [Alexandria](https://github.com/Trinkler/alexandria) collection.

All writing produced that is inspired by these ideas is reflective of my own opinions, and therefore all errors in the text are also my own. The mentioning of ideas or developments within the blockchain ecosystem or the usage of said ideas as examples within the text is not intended to be suggestive of endorsement. All references made herein seek the purpose of further educating the reader.

## Contribute

Seen a mistake? We ❤ [issues](https://github.com/Trinkler/website/issues/new) and [pull requests](https://github.com/Trinkler/website/fork)!

Don't be left out of the discussion - join us on [Discord](https://discord.gg/Te7sWv3).

Thanks for reading!
