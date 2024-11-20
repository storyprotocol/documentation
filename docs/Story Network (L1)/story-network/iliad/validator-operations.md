---
title: Validator Operations
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
## Quick Links

* [Story Geth Releases (Use Versions \<= v0.9.4)](https://github.com/piplabs/story-geth/releases)
* [Story Releases (Use Versions \<= v0.11.0)](https://github.com/piplabs/story/releases/)  

# Overview

This section will guide you through how you can run your own validator. Validator operations may be done via the `story` consensus client.

> 📘 Note
>
> The below operations do not requiring running a node! However, if you would like to participate in staking rewards, you must run a validator node.

Before proceeding, it is important to familiarize yourself with the difference between a delegator and a validator:

* A **validator** is a full node that participates in consensus whose signed key resides in the `priv_validator_key.json` file under your `story` data directory. To print out your validator key details you may refer to the [validator key export section](https://docs.story.foundation/docs/validator-operations#validator-key-export)
* A **delegator** refers to an account operator that holds `IP` and wishes to participate in consensus rewards but without needing to run a validator themselves. 

In the same folder as where your `story` binary resides, add a `.env` file with a `PRIVATE_KEY` whose account has `IP` funded (*you may see the[Faucet page](doc:faucet) for details on how to fund an account).* **We recommend using your delegator account for all below operations.**

> 📘 Note
>
> You may also issue transactions as the validator itself. To get the EVM private key corresponding to your validator, please refer to the [Validator Key Export](https://docs.story.foundation/docs/validator-operations#validator-key-export) section.

The `.env` file should look like the following *(make sure not to add a 0x prefix):*

```bash
# ~/story/.env
PRIVATE_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

With this, you are all set to perform different validator operations! Below, we will guide you through all of those supported via the CLI:

## Validator Key Export

By default, when you run `./story init` a validator key is created for you. To view your validator key, run the following command:

```bash
./story validator export
```

This will print out your validator public key file in compressed and uncompressed formats. By default, we use the base64-encoded compressed key for public identification.

In addition, if you want to export the derived EVM private key of your validator into the default data config directory, please run the following:

```bash
./story validator export --export-evm-key
```

* You may add `--evm-key-path` to specify a different download location

*If you would like to issue transactions as your validator, and not as a delegator, you may export the key to your`.env` file and ensure it has IP sent to it, e.g. via`./story validator export --export-evm-key --evm-key-path .env`*

## Validator Creation

To create a new validator, run the following command:

```bash
./story validator create --stake ${AMOUNT_TO_STAKE_IN_WEI}
```

This will create the validator corresponding to your validator key saved in `priv_validator_key.json`, providing the validator with `{$AMOUNT_TO_STAKE_IN_WEI}` IP to self-stake. *Note that to participate in consensus, at least 1 IP must be staked (equivalent to`1000000000000000000 wei`)!*

Once created, please use the `Explorer URL` to confirm the transaction. If successful, you should see your validator pub key (*found in your`priv_validator_key.json` file)* listed as part of the following endpoint:

```bash
curl https://testnet.storyrpc.io/validators | jq .
```

Congratulations, you are now one of Story’s very first IP validators!

### Example Create command use

```bash
./story validator create --stake 1000000000000000000
```

## Validator Staking

To stake to an existing validator, run the following command:

```bash
./story validator stake \
   --validator-pubkey ${VALIDATOR_PUB_KEY_IN_BASE64} \
   --stake ${AMOUNT_TO_STAKE_IN_WEI}
```

* *Note that your own`${VALIDATOR_PUB_KEY_IN_BASE64}`may be found by running the `./iliad validator export` command as the `Compresses Public Key`.*
* You must stake at least 1 ETH worth (`*1000000000000000000 wei`) for the transaction to be valid

Once staked, you may use the `Explorer URL` to confirm the transaction. As mentioned earlier, you may use our [validator endpoint](https://testnet.storyrpc.io/validators) to confirm the new voting power of the validator.

### Example Staking command use

```bash
./story validator stake \
  --validator-pubkey A0PSykpcrXXomPnNJcITuCawqg+G0JokQBGd9hU2f5CM \
  --stake 1000000000000000000
```

## Validator Unstaking

To unstake from a validator, run the following command:

```bash
./story validator unstake \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_BASE64} \
  --unstake ${AMOUNT_TO_UNSTAKE_IN_WEI} \
```

This will unstake `${AMOUNT_TO_UNSTAKE_IN_WEI}` IP from the selected validator.

Like in the staking operation, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://testnet.storyrpc.io/validators) to double-check the newly reduced voting power of the validator.

### Example Unstaking command use

```bash
./story validator unstake \
   --validator-pubkey A0PSykpcrXXomPnNJcITuCawqg+G0JokQBGd9hU2f5CM \
   --unstake 1000000000000000000
```

## Validator Stake-on-behalf

To stake on behalf of another delegator, run the following command:

```bash
./story validator stake-on-behalf \
  --delegator-pubkey ${DELEGATOR_PUB_KEY_IN_BASE64} \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_BASE64} \
  --stake ${AMOUNT_TO_STAKE_IN_WEI} \
```

This will stake `${AMOUNT_TO_STAKE_IN_WEI}` IP to the validator on behalf of the provided delegator.

Like in the other staking operations, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://testnet.storyrpc.io/validators) to double-check the increased voting power of the validator.

### Example Stake-on-behalf command use

```bash
./story validator stake-on-behalf \
   --delegator-pubkey An/k/1HntF2r8ZaqrwxEOTKSIXdVsZk5wxkt4i5uhk3V \
   --validator-pubkey A0PSykpcrXXomPnNJcITuCawqg+G0JokQBGd9hU2f5CM \
   --stake 1000000000000000000
```

## Validator Unstake-on-behalf

You may also unstake on behalf of delegators. However, to do so, you must be registered as an authorized operator for that delegator. To unstake on behalf of another delegator as an operator, run the following command:

```bash
./story validator unstake-on-behalf \
  --delegator-pubkey ${DELEGATOR_PUB_KEY_IN_BASE64} \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_BASE64} \
  --unstake ${AMOUNT_TO_STAKE_IN_WEI} \
```

This will unstake `${AMOUNT_TO_STAKE_IN_WEI}` IP from the validator on behalf of the delegator, assuming you are a registered operator for that delegator.

Like in the other staking operations, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://testnet.storyrpc.io/validators) to double-check the decreased voting power of the validator.

### Example Unstake-on-behalf command use

```bash
./story validator unstake-on-behalf \
   --delegator-pubkey An/k/1HntF2r8ZaqrwxEOTKSIXdVsZk5wxkt4i5uhk3V \
   --validator-pubkey A0PSykpcrXXomPnNJcITuCawqg+G0JokQBGd9hU2f5CM \
   --unstake 1000000000000000000
```

## Validator Unjail

In case a validator becomes jailed, for example if it experiences substantial downtime, you may use the following command to unjail the targeted validator:

```
./story validator unjail \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_BASE64}
```

### Example Unjail command use

```bash
./story validator unjail \
  --validator-pubkey A0PSykpcrXXomPnNJcITuCawqg+G0JokQBGd9hU2f5CM
```

## Add Operator

Delegators may add operators to unstake or redelegate on their behalf. To add an operator, run the following command:

```bash
./story validator add-operator \
  --operator ${OPERATOR_EVM_ADDRESS}
```

### Example Add Operator command use

```bash
./story validator add-operator \
  --operator 0xf398C12A45Bc409b6C652E25bb0a3e702492A4ab
```

## Remove Operator

To remove an operator, run the following command:

```bash
./story validator remove-operator \
  --operator ${OPERATOR_EVM_ADDRESS}
```

### Example Remove Operator command use

```bash
./story validator remove-operator \
  --operator 0xf398C12A45Bc409b6C652E25bb0a3e702492A4ab
```

## Set Withdrawal Address

To change the address that your delegator receives staking and withdrawal rewards from, you can run the following:

```bash
./story validator set-withdrawal-address \
  --withdrawal-address ${OPERATOR_EVM_ADDRESS}
```

### Example Set Withdrawal Address command use

```bash
./story validator set-withdrawal-address \
  --withdrawal-address 0xf398C12A45Bc409b6C652E25bb0a3e702492A4ab
```
