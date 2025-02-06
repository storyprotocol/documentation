---
title: Engine API
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---

# Engine API

The Engine API is a collection of JSON-RPC methods that enables communication between execution layer and consensus layer of an EVM node.
Story's execution layer,which offers full EVM compatibility, supports all standard JSON-RPC methods defined by [Ethereum Engine API](https://github.com/ethereum/execution-apis/blob/main/src/engine/common.md).
Meanwhile, Story's consensus layer, built on Cosmos modules, utilizes the Engine API to coordinate with the execution layer.