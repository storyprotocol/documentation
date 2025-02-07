---
title: Overview
excerpt: Overview of Story's modular architecture
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
![]()

Story is a purpose-built modular blockchain that is fully EVM compatible and uses Cosmos SDK and CometBFT to achieve fast block time and one-shot finality. A Story node consists of two clients: a `story-geth` client as the execution client (EL) and a `story` client as the consensus client (CL). The two clients communicate with each other via the [Engine API interface](doc:engine-api)  defined by [Ethereum](https://hackmd.io/@danielrachi/engine_api).

`story-geth` client is a fork of the Geth client, with the addition of the [IPGraph Precompile](doc:precompile)  and [RIP-7212](https://github.com/ethereum/RIPs/blob/master/RIPS/rip-7212.md) precompile. It is responsible for transaction execution, broadcasting and state storage. It's fully compatible with the Ethereum Virtual Machine (EVM) and supports all Ethereum JSON-RPC methods.

`story` is built on top of the Cosmos SDK and CometBFT. Cosmos SDK is a framework for building blockchain applications. It provides a modular architecture that allows for easy integration of new modules and features, and for the network to be easily extended and customized. The `story` client upgrades and added new Cosmos SDK modules to support EngineAPI integration and novel staking mechanisms. Meanwhile, CometBFT is a high-performance, scalable, and secure blockchain consensus engine that is battle-tested in the Cosmos ecosystem. CometBFT and Cosmos SDK communicate through ABCI++ interface(link to ABCI++ spec).

(insert architecture diagram)