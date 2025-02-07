---
title: Engine API
deprecated: false
hidden: false
metadata:
  robots: index
---
## Overview

The Engine API is a collection of JSON-RPC methods that facilitate communication between the execution layer (EL) and the consensus layer (CL) of an EVM node. Story's execution layer, which offers full EVM compatibility, supports all standard JSON-RPC methods defined by the [Ethereum Engine API](https://github.com/ethereum/execution-apis/blob/main/src/engine/common.md). Meanwhile, Story's consensus layer, built on Cosmos modules, utilizes the Engine API to coordinate with the execution layer.

## Functionalities

The Engine API facilitates seamless interaction between the EL and the CL by providing essential coordination mechanisms, including:

1. **Handshake**
2. **Synchronization**
3. **Block Validation**
4. **Block Proposal**

## Execution Layer Implementation

The EL in Story implements the following standard Engine API methods to support these functionalities:

- `engine_exchangeCapabilities`: Exchanges supported methods.
- `engine_getClientVersion`: Exchanges client version data.
- `engine_newPayload`: Inserts the given payload into the local chain.
- `engine_forkchoiceUpdate`: Updates the canonical chain marker and generates the payload with given attributes.
- `engine_getPayload`: Retrieves the pre-generated payload.


## Consensus Layer Interaction

How does Story’s CL interact with these methods? The CL, leveraging the Cosmos SDK, calls the following methods:

### **1. PrepareProposal** (Proposing a New Block)

- The CL checks whether a payload is already being generated using `payloadID`.
- If not, the CL calls `engine_forkchoiceUpdate` to trigger a new payload generation.
- The CL then calls `engine_getPayload` with `payloadID` to fetch the payload and propose a new block.

### **2. FinalizeBlock** (Finalizing a Decided Block)

- The CL calls `engine_newPayload` to  delivers the finalized block to the EL.
- The EL executes transactions deterministically and updates its state.
- Since CometBFT provides instant finality, the CL calls `engine_forkchoiceUpdate` to finalize the block.
- Finally, the CL calls `engine_forkchoiceUpdate` again to start generating the payload for the next block.

This interaction ensures smooth coordination between the EL and the CL, maintaining the integrity and efficiency of Story's blockchain network.