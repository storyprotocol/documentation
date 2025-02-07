---
title: Run a Localnet
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

You can easily set up your own local Story network using docker compose, consisting of one boot node and four validator nodes. With this local network, you can test the consensus layer of the Story network or deploy your application using the precompiled primitive, the IP graph, to conduct various tests. Additionally, you can reset the network at any time as needed.

# Run a Local Story Network

> For more detailed information for running Story local network, please refer the repository:\
> [https://github.com/piplabs/story-localnet](https://github.com/piplabs/story-localnet)

## Prerequisite

To set up a local network, [Docker](https://docs.docker.com/get-started/get-docker/) is required.

## Step 1 - Start Docker

Please run Docker.

## Step 2 - Clone Repository

You need to clone three repositories: `story`, `story-geth`, and `story-localnet`.\
Make sure all three repositories are located within the same subfolder.

```bash
# clone repositories
git clone https://github.com/piplabs/story.git
git clone https://github.com/piplabs/story-geth.git
git clone https://github.com/piplabs/story-localnet.git
```

## Step 3 - Start Nodes

Navigate to story-localnet project and start the local network.

```bash
# move to story-localnet
cd story-localnet

# start story local network
./start.sh
```

## Step 4 - Terminate Nodes

If you want to stop the Story local network, you can do so by executing the script below.

```bash
# terminate story local network
./terminate.sh 
```

***

## How to Allocate Token to Your Account from Genesis

You may need to allocate IP tokens to your account for testing in the local network.\
To allocate tokens to your account in the genesis block, follow these steps:

1. Add your account information to the alloc section in `config/story/genesis-geth.json`:

```json
"<hex-encoded-account-address>": {
  "nonce": "0x0",
  "balance": "<hex-encoded-balance>",
  "code": "0x",
  "storage": {}
}
```

2. Run the `update-genesis-hash.sh` script to update the genesis block hash:

```bash
./update-genesis-hash.sh
```

***

## How to interact with Story Local Network

By default, the Story local network has the following ports open for interaction.

| **Port**  | **Service** | **Role**                                                                |
| --------- | ----------- | ----------------------------------------------------------------------- |
| **8545**  | story-geth  | endpoint of RPC server for Story execution client.                      |
| **1317**  | story-node  | endpoint of API server for interacting with the Story consensus client. |
| **26657** | story-node  | endpoint of cosmos-sdk RPC server for Story consensus client.           |

***

## Monitoring Systems

This setup includes a monitoring stack to provide centralized metrics and logs\
visualization for the blockchain network. Tools include **Prometheus**,
**Loki**, **Promtail**, and **Grafana**, all integrated through Docker Compose.

### **Components and Access Information**

| **Service**    | **Role**                                                           | **Default Port**               | **Access URL**          |
| -------------- | ------------------------------------------------------------------ | ------------------------------ | ----------------------- |
| **Prometheus** | Collects metrics from nodes and itself for performance monitoring. | `9090`                         | `http://localhost:9090` |
| **Loki**       | Aggregates and stores logs from the network nodes via Promtail.    | `3100`                         | `http://localhost:3100` |
| **Promtail**   | Scrapes logs from Docker containers and sends them to Loki.        | `9080` (API), `9095` (Metrics) | `http://localhost:9080` |
| **Grafana**    | Provides a dashboard interface for metrics and logs visualization. | `3000`                         | `http://localhost:3000` |