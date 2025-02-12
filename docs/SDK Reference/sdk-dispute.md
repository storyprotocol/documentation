---
title: Dispute
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
## DisputeClient

### Methods

* raiseDispute
* cancelDispute
* resolveDispute

### raiseDispute

Raises a dispute on a given ipId

| Method         | Type                                                              |
| -------------- | ----------------------------------------------------------------- |
| `raiseDispute` | `(request: RaiseDisputeRequest) => Promise<RaiseDisputeResponse>` |

Parameters:

* `request.targetIpId`: The IP ID that is the target of the dispute.
* `request.targetTag`: The target tag of the dispute.
* `request.cid`: CID (Content Identifier) is a unique identifier in IPFS, including CID v0 (base58) and CID v1 (base32).
* `request.data`: \[Optional] The data to initialize the policy.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const disputeResponse = await client.dispute.raiseDispute({
  targetIpId: '0xC92EC2f4c86458AFee7DD9EB5d8c57920BfCD0Ba',
  // NOTE: you must use your own CID here, because every time it is used,
  // the protocol does not allow you to use it again
  cid: 'QmbWqxBEKC3P8tqsKc98xmWNzrzDtRLMiMPL8wBuTGsMnR',
  // you must pick from one of the whitelisted tags here: 
  // https://docs.story.foundation/docs/dispute-module#/dispute-tags
  targetTag: 'IMPROPER_REGISTRATION',
  bond: 0,
  liveness: 2592000,
  txOptions: { waitForTransaction: true },
})
console.log(`Dispute raised at transaction hash ${disputeResponse.txHash}, Dispute ID: ${disputeResponse.disputeId}`)
```
```typescript Request Type
export type RaiseDisputeRequest = {
  targetIpId: Address;
  cid: string;
  targetTag: string;
  liveness: bigint | number | string;
  bond: bigint | number | string;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type RaiseDisputeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
  disputeId?: bigint;
};
```

### cancelDispute

Cancels an ongoing dispute

| Method          | Type                                                                |
| --------------- | ------------------------------------------------------------------- |
| `cancelDispute` | `(request: CancelDisputeRequest) => Promise<CancelDisputeResponse>` |

Parameters:

* `request.disputeId`: The ID of the dispute to be cancelled.
* `request.data`: \[Optional] Additional data used in the cancellation process.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type CancelDisputeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```

### resolveDispute

Resolves a dispute after it has been judged

| Method           | Type                                                                  |
| ---------------- | --------------------------------------------------------------------- |
| `resolveDispute` | `(request: ResolveDisputeRequest) => Promise<ResolveDisputeResponse>` |

Parameters:

* `request.disputeId`: The ID of the dispute to be resolved.
* `request.data`: The data to resolve the dispute.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type ResolveDisputeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```