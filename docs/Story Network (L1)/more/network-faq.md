---
title: Troubleshooting
deprecated: false
hidden: false
metadata:
  robots: index
---
Welcome to Story node troubleshooting! This section covers common problems and solutions when running Story nodes.

### Node Setup

<details>
  <summary>What are the hardware requirements?</summary>

  <br />

  See the <a href="https://docs.story.foundation/docs/node-setup#system-specs">system specs</a>
</details>

***

<details>
  <summary>What's the max expected TPS?</summary>

  <br />

  \~700
</details>

***

<details>
  <summary>Is it fully EVM-compatible? Is there any customization already being made on the IP blockchain? Or are there any coming customization to be applied?</summary>

  <br />

  Yes, it's EVM-compatible. Story's execution client is a fork of Geth with our custom precompiles, which enhance the IP graph's performance while maintaining strict EVM compatibility. Other Ethereum execution clients, such as RETH and Erigon, can be supported later.
</details>

***

<details>
  <summary>Which is your consensus mechanism?</summary>

  <br />

  Our consensus mechanism is CometBFT
</details>

***

<details>
  <summary>Batches support? Limit on batch request?</summary>

  <br />

  Batch RPCs are supported - for Geth there is a 1K limit and on the consensus side there is 10 request limit
</details>

***

<details>
  <summary>WS connections? (if yes, how do they work)</summary>

  <br />

  Yes, WS is enabled on the execution client, and is recommended for subscription use-cases. It is open on port 8546
</details>

***

<details>
  <summary>How many different paths does node serves (several path with diff methods RPC)?</summary>

  <br />

  Please see Geth's latest JSON-RPC documentation for a full comprehensive list <a href="https://ethereum.org/en/developers/docs/apis/json-rpc/#web3_clientversion">here</a>. In the future, we may add more.
</details>

***

<details>
  <summary>Caching rules for RPC method?</summary>

  <br />

  We recommend employing standard in-memory caching with a 1-10 min TTL based on the RPC method
</details>

***

<details>
  <summary>What is the best method to get latest block and check node is healthy and in sync?</summary>

  <br />

  Use `eth_syncing` RPC call on the execution client to check if the node is sync and `eth_blockNumber` for getting the latest block
</details>

***

<details>
  <summary>What are the heaviest RPC methods? How much time does it take to respond to request with such method?</summary>

  <br />

  `eth_call` / `eth_getLogs` / `eth_getBlockByNumber` \
  We are still running latency tests to get a sense of response times.
</details>

***

<details>
  <summary>Is archive node provisioning a requirement? If yes how big?</summary>

  <br />

  No, not at the moment.
</details>

***

<details>
  <summary>Are there snapshots available for full / archive?</summary>

  <br />

  Not yet, but we are working on it.
</details>

<br />

### Common Issues

***

<details>
  <summary>Database Initialization Failure</summary>

  <br />

  **Error:**

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly !! err="create db: failed to initialize database:
  ```

  **Solution:**

  1. Save your validator state:

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
</details>

***

<details>
  <summary>High Gas Fees</summary>

  **Problem:** Need to adjust gas fees on RPC node

  **Solution:**
  Add the `--rpc.txfee` flag to your geth startup command:

  ```bash

  sudo tee /etc/systemd/system/story-geth.service > /dev/null <<EOF
  [Unit]
  Description=Story-Geth Node
  After=network.target

  [Service]
  User=$USER
  Type=simple
  WorkingDirectory=$HOME/.story/geth
  ExecStart=$(which geth) --story --syncmode full --rpc.txfee 2
  Restart=on-failure
  LimitNOFILE=65535

  [Install]
  WantedBy=multi-user.target
  EOF
  ```
</details>

***

<details>
  <summary>Failed to send PacketPing</summary>

  <br />

  **Error:**

  ```bash
  ERRO Failed to send PacketPing module=p2p peer=19fa6dd52e72e4e85bbb873b705282cf73217a6b@158.220.80.96:40128 err="write tcp 139.59.139.135:26656->158.220.80.96:40128: write: broken pipe"
  ```

  Solution:

  * If the node is synchronized, you can ignore this error. Your client may be a little behind.
  * If the node stops, you should restart the services.
</details>

***

<details>
  <summary>Cosmovisor: failed to read upgrade info</summary>

  <br />

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

  If you don\`t have create new one:

  ```bash
  echo '{"name":"v0.13.0","time":"0001-01-01T00:00:00Z","height":858000}' > $HOME/.story/story/cosmovisor/upgrades/v0.13.0/upgrade-info.json
  ```

  Find out more about automatic updates with cosmovisor [here](https://docs.story.foundation/docs/odyssey-node-setup#automated-upgrades).
</details>

***

<details>
  <summary>IPC endpoint closed</summary>

  <br />

  Error:

  ```bash
  INFO HTTP server stopped
  INFO IPC endpoint closed
  ```

  Solution:

  * It looks like port 8551 stopping, the background process running `iptables` blocking ip and port and access posix.
  * For solution try uninstall `ufw posix` and `iptables`:

  ```bash
  iptables -I INPUT -s localhost -j ACCEPT
  ```
</details>

***

<details>
  <summary>Found signature from the same key</summary>

  <br />

  Error:

  ```bash
  panic: Faile to consensus  state: found signature from the same key
  ```

  Solution:

  * The validator has been double signed. It is currently not possible to restore the validator after it has been double signed.
  * To avoid such situations, see this post on how to correctly [migrate a validator to another machine](https://docs.story.foundation/docs/odyssey-validator-operations#migrating-a-validator-to-another-machine).
</details>

***

<details>
  <summary>Failed to validate create flags: missing required flag(s): moniker</summary>

  <br />

  Error:

  ```bash
  4-11-26 08:42:20.302 ERRO !! Fatal error occurred, app died️ unexpectedly !! err="failed to validate create flags: missing required flag(s): moniker" stacktrace="[errors.go:39 flags.go:173 validator.go:168 validator.go:384 command.go:985 command.go:1117 command.go:1041 command.go:1034 cmd.go:34 main.go:10 proc.go:271 asm_amd64.s:1695]"
  ```

  Solution:

  * You missed flag `--moniker`.
  * The command to create a new validator should look like this:

  ```bash
  ./story validator create --stake ${AMOUNT_TO_STAKE_IN_WEI} --moniker ${VALIDATOR_NAME}
  ```

  See more options [here](https://docs.story.foundation/docs/odyssey-validator-operations#validator-creation).
</details>

***

<details>
  <summary>Error adding vote</summary>

  <br />

  Error:

  ```bash
  ERRO failed to process message msg_type= *consensus.VoteMessage err:" error adding vote"
  ```

  Solution:

  * It looks like your node is down. To get started, check the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
  * If you have up-to-date binary - try updating peers, this usually happens when a node loses p2p communication:

  ```bash
  PEERS="..."
  sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Error signing vote</summary>

  <br />

  Error:

  ```bash
  ERRO failed signing vote module=consensus height=403750 round=0 vote="Vote{23:B12C6AE31E8E 403750/00/SIGNED_MSG_TYPE_PREVOTE(Prevote) FA591EB1E540 000000000000 000000000000 @ 2024-11-08T16:58:10.375918193Z}" err="error signing vote: height regression. Got 403750, last height 420344"
  ```

  Solution:

  * Looks like you have a problem with your `priv_validator_state` of validator.
    > 🚧 Be very careful with this file, especially if your validator is already signing blocks.
  * You can make a copy of your state with a command:

  ```bash
  cp $HOME/.story/story/data/priv_validator_state.json $HOME/.story/story/priv_validator_state.json.backup
  ```

  Check your validator state:

  ```bash
  cat $HOME/.story/story/data/priv_validator_state.json
  ```

  * If you get this error, you can reset your state (🚧 ONLY IF YOUR VALIDATOR HAS NOT YET SIGNET BLOCKS).
  * Stop node.

  ```bash
  sudo tee $HOME/.story/story/data/priv_validator_state.json > /dev/null <<EOF
  {
    "height": "0",
    "round": 0,
    "step": 0
  }
  EOF
  ```

  * Start node.
</details>

***

<details>
  <summary>Unknown flag: --home</summary>

  <br />

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly !! err="unknown flag: --home"
  ```

  Solution:

  * It looks like a misconfiguration. You must try to remove the `--home` flag from the startup command.
  * Your systemd to run might look like this:
</details>

***

<details>
  <summary>Failed to register the Ethereum service</summary>

  <br />

  Error:

  ```bash
  Fatal: Failed to register the Ethereum service: incompatible state scheme, stored: path, provided: hash
  ```

  Solution:

  * You have problems with the state of validator or a corrupted database.
  * Try using a snapshot.
    > 🚧 Be very careful with this file, especially if your validator is already signing blocks.
  * We have described how to reset your state [here](https://docs.story.foundation/docs/troubleshooting#error-signing-vote).

  ## Failed to reconnect to peer

  Error:

  ```bash
  24-09-25 06:38:45.235 ERRO Failed to reconnect to peer. Beginning exponential backoff module=p2p addr=e0600fa5f2129e647ef30a942aac1695201ff135@65.109.115.98:26656 elapsed=2m29.598884906s
  ```

  Solution:

  * If the node is synchronized and not far behind, you can ignore this error.
  * If the node is lagging or has stopped completely, try updating peers, this usually happens when a node loses p2p communication:

  ```bash
  PEERS="..."
  sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers =./persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Processing finalized payload halted while evm syncing</summary>

  <br />

  Warn:

  ```bash
  WARN Processing finalized payload halted while evm syncing (will retry) payload_height=...
  ```

  Solution:

  * It just means that story-geth is syncing, you can ignore this warn.
  * However, if it takes a long time, we recommend that you stop the processes one at a time and start them again later in the following order:

  ```bash
  sudo systemctl stop story-geth story
  sudo systemctl daemon-reload
  sudo systemctl start story-geth
  sudo systemctl enable story-geth

  sudo systemctl daemon-reload
  sudo systemctl start story
  sudo systemctl enable story
  ```
</details>

***

<details>
  <summary>Upgrade handler is missing</summary>

  <br />

  Error:

  ```bash
  ERRO error in proxyAppConn.FinalizeBlock      module=consensus err="module manager preblocker: wrong app version 0, upgrade handler is missing for upgrade plan"
  ```

  Solution:

  * Looks like you missed an update.
  * To get started, check the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
</details>

***

<details>
  <summary>Home directory contains unexpected file</summary>

  <br />

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly !! err="home directory contains unexpected file(s), use --force to initialize anyway"
  ```

  Solution:

  * This means that you have already initialized the node.
  * `$HOME/.story/story` directory created, and there are files in it. Delete it, or try with it.
</details>

***

<details>
  <summary>Err="create comet node: create node</summary>

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly ! err="create comet node: create node
  ```

  Solution:

  * It appears that your node is using incorrect versions.
  * Check the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
  * And most likely you need to perform a rollback binary to current versions.
</details>

***

<details>
  <summary>WAL does not contain</summary>

  <br />

  Error:

  ```bash
  ERRO catchup replay: WAL does not contain
  ```

  Solution:

  * Looks like an `AppHash` issue.
  * To get started, upgrade to the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
  * If your versions are newer than the current ones, perform a rollback.
</details>

***

<details>
  <summary>Err="load engine JWT file: read jwt file</summary>

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly !! err="load engine JWT file: read jwt file: open /root/.story/geth/odyssey/geth/jwtsecret: no such file or directory
  ```

  Solution:

  * It seems your node can't get `jwtsecret`.
  * Check your `WorkingDirectory` in your `geth-service` , by default `WorkingDirectory=$HOME/.story/geth`.
  * Check all paths, you can get your `jwtsecret`with command (for odyssey network):

  ```bash
  cat .story/geth/odyssey/geth/jwtsecret
  ```
</details>

***

<details>
  <summary>Couldn't connect to any seeds</summary>

  <br />

  Error:

  ```bash
  ERRO Couldn't connect to any seeds module=p2p
  ```

  Solution:

  * If the node is synchronized and not far behind, you can ignore this error.
  * If the node is lagging or has stopped completely, try updating seeds/peers, it usually happens when a node loses p2p communication (we recommend that you stop the node and delete the addrbook).

  ```bash
  rm -rf $HOME/.story/story/config/addrbook.json
  SEEDS="..."
  PEERS="..."
  sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*seeds *=.*/seeds = \"$SEEDS\"/}" \
         -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Processing finalized payload failed err="rpc forkchoice updated</summary>

  <br />

  Warn:

  ```bash
  WRN Processing finalized payload; evm syncing
  WRN Processing finalized payload failed: evm fork choice update (will retry) status="" err="rpc forkchoice updated v3: beacon syncer reorging"
  ```

  Solution:

  * Everything is fine, it just means that `story-geth` is syncing, which takes some time.
  * If the node is not far behind, you can ignore this warning.

  ## Dial tcp 127.0.0.1:9090

  Warn:

  ```bash
  WRN error getting latest block error:"rpc error: dial tcp 127.0.0.1:9090"
  ```

  Solution:

  * The logs show a connection failure on port `9090`.
  * Check the listening ports:

  ```bash
  sudo ss -tulpn  | grep LISTEN
  ```

  * If other node uses `9090`, then modify it to another.
  * Normally, this WARNING should not affect the performance of your node.
</details>

***

<details>
  <summary>Wrong AppHash</summary>

  Error:

  ```bash
  ERRO Error in validation module=blocksync err="wrong Block[dot]Header[dot]AppHash  Expected [...]
  ```

  Solution:

  * `Wrong AppHash` type logs means the story node version you are using is wrong.
  * Upgrade to the current versions of the binaries [here](https://docs.story.foundation/docs/odyssey-node-setup).
  * If your versions are newer than the current ones, perform a rollback.
</details>

***

<details>
  <summary>Connection failed sendRoutine / Stopping peer</summary>

  Error:

  ```bash
  ERRO Connection failed @ sendRoutine module=p2p peer=...
  ERRO Stopping peer for error module=p2p peer=...
  ```

  Solution:

  * If the node is synchronized and not far behind, you can ignore this error.
  * If the node is lagging or has stopped completely, try updating peers, this usually happens when a node loses p2p communication:

  ```bash
  PEERS="..."
  sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers =./persistent_peers = \"$PEERS\"/}" $HOME/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Moniker must be valid non-empty</summary>

  Error:

  ```bash
  ERRO !! Fatal error occurred, app died️ unexpectedly ! err="create comet node: create node: info.Moniker must be valid non-empty
  ```

  Solution:

  * Looks like a problem with your node moniker.
  * Be sure to use `""` when executing init:

  ```bash
  story init --network "..." --moniker "..."
  ```

  * Go to config, find the moniker and put it inside `""` only:

  ```bash
  sudo nano ~/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Invalid address (26656)</summary>

  Error:

  ```bash
  Fatal error occurred, app died️ unexpectedly ! err="create comet node: create node: invalid address (26656):
  ```

  Solution:

  * The logs report a connection failure on port `26656`.
  * Check the listening ports:

  ```bash
  sudo ss -tulpn  | grep LISTEN
  ```

  * If another node is using `26656`, change it to another and keep the default `26656` for story in the `P2P configuration` options in `config`:

  ```bash
  sudo nano ~/.story/story/config/config.toml
  ```
</details>

***

<details>
  <summary>Eth\_coinbase does not exist</summary>

  Warn:

  ```bash
  WARN Beacon client online, but no consensus updates received in a while. Please fix your beacon client to follow the chain!
  Served eth_coinbase eth_coinbase does not exist
  ```

  Solution:

  * This error indicates that the network has stopped.
</details>

***

<details>
  <summary>Verifying proposal failed</summary>

  Warn:

  ```bash
  WARN Verifying proposal failed: push new payload to evm (will retry) status="" err="new payload: rpc new payload v3: Post \"http://localhost:8551\": round trip: dial tcp 127.0.0.1:8551: connect: connection refused" stacktrace="[errors.go:39 jwt.go:41 client.go:259 client.go:180 client.go:724 client.go:590 http.go:229 http.go:173 client.go:351 engineclient.go:101 msg_server.go:183 proposal_server.go:34 helpers.go:30 proposal_server.go:33 tx.pb.go:299 msg_service_router.go:175 tx.pb.go:301 msg_service_router.go:198 prouter.go:74 abci.go:520 cmt_abci.go:40 abci.go:85 local_client.go:164 app_conn.go:89 execution.go:166 state.go:1381 state.go:1338 state.go:2055 state.go:910 state.go:836 asm_amd64.s:1695]"
  WARN Verifying proposal
  ```

  Solution:

  * It looks like port 8551 stopping, the background process running `iptables` blocking ip and port and access posix.
  * For solution try uninstall `ufw posix` and `iptables`:

  ```bash
  iptables -I INPUT -s localhost -j ACCEPT
  ```
</details>