---
title: Node Upgrade
deprecated: false
hidden: false
metadata:
  robots: index
---
There are three types of upgrades

1. Upgrade the story geth client
2. Upgrade the story client manually
3. Schedule the upgrade with Cosmovisor

### Upgrade the story geth client

```bash
# Stop the services
sudo systemctl stop story
sudo systemctl stop story-geth

# Download the new binary
wget ${STORY_GETH_BINARY_URL}
sudo mv ./geth-linux-amd64 story-geth
sudo chmod +x story-geth
sudo mv ./story-geth $HOME/go/bin/story-geth
source $HOME/.bashrc

# Restart the service
sudo systemctl start story-geth
sudo systemctl start story
```

### Upgrade the story client manually

```bash
# Stop the service
sudo systemctl stop story

# Download the new binary
wget ${STORY_BINARY_URL}
sudo mv story-linux-amd64 story
sudo chmod +x story
sudo mv ./story $HOME/go/bin/story

# Schedule the update
sudo systemctl start story
```

### Schedule the upgrade with Cosmovisor

The following steps outline how to schedule an upgrade using Cosmovisor:

1. Create the upgrade directory and download the new binary

```bash
# Download the new binary
wget ${STORY_BINARY_URL}

# Schedule the upgrade
source $HOME/.bash_profile
cosmovisor add-upgrade ${UPGRADE_NAME} ${UPGRADE_PATH} \
  --force \
  --upgrade-height ${UPGRADE_HEIGHT}
```

2. Verify the upgrade configuration

```bash
# Check the upgrade info
cat $HOME/.story/data/upgrade-info.json
```

The upgrade-info.json should show:

```json
{
  "name": "v1.0.0",
  "time": "2025-02-05T12:00:00Z",
  "height": 858000
}
```

3. Monitor the upgrade

```bash
# Watch the node logs for the upgrade
journalctl -u story -f -o cat
```

Note: Cosmovisor will automatically handle the binary switch once the specified block height is reached. Before the upgrade, confirm that your node is fully synced and has enough disk space available.