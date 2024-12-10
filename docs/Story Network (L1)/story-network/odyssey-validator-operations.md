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

* [Story Geth Releases](https://github.com/piplabs/story-geth/releases)
* [Story Releases](https://github.com/piplabs/story/releases/)  

# Overview

This section will guide you through how you can run your own validator. Validator operations may be done via the `story` consensus client.

> 📘 Note
>
> The below operations do not requiring running a node! However, if you would like to participate in staking rewards, you must run a validator node.

Before proceeding, it is important to familiarize yourself with the difference between a delegator and a validator:

* A **validator** is a full node that participates in consensus whose signed key resides in the `priv_validator_key.json` file under your `story` data directory. To print out your validator key details you may refer to the [validator key export](https://docs.story.foundation/docs/odyssey-validator-operations#validator-key-export) section.
* A **delegator** refers to an account operator that holds `IP` and wishes to participate in consensus rewards but without needing to run a validator themselves.

In the same folder as where your `story` binary resides, add a `.env` file with a `PRIVATE_KEY` whose account has `IP` funded (*you may see the [Faucet page](https://docs.story.foundation/docs/story-network#-faucets) for details on how to fund an account).* **We recommend using your delegator account for all below operations.**

> 📘 Note
>
> You may also issue transactions as the validator itself. To get the EVM private key corresponding to your validator, please refer to the [Validator Key Export](https://docs.story.foundation/docs/odyssey-validator-operations#validator-key-export) section.

The `.env` file should look like the following *(make sure not to add a 0x prefix):*

```bash
# ~/.env
PRIVATE_KEY=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

With this, you are all set to perform different validator operations! Below, we will guide you through all of those supported via the CLI:

## Validator Key Export

By default, when you run `./story init` a validator key is created for you. To view your validator key, run the following command:

```bash
./story validator export
```

This will print out your validator public key file in compressed and uncompressed formats. By default, we use the hex-encoded compressed key for public identification.

```text
Compressed Public Key (hex): 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984
Compressed Public Key (base64): A73HuJQLq+kibVLX+imaH689ZKgvgJiJJWyPFGlYpjmE
Uncompressed Public Key (hex): 04bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a6398496b9e2af0a3a1d199c3cc1d09ee899336a530c185df6b46a9735b25e79a493af
EVM Address: 0x9EacBe2C3B1eb0a9FC14106d97bd3A1F89efdDCc
Validator Address: storyvaloper1p470h0jtph4n5hztallp8vznq8ehylsw9vpddx
Delegator Address: story1p470h0jtph4n5hztallp8vznq8ehylswtr4vxd
```

In addition, if you want to export the derived EVM private key of your validator into the default data config directory, please run the following:

```bash
./story validator export --export-evm-key
```

* You may add `--evm-key-path` to specify a different download location

*If you would like to issue transactions as your validator, and not as a delegator, you may export the key to your`.env` file and ensure it has IP sent to it, e.g. via`./story validator export --export-evm-key --evm-key-path .env`*

## Validator Creation

To create a new validator, run the following command:

```bash
./story validator create --stake ${AMOUNT_TO_STAKE_IN_WEI} --moniker ${VALIDATOR_NAME}
```

This will create the validator corresponding to your validator key saved in `priv_validator_key.json`, providing the validator with `{$AMOUNT_TO_STAKE_IN_WEI}` IP to self-stake. *Note that to participate in consensus, at least 1024 IP must be staked (equivalent to`1024000000000000000000 wei`)!*

Below is a list of optional flags to further customize your validator setup:

**Available Flags:**

* `--stake`: Sets the amount the validator will self-delegate in wei (default is `1024000000000000000000` wei).
* `--moniker`: Defines a custom name for the validator, visible to users on the network.
* `--chain-id`: Specifies the Chain ID for the transaction. By default, this is set to `1516`.
* `--commission-rate`: Sets the validator's commission rate in bips (1% = 100 bips). For instance, `1000` represents a 10% commission (default is `1000`).
* `--explorer`: Specifies the URL of the blockchain explorer (default: [https://odyssey.storyscan.xyz](https://odyssey.storyscan.xyz)).
* `--keyfile`: Points to the path of the Tendermint key file (default: `/home/node_story_odyssey/.story/story/config/priv_validator_key.json`).
* `--max-commission-change-rate`: Sets the maximum rate at which the validator's commission can change, in bips. For example, `100` represents a maximum change of 1% (default is `1000`).
* `--max-commission-rate`: Defines the maximum commission rate the validator can charge, in bips. For instance, `5000` allows a 50% maximum rate (default is `5000`).
* `--private-key`: Uses a specified private key for signing the transaction. If not set, the key in `priv_validator_key.json` will be used.
* `--rpc`: Sets the RPC URL to connect to the network (default: [https://odyssey.storyrpc.io](https://odyssey.storyrpc.io)).
* `--unlocked`: Determines if unlocked token staking is supported (`true` for unlocked staking, `false` for locked staking). By default, this is set to `true`.

### Example creation command use

```bash
./story validator create --stake 1024000000000000000000 --moniker testValidator
```

### Example creation command with custom flags

```bash
./story validator create --stake 1024000000000000000000 --moniker "Story Validator" --chain-id 1516 --commission-rate 1500 --max-commission-change-rate 500 --max-commission-rate 5000 --rpc "https://odyssey.storyrpc.io"
```

### Verifying your validator

Once created, please use the `Explorer URL` to confirm the transaction. If successful, you should see your validator pub key (*found in your`priv_validator_key.json` file)* listed as part of the following endpoint:

```bash
curl https://testnet.storyrpc.io/validators | jq .
```

Congratulations, you are now one of Story’s very first IP validators!

## Validator Staking

To stake to an existing validator, run the following command:

```bash
./story validator stake \
   --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
   --stake ${AMOUNT_TO_STAKE_IN_WEI}
```

* Note that your own `${VALIDATOR_PUB_KEY_IN_HEX}`may be found by running the `./story validator export` command as the `Compressed Public Key (hex)`. 
* You must stake at least 1024 IP worth (`*1024000000000000000000 wei`) for the transaction to be valid

Once staked, you may use the `Explorer URL` to confirm the transaction. As mentioned earlier, you may use our [validator endpoint](https://rpc.odyssey.storyrpc.io/validators) to confirm the new voting power of the validator.

### Example staking command use

```bash
./story validator stake \
  --validator-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
  --stake 1024000000000000000000
```

## Validator Unstaking

To unstake from a validator, run the following command:

```bash
./story validator unstake \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
  --unstake ${AMOUNT_TO_UNSTAKE_IN_WEI} \
```

This will unstake `${AMOUNT_TO_UNSTAKE_IN_WEI}` IP from the selected validator. You must unstake at least 1024 IP worth (`*1024000000000000000000 wei`) for the transaction to be valid.

Like in the staking operation, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://rpc.odyssey.storyrpc.io/validators) to double-check the newly reduced voting power of the validator.

### Example unstaking command use

```bash
./story validator unstake \
   --validator-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
   --unstake 1024000000000000000000
```

## Validator Stake-on-behalf

To stake on behalf of another delegator, run the following command:

```bash
./story validator stake-on-behalf \
  --delegator-pubkey ${DELEGATOR_PUB_KEY_IN_HEX} \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
  --stake ${AMOUNT_TO_STAKE_IN_WEI} \
```

This will stake `${AMOUNT_TO_STAKE_IN_WEI}` IP to the validator on behalf of the provided delegator. You must stake at least 1024 IP worth (`*1024000000000000000000 wei`) for the transaction to be valid.

Like in the other staking operations, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://rpc.odyssey.storyrpc.io/validators) to double-check the increased voting power of the validator.

### Example Stake-on-behalf command use

```bash
./story validator stake-on-behalf \
   --delegator-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
   --validator-pubkey 02ed58a9319aba87f60fe08e87bc31658dda6bfd7931686790a2ff803846d4e59c \
   --stake 1024000000000000000000
```

## Validator Unstake-on-behalf

You may also unstake on behalf of delegators. However, to do so, you must be registered as an authorized operator for that delegator. To unstake on behalf of another delegator as an operator, run the following command:

```bash
./story validator unstake-on-behalf \
  --delegator-pubkey ${DELEGATOR_PUB_KEY_IN_HEX} \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX} \
  --unstake ${AMOUNT_TO_STAKE_IN_WEI} \
```

This will unstake `${AMOUNT_TO_STAKE_IN_WEI}` IP from the validator on behalf of the delegator, assuming you are a registered operator for that delegator. You must unstake at least 1024 IP worth (`*1024000000000000000000 wei`) for the transaction to be valid.

Like in the other staking operations, please use the `Explorer URL` to confirm the transaction and our [validator endpoint](https://rpc.odyssey.storyrpc.io/validators) to double-check the decreased voting power of the validator.

### Example Unstake-on-behalf command use

```bash
./story validator unstake-on-behalf \
   --delegator-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984 \
   --validator-pubkey 02ed58a9319aba87f60fe08e87bc31658dda6bfd7931686790a2ff803846d4e59c \
   --unstake 1024000000000000000000
```

## Validator Unjail

In case a validator becomes jailed, for example if it experiences substantial downtime, you may use the following command to unjail the targeted validator:

```Text Bash
./story validator unjail \
  --validator-pubkey ${VALIDATOR_PUB_KEY_IN_HEX}
```

Note that you will need at least 1 IP in the wallet submitting the transaction for the transaction to be valid.

### Example unjail command use

```bash
./story validator unjail \
  --validator-pubkey 03bdc7b8940babe9226d52d7fa299a1faf3d64a82f809889256c8f146958a63984
```

## Add Operator

Delegators may add operators to unstake or redelegate on their behalf. To add an operator, run the following command:

```bash
./story validator add-operator \
  --operator ${OPERATOR_EVM_ADDRESS}
```

Note that you will need at least 1 IP in the wallet submitting the transaction for the transaction to be valid.

### Example add operator command use

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

Note that you will need at least 1 IP in the wallet submitting the transaction for the transaction to be valid.

### Example Set Withdrawal Address command use

```bash
./story validator set-withdrawal-address \
  --withdrawal-address 0xf398C12A45Bc409b6C652E25bb0a3e702492A4ab
```

## Migrating a validator to another machine

> 🚧 Important
>
> Before migrating your validator node to a new machine, make sure the current node is fully shut down. Attempting to restore an active validator could result in "double signing," a critical error that may lead to the slashing of your delegated shares.

1. Begin by configuring a new environment for your validator. Ensure that the new full node is fully synced to the latest block on the network.
2. To avoid accidental double-signing, it’s essential to fully shut down the original validator node before activating the new instance. We recommend deleting the Story service file to prevent it from automatically restarting after a system reboot. Additionally, back up your `priv_validator_key.json` file and remove it from the current server running the active validator. Skipping these steps could result in missed blocks or other penalties.

```bash
# Step 1: Stop the original validator node
sudo systemctl stop <your_service_file_name>.service

# Step 2: Disable the Story service to prevent automatic restarts
sudo systemctl disable <your_service_file_name>.service

# Step 3: Delete the Story service file to prevent it from starting on reboot
sudo rm /etc/systemd/system/<your_service_file_name>.service

# Step 4: Back up the `priv_validator_key.json` file securely, e.g., using SFTP:
# Use an SFTP client or a secure method to download the file without displaying it in the terminal
# If needed for verification purposes only, you may view it with the following command:
cat ~/.story/story/config/priv_validator_key.json

# Step 5: Remove the `priv_validator_key.json` file from the current server
rm ~/.story/story/config/priv_validator_key.json
```

3. Locate the `priv_validator_key.json` file in the `~/.story/story/config/` directory on your new machine. Replace this file with the backup copy from your old validator.

***IMPORTANT: Before you proceed, make sure you STOPPED your validator on the old server and do not start it again there.***

4. After transferring the private key file, restart the validator node on your new setup. This will reintegrate your validator with the network, enabling it to resume its validation role.
