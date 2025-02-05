---
title: Story Mainnet Guide
excerpt: ""
deprecated: false
hidden: false
metadata:
  title: ""
  description: ""
  robots: index
next:
  description: ""
---

# Overview

Story Network is a purpose-built layer 1 blockchain achieving the best of EVM and Cosmos SDK. It is 100% EVM-compatible alongside deep execution layer optimizations to support graph data structures, purpose-built for handling complex data structures like IP quickly and cost-efficiently. It does this by:

- using precompiled primitives to traverse complex data structures like IP graphs within seconds at marginal costs
- a consensus layer based on the mature CometBFT stack to ensure fast finality and cheap transactions
- a modular architecture that decouples consensus from execution via Ethereum’s Engine-API

# Resources

**Network Name**: Story Mainnet

**Chain ID**: 1514

## :link: RPCs

<Table align={["left","left","left"]}>
  <thead>
    <tr>
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
        Story
      </td>

      <td style={{ textAlign: "left" }}>
        `https://mainnet.storyrpc.io`
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

  </tbody>
</Table>

## :mag: Block Explorers

<Table align={["left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Explorer
      </th>

      <th style={{ textAlign: "left" }}>
        URL
      </th>

      <th style={{ textAlign: "left" }}>
        Official
      </th>
    </tr>

  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        <a href="https://www.storyscan.xyz/" target="_blank">Blockscout Explorer ↗️</a>
      </td>

      <td style={{ textAlign: "left" }}>
        `https://www.storyscan.xyz/`
      </td>

      <td style={{ textAlign: "left" }}>
        :white_check_mark:
      </td>
    </tr>

  </tbody>
</Table>

## :mag: IP-related Explorer

Specifically for IP-related transactions like registering an IPA, minting a license, attaching license terms, etc.

> 🚧 Coming soon!

| Explorer | URL |
| :------- | :-- |
| N/A      | N/A |

## :money_with_wings: Faucets

> 🚧 Coming soon!

| Faucet | Amount | Requirement |
| :----- | :----- | :---------- |
| N/A    | N/A    | N/A         |

## :moneybag: Staking

> 🚧 Coming soon!

| Dashboard |
| :-------- |
| N/A       |

## Ports

The following ports are available for `story-geth` and `story` clients:

Geth:

- RPC: 8545
- WS: 8546
- P2P: 30303

Metrics:

- Prometheus: 9100
- Geth: 6060
- Story: 26660

# Further Sections

- [Node Setup](doc:node-setup-dev-mainnet)
- [Validator Operations](doc:validator-operations)
- [Tokenomics & Staking](doc:tokenomics-staking)
