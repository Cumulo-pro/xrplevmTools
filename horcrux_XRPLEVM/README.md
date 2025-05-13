# How to Implement Horcrux with XRPL EVM

This guide walks you through deploying **Horcrux**, a distributed signing (MPC) tool that protects your XRPL EVM validator private key by splitting it across multiple nodes. We will use a setup with **3 signer nodes (cosigners)** and **3 sentry nodes** to achieve fault tolerance and high availability.

## Table of Contents

* [Introduction](#introduction)
* [Architecture](#architecture)

  * [Deployment Model](#deployment-model)
  * [Hardware Requirements](#hardware-requirements)
* [Network and Firewall Setup](#network-and-firewall-setup)
* [1. Horcrux Installation](#1-horcrux-installation)
* [2. Initialize Shared Configuration](#2-initialize-shared-configuration)
* [3. Generate Key Shares](#3-generate-key-shares)

  * [ECIES Communication](#ecies-communication)
  * [Validator Key Shards](#validator-key-shards)
* [4. Share Consensus State](#4-share-consensus-state)

  * [Stop the Original Validator Node](#stop-the-original-validator-node)
* [5. Create Horcrux Systemd Service](#5-create-horcrux-systemd-service)
* [6. Configure Sentry Nodes](#6-configure-sentry-nodes)

  * [Enable Remote Validator](#enable-remote-validator)
* [Verification](#verification)
* [Best Practices](#best-practices)
* [Useful Resources](#useful-resources)

## Introduction

Validator private key security is one of the most sensitive aspects of node operation. A leaked, lost, or duplicated signature can have serious consequences—from fund loss to validator slashing. **Horcrux**, developed by Strangelove Labs, implements a threshold signature scheme (TSS) to distribute signing across multiple nodes. Originally for Cosmos SDK, it is now compatible with networks like **XRPL EVM**.

Horcrux splits the validator private key into multiple shards and delegates block signing to a cluster of signers. This eliminates single points of failure and enables **high availability**. Even if one signer fails, block signing continues securely.

**XRPL EVM** is an Ethereum-compatible chain built on the XRP Ledger infrastructure. It combines XRP's speed and liquidity with the flexibility of the EVM, enabling smart contracts in Solidity. For validators, secure consensus participation is key. Horcrux is a robust, secure, and modular way to achieve it.

## Architecture

### Deployment Model

* **3 signer nodes** – distribute signatures via MPC
* **3 sentry nodes** – act as a secure interface between the network and signers

Each signer connects to all sentries through the private validator port.

![Horcrux XRPL EVM Setup](link-to-diagram.png)

### Hardware Requirements

| Node Type | Recommended Specs                            | Suggested Location               |
| --------- | -------------------------------------------- | -------------------------------- |
| Sentry    | Baremetal (per XRPL EVM specs)               | Public datacenters               |
| Signer    | VPS: 4 vCore / 4 GB RAM / 80 GB SSD / 1 Gbps | Different regions for resilience |

## Network and Firewall Setup

### Signers

* Open port `2222` for signing requests only
* Only SSH (`22`) and Prometheus (optional) should remain open
* All other ports should be closed to the public

### Sentry Nodes

* Expose ports `1234`, `1235`, `1236` **only to signer IPs**
* Open `26656` to the public for P2P discovery

> **Important:** `priv_validator_key.json` **must not exist** on sentry nodes

## 1. Horcrux Installation

```bash
mkdir horcrux_xrpl
cd horcrux_xrpl

wget https://github.com/strangelove-ventures/horcrux/releases/download/v3.2.3/horcrux_linux-amd64
mv horcrux_linux-amd64 horcrux
sudo cp horcrux /usr/local/bin/
sudo chmod +x /usr/local/bin/horcrux

horcrux version
```

## 2. Initialize Shared Configuration

```bash
horcrux config init \
  --node "tcp://10.0.0.1:1234" \
  --node "tcp://10.0.0.2:1235" \
  --node "tcp://10.0.0.3:1236" \
  --cosigner "tcp://10.0.1.1:2222" \
  --cosigner "tcp://10.0.1.2:2222" \
  --cosigner "tcp://10.0.1.3:2222" \
  --threshold 2 \
  --grpc-timeout 1000ms \
  --raft-timeout 1000ms
```

## 3. Generate Key Shares

### ECIES Communication

```bash
horcrux create-ecies-shards --shards 3
```

Generates:

```
cosigner_1/ecies_keys.json
cosigner_2/ecies_keys.json
cosigner_3/ecies_keys.json
```

### Validator Key Shards

```bash
horcrux create-ed25519-shards \
  --chain-id xrplevm_1449000-1 \
  --key-file ./priv_validator_key.json \
  --threshold 2 \
  --shards 3
```

Results:

```
cosigner_1/xrplevm_1449000-1_shard.json
cosigner_2/xrplevm_1449000-1_shard.json
cosigner_3/xrplevm_1449000-1_shard.json
```

## 4. Share Consensus State

### Stop the Original Validator Node

```bash
sudo systemctl stop exrpd
```

### View the Current State

```bash
cat ~/.exrpd/data/priv_validator_state.json
```

Example:

```json
{
  "height": "361402",
  "round": 0,
  "step": 3,
  "signature": "...",
  "signbytes": "..."
}
```

### Create a Safe Truncated Version

```json
{
  "height": "361402",
  "round": 0,
  "step": 3
}
```

Copy to:

```bash
~/.horcrux/state/xrplevm_1449000-1_priv_validator_state.json
```

Repeat this on all signer nodes.

## 5. Create Horcrux Systemd Service

```bash
sudo tee /etc/systemd/system/horcrux.service > /dev/null <<EOF
[Unit]
Description=MPC Signer node
After=network-online.target

[Service]
User=$USER
WorkingDirectory=/home/$USER
ExecStart=$(which horcrux) start
Restart=on-failure
RestartSec=3
LimitNOFILE=4096

[Install]
WantedBy=multi-user.target
EOF

sudo systemctl daemon-reload
sudo systemctl enable horcrux
sudo systemctl start horcrux
sudo journalctl -u horcrux -f
```

## 6. Configure Sentry Nodes

### Enable Remote Validator

```bash
sed -i 's#priv_validator_laddr = ""#priv_validator_laddr = "tcp://0.0.0.0:1234"#g' ~/.exrpd/config/config.toml
```

Confirm:

```bash
grep priv_validator_laddr ~/.exrpd/config/config.toml
```

### Remove Local Private Key

```bash
rm ~/.exrpd/config/priv_validator_key.json
```

### Restart Node

```bash
sudo systemctl restart exrpd
journalctl -u exrpd -f
```

## Verification

```bash
journalctl -u horcrux -f
```

Expected output:

```
✅ Successfully signed proposal at height 361403
```

## Best Practices

* Restrict sentry validation ports to signer IPs only
* Securely back up all `*_shard.json`, `ecies_keys.json`, and `config.yaml`
* Keep track of which signer owns which shard file

## Useful Resources

* [Horcrux GitHub](https://github.com/strangelove-ventures/horcrux)
* [Cumulo Horcrux Architecture](https://github.com/Cumulo-pro/Horcrux-Architecture)
