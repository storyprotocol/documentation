---
title: Node Setup
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

This section will guide you through how to setup a Story node. Story draws inspiration from ETH PoS in decoupling execution and consensus clients. The execution client `story-geth` relays EVM blocks into the `story` consensus client via Engine API, using an ABCI++ adapter to make EVM state compatible with that of CometBFT. With this architecture, consensus efficiency is no longer bottlenecked by execution transaction throughput.

![](https://files.readme.io/7dee0e873bcb2aeeaf12c3c0d63db44692c1bfe5cee599c52ea5c465240967a4-image.png)

The `story` and `geth` binaries, which make up the clients required for running Story nodes, are available from our latest `release` pages:

- **`story-geth`execution client:**
  - Release Link: [**Click here**](https://github.com/piplabs/story-geth/releases)
  - Latest Stable Binary (v0.10.1): [**Click here**](https://github.com/piplabs/story-geth/releases/tag/v0.10.1)
- **`story`consensus client:**
  - Releases link: [**Click here**](https://github.com/piplabs/story/releases)
  - Latest Stable Binary (v0.13.0): [**Click here**](https://github.com/piplabs/story/releases/tag/v0.13.0)

**_IMPORTANT: For the Odyssey testnet, it is crucial to start with version v0.12.0, as this version is required before applying any subsequent upgrades. Download this version first to ensure compatibility with the testnet environment. Also, verify that you are downloading the binary matching your system architecture_**

## Prerequisites

For optimal performance and reliability, we recommend running your node on either:

- A Virtual Private Server (VPS)
- A dedicated Linux-based machine

### System Requirements

- Operating System: Ubuntu 22.04 LTS or later

### System Specs

| Hardware  | Requirement       |
| --------- | ----------------- |
| CPU       | 8 Cores           |
| RAM       | 32 GB             |
| Disk      | 500 GB NVMe Drive |
| Bandwidth | 25 MBit/s         |

## Ports

_Ensure all ports needed for your node functionality are needed, described below_

- `story-geth`
  - 8545
    - Required if you want your node to interface via JSON-RPC API over HTTP
  - 8546
    - Required for websockets interaction
  - 30303 (TCP + API)
    - MUST be open for p2p communication
- `story`
  - 26656
    - MUST be open for consensus p2p communication
  - 26657
    - Required if you want your node interfacing for Tendermint RPC
  - 26660
    - Needed if you want to expose prometheus metrics

## Default Folder

By default, we setup the following default data folders for consensus and execution clients:

- Mac OS X
  - `story` data root: `~/Library/Story/story`
  - `story-geth` data root: `~/Library/Story/geth`
- Linux
  - `story` data root: `~/.story/story`
  - `story-geth` data root: `~/.story/geth`

_For the remainder of this tutorial, we will refer to the`story` data root as `${STORY_DATA_ROOT}` and the `geth` data root as `${GETH_DATA_ROOT}`._

_You are able to override these configs on the`story` client side by passing `--home ${STORY_CONFIG_FOLDER}`. Similarly, for `geth`, you may use `--config ${GETH_CONFIG_FOLDER}`. For information on how overrides work, view our readme on [setting up a private network](https://github.com/piplabs/story?tab=readme-ov-file#creating-a-private-network)._

When downloading the Story binaries, note that the file name will vary based on your operating system. For example, on a Linux system with an AMD64 architecture, the binary might be named `story-linux-amd64` or `geth-linux-amd`. This naming convention helps with compatibility identification, but for simplicity, we recommend renaming the binary file to story after download.

```
mv story-linux-amd64 story
```

This allows you to execute the program directly using the story command in your terminal. For the remainder of this documentation, we will use the `story` name convention.

### Dependencies setup

```
sudo apt update && sudo apt-get update
sudo apt install -y \
  curl \
  git \
  make \
  jq \
  build-essential \
  gcc \
  unzip \
  wget \
  lz4 \
  aria2 \
  gh
```

### Go Dependencies

```
cd $HOME && \
ver="1.22.0" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile && \
source ~/.bash_profile && \
go version
```

Make sure you see `go version go1.22.0 xxx`

## Execution Client Setup (`story-geth`)

### Download the binary

```
cd $HOME && \
wget {geth-all-1.0.0-9d3f9d4.tar.gz}
# Extract the binaries
tar -xvzf geth-all-1.0.0-9d3f9d4.tar.gz
tar -xvf geth-linux-arm64-1.0.0-9d3f9d4.tar
chmod +x geth-linux-arm64-1.0.0-9d3f9d4/geth
sudo mv ./geth-linux-arm64-1.0.0-9d3f9d4/geth $HOME/go/bin/story-geth
source $HOME/.bashrc
story-geth version
```

You will see the version of the geth binary.

```
Geth
version: 1.0.0-stable
...
```

1. (Mac OS X only) The OS X binaries have yet to be signed by our build process, so you may need to unquarantine them manually:

   ```bash
   sudo xattr -rd com.apple.quarantine ./geth
   ```

2. You may now run `geth` with the following command:

   ```bash
   ./geth --odyssey --syncmode full
   ```

   - Currently, `snap` sync mode, the default, is still undergoing development

### Clear State

If you ever run into issues and would like to try joining the network from a cleared state, run the following:

```bash
rm -rf ${GETH_DATA_ROOT} && ./geth --odyssey --syncmode full
```

- Mac OS X: `rm -rf ~/Library/Story/geth/* && ./geth --odyssey --syncmode full`
- Linux: `rm -rf ~/.story/geth/* && ./geth --odyssey --syncmode full`

### Debugging

If you would like to check the status of `geth` while it is running, it is helpful to communicate via its built-in IPC-RPC server by running the following:

```bash
geth attach ${GETH_DATA_ROOT}/geth.ipc
```

- Mac OS X:
  - `geth attach ~/Library/Story/geth/odyssey/geth.ipc`
- Linux:
  - `geth attach ~/.story/geth/odyssey/geth.ipc`

This will connect you to the IPC server from which you can run some helpful queries:

- `eth.blockNumber` will print out the latest block geth is sync’d to - if this is `undefined` there is likely a peer connection or syncing issue
- `admin.peers` will print out a list of other `geth` nodes your client is connected to - if this is blank there is a peer connectivity issue
- `eth.syncing` will return `true` if geth is in the process of syncing, `false` otherwise

## Consensus Client Setup (`story`)

1. (Mac OS X Only) The OS X binaries have yet to be signed by our build process, so you may need to unquarantine them manually:

   ```bash
   sudo xattr -rd com.apple.quarantine ./story
   ```

### Cosmovisor installation

For updating the story client, we recommend using Cosmovisor.

1. Install Cosmovisor

```bash
go install cosmossdk.io/tools/cosmovisor/cmd/cosmovisor@v1.6.0
cosmovisor version
```

2. Configure Cosmovisor

```bash
# Set daemon configuration
export DAEMON_NAME=story
export DAEMON_HOME=$HOME/.story/story
export DAEMON_DATA_BACKUP_DIR=${DAEMON_HOME}/cosmovisor/backup
sudo mkdir -p $DAEMON_HOME/cosmovisor/backup $DAEMON_HOME/data


# Persist configuration
echo "export DAEMON_NAME=story" >> $HOME/.bash_profile
echo "export DAEMON_HOME=$HOME/.story/story" >> $HOME/.bash_profile
echo "export DAEMON_DATA_BACKUP_DIR=${DAEMON_HOME}/cosmovisor/backup" >> $HOME/.bash_profile
echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> $HOME/.bash_profile
```

### Install Story Client

```bash
cd $HOME
tar -xvzf story-all-1.0.0-2dd5638.tar.gz
tar -xvf story-linux-amd64-1.0.0-2dd5638.tar
chmod +x story-linux-amd64-1.0.0-2dd5638/story
sudo mv story-linux-amd64-1.0.0-2dd5638/story $HOME/go/bin/story
source $HOME/.bashrc
story version
```

You should expect to see version 1.0.0-stable

### Init Story with Cosmovisor

```bash
cosmovisor init ./story
cosmovisor run init --network story --moniker ${moniker_name}
cosmovisor version
```

### Clear State

If you ever run into issues and would like to try joining the network from a fresh state, run the following:

```bash
rm -rf ${STORY_DATA_ROOT} && ./story init --network odyssey && ./story run
```

- Mac OS X:
  - `rm -rf ~/Library/Story/story/* && ./story init --network odyssey && ./story run`
- Linux:
  - `rm -rf ~/.story/story/* && ./story init --network odyssey && ./story run`

To quickly check if the node is syncing, you could

- Check the geth RPC endpoint to see if blocks are increasing:
  ```bash
  curl -X POST -H "Content-Type: application/json" --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' [http://localhost:8545](http://localhost:8545/)
  ```
- Attach to `geth` as explained above and see if the `eth.blockNumber` is increasing

### Custom Configuration

To override your own node settings, you can do the following:

- `${STORY_DATA_ROOT}/config/config.toml` can be modified to change network and consensus settings
- `${STORY_DATA_ROOT}/config/story.toml` to update various client configs
- `${STORY_DATA_ROOT}/priv_validator_key.json` is a sensitive file containing your validator key, but may be replaced with your own

### Custom Automation

Below we list a sample `Systemd` configuration you may use on Linux

```bash
# geth
sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
[Unit]
Description=Story Geth Client
After=network.target

[Service]
User=${user}
ExecStart=${path_to_geth_binary} --story --syncmode full
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF
```

```bash
# story
sudo tee /etc/systemd/system/cosmovisor.service > /dev/null <<EOF
[Unit]
Description=Cosmovisor
After=network.target

[Service]
Type=simple
User=$USER
Group=$GROUP
ExecStart=/usr/local/bin/cosmovisor run run \
--api-enable \
--api-address=0.0.0.0:1317
Restart=on-failure
RestartSec=5s
LimitNOFILE=65535
Environment="DAEMON_NAME=$DAEMON_NAME"
Environment="DAEMON_HOME=$DAEMON_HOME"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_DATA_BACKUP_DIR=$DAEMON_HOME/cosmovisor/backup"
WorkingDirectory=$DAEMON_HOME

[Install]
WantedBy=multi-user.target
EOF

```

### Start Service

```bash
# Load the systemd for story client
sudo systemctl daemon-reload
sudo systemctl enable cosmovisor
sudo systemctl start cosmovisor

# Monitor logs
journalctl -u cosmovisor -f -o cat

# Load the systemd for geth client
sudo systemctl daemon-reload
sudo systemctl enable story-geth
sudo systemctl start story-geth

# Monitor logs
journalctl -u story-geth -f -o cat
```

### Debugging

If you would like to check the status of `story` while it is running, it is helpful to query its internal JSONRPC/HTTP endpoint. Here are a few helpful commands to run:

- `curl localhost:26657/net_info | jq '.result.peers[].node_info.moniker'`
  - This will give you a list of consesus peers the node is sync’d with by moniker
- `curl localhost:26657/health`
  - This will let you know if the node is healthy - `{}` indicates it is

### Common Issues

1. `auth failure: secret conn failed: read tcp ${IP_A}:${PORT_A}->${IP_B}:{PORT_B}: i/o timeout`
   - This issue occurs when the default `external_address` listed in `config.toml` (`””`) is not being introspected properly via the listener. To fix it, please remove `addrbook.json` in `{STORY_DATA_ROOT}/config/addrbook.json` and add `exteral_address = {YOUR_NODE_PUBLIC_IP_ADDRESS}` in `config.toml`

### Automated Upgrades

To manage consensus client upgrades more easily, especially for hard forks, we recommend using [Cosmovisor](https://docs.cosmos.network/v0.45/run-node/cosmovisor.html), which allows you to automate the process of upgrading client binaries without having to restart your client.

To get started, **your client must be upgraded to at least version 0.9.13**. [Here](https://medium.com/story-protocol/story-v0-10-0-node-upgrade-guide-42e2fbcfcb9a) is a guide to help you with the setup of automated upgrades with Cosmovisor.
