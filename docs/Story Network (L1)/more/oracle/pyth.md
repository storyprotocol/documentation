---
title: Pyth
excerpt: >-
  Pyth Network is a decentralized oracle providing market data and verifiable
  random functions (VRF) for smart contracts. Its VRF service enables on-chain
  generation of provably fair random numbers. By sourcing data directly from
  institutional providers, Pyth ensures secure, low-latency updates while
  maintaining transparency and efficiency.
deprecated: false
hidden: true
metadata:
  robots: index
---
## VRF

### Documentation

To integrate Pyth Entropy, you need to invoke an on-chain function to request a random number from Entropy. This function accepts a randomly generated number, which can be created off-chain and sent to the Entropy contract. In return, the contract provides a sequence number. Once the request is processed, Pyth Entropy will send a callback to your contract, delivering the generated random number.

See Pyth's [How to Generate Random numbers in EVM dApps](https://docs.pyth.network/entropy/generate-random-numbers/evm) guide to integrate your application with Pyth Entropy.

### Contracts

#### Mainnet

##### ERC1967Proxy.sol

```
address: 0xdF21D137Aadc95588205586636710ca2890538d5
```

##### EntropyUpgradeable.sol

```
address: 0x4374e5a8b9C22271E9EB878A2AA31DE97DF15DAF
```

<br />

#### Testnet (Aeneid)

##### ERC1967Proxy.sol

```
address: 0x5744Cbf430D99456a0A8771208b674F27f8EF0Fb
```

##### EntropyUpgradeable.sol

```
address: 0x74f09cb3c7e2A01865f424FD14F6dc9A14E3e94E
```