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

| Method    | Type                                                    |
| --------- | ------------------------------------------------------- |
| `approve` | `(request: ApproveRequest) => Promise<{ txHash: Hex }>` |

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