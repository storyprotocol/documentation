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

Story Protocol’s execution layer supports all standard EVM precompiled contracts, ensuring full compatibility with Ethereum-based tooling and applications.\
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

<Table>
  <thead>
    <tr>
      <th>
        Function Selector
      </th>

      <th>
        Description
      </th>

      <th>
        Gas computation formula
      </th>

      <th>
        Gas Cost
      </th>
    </tr>
  </thead>

  <tbody>
    <tr>
      <td>
        `addParentIp`
      </td>

      <td>
        Adds a parent IP record
      </td>

      <td>
        `intrinsicGas + (ipGraphWriteGas * parentCount)`
      </td>

      <td>
        > \= 1100
      </td>
    </tr>

    <tr>
      <td>
        `hasParentIp`
      </td>

      <td>
        Checks if an IP is parent of another IP
      </td>

      <td>
        `ipGraphReadGas * averageParentIpCount`
      </td>

      <td>
        40
      </td>
    </tr>

    <tr>
      <td>
        `getParentIps`
      </td>

      <td>
        Retrieves parent IPs
      </td>

      <td>
        `ipGraphReadGas * averageParentIpCount`
      </td>

      <td>
        40
      </td>
    </tr>

    <tr>
      <td>
        `getParentIpsCount`
      </td>

      <td>
        Gets the number of parent IPs
      </td>

      <td>
        `ipGraphReadGas`
      </td>

      <td>
        10
      </td>
    </tr>

    <tr>
      <td>
        `getAncestorIps`
      </td>

      <td>
        Retrieves ancestor IPs
      </td>

      <td>
        `ipGraphReadGas * averageAncestorIpCount * 2`
      </td>

      <td>
        600
      </td>
    </tr>

    <tr>
      <td>
        `getAncestorIpsCount`
      </td>

      <td>
        Gets the number of ancestor IPs
      </td>

      <td>
        `ipGraphReadGas * averageParentIpCount * 2`
      </td>

      <td>
        80
      </td>
    </tr>

    <tr>
      <td>
        `hasAncestorIp`
      </td>

      <td>
        Checks if an IP is ancestor of another IP
      </td>

      <td>
        `ipGraphReadGas * averageAncestorIpCount * 2`
      </td>

      <td>
        600
      </td>
    </tr>

    <tr>
      <td>
        `setRoyalty`
      </td>

      <td>
        Sets royalty details of an IP
      </td>

      <td>
        `ipGraphWriteGas`
      </td>

      <td>
        1000
      </td>
    </tr>

    <tr>
      <td>
        `getRoyalty`
      </td>

      <td>
        Retrieves royalty details of an IP
      </td>

      <td>
        `varies by royalty policy`
      </td>

      <td>
        LAP:900, LRP:620, other:1000
      </td>
    </tr>

    <tr>
      <td>
        `getRoyaltyStack`
      </td>

      <td>
        Retrieves royalty stack  of an IP
      </td>

      <td>
        `varies by royalty policy`
      </td>

      <td>
        LAP:50, LRP: 600, other:1000
      </td>
    </tr>

    <tr>
      <td>
        `hasParentIpExt`
      </td>

      <td>
        Checks if an IP is parent of another IP through external call
      </td>

      <td>
        `ipGraphExternalReadGas * averageParentIpCount`
      </td>

      <td>
        8400
      </td>
    </tr>

    <tr>
      <td>
        `getParentIpsExt`
      </td>

      <td>
        Retrieves parent IPs through external call
      </td>

      <td>
        `ipGraphExternalReadGas * averageParentIpCount`
      </td>

      <td>
        8400
      </td>
    </tr>

    <tr>
      <td>
        `getParentIpsCountExt`
      </td>

      <td>
        Gets the number of parent IPs through external call
      </td>

      <td>
        `ipGraphExternalReadGas`
      </td>

      <td>
        2100
      </td>
    </tr>

    <tr>
      <td>
        `getAncestorIpsExt`
      </td>

      <td>
        Retrieve ancestor IPs through external call
      </td>

      <td>
        `ipGraphExternalReadGas * averageAncestorIpCount * 2`
      </td>

      <td>
        126000
      </td>
    </tr>

    <tr>
      <td>
        `getAncestorIpsCountExt`
      </td>

      <td>
        Gets the number of ancestor IPs through external call
      </td>

      <td>
        `ipGraphExternalReadGas * averageParentIpCount * 2`
      </td>

      <td>
        16800
      </td>
    </tr>

    <tr>
      <td>
        `hasAncestorIpExt`
      </td>

      <td>
        Checks if an IP is ancestor of another IP through external call
      </td>

      <td>
        `ipGraphExternalReadGas * averageAncestorIpCount * 2`
      </td>

      <td>
        126000
      </td>
    </tr>

    <tr>
      <td>
        `getRoyaltyExt`
      </td>

      <td>
        Retrieves royalty details of an IP through external call
      </td>

      <td>
        `varies by royalty policy`
      </td>

      <td>
        LAP:189000, LRP:130200, other:1000
      </td>
    </tr>

    <tr>
      <td>
        `getRoyaltyStackExt`
      </td>

      <td>
        Retrieves royalty stack of an IP through external call
      </td>

      <td>
        `varies by royalty policy`
      </td>

      <td>
        LAP:10500, LRP:126000, other:1000
      </td>
    </tr>
  </tbody>
</Table>

Refer to [Royalty Module](doc:royalty-module) for detailed information on royalty policies.