---
title: Group
excerpt: GroupClient allows you to create groups and add IP Assets to them.
deprecated: false
hidden: false
metadata:
  title: ''
  description: ''
  robots: index
next:
  description: ''
---
## GroupClient

### Methods

* registerGroup
* mintAndRegisterIpAndAttachLicenseAndAddToGroup
* registerIpAndAttachLicenseAndAddToGroup
* registerGroupAndAttachLicense
* registerGroupAndAttachLicenseAndAddIps

### registerGroup

Registers a Group IPA.

| Method          | Type                                                                |
| --------------- | ------------------------------------------------------------------- |
| `registerGroup` | `(request: RegisterGroupRequest) => Promise<RegisterGroupResponse>` |

Parameters:

* `request.groupPool`: The address specifying how royalty will be split amongst the pool of IPs in the group.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterGroupResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  groupId?: Address;
};
```

### mintAndRegisterIpAndAttachLicenseAndAddToGroup

Mint an NFT from a SPGNFT collection, register it with metadata as an IP, attach license terms to the registered IP, and add it to a group IP.

| Method                                           | Type                                                                                                                                  |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------- |
| `mintAndRegisterIpAndAttachLicenseAndAddToGroup` | `(request: MintAndRegisterIpAndAttachLicenseAndAddToGroupRequest) => Promise<MintAndRegisterIpAndAttachLicenseAndAddToGroupResponse>` |

Parameters:

* `request.nftContract`: The address of the NFT collection.
* `request.groupId`: The ID of the group IP to add the newly registered IP.
* `request.licenseTermsId`: The ID of the registered license terms that will be attached to the new IP.
* `request.recipient`: [Optional] The address of the recipient of the minted NFT,default value is your wallet address.
* `request.licenseTemplate`: [Optional] The address of the license template to be attached to the new group IP,default value is Programmable IP License.
* `request.deadline`: [Optional] The deadline for the signature in milliseconds,default value is 1000ms.
* `request.ipMetadata`: [Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` [Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` [Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` [Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` [Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type MintAndRegisterIpAndAttachLicenseAndAddToGroupResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
};
```

### registerIpAndAttachLicenseAndAddToGroup

Register an NFT as IP with metadata, attach license terms to the registered IP, and add it to a group IP.

| Method                                    | Type                                                                                                                    |
| ----------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| `registerIpAndAttachLicenseAndAddToGroup` | `(request: RegisterIpAndAttachLicenseAndAddToGroupRequest) => Promise<RegisterIpAndAttachLicenseAndAddToGroupResponse>` |

Parameters:

* `request.spgNftContract`: The address of the NFT collection.
* `request.tokenId`: The ID of the NFT.
* `request.groupId`: The ID of the group IP to add the newly registered IP.
* `request.licenseTermsId`: The ID of the registered license terms that will be attached to the new IP.
* `request.licenseTemplate`: [Optional] The address of the license template to be attached to the new group IP, default value is Programmable IP License.
* `request.deadline`: [Optional] The deadline for the signature in milliseconds, default is 1000ms.
* `request.ipMetadata`: [Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` [Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` [Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` [Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` [Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterIpAndAttachLicenseAndAddToGroupResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  ipId?: Address;
  tokenId?: bigint;
};
```

### registerGroupAndAttachLicense

Register a group IP with a group reward pool and attach license terms to the group IP.

| Method                          | Type                                                                                                |
| ------------------------------- | --------------------------------------------------------------------------------------------------- |
| `registerGroupAndAttachLicense` | `(request: RegisterGroupAndAttachLicenseRequest) => Promise<RegisterGroupAndAttachLicenseResponse>` |

Parameters:

* `request.groupPool`: The address specifying how royalty will be split amongst the pool of IPs in the group.
* `request.licenseTermsId`: The ID of the registered license terms that will be attached to the new group IP.
* `request.licenseTemplate`: [Optional] The address of the license template to be attached to the new group IP, default value is Programmable IP License.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterGroupAndAttachLicenseResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  groupId?: Address;
};
```

### registerGroupAndAttachLicenseAndAddIps

Register a group IP with a group reward pool, attach license terms to the group IP, and add individual IPs to the group IP.

| Method                                   | Type                                                                                                                  |
| ---------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| `registerGroupAndAttachLicenseAndAddIps` | `(request: RegisterGroupAndAttachLicenseAndAddIpsRequest) => Promise<RegisterGroupAndAttachLicenseAndAddIpsResponse>` |

Parameters:

* `request.pIds`: must have the same PIL terms as the group IP.
* `request.groupPool`: The address specifying how royalty will be split amongst the pool of IPs in the group.
* `request.licenseTermsId`: The ID of the registered license terms that will be attached to the new group IP.
* `request.licenseTemplate`: [Optional] The address of the license template to be attached to the new group IP,default value is Programmable IP License.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type RegisterGroupAndAttachLicenseAndAddIpsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  groupId?: Address;
};
```
