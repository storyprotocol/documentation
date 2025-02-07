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

Precompiled contracts are specialized smart contracts implemented directly in the execution layer of a blockchain. Unlike user-deployed smart contracts that execute EVM bytecode, precompiled contracts offer optimized native implementations for complex cryptographic and computational operations. This significantly improves efficiency and reduces gas costs.

Precompiled contracts exist at fixed addresses within the execution environment and each precompile has a predefined gas cost based on its computational complexity, ensuring predictable execution fees.

Story Protocol’s execution layer supports all standard EVM precompiled contracts, ensuring full compatibility with Ethereum-based tooling and applications.

Additionally, Story Protocol introduces two extra precompiled contracts:

* `p256Verify` precompile to support signature verifications in the secp256r1 elliptic curve.
* `ipgraph` precompile to enhance on-chain intellectual property management.

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

<Table align={["left","left","left","left"]}>
  <thead>
    <tr>
      <th style={{ textAlign: "left" }}>
        Function Selector
      </th>

      <th style={{ textAlign: "left" }}>
        Description
      </th>

      <th style={{ textAlign: "left" }}>
        Gas computation formula
      </th>

      <th style={{ textAlign: "left" }}>
        Gas Cost
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td style={{ textAlign: "left" }}>
        `addParentIp`
      </td>

      <td style={{ textAlign: "left" }}>
        Adds a parent IP record
      </td>

      <td style={{ textAlign: "left" }}>
        `intrinsicGas + (ipGraphWriteGas * parentCount)`
      </td>

      <td style={{ textAlign: "left" }}>
        > \= 1100
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `hasParentIp`
      </td>

      <td style={{ textAlign: "left" }}>
        Checks if an IP is parent of another IP
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphReadGas * averageParentIpCount`
      </td>

      <td style={{ textAlign: "left" }}>
        40
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getParentIps`
      </td>

      <td style={{ textAlign: "left" }}>
        Retrieves parent IPs
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphReadGas * averageParentIpCount`
      </td>

      <td style={{ textAlign: "left" }}>
        40
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getParentIpsCount`
      </td>

      <td style={{ textAlign: "left" }}>
        Gets the number of parent IPs
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphReadGas`
      </td>

      <td style={{ textAlign: "left" }}>
        10
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getAncestorIps`
      </td>

      <td style={{ textAlign: "left" }}>
        Retrieves ancestor IPs
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphReadGas * averageAncestorIpCount * 2`
      </td>

      <td style={{ textAlign: "left" }}>
        600
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getAncestorIpsCount`
      </td>

      <td style={{ textAlign: "left" }}>
        Gets the number of ancestor IPs
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphReadGas * averageParentIpCount * 2`
      </td>

      <td style={{ textAlign: "left" }}>
        80
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `hasAncestorIp`
      </td>

      <td style={{ textAlign: "left" }}>
        Checks if an IP is ancestor of another IP
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphReadGas * averageAncestorIpCount * 2`
      </td>

      <td style={{ textAlign: "left" }}>
        600
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `setRoyalty`
      </td>

      <td style={{ textAlign: "left" }}>
        Sets royalty details of an IP
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphWriteGas`
      </td>

      <td style={{ textAlign: "left" }}>
        1000
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getRoyalty`
      </td>

      <td style={{ textAlign: "left" }}>
        Retrieves royalty details of an IP
      </td>

      <td style={{ textAlign: "left" }}>
        `varies by royalty policy`
      </td>

      <td style={{ textAlign: "left" }}>
        LAP:900, LRP:620, other:1000
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getRoyaltyStack`
      </td>

      <td style={{ textAlign: "left" }}>
        Retrieves royalty stack  of an IP
      </td>

      <td style={{ textAlign: "left" }}>
        `varies by royalty policy`
      </td>

      <td style={{ textAlign: "left" }}>
        LAP:50, LRP: 600, other:1000
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `hasParentIpExt`
      </td>

      <td style={{ textAlign: "left" }}>
        Checks if an IP is parent of another IP through external call
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphExternalReadGas * averageParentIpCount`
      </td>

      <td style={{ textAlign: "left" }}>
        8400
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getParentIpsExt`
      </td>

      <td style={{ textAlign: "left" }}>
        Retrieves parent IPs through external call
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphExternalReadGas * averageParentIpCount`
      </td>

      <td style={{ textAlign: "left" }}>
        8400
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getParentIpsCountExt`
      </td>

      <td style={{ textAlign: "left" }}>
        Gets the number of parent IPs through external call
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphExternalReadGas`
      </td>

      <td style={{ textAlign: "left" }}>
        2100
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getAncestorIpsExt`
      </td>

      <td style={{ textAlign: "left" }}>
        Retrieve ancestor IPs through external call
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphExternalReadGas * averageAncestorIpCount * 2`
      </td>

      <td style={{ textAlign: "left" }}>
        126000
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getAncestorIpsCountExt`
      </td>

      <td style={{ textAlign: "left" }}>
        Gets the number of ancestor IPs through external call
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphExternalReadGas * averageParentIpCount * 2`
      </td>

      <td style={{ textAlign: "left" }}>
        16800
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `hasAncestorIpExt`
      </td>

      <td style={{ textAlign: "left" }}>
        Checks if an IP is ancestor of another IP through external call
      </td>

      <td style={{ textAlign: "left" }}>
        `ipGraphExternalReadGas * averageAncestorIpCount * 2`
      </td>

      <td style={{ textAlign: "left" }}>
        126000
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getRoyaltyExt`
      </td>

      <td style={{ textAlign: "left" }}>
        Retrieves royalty details of an IP through external call
      </td>

      <td style={{ textAlign: "left" }}>
        `varies by royalty policy`
      </td>

      <td style={{ textAlign: "left" }}>
        LAP:189000, LRP:130200, other:1000
      </td>
    </tr>

    <tr>
      <td style={{ textAlign: "left" }}>
        `getRoyaltyStackExt`
      </td>

      <td style={{ textAlign: "left" }}>
        Retrieves royalty stack of an IP through external call
      </td>

      <td style={{ textAlign: "left" }}>
        `varies by royalty policy`
      </td>

      <td style={{ textAlign: "left" }}>
        LAP:10500, LRP:126000, other:1000
      </td>
    </tr>
  </tbody>
</Table>

Refer to [Royalty Module](doc:royalty-module) for detailed information on royalty policies.