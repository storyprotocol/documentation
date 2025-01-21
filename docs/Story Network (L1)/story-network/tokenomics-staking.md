---
title: Tokenomics & Staking
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
# Purpose

This document walks through the staking specification for Story. The goal is to provide clarity to network participants and technical partners on how Story’s staking mechanics work and how users can interface with our chain.

# Tokenomics

## Genesis

The story genesis allocation will consist of 1 billion tokens, distributed among ecosystem participants, the foundation, investors, and the core team. Tokens for investors, the core team, and a portion of those of the foundation and ecosystem will start out locked.

## Locked vs Unlocked tokens

Unlocked tokens have no restrictions imposed on them and can be used for gas consumption, transfers, and staking.

Unlike unlocked tokens, locked tokens cannot be transferred or traded and are unlocked based on an unlock schedule. However, locked tokens may be staked to earn staking rewards, with the locked staking reward rate being half of that of unlocked tokens.

Staked locked and unlocked tokens have the same voting power. That means that a validator with 100 staked locked tokens has the same network voting power as a validator with 100 staked unlocked tokens.

Both types of tokens can be slashed if their validators get slashed.

## Token emissions

A fixed number of tokens will be allocated for emissions in the first year, with the quantity determined by the foundation at Genesis. For subsequent years, the number of emitted tokens will be controlled by an emissions algorithm whose parameters may be updated via governance or subject to change via hard forks. The emissions per block are controlled by the following two parameters, whose initial values are still yet to be determined:

* blocks\_per\_year
  * The number of blocks expected to be produced in a year
* inflations\_per\_year
  * The total number of inflationary tokens to be emitted in a year

New emissions will flow to two places:

1. Block Rewards
2. UBI (currently set to 0, explained later)

## Token burn

Since story uses a fork of geth as the execution client, the burning mechanism follows Ethereum’s EIP-1559.

# Staking

> 🔗 <a href="https://staking.story.foundation/" target="_blank">Go to the Staking Dashboard ↗️</a>

Story supports the below staking-related operations

* Create validator
* Update validator commission
* Stake
* Stake on behalf
* Unstake
* Unstake on behalf
* Redelegate
* Redelegate on behalf
* Set withdraw address
* Set reward address
* Unjail
* Unjail on behalf

Before explaining the behavior of each of these operations, some high-level concepts like **Token Staking Types**, **Validator Set Status**, **Unbonding**, and **Staking Period** will be explained first:

## Token Staking Types

As staking is enabled for both locked and unlocked tokens, validators must choose which type of token staking they want to support. Once a token staking type is selected, validators cannot switch to a different type.

## Validator Set Status

In Story, validators are grouped into one of two sets, (1) the active (bonded) validator set, which participates in consensus and receives block rewards, or (2) the non-active (unbonded) validator set, which does not contribute to the consensus process. To be selected as part of the active validator set, a validator must be one of the top 64 validators ranked by staked tokens.

## Unbonding

Unstaking for delegators is subject to an unbonding process. Users must wait for an unbonding time before any tokens return to their accounts.

This is the same for validators who self-delegate to themselves. They also need to go through the unbonding process when they want to unstake.

The unbonding time is 14 days. During the unbonding period, the delegator/validator will not earn block rewards. But they may still be slashed.

For each validator/delegator pair, the maximum ongoing unbonding transactions is 14. More unbonding requests beyond this limit will fail.

## Staking period

Delegators can decide how flexible and how long they want to stake their tokens. By default, for both locked and unlocked tokens, delegators can stake and then unstake immediately and get their token back after the unbonding time. We call this **flexible staking** in this document.

For unlocked tokens, a few more fixed staking periods are supported: 90 days, 360 days, and 540 days. In this case, users can only call unstake after the staking period is mature. Any call earlier than the mature day will be discarded. Unstaking from a mature staking period is still subject to the unbonding process, meaning users will get their staked tokens back after 14 days of unbonding time.

For locked tokens, only flexible staking is allowed. If a user delegates their locked tokens to a staking period, we will convert that to a flexible staking delegation.

Staking in these fixed staking periods earns more rewards. The longer the period, the bigger the reward weight multiplier. Reward multiplier for different periods:

* 90 days - 1.051
* 360 days - 1.16
* 540 days - 1.34

After the staking period ends, users can choose not to unstake. In this case, they will continue earning the same reward rate based on the reward rate of the corresponding staking period until they unstake manually. They can unstake at any time after the staking period ends. For example, if the 1-year staking period’s reward rate is 0.02% per block, after staking for 1 year, users can still earn 0.02% per block of the reward until they unstake.

# Staking Operations

## Create validator

To become a validator, the validator must first run a validator node based on the latest released story binaries, then call the CreateValidator function with an initial staking amount, moniker, and commission rate. It also needs to set the max commission rate and max commission rate change to make sure it doesn’t change the commission rate later dramatically. The minimum commission rate that a validator can set is 5%.

The initial staking amount needs to be larger than a threshold, which is 1024 IP. The amount will be deducted from the caller’s wallet. It can only be staked to a flexible period.

If a validator tries to call create validator function the second time, it will be ignored.

## Update validator commission

This operation allows validators to edit their validator commission rate. If the updated commission rate is larger than max commission rate or the commission rate change delta is larger than max commission rate change, the operation will fail.

A fee of 1 IP will be charged for updating a validator to prevent spamming. The fee will be burnt by the contract.

The commission rate can only be updated once per day. It will not throw an error from the contract. But it won’t take effect in the consensus layer.

## Stake

Both the validator and delegator can stake tokens to a validator. A validator can stake to itself, which is called self-delegation. Users can decide if they want to stake with a fixed staking period or stake without a period (flexible staking).

If a fixed period is chosen, a delegation id will be returned to the users. Users must use this delegation id to unstake tokens from this stake operation. If flexible staking is chosen, the returned delegation id will be 0.

The staking amount needs to be larger than a threshold, which is 1024 IP.

If a delegator delegates to a non-existent validator, the tokens will NOT be refunded.

## Unstake

When staking without a staking period, users can unstake anytime. The tokens will be distributed to the user’s account after the unbonding time.

A fee of 1 IP will be charged for unstaking to prevent spamming. The fee will be burnt by the contract.

When staking with a staking period, users can only unstake after the staking period is mature. The tokens will be distributed to the user’s account after the unbonding time. Unstaking requests before the staking period matures will be ignored.

The minimum unstaking amount is 1024 IP. After the unstaking request is processed, if the remaining staked amount is less than 1024 IP, the remaining part will also be unstaked together.

The unstaking request will first go through the unbonding process, which is 14 days. After that, the unbonded requests are sent to a withdrawal queue, distributing a maximum of 32 withdrawals per block. If there are more than 32 withdrawal requests in the withdrawal queue, the next 32 withdrawal requests will be processed in the next block.

Partial unstake of a delegation is supported. For example, if a 1-year long delegation has 1 million tokens, after 1 year, users can unstake 500k from this delegation and keep the remaining staked to continue earning rewards.

Unstake can fail if the validator, delegator and delegation id passed in is incorrect.

Unstake can also fail if the maximum concurrent unbonding request (currently 14) has been reached for the validator/delegator pair.

If the unstake amount passed in is larger than the total unstakable tokens, the current total unstakable amounts will be unstaked. For example, if users unstake 1024 IP and only have 1023 IP stake, 1023 IP will be withdrawn.

If a validator exits, by either being offline and getting jailed, or not having enough stakes to be in the top 64 validator set, the delegators can unstake their tokens if the tokens are not in a staking period or their staking period is mature. Otherwise, delegators must wait until the staking period matures to unstake.

## Redelegate

Redelegate operation allows a delegator to move its staked tokens from one validator to another. The tokens can be redelegated to the new validator immediately and start earning rewards. However, the redelegated tokens are still subject to the unbonding process, IF the source validator is in the active validator set or unbonding from the active validator set. During this 14 days unbounding time, it will be slashed if the original validator gets slashed.

A fee of 1 IP will be charged for redelegation to prevent spamming. The fee will be burnt by the contract.

The minimum redelegation amount is 1024 IP. If a delegator’s initial stake is 1024 IP but later gets slashed, it can still redelegate its tokens to another validator even if the token amount is less than 1024 IP.

Similarly to unstaking, if the redelegation amount passed in is larger than the total redelegatable tokens, the total redelegatable amounts will be redelegated. If the remaining balance after redelegation is less than 1024 IP, all remaining tokens will be redelegated together.

The delegation id will stay the same after the redelegation.

Redelegation has its own maximum ongoing unbonding transaction limit per delegator/source validator/destination validator pair, which is also 14.

Delegators can choose to redelegate their tokens to another active validator even if their tokens are still in an immature staking period. Their staking period maturation date and reward rate will stay the same.

Redelegation can only be triggered when the source and destination validators support the same token type.

## Set withdrawal/reward address

Delegators can call the staking contract to set a withdrawal address. The unstaked tokens will be sent to this withdrawal address. Similarly, delegators can set a separate reward address. All reward distributions will be sent to this address.

A fee of 1 IP will be charged for updating either the withdrawal address or the reward address to prevent spamming. The fee will be burnt by the contract.

The address change will take effect in the next block.

## Slash/Unjail

Slashing penalizes bad behaviors on the validators by slashing out a fraction of their staked tokens. Two types of behaviors can get slashed in Story: **double sign** and **downtime**.

* **double sign**: If a validator double signs for a block, they will get slashed 5% of their tokens and get permanently jailed (called tombstoned).
* **downtime**: If a validator is offline for too long and misses 95% of the past 28,800 blocks, they will get slashed 0.02% of their tokens and get jailed.

A validator will also get jailed after self-undelegation if the validator’s remaining self-delegation amount is smaller than the minimum self-delegation (1024 IP).

A jailed validator cannot participate in the consensus and earn any reward. But they can unjail themselves after a cooldown time, which is currently set to 10 minutes. After 10 minutes, it can call story’s staking contract to unjail itself IF their stake is more than minimum stake amount (1024 IP), after which it can participate in the consensus again if it’s still within the top 64 validators.

A jailed validator can still withdraw all their stakes.

Delegators can still stake and unstake from a jailed validator as long as there are remaining stakes on this jailed validator. The jailed validator will only be removed from the chain (hence not able to be staked/unstaked) when there is no remaining stake on it.

A fee of 1 IP will be charged for unjailing a validator to prevent spamming. The fee will be burnt by the contract.

## On behalf functions

Most of the staking-related operations can be done from another wallet on behalf of the validators or delegators. Most of these on-behalf functions are permissionless since they spend tokens from the wallet that calls the on-behalf operations, not from the actual validators or delegators.

## Add operator

If a delegator wants to allow another wallet to unstake or redelegate on their behalf, they must call the staking contract to add that wallet as the operator for their delegator. After that, the operator can unstake and redelegate the delegator’s tokens on behalf of the delegator.

The same applies to a validator who wants to allow another wallet to unjail on its behalf.

A fee of 1 IP will be charged for adding an operator.

## An additional data field

Each function will include an additional unformatted `data` input field to accommodate potential future changes. It can avoid changing user interfaces in the future.

## Validator and delegator key format

Validator and delegator public keys are secp256k1 keys. The keys have a 33 bytes compressed version and 65 bytes uncompressed version. When interacting with the story's smart contracts, a 65 bytes uncompressed key is used to identify validators and delegators. When using story’s staking APIs, the bech32 address that can be derived from the compressed key is used.

# Rewards

## Rewards Pool Allocation

For every block, a fixed proportion of token inflation will go to the rewards distribution pool, which will be shared among all 64 active validators according to each of their share weights. *These allocated tokens will then be shared among the validator and its delegators in a fashion described by the next section.* The validator share weight is calculated based on the total token staking amount, and whether or not the token staking type is locked or unlocked.

As an example, assume that we have 100 tokens allocated for the validator rewards distribution pool, and assume that we only have 3 active validators:

* validatorA with 10 locked tokens staked
* validatorB with 10 locked tokens staked
* validatorC with 10 unlocked tokens staked

To calculate how many tokens each validator receives, we first calculate each of their weighted shares, which is defined as the number of staked tokens multiplied by their rewards multiplier (0.5 if staking locked tokens, 1 if staking unlocked tokens). This gives us:

* validatorA with 10 \* 0.5 = 5 shares
* validatorB with 10 \* 0.5 = 5 shares
* validatorC with 10 \* 1 = 10 shares

With the weighted and total shares calculated, we can then get the total number of inflationary tokens allocated for each validator:

* validatorA with 100 \* (5 / 20) = 25 tokens
* validatorB with 100 \* (5 / 20) = 25 tokens
* validatorC with 100 \* (10 / 20) = 50 tokens

The formula for calculating the total number of tokens allocated for a validator is as follows:

<Image align="center" src="https://files.readme.io/833d419fc139ba363c56aef263dcca571fe449ab824a2349a69d7419ee658bd0-Screenshot_2024-10-30_at_8.13.27_PM.png" />

where

* R\_i is the total inflationary token rewards for validator i
* S\_i is the staked tokens for validator i
* M\_i is the rewards multiplier (0.5 for locked tokens, 1 for unlocked tokens)
* R\_total is the total inflationary tokens allocated for the rewards pool

## Validator And Delegator Rewards

Total rewards allocations (*whose calculations are shown in the prior section*) for each validator are shared between the validator itself and all of its delegators:

* The validator takes a fixed percentage commission, set by the validator itself
* Remaining rewards are distributed among delegators according to their share weights

Calculation of delegator rewards is similar to that of validator rewards, where the proportion of tokens received for each delegator out of the remaining validator rewards is calculated based on each delegator’s staking multiplier (described in the staking section).

As an example, assume a validator has 100 total rewards allocated to it, with a validator commission of 20%, and 3 delegators delegating to it:

* delegatorA with 10 tokens staked and a staking multiplier of 1
* delegatorB with 10 tokens staked and a staking multiplier of 1
* delegatorC with 10 tokens staked and a staking multiplier of 2

To calculate how many tokens each delegator receives, we first calculate each of their weighted shares, which is defined as the number of staked tokens multiplied by their staking rewards multiplier. This gives us:

* delegatorA with 10 \* 1 = 10 shares
* delegatorB with 10 \* 1 = 10 shares
* delegatorC with 10 \* 2 = 20 shares

With the weighted and total shares calculated, we can then get the total number of inflationary tokens allocated for each delegator, noting that the total number of tokens to be distributed among delegators is give by 100 - (100 \* 0.20) = 80:

* delegatorA with 80 \* (10 / 40) = 20 tokens
* delegatorB with 80 \* (10 / 40) = 20 tokens
* delegatorC with 80 \* (20 / 40) = 40 tokens

The formula for calculating the delegator token reward can be found below:

<Image align="center" src="https://files.readme.io/429c0eff2f0acddcabfa3e6259e427b47156aed244020bfb7f11a5b63387fec9-Screenshot_2024-10-30_at_8.15.51_PM.png" />

where

* D\_i is the total inflationary token rewards for delegator i
* S\_i is the staked tokens for delegator i
* M\_i is the staked rewards multiplier for delegator i
* R\_total is the total inflationary tokens allocated for the validator
* C is the commission rate for the validator

The validator commission is also treated as a reward and will follow the same auto-reward distribution rule described below. The minimal validator commission is set to 5% to avoid a cut-throat competition of lower commission rates among validators.

The reward calculation results will be rounded down to gwei. Anything smaller than 1 gwei will be truncated.

## Auto reward distribution

The reward is accumulated per block and can be distributed per block. However, it will only be automatically distributed to the delegator’s account when it is larger than a threshold. The default and also minimal threshold is 8 IP, which means that only if the delegator’s reward is more than 8 IP, it will be sent to the delegator’s account.

The reward distribution will go to a reward distribution queue, which only processes a fixed amount of reward distribution requests per block. The reward distribution per block is 32.

The staking reward cannot be manually withdrawn by design.

# UBI for validators

In every block, a percentage (currently 0% at Genesis) of the newly minted tokens will go to a UBI pool contract. The pool is to incentivize validators to validate the blocks. At the beginning of each month, the foundation will set the UBI percentage for the next month based on the token price. The maximum UBI percentage that can be set is 20%.

## Distribution process

Every month, the story foundation will get the validator consensus participation rate based on the on-chain metrics for the previous month and calculate how many UBI tokens each validator can claim and set this in the UBI pool contract. Each validator then can claim the token from the UBI pool contract.

The UBI calculation and claim process shall be verifiable by the public.

The UBI contract address: **0xcccccc0000000000000000000000000000000002**

# Singularity

The first 1,580,851 blocks after the genesis is called Singularity, during which everyone can create a validator and stake tokens but the active validator set will only have the genesis validators. There is also no new token emission, hence no reward. Unstake and redelegate are also not supported.

The Genesis validator set consists of 8 validators, setup by the foundation and trusted staking institutions. 4 of them support locked tokens and the other 4 support unlocked tokens. Each of them has an initial stake of 0.001 IP. Each of them will set a commission rate. During the Singularity, the genesis valdiators will need to self delegate at least 1024 IP to perform validator operations like editing validator commission rate.

After Singularity, the top 64 validator nodes with the highest stakes will be selected to participate in consensus and receive rewards.

Slashing/Jail won’t happen during Singularity.

# Staking contract

Story’s staking contract will handle all validators/delegators related operations. It’s deployed to address: **0xcccccc0000000000000000000000000000000001**

Below are the interface definitions

## Create validator

```
function createValidator(  
  bytes validatorUncmpPubkey,  
  string moniker,  
  uint32 commissionRate,  
  uint32 maxCommissionRate,  
  uint32 maxCommissionChangeRate,  
  bool supportsUnlocked,  
  bytes data  
)

function createValidatorOnBehalf(  
  bytes validatorUncmpPubkey,  
  string moniker,  
  uint32 commissionRate,  
  uint32 maxCommissionRate,  
  uint32 maxCommissionChangeRate,  
  bool supportsUnlocked,  
  bytes data  
)

emit CreateValidator({  
  validatorUncmpPubkey,  
  moniker,  
  stakeAmount,  
  commissionRate,  
  maxCommissionRate,  
  maxCommissionChangeRate,  
  supportsUnlocked,  // 1 unlocked, 0 locked  
  operatorAddress,   // the wallet that sends this tx  
  data: data,  
});
```

Verifications:

1. Verify sender is the validator if not creating validator on behalf
2. Verify keys are correctly formatted
3. Verify the amount and rate are valid
4. Verify token type: only locked or unlocked, translate to 0 and 1
5. The amount needs to be rounded to gwei
6. The amount needs to be more than the minimum staking amount.

## UpdateValidatorCommission

```
function updateValidatorCommission(  
  bytes validatorUncmpPubkey,  
  uint32 commissionRate  
)

emit UpdateValidatorCommission({  
  validatorUncmpPubkey,  
  commissionRate  
});
```

Verifications:

1. Verify sender is the validator
2. Verify commission rate is valid

## Stake

```
function stake(  
  bytes calldata delegatorUncmpPubkey,  
  bytes calldata validatorUncmpPubkey,  
  IIPTokenStaking.StakingPeriod stakingPeriod,  
  bytes data  
) uint256 delegationId

function stakeOnBehalf(  
  bytes calldata delegatorUncmpPubkey,  
  bytes calldata validatorUncmpPubkey,  
  IIPTokenStaking.StakingPeriod stakingPeriod,  
  bytes data  
) uint256 delegationId

emit Deposit(
  delegatorUncmpPubkey, 
  validatorUnCmpPubkey, 
  stakeAmount, 
  stakingPeriod, 
  delegationId, 
  operatorAddress, 
  data
);
```

If staking\_period is 0, it uses flexible staking.

* 1 = 90 days
* 2 = 360 days
* 3 = 540 days

Verifications:

1. Verify sender is the delegator if not staking on behalf
2. Verify keys are correctly formatted
3. Verify the amount and rate are valid.
4. Amount needs to be rounded to gwei
5. Verify the staking period is valid
6. The amount needs to be more than the minimum staking amount.

## Unstake

```
function unstake(  
  bytes calldata delegatorUncmpPubkey,  
  bytes calldata validatorUncmpPubkey,  
  bytes delegationId,  
  uint256 amount,  
  bytes data  
)

function unstakeOnBehalf(  
  bytes calldata delegatorUncmpPubkey,  
  bytes calldata validatorUncmpPubkey,  
  bytes delegationId,  
  uint256 amount,  
  bytes data  
)

emit Withdraw(
  delegatorUncmpPubkey, 
  validatorUncmpPubkey, 
  amount, 
  delegationId, 
  operatorAddress, 
  data
);
```

Verifications:

1. Verify sender is the delegator if not unstaking on behalf
2. Verify keys are correctly formatted
3. Verify the amount is a valid number
4. Verify delegation id is a valid number

## Redelegate

```
function redelegate(  
  bytes delegatorUncmpPubkey;  
  bytes validatorUncmpSrcPubkey;  
  bytes validatorUncmpDstPubkey;  
  uint256 delegationId,  
  uint256 amount;  
)

function redelegateOnBehalf(  
  bytes delegatorUncmpPubkey;  
  bytes validatorUncmpSrcPubkey;  
  bytes validatorUncmpDstPubkey;  
  uint256 delegationId,  
  uint256 amount;  
)

emit Redelegate(
  delegatorUncmpPubkey, 
  validatorUncmpSrcPubkey, 
  validatorUncmpDstPubkey, 
  delegationId,
  operatorAddress, 
  amount
);
```

Verifications:

1. Verify sender is the delegator if not delegating on behalf
2. Verify keys are correctly formatted
3. Verify the amount is a valid number
4. Verify delegation id is a valid number

## Unjail

```
function unjail(  
  bytes calldata validatorUncmpPubkey,  
  bytes data  
)

function unjailOnBehalf(  
  bytes calldata validatorUncmpPubkey,  
  bytes data  
)

emit Unjail(
  msg.sender, 
  validatorUncmpPubkey, 
  data
);
```

Verification:

1. Verify sender is the validator if not unjailing on behalf
2. Verify correct unjail fee

## Set withdrawal address

```
function setWithdrawalAddress(  
  bytes calldata delegatorUncmpPubkey,  
  address newWithdrawalAddress  
)

emit SetWithdrawalAddress({  
  delegatorUncmpPubkey,  
  executionAddress  
});
```

Verification:

1. Verify sender is the delegator
2. Verify key and address is correctly formatted

## Set reward address

```
function setRewardAddress(  
  bytes calldata delegatorUncmpPubkey,  
  address newRewardAddress  
)

emit SetRewardAddress({  
  delegatorUncmpPubkey,  
  executionAddress  
});
```

Verification:

1. Verify sender is the delegator
2. Verify key and address is correctly formatted

## Add operator

```
function addOperator(  
  bytes calldata delegatorUncmpPubkey,  
  address operator  
)
emit AddOperator({  
  delegatorUncmpPubkey,  
  operator  
});
```

Verification:

1. Verify sender is the delegator
2. Verify key and address is correctly formatted

## Remove operator

```
function removeOperator(  
  bytes calldata delegatorUncmpPubkey,  
  address operator  
)

emit RemoveOperator({  
  delegatorUncmpPubkey,  
  operator  
});
```

Verification:

1. Verify sender is the delegator
2. Verify key and address is correctly formatted