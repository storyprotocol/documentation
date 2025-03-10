---
title: Operating Your Node
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
## **1. Setting Up a Geth Archive Node**
To run a Geth archive node, use `--gcmode=archive` instead of `--gcmode=full`. This ensures that Geth retains all historical blockchain state data, making it ideal for indexing services and blockchain analytics.

- `--syncmode=full`: Ensures a complete blockchain sync.
- `--gcmode=archive`: Retains full historical state data without pruning.

---

## **2. Enabling RPC (HTTP) and WebSocket in Geth**
### **HTTP (RPC) Options**
| Option | Description |
|--------|------------|
| `--http` | Enables the HTTP-RPC server. |
| `--http.addr=0.0.0.0` | Binds the HTTP server to all network interfaces. |
| `--http.port=8545` | Sets the HTTP-RPC port (default: 8545). |
| `--http.vhosts=*` | Allows requests from any domain (use with caution in production). |
| `--http.api=web3,eth,txpool,net,engine,debug` | Specifies the available APIs for HTTP requests. |

### **WebSocket (WS) Options**
| Option | Description |
|--------|------------|
| `--ws` | Enables the WebSocket server. |
| `--ws.addr=0.0.0.0` | Binds the WebSocket server to all network interfaces. |
| `--ws.port=8546` | Sets the WebSocket port (default: 8546). |
| `--ws.origins=*` | Allows WebSocket connections from any domain (use with caution in production). |
| `--ws.api=web3,eth,txpool,net,engine,debug` | Specifies the available APIs for WebSocket connections. |

These configurations ensure external applications can interact with the Geth node using both HTTP-RPC and WebSocket.

---

## **3. Monitoring Geth and Story Protocol**
### **Geth Monitoring Configuration**
- `--metrics`: Enables Prometheus-compatible metrics for Geth.
- `--metrics.addr=0.0.0.0`: Binds the metrics server to all interfaces.
- `--metrics.port=6060`: Exposes metrics on port `6060`.

### **Story Protocol Monitoring**
- Modify `config.toml` and set:
  ```toml
  prometheus = true
  ```
- The default Prometheus metrics port for Story Protocol is `26660`.

With these settings, both Geth and Story Protocol expose monitoring metrics that can be collected using Prometheus and visualized with Grafana.