---
title: "FAQs"
description: "Got an FVM question that isn't answered by the rest of the docs? Check out this currated list of commonly asked questions from the community."
lead: "Got an FVM question that isn't answered by the rest of the docs? Check out this currated list of commonly asked questions from the community. Still have a question after reading through this list? Create a post on fvm-forum.filecoin.io"
weight: 50
menu:
    fvm:
        parent: "fvm-basics"
---

{{< beta-warning >}}

## Concepts

Here are some conceptual FAQs.

### What is FVM

The FVM (Filecoin virtual machine) enables developers to write and deploy custom code to run on top of the Filecoin blockchain. This means developers can create apps, markets, and organizations built around data stored on Filecoin.

### What broader implications does FVM have

FVM allows us to think about data stored on Filecoin differently. Apps can now build a new layer on the Filecoin network to enable trading, lending, data derivatives, and decentralized organizations built around datasets.

### What problems does FVM solve

FVM can create incentives to solve problems that Filecoin participants face today around data replication, data aggregation, and liquidity for miners. Beyond these, there is a long tail of data storage and retrieval problems that will also be resolved by user programmability on top of Filecoin.

### How does the FVM directly interact with data on Filecoin

The FVM operates on blockchain state data — it does _not_ operate on data stored in the Filecoin network. This is because access to that data depends on network requests, an unsealed copy's availability, and the SPs' availability to supply that data.

Access and manipulation of data stored in the network will happen via L2 solutions, for example, retrieval networks or compute-over-data networks, e.g., Saturn or CoD.

### How do other EVMs compare to FEVM

Unlike other EVM chains, FEVM specifically allows you to write contracts that orchestrate programmable storage. This means contracts that can coordinate storage providers, data health, perpetual storage mechanisms, and more. Other EVM chains do not have direct access to Filecoin blockchain state data.

### What is an actor

An actor is code that the Filecoin virtual machine can run. Actors are also referred to as smart contracts.

### What are built-in actors

[Built-in actors](https://github.com/filecoin-project/builtin-actors) are code that come precompilied into the Filecoin clients and can be run using the FVM. They are similar to [Ethereum precompiles](https://www.evm.codes/precompiled?fork=merge).

### Why use the FEVM vs any other EVM compatible chain

Having storage contracts as a native primitive open to smart contract developers. Reduce costs of writing to storage from an EVM smart contract to a separate storage service.

### Why FEVM vs native FVM

FEVM allows Solidity devs to easily write/port actors to the FVM using the tools that have already been introduced in the Ethereum ecosystem.

### What applications make FVM/FEVM unique

Applications that natively make use of storage contracts. Perpetual storage contracts, Data Daos, etc.

### What is perpetual storage

Perpetual storage is a unique actor design paradigm only available on the FVM that allows users the ability to renew Filecoin storage deals and to keep them active indefinitely. This could be achieved by using a Decentralized Autonomous Organization (DAO) structure for example.

### What are DataDAOs

DataDAOs are a unique design paradigm FVM developers could create which use Filecoin storage to store all their data instead of a service like AWS (which is currently used).

### Is FVM part of Filecoin clients like Lotus

Yes.

### Do I have to install Lotus to work with FVM

Not necessarily. You can use one of the two public Wallaby nodes:

- [wallaby.node.glif.io](https://wallaby.node.glif.io/)
- [api.zondax.ch/fil/node/wallaby/rpc/v0](https://api.zondax.ch/fil/node/wallaby/rpc/v0)

### How do I install a node on the Wallaby testnet?

Factor8, the team that runs the Wallaby testnet, has a [guide on how to spin up a Lotus node on the Wallaby testnet](https://kb.factor8.io/en/docs/fil/wallabynet).

### What is the difference between the FVM and Bacalhau

They are synergistic. Compute over data solutions such as [Bacalhau](https://github.com/filecoin-project/bacalhau) can use the FVM.

### Why does the FVM use WASM

Many [different languages](https://github.com/appcypher/awesome-wasm-langs) already compile to WASM so developers can pick their favorite.

### Is the FEVM a bridge to the EVM

No, the FEVM is its own instance of the EVM built on top of Filecoin. You will need to redeploy smart contracts that exist in the EVM to the FEVM. Bridges can be built to top of the FEVM which connect it to other blockchains however.

### How is the Filecoin network accessed through Solidity

When an EVM is deployed to FEVM, it is compiled with WASM and an actor instance is created in FEVM that runs the EVM bytecode. The user-defined FEVM actor is then able to interact with the Filecoin network via built-in actors like the Market and Miner APIs.

### Can I deploy EVM bytecode to the native FVM

No, it must be deployed to the FEVM.

## Developers

Here are some FAQs raised by developers.

### How does Aptos compare to FVM

[Aptos](https://aptoslabs.com/) is a Move-based L1 chain, whereas FVM is a WASM runtime on the Filecoin chain. The latter comes with an EVM right of the box; the former does not. The FVM also supports programmable storage with deals on Filecoin.

### What frontend framework should I use?

React, Ether.js, web.js, ReactJs work well.

### How do we convert from msg.sender in a FEVM contract, which returns an EVM `0x` address, to the underlying Filecoin `f` address?

You can use the npm [@glif/filecoin-address](https://www.npmjs.com/package/@glif/filecoin-address) package or the [Zondax mock api](https://github.com/Zondax/fevm-solidity-mock-api) has the constructor that calls `mock_generate_deals();`.

### How do I bound the replicator factor from solidity fevm?

Store a number limit on running `DealClient` and `publish_deal` and have it authorized to replicate.

## Storage

These are some questions frequently raised in regards to storage.

### How can I use FVM to store data to Filecoin

The intent of FEVM/FVM is to compute over state data (the metadata of your stored data). Storage Providers are the ones that are able to store your data and upload the deal to the Filecoin network. Data retrieval happens via Retrieval Providers, accepting the client’s ask to retrieve and working with Storage Providers to decrypt the data to deliver to the client. FEVM/FVM is able to build logic around these 2 processes and automate, add verification and proofs, time-lock retrievals etc.

### How do I close a storage deal on Filecoin and stop Storage Providers (SP) from storing my data on-chain

It’s not impossible but SPs are incentivized not to close the storage deal as they are slashed for not providing [Proof of Spacetime (PoST)](https://spec.filecoin.io/algorithms/pos/). Someone has to pay for the broken promise a miner makes to the chain and you need a custom market actor for it most likely to make the deal. You need to make deals for a certain amount of time - right now the boundaries are 6-18 months. You cannot ask a storage provider to take down your data without contacting them off-chain.

### How do I check SP’s balance with their FEVM address

You can query balance of any address using [Zondax's API](https://docs.zondax.ch/openapi#tag--Account).
