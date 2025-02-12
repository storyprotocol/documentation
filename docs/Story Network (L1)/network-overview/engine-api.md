---
title: Engine API
deprecated: false
hidden: false
metadata:
  robots: index
---
The Engine API is a collection of JSON-RPC methods that facilitate communication between the execution layer (EL) and the consensus layer (CL) of an EVM node. Story's execution layer, which offers full EVM compatibility, supports all standard JSON-RPC methods defined by the [Ethereum Engine API](https://github.com/ethereum/execution-apis/blob/main/src/engine/common.md). Meanwhile, Story's consensus layer, built on Cosmos modules, utilizes the Engine API to coordinate with the execution layer.

## Functionalities

The Engine API facilitates seamless interaction between the EL and the CL by providing essential coordination mechanisms, including:

* **Handshake**
* **Synchronization**
* **Block Validation**
* **Block Proposal**

## Execution Layer Implementation

The EL in Story implements the following standard Engine API methods to support these functionalities:

* `engine_exchangeCapabilities`: Exchanges supported methods.
* `engine_getClientVersion`: Exchanges client version data.
* `engine_newPayload`: Inserts the given payload into the local chain.
* `engine_forkchoiceUpdate`: Updates the canonical chain marker and generates the payload with given attributes.
* `engine_getPayload`: Retrieves the pre-generated payload.

## Consensus Layer Interaction

How does Story's Consensus Layer (CL) interact with these methods? The answer lies in CometBFT ABCI++.

CometBFT is a state machine replication engine which provides consensus and security for Cosmos modules. ABCI++, also known as ABCI 2.0, is the interface between CometBFT and the actual state machine being replicated(i.e. EL's state machine).

ABCI++ comprises of a set of methods that interact with the Engine API, as outlined below:

### **1. PrepareProposal** (Proposing a New Block)

* The CL checks whether a payload is already being generated using `payloadID`.
* If not, the CL calls `engine_forkchoiceUpdate` to trigger a new payload generation.
* The CL then calls `engine_getPayload` with `payloadID` to fetch the payload and propose a new block.

### \*\*2. ProcessProposal (Processing a New Block)

* The CL calls `engine_newPayload` to  delivers the new block to the EL.
* The EL validates payload of the new block, executes transactions deterministically and updates its state.

### **3. FinalizeBlock** (Finalizing a Decided Block)

* The CL calls `engine_newPayload` to  delivers the finalized block to the EL.
* If the block has not yet been incorporated into the EL, the EL validates payload of the new block, executes transactions deterministically and updates its state.
* Since CometBFT provides instant finality, the CL calls `engine_forkchoiceUpdate` to finalize the block.
* Finally, the CL calls `engine_forkchoiceUpdate` again, with extra attributes,  to start an optimistic build of the next block if enabled, and if the validator is the next proposer.

This interaction ensures smooth coordination between the EL and the CL, maintaining the integrity and efficiency of Story's blockchain network.
