---
title: Precompiles
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
## Introduction

Precompiled contracts are specialized smart contracts implemented directly in the execution layer of a blockchain. Unlike user-deployed smart contracts that execute EVM bytecode, precompiled contracts offer optimized native implementations for complex cryptographic and computational operations. This significantly improves efficiency and reduces gas costs. Precompiled contracts exist at fixed addresses within the execution client and each precompile has a predefined gas cost based on its computational complexity, ensuring predictable execution fees.

Story Protocol introduces two precompiled contracts:

* `p256Verify` precompile to support signature verifications in the secp256r1 elliptic curve.
* `ipgraph` precompile to enhance on-chain intellectual property management.

In addition, Story Protocol’s execution layer supports all standard EVM precompiled contracts, ensuring full compatibility with Ethereum-based tooling and applications.

## Precompiled Contracts

| Address          | Functionality                                                 |
| ---------------- | ------------------------------------------------------------- |
| byte{0x01}       | `ecrecover`- ECDSA signature recovery                         |
| byte{0x02}       | `sha256` - SHA-256 hash computation                           |
| byte{0x03}       | `ripemd160` - RIPEMD-160 hash computation                     |
| byte{0x04}       | `identity` - Identity function                                |
| byte{0x05}       | `modexp` - Modular exponentiation                             |
| byte{0x06}       | `bn256Add` - BN256 elliptic curve addition                    |
| byte{0x07}       | `bn256ScalarMul` - BN256 elliptic curve scalar multiplication |
| byte{0x08}       | `bn256Pairing` - BN256 elliptic curve pairing check           |
| byte{0x09}       | `blake2f` - Blake2 hash function                              |
| byte{0x0a}       | `kzgPointEvaluation` - KZG polynomial commitment evaluation   |
| byte{0x01, 0x00} | `p256Verify` -  Secp256r1 signature verification              |
| byte{0x01, 0x01} | `ipgraph` - Intellectual property management                  |

## p256Verify precompile

Refer to [RIP-7212](https://github.com/ethereum/RIPs/blob/master/RIPS/rip-7212.md) for more information.

## IPgraph precompile

The `ipgraph` precompile enables efficient querying and modification of IP relationships and royalty structures while minimizing gas costs.

This precompile provides multiple functions based on the function selector—the first 4 bytes of the input.

| Function Selector        | Description                                                     | Gas computation formula                               | Gas Cost                           |
| :----------------------- | :-------------------------------------------------------------- | :---------------------------------------------------- | :--------------------------------- |
| `addParentIp`            | Adds a parent IP record                                         | `intrinsicGas + (ipGraphWriteGas * parentCount)`      | Larger than 1100                   |
| `hasParentIp`            | Checks if an IP is parent of another IP                         | `ipGraphReadGas * averageParentIpCount`               | 40                                 |
| `getParentIps`           | Retrieves parent IPs                                            | `ipGraphReadGas * averageParentIpCount`               | 40                                 |
| `getParentIpsCount`      | Gets the number of parent IPs                                   | `ipGraphReadGas`                                      | 10                                 |
| `getAncestorIps`         | Retrieves ancestor IPs                                          | `ipGraphReadGas * averageAncestorIpCount * 2`         | 600                                |
| `getAncestorIpsCount`    | Gets the number of ancestor IPs                                 | `ipGraphReadGas * averageParentIpCount * 2`           | 80                                 |
| `hasAncestorIp`          | Checks if an IP is ancestor of another IP                       | `ipGraphReadGas * averageAncestorIpCount * 2`         | 600                                |
| `setRoyalty`             | Sets royalty details of an IP                                   | `ipGraphWriteGas`                                     | 1000                               |
| `getRoyalty`             | Retrieves royalty details of an IP                              | `varies by royalty policy`                            | LAP:900, LRP:620, other:1000       |
| `getRoyaltyStack`        | Retrieves royalty stack  of an IP                               | `varies by royalty policy`                            | LAP:50, LRP: 600, other:1000       |
| `hasParentIpExt`         | Checks if an IP is parent of another IP through external call   | `ipGraphExternalReadGas * averageParentIpCount`       | 8400                               |
| `getParentIpsExt`        | Retrieves parent IPs through external call                      | `ipGraphExternalReadGas * averageParentIpCount`       | 8400                               |
| `getParentIpsCountExt`   | Gets the number of parent IPs through external call             | `ipGraphExternalReadGas`                              | 2100                               |
| `getAncestorIpsExt`      | Retrieve ancestor IPs through external call                     | `ipGraphExternalReadGas * averageAncestorIpCount * 2` | 126000                             |
| `getAncestorIpsCountExt` | Gets the number of ancestor IPs through external call           | `ipGraphExternalReadGas * averageParentIpCount * 2`   | 16800                              |
| `hasAncestorIpExt`       | Checks if an IP is ancestor of another IP through external call | `ipGraphExternalReadGas * averageAncestorIpCount * 2` | 126000                             |
| `getRoyaltyExt`          | Retrieves royalty details of an IP through external call        | `varies by royalty policy`                            | LAP:189000, LRP:130200, other:1000 |
| `getRoyaltyStackExt`     | Retrieves royalty stack of an IP through external call          | `varies by royalty policy`                            | LAP:10500, LRP:126000, other:1000  |

Refer to the [Royalty Module](doc:royalty-module) for detailed information on royalty policies.
