---
title: staking-module
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

Since each validator receives different inflation distribution based on delegations, the inflation distribution $I_v_i$ for validator $v_i$ in the rewards pool is calculated as follows:
