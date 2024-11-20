---
title: Register Derivative IP With Signature
excerpt: ''
deprecated: false
hidden: true
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
The `registerDerivativeIp` function should be used whenever you want to enroll a prospective IP that already exists as an ERC-721 (e.g. your esteemed Punk or your favorite Noun), and want to register it as a derivative of some existing work. We refer to this process as "remixing" an existing work. This function is most commonly called by users who have existing licenses associated with an IP, and would like to use them to create derivative works that their registered IP then represents.

**Function Interface**:

```
    /// @notice Registers an existing NFT into the protocol as an IP asset derivative.
    /// @param licenseIds The licenses to incorporate for the new IP.
    /// @param royaltyContext The bytes-encoded context for royalty policy to process.
    /// @param tokenContract The address of the NFT bound to the root-level IP.
    /// @param tokenId The token id of the NFT bound to the root-level IP.
    /// @param ipMetadata Metadata related to IP attribution.
    /// @return The address identifier of the enrolled IP asset.
    function registerDerivativeIpWithSig(
        uint256[] calldata licenseIds,
        bytes calldata royaltyContext,
        address tokenContract,
        uint256 tokenId,
        Metadata.IPMetadata calldata ipMetadata,
        SPG.Signature calldata signature
    ) external onlyAuthorized(tokenContract, tokenId) returns (address) {

```

**Applicable structs** 

```
		/// @notice Describes a custom string key-value pair attribute.
    struct Attribute {
        string key;
        string value;
    }

    /// @notice Attributes related to IP metadata.
    struct IPMetadata {
        string name;
        bytes32 hash;
        string url;
        Attribute[] customMetadata;
    }

    /// @notice Signature of ERC712.
    struct Signature {
        // The address of the signer.
        address signer;
        // The time at which the signature expires.
        uint256 deadline;
        // The signature itself.
        bytes signature;
    }

```

## Detailed Overview

The `registerDerivativeIp` function takes seven parameters

**`uint256[] calldata licenseIds`**

The `licenseIds` array represents a list of licenses identifiers that the deriving IP originates from. To get a `licenseId`, a user must create a new license out of an existing IP with a policy attached to it. For instructions on how to do so, please see the [Licensing (Module)](doc:licensing-1) section.

**`bytes royaltyContext`**

In Story Protocol, IPs that are configured with a royalty policy require all derivative IP assets to specify a royalty context, which is an abi-encoding of all the relevant settings that dictate how royalty should work. For more information, on the details of this, please see the [Royalty (Module)](doc:royalty-module) section.

**`address tokenContract`**

The `tokenContract` identifies the address of the ERC-721 you wish to enroll as IP. For example, this would be `0x9c8ff314c9bc7f6e59a9d9225fb22946427edc03` in the case of Nouns on Ethereum.

**`uint256 tokenId`**

The `tokenId` identifies the token identifier of your NFT. 

**`IPMetadata ipMetadata`**

The`ipMetadata` refers to the metadata you would like to associate with the IP. It is composed of the following fields:

- **`string name`**
  - The name you would like to assign to the registered IP asset. For simple on-chain NFT assets, this can simply be a concatenation of the collection name and your unique NFT identifier. However, for something more real-world applicable, such as a tokenized T-shirt logo, this can be something a bit more specific, such as "Story Protocol LS Tee Logo".
- **`bytes32 contentHash`**
  - The `contentHash` refers to a unique hash of the IP being registered. How this content hash is generated should be based on the requirements of the type of IP being registered, but for most intents and purposes, a simple SHA-256 checksum can work.
- **`string externalURL`**
  - The `externalURL` refers to an external URL that users may be redirected to when they want to learn more about the IP component of your registered asset. This, for example, may be a link to an IPFS PDF providing contextual information regarding how this IP was created in the first place.
- **`Attribute[] ipMetadata`**
  - The `ipMetadata` refers to additional IP metadata string key-value pairs that you may bind to the IP on registration. For example, this could be an array such as `[{"Copyright Type", "Literary Work"}, {"Literary Type", "Novel"}]`. Using custom IP metadata attribution is what allows extensibility for other types of on-chain IP composition. Note that for `v0.1-beta`, only string attribution is supported.
- **`Signature signature`**
  - The EIP-712 signature enables users to grant permissions to SPG, authorizing it to interact with other modules. This specifically includes the Licensing Module and the Metadata Resolver Module, which are utilized within the function.
  - **`address signer`** the address sign the message
  - **`uint256 deadline`** The time at which the signature expires.
  - **`uint256 signature`** The signature itself

```
				uint deadline = block.timestamp + 1000;
        AccessPermission.Permission[] memory permissionList = new AccessPermission.Permission[](2);
        permissionList[0] = AccessPermission.Permission({
            ipAccount: ipId,
            signer: address(spg),
            to: licensingModuleAddr,
            func: bytes4(0),
            permission: AccessPermission.ALLOW
        });
        permissionList[1] = AccessPermission.Permission({
            ipAccount: ipId,
            signer: address(spg),
            to: ipResolverAddr,
            func: bytes4(0),
            permission: AccessPermission.ALLOW
        });
        bytes32 digest = MessageHashUtils.toTypedDataHash(
            MetaTx.calculateDomainSeparator(ipId),
            MetaTx.getExecuteStructHash(
                MetaTx.Execute({
                    to: accessControllerAddr,
                    value: 0,
                    data: abi.encodeWithSignature(
                        "setBatchPermissions((address,address,address,bytes4,uint8)[])",
                        permissionList
                    ),
                    nonce: 1,
                    deadline: deadline
                })
            )
        );

        (uint8 v, bytes32 r, bytes32 s) = vm.sign(alicePk, digest);
        bytes memory signature = abi.encodePacked(r, s, v);
```

### 

### IMPORTANT

In the current version of Story Protocol, `v0.1-beta`, all metadata is assumed to be immutable upon registration. Note that this will likely change in the future.