---
title: Gelato
excerpt: >-
  Gelato Network automates smart contract execution across major blockchains.
  Its products include decentralized automation, Web3 Functions, Gasless
  Transactions, and Gelato VRF for secure, verifiable randomness.
deprecated: false
hidden: false
metadata:
  robots: index
---
# VRF

Gelato VRF provides verifiable randomness for blockchain applications by utilizing Drand, a decentralized and trusted source of random numbers. It ensures that developers receive truly random values that are both provable and tamper-resistant.

See Gelato's [Documentation](\[https://docs.gelato.network/web3-services/vrf]\(https://docs.gelato.network/web3-services/vrf\)) guide to integrate your application with their Price Feeds.

## Smart Contracts

### Functions and VRF

#### Mainnet

##### [EIP173Proxy.sol](https://www.storyscan.xyz/address/0xafd37d0558255aA687167560cd3AaeEa75c2841E)

```
0xafd37d0558255aA687167560cd3AaeEa75c2841E
```

##### [Automate.sol](https://www.storyscan.xyz/address/0xab2c44495F5F954149b94C750ca20B64ea60B51c)

```
0xab2c44495F5F954149b94C750ca20B64ea60B51c
```

### Relays

#### Mainnet

##### GelatoRelay

Relay method: `callWithSyncFee`

###### EIP173Proxy.sol

```
0xcd565435e0d2109feFde337a66491541Df0D1420
```

#####

###### GelatoRelay.sol

```
0xA75983F686999843804a2ECC0E93C35d39a4F750
```

##### GelatoRelayERC2771.sol

Relay method: `callWithSyncFeeERC2771`

```
0x8aCE64CEA52b409F930f60B516F65197faD4B056
```

##### GelatoRelayConcurrentERC2771.sol

Relay method: `callWithSyncFeeERC2771` with `isConcurrent: true`

```
0xc7739c195618D314C08E8626C98f8573E4E43634
```

##### GelatoRelay1BalanceERC2771.sol

Relay method: `sponsoredCallERC2771`

```
0x61F2976610970AFeDc1d83229e1E21bdc3D5cbE4
```