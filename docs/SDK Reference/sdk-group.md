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
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
* `request.recipient`: \[Optional] The address of the recipient of the minted NFT,default value is your wallet address.
* `request.licenseTemplate`: \[Optional] The address of the license template to be attached to the new group IP,default value is Programmable IP License.
* `request.deadline`: \[Optional] The deadline for the signature in milliseconds,default value is 1000ms.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
* `request.licenseTemplate`: \[Optional] The address of the license template to be attached to the new group IP, default value is Programmable IP License.
* `request.deadline`: \[Optional] The deadline for the signature in milliseconds, default is 1000ms.
* `request.ipMetadata`: \[Optional] The desired metadata for the newly minted NFT and newly registered IP.
  * `request.ipMetadata.ipMetadataURI` \[Optional] The URI of the metadata for the IP.
  * `request.ipMetadata.ipMetadataHash` \[Optional] The hash of the metadata for the IP.
  * `request.ipMetadata.nftMetadataURI` \[Optional] The URI of the metadata for the NFT.
  * `request.ipMetadata.nftMetadataHash` \[Optional] The hash of the metadata for the IP NFT.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
* `request.licenseTemplate`: \[Optional] The address of the license template to be attached to the new group IP, default value is Programmable IP License.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

* `request.ipIds`: The IP IDs of the IPs to be added to the group.
* `request.groupPool`: The address specifying how royalty will be split amongst the pool of IPs in the group.
* `request.maxAllowedRevShare`: The maximum reward share percentage that can be allocated to each member IP.
* `request.licenseData`: The data of the license and its configuration to be attached to the new group IP.
  * `request.licenseData.licenseTermsId`: The ID of the registered license terms that will be attached to the new group IP.
  * `request.licenseData.licensingConfig`: See the [LicensingConfig type](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/common.ts#L15).
  * `request.licenseData.licenseTemplate`: \[Optional] The address of the license template to be attached to the new group IP, default value is Programmable IP License.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const response = await client.groupClient.registerGroupAndAttachLicenseAndAddIps({
  groupPool: "0xf96f2c30b41Cb6e0290de43C8528ae83d4f33F89", // EvenSplitGroupPool from https://docs.story.foundation/docs/deployed-smart-contracts
  maxAllowedRewardShare: 5,
  ipIds: ['0x01'],
  licenseData: {
    licenseTermsId: '5',
    licensingConfig: {
      isSet: false,
      mintingFee: 0n,
      licensingHook: zeroAddress,
      hookData: zeroAddress,
      commercialRevShare: 0,
      disabled: false,
      expectMinimumGroupRewardShare: 0,
      expectGroupRewardPool: zeroAddress,
    },
  },
  txOptions: { waitForTransaction: true },
});
```
```typescript Request Type
export type RegisterGroupAndAttachLicenseAndAddIpsRequest = {
  groupPool: Address;
  ipIds: Address[];
  licenseData: LicenseData;
  maxAllowedRewardShare: number | string;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RegisterGroupAndAttachLicenseAndAddIpsResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  groupId?: Address;
};
```