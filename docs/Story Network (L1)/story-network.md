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

- using precompiled primitives to traverse complex data structures like IP graphs within seconds at marginal costs
- a consensus layer based on the mature CometBFT stack to ensure fast finality and cheap transactions
- a modular architecture that decouples consensus from execution via Ethereum’s Engine-API

# Resources

## :link: RPCs

| Network | RPC Name     | RPC URL                                                      | Official           |
| :------ | :----------- | :----------------------------------------------------------- | :----------------- |
| Odyssey | Story        | `https://rpc.odyssey.storyrpc.io/`                           | :white_check_mark: |
| Iliad   | Story        | `https://testnet.storyrpc.io/`                               | :white_check_mark: |
| Iliad   | QuickNode    | `https://lingering-frequent-dew.story-testnet.quiknode.pro/` |                    |
| Iliad   | NodeFleet    | `https://story-testnet.us.nodefleet.net`                     |                    |
| Iliad   | Aura Network | `https://story-testnet.aura.network`                         |                    |

## :mag: Block Explorers

[block:parameters]
{
  "data": {
    "h-0": "Network",
    "h-1": "Explorer",
    "h-2": "Official",
    "0-0": "Odyssey",
    "0-1": "<a href=\"https://odyssey.storyscan.xyz/\" target=\"_blank\">Story Odyssey Block Explorer ↗️</a>",
    "0-2": ":white_check_mark:",
    "1-0": "Odyssey",
    "1-1": "<a href=\"https://odyssey.story.explorers.guru/\" target=\"_blank\">Nodes.Guru Explorer ↗️</a>",
    "1-2": "",
    "2-0": "Odyssey",
    "2-1": "<a href=\"https://www.okx.com/web3/explorer/story-odyssey\" target=\"_blank\">OKX Explorer ↗️</a>",
    "2-2": "",
    "3-0": "Odyssey",
    "3-1": "<a href=\"https://story.aurascan.io/\" target=\"_blank\">Story Scan by Aura Network ↗️</a>",
    "3-2": "",
    "4-0": "Iliad",
    "4-1": "<a href=\"https://testnet.storyscan.xyz\" target=\"_blank\">Story Iliad Block Explorer ↗️</a>",
    "4-2": ":white_check_mark:",
    "5-0": "Iliad",
    "5-1": "<a href=\"https://testnet.story.explorers.guru/\" target=\"_blank\">Nodes.Guru Explorer ↗️</a>",
    "5-2": "",
    "6-0": "Iliad",
    "6-1": "<a href=\"https://story-testnet.socialscan.io/\" target=\"_blank\">Social Scan Explorer ↗️</a>",
    "6-2": ""
  },
  "cols": 3,
  "rows": 7,
  "align": [
    "left",
    "left",
    "left"
  ]
}
[/block]


## :mag: IP-related Explorer

Specifically for IP-related transactions like registering an IPA, minting a license, attaching license terms, etc.

[block:parameters]
{
  "data": {
    "h-0": "Network",
    "h-1": "Explorer",
    "0-0": "Odyssey",
    "0-1": "<a href=\"https://odyssey.explorer.story.foundation\" target=\"_blank\">Story Odyssey Explorer ↗️</a>",
    "1-0": "Iliad",
    "1-1": "<a href=\"https://iliad.explorer.story.foundation\" target=\"_blank\">Story Iliad Explorer ↗️</a>"
  },
  "cols": 2,
  "rows": 2,
  "align": [
    "left",
    "left"
  ]
}
[/block]


## :money_with_wings: Faucets

[block:parameters]
{
  "data": {
    "h-0": "Network",
    "h-1": "Faucet",
    "h-2": "Amount",
    "h-3": "Requirement",
    "0-0": "Odyssey",
    "0-1": "<a href=\"https://odyssey.faucet.story.foundation/\" target=\"_blank\">Story Faucet ↗️</a>",
    "0-2": "1 $IP",
    "0-3": "Every 24 hours",
    "1-0": "Odyssey",
    "1-1": "<a href=\"https://faucet.quicknode.com/story/odyssey\" target=\"_blank\">Quicknode Faucet ↗️</a>",
    "1-2": "1-2 $IP",
    "1-3": "Every 24 hours",
    "2-0": "Iliad",
    "2-1": "<a href=\"https://iliad.faucet.story.foundation/\" target=\"_blank\">Story Faucet ↗️</a>",
    "2-2": "1 $IP",
    "2-3": "Every 24 hours"
  },
  "cols": 4,
  "rows": 3,
  "align": [
    "left",
    "left",
    "left",
    "left"
  ]
}
[/block]


## :moneybag: Staking

[block:parameters]
{
  "data": {
    "h-0": "Network",
    "h-1": "Dashboard",
    "0-0": "Odyssey",
    "0-1": "<a href=\"https://staking.story.foundation/\" target=\"_blank\">Story Validators ↗️</a>"
  },
  "cols": 2,
  "rows": 1,
  "align": [
    "left",
    "left"
  ]
}
[/block]


## Ports

The following  ports are available for `geth` and `story` clients:

Geth:

- RPC: 8545
- WS: 8546
- P2P: 30303

Metrics:

- Prometheus: 9100
- Geth: 6060
- Story (Iliad): 26660

## Infrastructure Providers

### Cross-chain

- [LayerZero](https://docs.layerzero.network/v2/developers/evm/technical-reference/deployed-contracts#odyssey-testnet) 

### Indexers/Data

- [Simplehash](https://simplehash.com/)
- [Goldsky](https://goldsky.com/)
- [Zettablock](https://zettablock.com/)

### Oracles/Automation

- [Gelato](https://www.gelato.network/)

### Tools

- [Protofire](https://protofire.io/)

### Wallets/AA

- [Dynamic](https://www.dynamic.xyz/)
- [Pimlico](https://www.pimlico.io/)
- [ZeroDev](https://zerodev.app/)

# Further Sections

- [Wallet Setup](doc:odyssey-wallet-setup)
- [Node Setup](doc:odyssey-node-setup)
- [Validator Operations](doc:odyssey-validator-operations)
- [Tokenomics & Staking](doc:tokenomics-staking)