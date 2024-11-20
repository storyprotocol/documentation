---
title: Network FAQ
excerpt: Common questions related to our L1 (Story Network).
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
<details>
<summary>What are the hardware requirements?</summary>

<br />

See the <a href="https://docs.storyprotocol.xyz/docs/node-setup#system-specs">system specs</a>
</details>

***

<details>
<summary>What's the max expected TPS?</summary>

<br />

\~700
</details>

***

<details>
<summary>Is it fully EVM-compatible? Is there any customization already being made on the IP blockchain? Or are there any coming customization to be applied?</summary>

<br />

Yes, it’s EVM-compatible. Story’s execution client is a fork of Geth with our custom precompiles, which enhance the IP graph's performance while maintaining strict EVM compatibility. Other Ethereum execution clients, such as RETH and Erigon, can be supported later.
</details>

***

<details>
<summary>Which is your consensus mechanism?</summary>

<br />

Our consensus mechanism is CometBFT
</details>

***

<details>
<summary>Batches support? Limit on batch request?</summary>

<br />

Batch RPCs are supported - for Geth there is a 1K limit and on the consensus side there is 10 request limit
</details>

***

<details>
<summary>WS connections? (if yes, how do they work)</summary>

<br />

Yes, WS is enabled on the execution client, and is recommended for subscription use-cases. It is open on port 8546
</details>

***

<details>
<summary>How many different paths does node serves (several path with diff methods RPC)?</summary>

<br />

Please see Geth’s latest JSON-RPC documentation for a full comprehensive list <a href="https://ethereum.org/en/developers/docs/apis/json-rpc/#web3_clientversion">here</a>. In the future, we may add more.
</details>

***

<details>
<summary>Caching rules for RPC method?</summary>

<br />

We recommend employing standard in-memory caching with a 1-10 min TTL based on the RPC method
</details>

***

<details>
<summary>What is the best method to get latest block and check node is healthy and in sync?</summary>

<br />

Use `eth_syncing` RPC call on the execution client to check if the node is sync and `eth_blockNumber` for getting the latest block
</details>

***

<details>
<summary>What are the heaviest RPC methods? How much time does it take to respond to request with such method?</summary>

<br />

`eth_call` / `eth_getLogs` / `eth_getBlockByNumber` \
We are still running latency tests to get a sense of response times.
</details>

***

<details>
<summary>Is archive node provisioning a requirement? If yes how big?</summary>

<br />

No, not at the moment.
</details>

***

<details>
<summary>Are there snapshots available for full / archive?</summary>

<br />

Not yet, but we are working on it.
</details>