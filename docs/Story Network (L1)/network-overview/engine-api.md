---
title: Engine API
deprecated: false
hidden: false
metadata:
  robots: index
---
# Engine API

The Engine API is a set of JSON-RPC methods facilitating communication between an EVM node’s execution and consensus layers. Story’s execution layer, fully EVM-compatible, supports all standard JSON-RPC methods defined by the [Ethereum Engine API](https://github.com/ethereum/execution-apis/blob/main/src/engine/common.md), while its consensus layer, built on Cosmos modules, uses the Engine API to coordinate with the execution layer.