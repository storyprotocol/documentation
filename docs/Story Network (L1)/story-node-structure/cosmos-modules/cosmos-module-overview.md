---
title: List of Modules
excerpt: ''
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
# List of Modules

Here is a list of all production-grade modules that can be used on the Story blockchain, along with their respective documentation:

* [evmengine](./evmengine-module) - Handles Cosmos-side logics on each EVM state transition via the [Engine API](engine-api).
* [evmstaking](./evmstaking-module) - Handles staking and network emission logics with queues.
* [mint](./mint-module)

## Cosmos SDK (modified)

Story network uses the following Cosmos SDK modules with some modifications:

* [staking](./staking-module)
* [distribution](https://docs.cosmos.network/main/build/modules/distribution)

## Cosmos SDK (unmodified)

Story network uses the following Cosmos SDK modules without non-trivial modifications:

* [auth](https://docs.cosmos.network/main/build/modules/auth)
* [bank](https://docs.cosmos.network/main/build/modules/bank)
* [consensusparams](https://docs.cosmos.network/main/build/modules/consensus)
* [gov](https://docs.cosmos.network/main/build/modules/gov)
* [slashing](https://docs.cosmos.network/main/build/modules/slashing)
* [upgrade](https://docs.cosmos.network/main/build/modules/upgrade)