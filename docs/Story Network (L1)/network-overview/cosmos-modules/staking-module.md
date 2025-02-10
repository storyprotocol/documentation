---
title: staking
deprecated: false
hidden: false
metadata:
  robots: index
---
## Abstrat

The staking module has been modified to accommodate for the following changes below. Refer to the Cosmos SDK's [staking module docs](https://docs.cosmos.network/main/build/modules/staking) for more information.

## Reward Multiplier

### Validators

Validators can choose to accept either locked tokens or unlocked tokens as delegations. Validators for locked tokens are conditioned to half the inflation allocation of validators for unlocked tokens.

Since each validator receives different inflation distribution based on delegations, the inflation distribution I\_v\_i for validator v\_i in the rewards pool is calculated as follows:

<Image align="center" src="https://files.readme.io/3ee4914a7cc6036ceebbdd31ce93e525984a08364f8c3ab2152b86b3bcd5df7e-Screenshot_2025-02-11_at_8.30.07_AM.png" />

where

* I\_v\_i is the total inflationary token rewards for v\_i
* S\_v\_i is the staked tokens for v\_i
* M\_v\_i is the rewards multiplier for v\_i
  * 0.5 for locked tokens
  * 1 for unlocked tokens
* R\_n is the total inflationary tokens allocated for the rewards pool in block n, calculated in the [mint](./mint-module.md) module

### Delegations

Delegators can delegate with four different staking lock times, which results in different staking reward multiplier for each delegation (delegator-validator pair of stakes). The inflation distribution for each delegation $D\_i$ is calculated as follows:

<Image align="center" src="https://files.readme.io/002ae69aa3b3e52a33747452fe0c0b91b9120f20155deb19b56fb7917132b8de-Screenshot_2025-02-11_at_8.34.44_AM.png" />

where

* S\_d\_i is the staked tokens of delegation d\_i on validator v\_d
* M\_d\_i is the rewards multiplier of d\_i on v\_d
* I\_v is the total inflationary token rewards for v\_d
* C\_v is the commission rate for v\_d

#### Time-weighted Reward Multiplier M\_d\_i

* *Flexible* (no lockup): 1
* *Short* (90 days): 1.1
* *Medium* (360 days): 1.5
* *Long* (540 days): 2.0