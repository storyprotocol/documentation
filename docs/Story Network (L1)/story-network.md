---
title: Story Network Guide
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
> 🚧 We are still in testnet!
>
> Please note that Story Network (our purpose-built L1) is still in **testnet**. This means things are subject to change or break along the way.

> 📘 **Odyssey** is our current testnet. **Iliad** is our old testnet, and is being sunset soon.

# Overview

Story Network is a purpose-built layer 1 blockchain achieving the best of EVM and Cosmos SDK. It is 100% EVM-compatible alongside deep execution layer optimizations to support graph data structures, purpose-built for handling complex data structures like IP quickly and cost-efficiently. It does this by:

* using precompiled primitives to traverse complex data structures like IP graphs within seconds at marginal costs
* a consensus layer based on the mature CometBFT stack to ensure fast finality and cheap transactions
* a modular architecture that decouples consensus from execution via Ethereum’s Engine-API

# Resources

## :link: RPCs

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Network
      </th>

      <th style={{ textAlign: "left" }}>
        RPC Name
      </th>

      <th style={{ textAlign: "left" }}>
        RPC URL
      </th>

      <th style={{ textAlign: "left" }}>
        Official
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        Story
      </td>

      <td style={{ textAlign: "left" }}>
        `https://rpc.odyssey.storyrpc.io/`
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        Story
      </td>

      <td style={{ textAlign: "left" }}>
        `https://testnet.storyrpc.io/`
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        QuickNode
      </td>

      <td style={{ textAlign: "left" }}>
        `https://lingering-frequent-dew.story-testnet.quiknode.pro/`
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        NodeFleet
      </td>

      <td style={{ textAlign: "left" }}>
        `https://story-testnet.us.nodefleet.net`
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        Aura Network
      </td>

      <td style={{ textAlign: "left" }}>
        `https://story-testnet.aura.network`
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>
  </tbody>
</Table>

## :mag: Block Explorers

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Network
      </th>

      <th style={{ textAlign: "left" }}>
        Explorer
      </th>

      <th style={{ textAlign: "left" }}>
        Official
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://odyssey.storyscan.xyz/" target="_blank">Story Odyssey Block Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://odyssey.story.explorers.guru/" target="_blank">Nodes.Guru Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://www.okx.com/web3/explorer/story-odyssey" target="_blank">OKX Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://story.aurascan.io/" target="_blank">Story Scan by Aura Network ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://testnet.storyscan.xyz" target="_blank">Story Iliad Block Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://testnet.story.explorers.guru/" target="_blank">Nodes.Guru Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://story-testnet.socialscan.io/" target="_blank">Social Scan Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>

      </td>
    </tr>
  </tbody>
</Table>

## :mag: IP-related Explorer

Specifically for IP-related transactions like registering an IPA, minting a license, attaching license terms, etc.

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Network
      </th>

      <th style={{ textAlign: "left" }}>
        Explorer
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://odyssey.explorer.story.foundation" target="_blank">Story Odyssey Explorer ↗️</a>
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://iliad.explorer.story.foundation" target="_blank">Story Iliad Explorer ↗️</a>
      </td>
    </tr>
  </tbody>
</Table>

## :money_with_wings: Faucets

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Network
      </th>

      <th style={{ textAlign: "left" }}>
        Faucet
      </th>

      <th style={{ textAlign: "left" }}>
        Amount
      </th>

      <th style={{ textAlign: "left" }}>
        Requirement
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://odyssey.faucet.story.foundation/" target="_blank">Story Faucet ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        1 $IP
      </td>

      <td style={{ textAlign: "left" }}>
        Every 24 hours
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://faucet.quicknode.com/story/odyssey" target="_blank">Quicknode Faucet ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        1-2 $IP
      </td>

      <td style={{ textAlign: "left" }}>
        Every 24 hours
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        Iliad
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://iliad.faucet.story.foundation/" target="_blank">Story Faucet ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        1 $IP
      </td>

      <td style={{ textAlign: "left" }}>
        Every 24 hours
      </td>
    </tr>
  </tbody>
</Table>

## :moneybag: Staking

<Table align={["left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Network
      </th>

      <th style={{ textAlign: "left" }}>
        Dashboard
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        Odyssey
      </td>

      <td style={{ textAlign: "left" }}>
        <a href="https://staking.story.foundation/" target="_blank">Story Validators ↗️</a>
      </td>
    </tr>
  </tbody>
</Table>

## Ports

The following  ports are available for `geth` and `story` clients:

Geth:

* RPC: 8545
* WS: 8546
* P2P: 30303

Metrics:

* Prometheus: 9100
* Geth: 6060
* Story (Iliad): 26660

## Infrastructure Providers

### Cross-chain

* [LayerZero](https://docs.layerzero.network/v2/developers/evm/technical-reference/deployed-contracts#odyssey-testnet) 

### Indexers/Data

* [Simplehash](https://simplehash.com/)
* [Goldsky](https://goldsky.com/)
* [Zettablock](https://zettablock.com/)

### Oracles/Automation

* [Gelato](https://www.gelato.network/)

### Tools

* [Protofire](https://protofire.io/)

### Wallets/AA

* [Dynamic](https://www.dynamic.xyz/)
* [Pimlico](https://www.pimlico.io/)
* [ZeroDev](https://zerodev.app/)

# Further Sections

* [Wallet Setup](doc:odyssey-wallet-setup)
* [Node Setup](doc:odyssey-node-setup)
* [Validator Operations](doc:odyssey-validator-operations)
* [Tokenomics & Staking](doc:tokenomics-staking)
