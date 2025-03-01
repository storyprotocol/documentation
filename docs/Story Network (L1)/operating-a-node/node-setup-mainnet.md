---
title: Full Node
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
This section will guide you through how to setup a Story node for mainnet. Story draws inspiration from ETH PoS in decoupling execution and consensus clients. The execution client `story-geth` relays EVM blocks into the `story` consensus client via Engine API, using an ABCI++ adapter to make EVM state compatible with that of CometBFT. With this architecture, consensus efficiency is no longer bottlenecked by execution transaction throughput.

The `story` and `geth` binaries, which make up the clients required for running Story nodes, are available from our latest `release` pages:

* **`story-geth`execution client:**
  * Release Link: [**Click here**](https://github.com/piplabs/story-geth/releases)
  * Latest Stable Binary (v1.0.2): [**Click here**](https://github.com/piplabs/story-geth/releases/tag/v1.0.2)
* **`story`consensus client:**
  * Releases link: [**Click here**](https://github.com/piplabs/story/releases)
  * Latest Stable Binary (v1.1.0): [**Click here**](https://github.com/piplabs/story/releases/tag/v1.1.0)

# Story Node Installation Guide

## Pre-Installation Checklist

* [ ] Verify system meets hardware requirements
* [ ] Operating system: Ubuntu 22.04 LTS
* [ ] Required ports are available
* [ ] Sufficient disk space available
* [ ] Root or sudo access

## Quick Reference

* Installation time: \~30 minutes
* Network: Story Mainnet or Story Aeneid Testnet
* Required versions:
  * Check Latest Release

## 1. System Preparation

### 1.1 System Requirements

For optimal performance and reliability, we recommend running your node on either:

* A Virtual Private Server (VPS)
* A dedicated Linux-based machine

### System Specs

| Hardware  | Minimal Requirement |
| --------- | ------------------- |
| CPU       | Dedicated 8 Cores   |
| RAM       | 32 GB               |
| Disk      | 500 GB NVMe Drive   |
| Bandwidth | 25 MBit/s           |

### 1.2 Required Ports

*Ensure all ports needed for your node functionality are needed, described below*

* `story-geth`
  * 8545
    * Required if you want your node to interface via JSON-RPC API over HTTP
  * 8546
    * Required for websockets interaction
  * 30303 (TCP + API)
    * MUST be open for p2p communication
* `story`
  * 26656
    * MUST be open for consensus p2p communication
  * 26657
    * Required if you want your node interfacing for Tendermint RPC
  * 26660
    * Needed if you want to expose prometheus metrics

### 1.3 Install Dependencies

```bash
# Update system
sudo apt update && sudo apt-get update

# Install required packages
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

### 1.4 Install Go

For Odyssey, we need to install Go 1.22.0

```bash
# Download and install Go 1.22.0
cd $HOME

# Set Go version
GO_VERSION="1.22.0"

# Download Go binary
wget "https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz"

# Remove existing Go installation and extract new version
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go${GO_VERSION}.linux-amd64.tar.gz"

# Clean up downloaded archive
rm "go${GO_VERSION}.linux-amd64.tar.gz"

# Add Go to PATH
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> ~/.bash_profile
source ~/.bash_profile

# Verify installation
go version
```

## 2. Story Node Installation

### 2.1 Install Story-Geth

1. Download and setup binary

```bash
cd $HOME
wget https://github.com/piplabs/story-geth/releases/download/v1.0.1/geth-linux-amd64
sudo mv ./geth-linux-amd64 story-geth
sudo chmod +x story-geth
sudo mv ./story-geth $HOME/go/bin/
source $HOME/.bashrc

# Verify installation
story-geth version
```

You will see the version of the geth binary.

```
Geth
version: 1.0.1-stable
...

```

(Mac OS X only) The OS X binaries have yet to be signed by our build process, so you may need to unquarantine them manually:

```bash
sudo xattr -rd com.apple.quarantine ./geth
```

2. Configure and start service

<Tabs>
  <Tab title="Mainnet">
    ```bash
           # Setup systemd service
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

    # Start service
    sudo systemctl daemon-reload
    sudo systemctl enable story-geth
    sudo systemctl start story-geth

    # Verify service status
    sudo systemctl status story-geth
    ```
  </Tab>

  <Tab title="Aeneid testnet">
    ```bash
    # Setup systemd service
    sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
    [Unit]
    Description=Story Geth Client
    After=network.target

    [Service]
    User=${user}
    ExecStart=${path_to_geth_binary} --aeneid --syncmode full
    Restart=on-failure
    RestartSec=3
    LimitNOFILE=4096

    [Install]
    WantedBy=multi-user.target
    EOF

    # Start service
    sudo systemctl daemon-reload
    sudo systemctl enable story-geth
    sudo systemctl start story-geth

    # Verify service status
    sudo systemctl status story-geth
    ```
  </Tab>
</Tabs>

### 2.2 Install Story Consensus Client

#### Install Story Client

```bash
cd $HOME
wget https://github.com/piplabs/story/releases/download/v1.0.0/story-linux-amd64
sudo mv story-linux-amd64 story
sudo chmod +x story
sudo mv ./story $HOME/go/bin/
source $HOME/.bashrc
story version
```

> You should expect to see version 1.0.0-stable

(Mac OS X Only) The OS X binaries have yet to be signed by our build process, so you may need to unquarantine them manually:

```bash
sudo xattr -rd com.apple.quarantine ./story
```

#### Cosmovisor installation

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
sudo mkdir -p \
  $DAEMON_HOME/cosmovisor/backup \
  $DAEMON_HOME/data


# Persist configuration
echo "export DAEMON_NAME=story" >> $HOME/.bash_profile
echo "export DAEMON_HOME=$HOME/.story/story" >> $HOME/.bash_profile
echo "export DAEMON_DATA_BACKUP_DIR=${DAEMON_HOME}/cosmovisor/backup" >> $HOME/.bash_profile
echo "export DAEMON_ALLOW_DOWNLOAD_BINARIES=false" >> $HOME/.bash_profile
```

#### Init Story with Cosmovisor

<Tabs>
  <Tab title="Mainnet">
    ```bash
    cosmovisor init ./story
    cosmovisor run init --network story --moniker ${moniker_name}
    cosmovisor version
    ```
  </Tab>

  <Tab title="Aeneid testnet">
    ```bash
    cosmovisor init ./story
    cosmovisor run init --network aeneid --moniker ${moniker_name}
    cosmovisor version
    ```
  </Tab>
</Tabs>

#### Custom Configuration

To override your own node settings, you can do the following:

* `${STORY_DATA_ROOT}/config/config.toml` can be modified to change network and consensus settings
* `${STORY_DATA_ROOT}/config/story.toml` to update various client configs
* `${STORY_DATA_ROOT}/priv_validator_key.json` is a sensitive file containing your validator key, but may be replaced with your own

#### Custom Automation

Below we list a sample `Systemd` configuration you may use on Linux

```bash
# story
sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
[Unit]
Description=Story Cosmovisor
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

<Tabs>
  <Tab title="With Cosmovisor">
    ```bash
    # story
    sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
    [Unit]
    Description=Story Cosmovisor
    After=network.target

    [Service]
    Type=simple
    User=${USER}
    Group=${GROUP}
    ExecStart=${path_to_story_binary} run run \
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
  </Tab>

  <Tab title="Without Cosmovisor">
    ```bash
    # story
    sudo tee /etc/systemd/system/story.service > /dev/null <<EOF
    [Unit]
    Description=Story Cosmovisor
    After=network.target

    [Service]
    Type=simple
    User=${USER}
    Group=${GROUP}
    ExecStart=${path_to_story_binary} run
    Restart=on-failure
    RestartSec=5s
    LimitNOFILE=65535
    WorkingDirectory=$HOME/.story/story

    [Install]
    WantedBy=multi-user.target
    EOF

    ```
  </Tab>
</Tabs>

#### Start the service

```bash
sudo systemctl daemon-reload
sudo systemctl enable story
sudo systemctl start story

# Monitor logs
journalctl -u cosmovisor -f -o cat
```

#### Debugging

If you would like to check the status of `story` while it is running, it is helpful to query its internal JSONRPC/HTTP endpoint. Here are a few helpful commands to run:

* `curl localhost:26657/net_info | jq '.result.peers[].node_info.moniker'`
  * This will give you a list of consesus peers the node is sync'd with by moniker
* `curl localhost:26657/health`
  * This will let you know if the node is healthy - `{}` indicates it is

## 3. Verify Installation

### 3.1 Check Geth Status

```bash
# Check sync status
curl -X POST -H "Content-Type: application/json" \
  --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' \
  http://localhost:8545

```

### 3.2 Check Consensus Client

```bash
# Check node status
curl localhost:26657/status

# Check peer connections
curl localhost:26657/net_info | jq '.result.peers[].node_info.moniker'
```

## Clean status

If you ever run into issues and would like to try joining the network from a cleared state, run the following:

### Geth

<Tabs>
  <Tab title="Mainnet">
    ```bash
    rm -rf ${GETH_DATA_ROOT} && ./geth --story --syncmode full
    ```

    Mac OS X: `rm -rf ~/Library/Story/geth/* && ./geth --story    --syncmode full`

    Linux: `rm -rf ~/.story/geth/* && ./geth --story --syncmode full`
  </Tab>

  <Tab title="Aeneid Testnet">
    ```bash
    rm -rf ${GETH_DATA_ROOT} && ./geth --aeneid --syncmode full
    ```

    Mac OS X: `rm -rf ~/Library/Story/geth/* && ./geth --aeneid    --syncmode full`

    Linux: `rm -rf ~/.story/geth/* && ./geth --aeneid --syncmode full`
  </Tab>
</Tabs>

### Story

<Tabs>
  <Tab title="Mainnet">
    ```bash
    rm -rf ${STORY_DATA_ROOT} && ./story init --network story && ./story run
    ```

    Mac OS X: `rm -rf ~/Library/Story/story/* && ./story init --network story && ./story run`

    Linux: `rm -rf ~/.story/story/* && ./story init --network story && ./story run`
  </Tab>

  <Tab title="Aeneid Testnet">
    ```bash
    rm -rf ${STORY_DATA_ROOT} && ./story init --network aeneid && ./story run
    ```

    Mac OS X: `rm -rf ~/Library/Story/story/* && ./story init --network aeneid && ./story run`

    Linux: `rm -rf ~/.story/story/* && ./story init --network aeneid && ./story run`
  </Tab>
</Tabs>
