---
layout: post
title: "DIY Blockchain - State"
author: "R. Trinkler"
toc: false
---

## Introduction

Shared data is data distributed amongst a network of participants; validation of distributed data can always occur locally without requiring trust. Furthermore, the distribution happens in a byzantine tolerant way. To elaborate, there is a mechanism in place with which network participants can come to consensus over which data to distribute and validate (canonicality).

The storage or **State** has two main goals

- Lower disk storage footprint (storage)
- Provide an efficient way to retrieve information (retrieval)

Note: `read` and `write` operations to the state are inherently different in blockchain networks. `Read` is possible to do efficiently as the database to read from (the state) is already on the local machine of the user. `Write` operations on the other hand are inefficient as each modification to the database requires propagation through the network and then canonicalisation. In this part we will focus on`read/retrieval` operations and on how to keep storage footprints low. In the validity part we will look into how we know that the state is valid. What this means is that we will look at how to ensure that the state only changes according to a predefined ruleset. Concluding with the canonicality part we will look into different networking protocols and canonicality algorithms which can make `write` operations more efficient.

## A bit of history

Blockchain state is a distributed key-value store or database. The database implementation has changed over time.

We went from Bitcoin's unspent transaction outputs to Ethereum's state.

An unspent transaction output (UTXO) is an output of a blockchain transaction that one has yet to spend, i.e. it is possible to use it as an input in a new transaction. Bitcoin is the most prominent example of a cryptocurrency that uses the UTXO model. The sum of unspent transaction outputs of a user is their cryptocurrency balance.

Hence within the Bitcoin blockchain a user goes through the historical transactions to add up all their unspent transaction outputs to know the balance.

This technique is undesirable since it makes it difficult to prune historical transactions which will lead to a bloating of the state and thus the requirements of how much data we need to retrieve from the network to validate the correctness of our blockchain.

An alternative way to maintain the state is by adding up balances on each transaction that modify a current _state_ of the blockchain. A prominent example for such a blockchain is Ethereum.

In this part we will only look into state as opposed to UTXO model.

### Definition (Proof-chain)

The state is stored using a Merkle tree and dubbed an `object tree`, where the root of the object tree is called `state root`. The state root can be seen as a tiny (in kilobytes, not importance) proof that the entries in the object tree are valid. The state root is secured using a `proof-chain`.

A **proof-chain** is a list of block headers from the Genesis block to the latest block, which is linked together using the hashes of previous blocks.

The proof-chain secures the state root and the state root secures the object tree. See also [the mini-blockchain scheme](http://cryptonite.info/files/mbc-scheme-rev3.pdf) for more information.

### Definition (State)

The leaves of the object tree form the state.

The **State** is a mapping of ObjectID to Object.

```
state := ObjectID -> Object
```

Where `ObjectID` is a unique Identifier of each Object. Usually the ObjectID is the user public key.

Each state can include a list of special **built-in** objects. Within Ethereum this is called a _precompiled contract_. It describes a special/non-standard entry into the mapping for example a `ECDSARECOV` function in Ethereum or a `Parachain` object in Polkdadot.

### Definition (Object)

An object is a collection of data allocated to each `ObjectID`.

### Examples (Object)

#### Relay chain (Polkadot)

A very elegant version of an **Object** is the one of [Polkadot's Relay chain](https://github.com/w3f/polkadot-spec/blob/master/spec.md#state), which is simply:

```
Object := {
    code_hash: Hash,
    storage_root: Hash
}
```

> The state of the relay chain has similarities to Ethereum: "objects" contained in it are a mapping from an ObjectID identifier to code (a SHA3 of Wasm code) and storage (a Merkle-trie root for a set of H256 to bytes mappings). Objects are bland Wasm code bundles with a couple of external facilities open to them as user-functions, primarily the ability to call into other objects and to access its own storage. - [Polkadot specs](https://github.com/w3f/polkadot-spec/blob/master/spec.md#state)

The Relaychain focuses on `Objects` rather than on cryptocurrency balances. Nevertheless cryptocurrency balances can be stored and accounted for in a dedicated Object.

> Unlike Ethereum and Bitcoin, balances are not a first-class part of the state-transition function (STF). Indeed the only aspect of the relay-chain which is first-class is the notion of an object. Each object is identified through an index (ObjectID) and has some code and storage (similar to Ethereum contract accounts). The code exports functions which can be called either from other objects or through a transaction. - [Polkadot specs](https://github.com/w3f/polkadot-spec/blob/master/spec.md#relay-chain)

#### Nimiq

Another very elegant approach is the one of [Nimiq](https://nimiq.com). It is somewhat antithetical to the one of the Relaychain. Nimiq focuses almost entirely on cryptocurrency payments, there is no notion of Turing-complete smart-contracts etc. This allows Nimiq to have an unprecedentedly low storage footprint, while still allowing for full decentralisation.

An **Object** in Nimiq looks as follows:

```
Object := {
    type: Type,
    balance: Number
}
```

where `Type` is one of the following:

- `Nimiq.Account.Type.BASIC`: A basic user account
- `Nimiq.Account.Type.VESTING`: A hardcoded type of a smart-contact for vesting of funds.
- `Nimiq.Account.Type.HTLC`: A hardcoded type of smart-contract for hashed timelock contracts. It's this contract that we use for [Agora Trade](https://agora.trade), our cross-chain, non-custodial, cryptocurrency exchange.

Noteworthy of Nimiq's approach is that it does not require a transaction counter, or _nonce_, entry in the object or anywhere else to protect against transaction replay attacks. Instead of a nonce, Nimiq decided to use the transaction `validityStartHeight` in combination with a transaction `uniqueness` constraint as a replay protection. This way they can get rid of empty objects in the object tree. In Ethereum, even if you completely empty an object, its nonce value would be stored forever in the objects tree, bloating it over time.

## Storage

The storage section is about how the state storage footprint from a user perspective can be lowered.

### Substates (Sharding)

One approach is to split up the state into substates, or shards.

**A sharded State** is a superset of _substates_ mappings of ObjectID to ObjectTypes. Note: each substate is a partition of the state.

```
state := {substate_1, ..., substate_n}
```

where:

```
substate_1 := ObjectID -> Object_1
substate_2 := ObjectID -> Object_2
...
substate_n := ObjectID -> Object_n
```

Where each shard can have unique built-in objects and objects of each shard can be of a different format.

### Checkpoints/Constant- sized blockchains

Checkpoints and constant- sized blockchains (see for example [Coda Protocol](https://codaprotocol.com/static/coda-whitepaper-05-10-2018-0.pdf) allow for smaller proof-chains, by removing the necessity to validate the state from Genesis or by using recursive composition of zk-SNARKs to compress the proof-chain to a few kilobytes.

### Off-chain computing

Another approach is to move a portion of the state off-chain.

Off-chain computing for example in the form of [Plasma](https://plasma.io/plasma.pdf), [TrueBit](https://people.cs.uchicago.edu/~teutsch/papers/truebit.pdf), or by using [Generalised State Channels](https://l4.ventures/papers/statechannels.pdf) allows for the ability to move computation off-chain and validate periodically on-chain.

## Retrieval

Once we have a `state root` we would like to get access to the `object tree` data.

The data requirements of the `object tree` are by far the largest. In some extreme examples such as highly scalable blockchains like [Zilliqa](https://zilliqa.com/), to completely validate the state will fast require terabytes of data.

Retrieval is all about providing an efficient way to retrieve and store this data.

### Archival nodes

One approach to providing data is by using community nodes or _archival nodes_. That is an archive, every new node can access the entire history of a blockchain, to validate their states.

E.g. by using third party providers, such as:

- https://ipfs.io,
- https://bluzelle.com/,
- https://gnxtech.io/en/,
- https://fluence.ai,
- http://www.pepperdb.org/.

### BitTorrent

Another approach is by hosting the blockchain history from the Genesis Block to a recent block on swarm.

## Remarks on Sustainablity

There are two user interactions which are distributed across a network.

- Modifiying state.
- Hosting.

Modifying state has an economic cost attached to it called a transaction fee. However continuous hosting as of now is not specifically taxed on almost every blockchain. This is not ideal as hosting should also have an economic cost attached to it.

For Blockchains with simple state it's possible to enforce a maintenance fee. Where `simple` is defined as Object X is not linked with Object Y, for all Objects X,Y in the State.

We can see that this is the case for Nimiq's blockchain.

## Acknowledgments

Some of the terms used in this blog, such as _Object_ or _Built-ins_ originate from the [polkadot specs paper](https://github.com/w3f/polkadot-spec/blob/master/spec.md#state).

Many thanks to Sophie, Mark, Hervé, Hector and Bruno for proof reading and providing feedback, as well as the Nimiq team for their insights.

Any faults are my own.

## Contribute

Seen a mistake? We ❤ [issues](https://github.com/Trinkler/trinkler.software/issues/new) and [pull requests](https://github.com/Trinkler/trinkler.software/fork)!

Don't be left out of the discussion - join us on [Discord](https://discord.gg/C9TPNQd).

Thank you for reading.
