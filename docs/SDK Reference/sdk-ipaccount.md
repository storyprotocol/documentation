---
title: IP Account
deprecated: false
hidden: false
metadata:
  robots: index
---
## IPAccountClient

### Methods

* setIpMetadata
* execute
* executeWithSig
* transferErc20

### setIpMetadata

Sets the metadataURI for an IP asset.

| Method          | Type                                     |
| --------------- | ---------------------------------------- |
| `setIpMetadata` | `(SetIpMetadataRequest) => Promise<Hex>` |

Parameters:

* `request.ipId`: The IP to set the metadata for.
* `request.metadataURI`: The metadataURI to set for the IP asset. Should be a URL pointing to metadata that fits the [📝 IPA Metadata Standard](doc:ipa-metadata-standard).
* `request.metadataHash`: The hash of metadata at metadataURI.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript TypeScript
const txHash = await client.ipAccount.setIpMetadata({
  ipId: "0x01",
  metadataURI: "https://ipfs.io/ipfs/bafkreiardkgvkejqnnkdqp4pamkx2e5bs4lzus5trrw3hgmoa7dlbb6foe",
  // example hash (not accurate)
  metadataHash: "0x129f7dd802200f096221dd89d5b086e4bd3ad6eafb378a0c75e3b04fc375f997",
});
```
```typescript Request Type
export type SetIpMetadataRequest = {
  ipId: Address;
  metadataURI: string;
  metadataHash: Hex;
  txOptions?: Omit<TxOptions, "encodedTxDataOnly">;
};
```

### execute

Executes a transaction from the IP Account.

| Method    | Type                                                             |
| --------- | ---------------------------------------------------------------- |
| `execute` | `(IPAccountExecuteRequest) => Promise<IPAccountExecuteResponse>` |

Parameters:

* `request.ipId`: The Ip Id to get ip account.
* `request.to`: The recipient of the transaction.
* `request.value`: The amount of Ether to send.
* `request.accountAddress`: The ipId to send.
* `request.data`: The data to send along with the transaction.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type IPAccountExecuteRequest = {
  ipId: Address;
  to: Address;
  value: number;
  data: Address;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type IPAccountExecuteResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
};
```

### executeWithSig

Executes a transaction from the IP Account.

| Method           | Type                                                             |
| ---------------- | ---------------------------------------------------------------- |
| `executeWithSig` | `(IPAccountExecuteRequest) => Promise<IPAccountExecuteResponse>` |

Parameters:

* `request.ipId`: The Ip Id to get ip account.
* `request.to`: The recipient of the transaction.
* `request.data`: The data to send along with the transaction.
* `request.signer`: The signer of the transaction.
* `request.deadline`: The deadline of the transaction signature.
* `request.signature`: The signature of the transaction, EIP-712 encoded.
* `request.value`: \[Optional] The amount of Ether to send.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type IPAccountExecuteWithSigRequest = {
  ipId: Address;
  to: Address;
  data: Address;
  signer: Address;
  deadline: number | bigint | string;
  signature: Address;
  value?: number | bigint | string;
  txOptions?: TxOptions;
};
```
```typescript Response Type
export type IPAccountExecuteWithSigResponse = {
  txHash?: Hex;
  encodedTxData?: EncodedTxData;
};
```

### transferErc20

Transfers ERC20 tokens from the IP Account to the target address.

| Method          | Type                                                     |
| --------------- | -------------------------------------------------------- |
| `transferErc20` | `(TransferErc20Request) => Promise<TransactionResponse>` |

Parameters:

* `request.ipId`: The `ipId` of the account
* `request.tokens`: The token info to transfer
  * `request.tokens.address`: The address of the ERC20 token including WIP and standard ERC20.
  * `request.tokens.amount`: The amount of tokens to transfer
  * `request.tokens.target`: The address of the recipient.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type TransferErc20Request = {
  ipId: Address;
  tokens: {
    address: Address;
    amount: bigint | number;
    target: Address;
  }[];
  txOptions?: Omit<TxOptions, "encodedTxDataOnly">;
};
```
```typescript Response Type
export type TransactionResponse = {
  txHash: Hex;

  /** Transaction receipt, only available if waitForTransaction is set to true */
  receipt?: TransactionReceipt;
};
```