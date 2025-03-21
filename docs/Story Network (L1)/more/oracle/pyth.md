---
title: Pyth
excerpt: >-
  Pyth Network is a decentralized oracle providing pricing data and verifiable
  random functions (VRF) for smart contracts. Pyth Entropy(VRF) service enables
  on-chain generation of provably fair random numbers. By sourcing data directly
  from institutional providers, Pyth ensures secure, low-latency price updates
  while maintaining transparency and efficiency.
deprecated: false
hidden: false
metadata:
  robots: index
---
# Price Feeds

Pyth Network provides real-time financial market data to smart contracts across 100+ blockchains, sourcing prices from over 100 exchanges and market makers. With 850+ price feeds covering equities, commodities, and cryptocurrencies, Pyth aggregates and updates prices multiple times per second.

See Pyth's [Documentation](https://docs.pyth.network/price-feeds/price-feeds) guide to integrate your application with Pyth Price Feeds.

## Smart Contracts

### Mainnet

#### [ERC1967Proxy.sol](https://www.storyscan.xyz/address/0xD458261E832415CFd3BAE5E416FdF3230ce6F134)

```
0xD458261E832415CFd3BAE5E416FdF3230ce6F134
```

#### [PythUpgradable.sol](https://www.storyscan.xyz/address/0x5f3c61944CEb01B3eAef861251Fb1E0f14b848fb)

```
0x5f3c61944CEb01B3eAef861251Fb1E0f14b848fb
```

### Testnet (Aeneid)

#### [ERC1967Proxy.sol](https://aeneid.storyscan.xyz/address/0x36825bf3Fbdf5a29E2d5148bfe7Dcf7B5639e320)

```
0x36825bf3Fbdf5a29E2d5148bfe7Dcf7B5639e320
```

#### [PythUpgradeable.sol](https://aeneid.storyscan.xyz/address/0x98046Bd286715D3B0BC227Dd7a956b83D8978603)

```
0x98046Bd286715D3B0BC227Dd7a956b83D8978603
```

<br />

# VRF

To integrate Pyth Entropy, you need to invoke an on-chain function to request a random number from Entropy. This function accepts a randomly generated number, which can be created off-chain and sent to the Entropy contract. In return, the contract provides a sequence number. Once the request is processed, Pyth Entropy will send a callback to your contract, delivering the generated random number.

See Pyth's [How to Generate Random numbers in EVM dApps](https://docs.pyth.network/entropy/generate-random-numbers/evm) guide to integrate your application with Pyth Entropy.

## Smart Contracts

### Mainnet

#### [ERC1967Proxy.sol](https://www.storyscan.xyz/address/0xdF21D137Aadc95588205586636710ca2890538d5)

```
0xdF21D137Aadc95588205586636710ca2890538d5
```

#### [EntropyUpgradeable.sol](\[https://www.storyscan.xyz/address/0x4374e5a8b9C22271E9EB878A2AA31DE97DF15DAF]\(https://www.storyscan.xyz/address/0x4374e5a8b9C22271E9EB878A2AA31DE97DF15DAF\))

```
0x4374e5a8b9C22271E9EB878A2AA31DE97DF15DAF

```

### Testnet (Aeneid)

#### [ERC1967Proxy.sol](\[https://aeneid.storyscan.xyz/address/0x5744Cbf430D99456a0A8771208b674F27f8EF0Fb]\(https://aeneid.storyscan.xyz/address/0x5744Cbf430D99456a0A8771208b674F27f8EF0Fb\))

```
0x5744Cbf430D99456a0A8771208b674F27f8EF0Fb
```

#### [EntropyUpgradeable.sol](https://aeneid.storyscan.xyz/address/0x74f09cb3c7e2A01865f424FD14F6dc9A14E3e94E)

```
0x74f09cb3c7e2A01865f424FD14F6dc9A14E3e94E
```