---
title: staking
deprecated: false
hidden: false
metadata:
  robots: index
---
---
title: staking
excerpt: ""
deprecated: false
hidden: false
metadata:
  title: ""
  description: ""
  robots: index
next:
  description: ""
---

## Abstrat

The staking module has been modified to accommodate for the following changes below. Refer to the Cosmos SDK's [staking module docs](https://docs.cosmos.network/main/build/modules/staking) for more information.

## Reward Multiplier

### Validators

Validators can choose to accept either locked tokens or unlocked tokens as delegations. Validators for locked tokens are conditioned to half the inflation allocation of validators for unlocked tokens.

Since each validator receives different inflation distribution based on delegations, the inflation distribution I_v_i for validator v_i in the rewards pool is calculated as follows:


where

* I_v_i is the total inflationary token rewards for v_i
* S_v_i is the staked tokens for v_i
* M_v_i is the rewards multiplier for v_i
  * 0.5 for locked tokens
  * 1 for unlocked tokens
* R_n is the total inflationary tokens allocated for the rewards pool in block n, calculated in the [mint](./mint-module.md) module

### Delegations

Delegators can delegate with four different staking lock times, which results in different staking reward multiplier for each delegation (delegator-validator pair of stakes). The inflation distribution for each delegation $D_i$ is calculated as follows:



where

* S_d_i is the staked tokens of delegation d_i on validator v_d
* M_d_i is the rewards multiplier of d_i on v_d
* I_v is the total inflationary token rewards for v_d
* C_v is the commission rate for v_d

#### Time-weighted Reward Multiplier M_d_i

* _Flexible_ (no lockup): 1
* _Short_ (90 days): 1.1
* _Medium_ (360 days): 1.5
* _Long_ (540 days): 2.0