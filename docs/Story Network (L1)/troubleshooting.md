---
title: Troubleshooting
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

# Overview

Welcome to the Troubleshooting a Story node!

The following section is a collection of the most common problems, errors, bugs that occur when interacting with the Story nodes.

Troubleshooting aims at:
* Quickly identify problems;
* Quickly solution that could affect your synchronization, performance, connectivity.

Let's build a healthy ecosystem.

## Failed to initialize database
Error:
```bash
ERRO !! Fatal error occurred, app died️ unexpectedly !! err="create db: failed to initialize database:
```
Solution:
* This indicates a corrupted database.
* You need to save your state and try to sync from a fresh snapshot.
* To save the state of the validator:
```bash
cp $HOME/.story/story/data/priv_validator_state.json $HOME/.story/story/priv_validator_state.json.backup
```
> 🚧 Be very careful with this file, especially if your validator is already signing blocks.
* Check your the database backend type, your node must support the same as you are using the snapshot:
```bash
cat $HOME/.story/story/config/story.toml
```
Default is `app-db-backend = "goleveldb”`. The fallback is the `db_backend` value set in CometBFT's `config.toml`.
```bash
cat $HOME/.story/story/config/config.toml
```

## Gas fee increase at RPC node
Problem:
* Gas fee are high and you need to edit it on the RPC node.

Solution:
* You will need to add the `--rpc.txfee` flag to your geth startup command.
* For example, it can look like this (meaning that the RPC node will skip all transactions with a fee of more than 2 IPs):
```bash
sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
[Unit]
Description=Story-Geth Node
After=network.target

[Service]
User=$USER
Type=simple
WorkingDirectory=$HOME/.story/geth
ExecStart=$(which geth)  --odyssey --syncmode full --rpc.txfee 2
Restart=on-failure
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

## Failed to send PacketPing
Error:
```bash
ERRO Failed to send PacketPing module=p2p peer=19fa6dd52e72e4e85bbb873b705282cf73217a6b@158.220.80.96:40128 err="write tcp 139.59.139.135:26656->158.220.80.96:40128: write: broken pipe"
```
Solution:
* If the node is synchronized, you can ignore this error. Your client may be a little behind.
* If the node stops, you should restart the services.

## Cosmovisor: failed to read upgrade info
An error occurs when starting the cosmovisor:
```bash
panic: failed to read upgrade info from disk unexpected end of JSON input
```
Solution:
* You must ensure that the installed cosmovisor version must be at least [v1.7.0.](https://docs.cosmos.network/main/build/tooling/cosmovisor)
* Then check your info file (edit version `v0.13.0` in your case):
```bash
cat $HOME/.story/story/cosmovisor/upgrades/v0.13.0/upgrade-info.json
```
If you don`t have create new one:
```bash
echo '{"name":"v0.13.0","time":"0001-01-01T00:00:00Z","height":858000}' > $HOME/.story/story/cosmovisor/upgrades/v0.13.0/upgrade-info.json
```
Find out more about automatic updates with cosmovisor [here](https://docs.story.foundation/docs/odyssey-node-setup#automated-upgrades).

## 
