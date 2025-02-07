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

Story is a purpose-built modular blockchain fully EVM compatible using Cosmos SDK and CometBFT to achieve fast block time and one-shot finality. A Story node consists of two clients: `story-geth` as the execution client (EL) and a `story` as the consensus client (CL). These clients communicate via the [Engine API interface](doc:engine-api)  defined by [Ethereum](https://hackmd.io/@danielrachi/engine_api).

`story-geth` is a fork of the Geth client, with the addition of the [IPGraph Precompile](doc:precompile)  and [RIP-7212](https://github.com/ethereum/RIPs/blob/master/RIPS/rip-7212.md) precompile. It handles transaction execution, broadcasting and state storage while maintaining full compatibility with the Ethereum Virtual Machine (EVM) and supporting all Ethereum JSON-RPC methods.

`story` is built on the Cosmos SDK and CometBFT. The Cosmos SDK provides a modular framework for building blockchain applications, enabling seamless integration of new modules and features while allowing the network to be easily extended and customized. `story` client introduces upgrades and additional Cosmos SDK modules to support Engine API integration and novel staking mechanisms. CometBFT, a high-performance, scalable, and secure consensus engine, has been extensively tested within the Cosmos ecosystem. CometBFT and Cosmos SDK communicate through ABCI++ interface(link to ABCI++ spec).

(insert architecture diagram)