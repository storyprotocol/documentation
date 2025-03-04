---
title: WIP Client
excerpt: Used to handle the wrapping/unwrapping of WIP.
deprecated: false
hidden: false
metadata:
  robots: index
---
## WipClient

### Methods

* deposit
* withdraw
* approve
* balanceOf
* transfer
* transferFrom

### deposit

Wraps the selected amount of IP to WIP. The WIP will be deposited to the wallet that transferred the IP.

| Method    | Type                        |
| --------- | --------------------------- |
| `deposit` | `(request: DepositRequest)` |

Parameters:

* `request.amount`: The amount to deposit.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type DepositRequest = WithTxOptions & {
  amount: TokenAmountInput;
};
```

### withdraw

Unwraps the selected amount of WIP to IP.

| Method     | Type                         |
| ---------- | ---------------------------- |
| `withdraw` | `(request: WithdrawRequest)` |

Parameters:

* `request.amount`: The amount to withdraw.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type WithdrawRequest = WithTxOptions & {
  amount: TokenAmountInput;
};
```

### approve

Approve a spender to use the wallet's WIP balance.

| Method    | Type                        |
| --------- | --------------------------- |
| `approve` | `(request: ApproveRequest)` |

Parameters:

* `request.amount`: The amount of WIP tokens to approve.
* `request.spender`: The address that will use the WIP tokens
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type ApproveRequest = WithTxOptions & {
  spender: Address;
  amount: TokenAmountInput;
};
```

### balanceOf

Returns the balance of WIP for an address.

| Method      | Type                                 |
| ----------- | ------------------------------------ |
| `balanceOf` | `(addr: Address) => Promise<bigint>` |

Parameters:

* `addr`: The address you want to check the baalnce for.

### transfer

Transfers `amount` of WIP to a recipient `to`.

| Method     | Type                         |
| ---------- | ---------------------------- |
| `transfer` | `(request: TransferRequest)` |

Parameters:

* `request.to`: Who you're transferring to.
* `request.amount`: The amount to transfer.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type TransferRequest = WithTxOptions & {
  to: Address;
  amount: TokenAmountInput;
};
```

### transferFrom

Transfers `amount` of WIP from `from` to a recipient `to`.

| Method         | Type                             |
| -------------- | -------------------------------- |
| `transferFrom` | `(request: TransferFromRequest)` |

Parameters:

* `request.to`: Who you're transferring to.
* `request.amount`: The amount to transfer.
* `request.from`: The address to transfer from.
* `request.txOptions`: \[Optional] The transaction [options](https://github.com/storyprotocol/sdk/blob/main/packages/core-sdk/src/types/options.ts).

```typescript Request Type
export type TransferFromRequest = WithTxOptions & {
  to: Address;
  amount: TokenAmountInput;
  from: Address;
};
```