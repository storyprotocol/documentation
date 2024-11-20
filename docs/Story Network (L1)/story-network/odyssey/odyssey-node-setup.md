---
title: Node Setup
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
This section will guide you through how to setup a Story node. Story draws inspiration from ETH PoS in decoupling execution and consensus clients. The execution client `story-geth` relays EVM blocks into the `story` consensus client via Engine ABI, using an ABCI++ adapter to make EVM state compatible with that of CometBFT. With this architecture, consensus efficiency is no longer bottlenecked by execution transaction throughput.

![](https://files.readme.io/7dee0e873bcb2aeeaf12c3c0d63db44692c1bfe5cee599c52ea5c465240967a4-image.png)

The `story` and `geth` binaries, which make up the clients required for running Story nodes, are available from our latest `release` pages:

- **`story-geth` execution client:**
  - Release Link: [**Click here**](https://github.com/piplabs/story-geth/releases)
  - Latest Stable Binary (v0.10.0): [**Click here**](https://github.com/piplabs/story-geth/releases/tag/v0.10.0)
- **`story` consensus client:**
  - Releases link: [**Click here**](https://github.com/piplabs/story/releases)
  - Latest Stable Binary (v0.12.1): [**Click here**](https://github.com/piplabs/story/releases/tag/v0.12.1)

_**IMPORTANT: For the Odyssey testnet, it is crucial to start with version v0.12.0, as this version is required before applying any subsequent upgrades. Download this version first to ensure compatibility with the testnet environment. Also, verify that you are downloading the binary matching your system architecture**_

## System Specs

| Hardware  | Requirement |
| --------- | ----------- |
| CPU       | 4 Cores     |
| RAM       | 16 GB       |
| Disk      | 200 GB      |
| Bandwidth | 25 MBit/s   |

On AWS, we recommend using the M6i, R6i, or C6i series.

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
  - `story-geth` data root:  `~/.story/geth`

_For the remainder of this tutorial, we will refer to the `story` data root as `${STORY_DATA_ROOT}` and the `geth` data root as `${GETH_DATA_ROOT}`._ 

_You are able to override these configs on the `story` client side by passing `--home ${STORY_CONFIG_FOLDER}`. Similarly, for `geth`, you may use `--config ${GETH_CONFIG_FOLDER}`. For information on how overrides work, view our readme on [setting up a private network](https://github.com/piplabs/story?tab=readme-ov-file#creating-a-private-network)._

When downloading the Story binaries, note that the file name will vary based on your operating system. For example, on a Linux system with an AMD64 architecture, the binary might be named `story-linux-amd64` or `geth-linux-amd`. This naming convention helps with compatibility identification, but for simplicity, we recommend renaming the binary file to story after download. 

```
mv story-linux-amd64 story
```

This allows you to execute the program directly using the story command in your terminal. For the remainder of this documentation, we will use the `story` name convention.

## Execution Client Setup (`story-geth`)

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
2. Initialize the `story` client with the following command:

   ```bash
   ./story init --network odyssey 
   ```

   - By default, this uses your username for the moniker (the human-readable identifier for your node), you may override this by passing in `--moniker ${NODE_MONIKER}`
   - If you would like to initialize the node using your own data directory, you can pass in `--home ${STORY_DATA_DIR}`
   - If you already have config and data files, and would like to re-initialize from scratch, you can add the `--clean` flag
3. Now, you may run `story` with the following command:

   ```bash
   ./story run
   ```

_**IMPORTANT: If you are setting up a new node, you must start with version v0.12.0**_, as this base version is required before applying any subsequent upgrades. Afterward, install each subsequent upgrade in order. Follow these steps carefully:

1. Download [version v0.12.0](https://github.com/piplabs/story/releases/tag/v0.12.0), ensuring you select the binary that matches your system architecture.
2. Initialize and run version v0.12.0 following the initialization and run instructions provided above.
3. Download and upgrade to the following releases using [this guide](https://medium.com/story-protocol/story-v0-10-0-node-upgrade-guide-42e2fbcfcb9a):
   1. [v0.12.1 ](https://github.com/piplabs/story/releases/tag/v0.12.1) upgrade at height 322000

Note: currently you might see a bunch of `Stopping peer for error` logs - this is a known issue around peer connection stability with our bootnodes that we are currently fixing - for now please ignore it and rest assured that it does not impact block progression.

_If you ever run into issues and would like to try re-joining the network **WHILE PRESERVING YOUR KEY,** run the following:_

```bash
rm -rf ${STORY_DATA_ROOT}/data/* && \
echo '{"height": "0", "round": 0, "step": 0}' > ${STORY_DATA_ROOT}/data/priv_validator_state.json && \
./story run
```

- Mac OS X:
  ```bash
  rm -rf ~/Library/Story/story/data/* && \
  echo '{"height": "0", "round": 0, "step": 0}' > ~/Library/Story/story/data/priv_validator_state.json && \
  ./story run
  ```
- Linux:
  ```bash
  rm -rf ~/.story/story/data/* && \
  echo '{"height": "0", "round": 0, "step": 0}' > ~/.story/story/data/priv_validator_state.json && \
  ./story run
  ```

\*If you ever run into issues and would like to try joining the network from a **COMPLETELY** fresh state, run the following (**\*WARNING: THIS WILL DELETE YOUR `priv_validator_key.json` FILE )**

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
[Unit]
Description=Node-Geth
After=network.target

[Service]
Type=simple
Restart=always
RestartSec=1
User=${USER_NAME}
WorkingDirectory=${YOUR_HOME_DIR}
ExecStart=geth --odyssey  --syncmode full
StandardOutput=journal
StandardError=journal
SyslogIdentifier=node-geth
StartLimitInterval=0
LimitNOFILE=65536
LimitNPROC=65536

[Install]
WantedBy=multi-user.target
```

```bash
# story
[Unit]
Description=Node-story
After=network.target node-geth.service

[Service]
Type=simple
Restart=always
RestartSec=1
User=ec2-user
WorkingDirectory=${YOUR_HOME_DIR}
ExecStart=story run
StandardOutput=journal
StandardError=journal
SyslogIdentifier=node-story
StartLimitInterval=0
LimitNOFILE=65536
LimitNPROC=65536

[Install]
WantedBy=multi-user.target

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

To get started, **your client must be upgraded to at least version 0.9.13**. [Here](https://medium.com/story-protocol/story-v0-10-0-node-upgrade-guide-42e2fbcfcb9a) is a  guide to help you with the setup of automated upgrades with Cosmovisor.