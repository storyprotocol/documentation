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
* `request.data`: [Optional] The data to initialize the policy.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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

* `request.disputeId`: The ID of the dispute to be canceled.
* `request.data`: [Optional] Additional data used in the cancellation process.
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

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
* `request.txOptions`: [Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Response Type
export type ResolveDisputeResponse = {
  txHash?: string;
  encodedTxData?: EncodedTxData;
};
```
