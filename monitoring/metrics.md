# XRPL EVM Consensus System Metrics Documentation

## Introduction

This document provides a comprehensive reference for the consensus metrics used in the **XRPL EVM** project, which integrates the Ethereum Virtual Machine (EVM) with the XRP Ledger (XRPL). These metrics, collected from the **CometBFT** consensus engine, are crucial for monitoring the performance, security, and health of validator nodes within the network.

The document is structured into different metric categories, including block synchronization, validator activity, transaction processing, memory and CPU usage, network performance, and mempool statistics. Each metric is explained with its description, value interpretation, and example values.

By leveraging these metrics, network operators and developers can ensure optimal performance, detect potential issues, and maintain a secure and efficient blockchain environment.

For ease of navigation, please refer to the **Table of Contents** below.

## Table of Contents

- [SYNC STATUS](#sync-status)
- [BLOCK GOSSIP STATUS](#block-gossip-status)
- [BLOCK SIZE](#block-size)
- [BYZANTINE VALIDATORS](#byzantine-validators)
- [MISSING VALIDATORS](#missing-validators)
- [CHAIN SIZE](#chain-size)
- [TRANSACTIONS](#transactions)
- [DUPLICATE EVENTS](#duplicate-events)
- [LATE VOTES](#late-votes)
- [PROPOSALS](#proposals)
- [ROUND VOTING POWER PERCENT](#round-voting-power-percent)
- [NUMBER OF ROUNDS](#number-of-rounds)
- [TOTAL TRANSACTIONS](#total-transactions)
- [VALIDATORS](#validators)
- [MEMPOOL METRICS](#mempool-metrics)
- [NETWORK PEERS](#network-peers)
- [BLOCK PROCESSING TIME](#block-processing-time)
- [CONSENSUS PARAMETER UPDATES](#consensus-parameter-updates)
- [CPU USAGE](#cpu-usage)
- [FILE DESCRIPTORS](#file-descriptors)
- [NETWORK USAGE](#network-usage)
- [MEMORY USAGE](#memory-usage)
- [PROCESS START TIME](#process-start-time)
- [HOW TO USE THESE METRICS](#how-to-use-these-metrics)
- [EXAMPLE PROMETHEUS OUTPUT](#example-prometheus-output)

_______________________________________

## SYNC STATUS
### Metric: `cometbft_blocksync_syncing`

**Description:**  
A metric that indicates whether a node in the XPRL EVM consensus system is syncing blocks. When this metric has a value of **1**, it means that the node is actively syncing blocks. Conversely, a value of **0** indicates that the node is not currently syncing blocks.

**Value Interpretation:**

| Value | Status |
|--------|--------------|
| `1`   | SYNC (syncing blocks) |
| `0`   | NO SYNC (not syncing blocks) |

---

## BLOCK GOSSIP STATUS
### Metric: `cometbft_consensus_block_gossip_parts_received`

**Description:**  
This metric represents the number of block parts received by a node. These parts are categorized based on whether they belong to the block the node is currently trying to assemble.

**Value Interpretation:**  

| Labels | Description |
|------------------------|----------------------------------------------|
| `matches_current="true"`  | Number of block parts received that belong to the block the node is assembling. |
| `matches_current="false"` | Number of block parts received that do not belong to the block the node is assembling. |

---

## BLOCK SIZE
### Metric: `cometbft_consensus_block_size_bytes`

**Description:**  
Indicates the size of the latest block in bytes.

**Example Value:**  
- `4929` bytes (for `chain_id="exrp_1440002-1"`)

---

## BYZANTINE VALIDATORS
### Metric: `cometbft_consensus_byzantine_validators`

**Description:**  
Represents the number of validators that attempted to double sign.

**Example Value:**  
- `0` (no byzantine validators detected)

### Metric: `cometbft_consensus_byzantine_validators_power`

**Description:**  
Indicates the total power of the byzantine validators.

**Example Value:**  
- `0` (no power attributed to byzantine validators)

---

## MISSING VALIDATORS
### Metric: `cometbft_consensus_missing_validators`

**Description:**  
Represents the number of validators who failed to sign.

**Example Value:**  
- `1` validator missed signing.

### Metric: `cometbft_consensus_missing_validators_power`

**Description:**  
Indicates the total power of the validators who missed signing.

**Example Value:**  
- `1` (total voting power of missing validators)

---

## CHAIN SIZE
### Metric: `cometbft_consensus_chain_size_bytes`

**Description:**  
Represents the total size of the blockchain in bytes.

**Example Value:**  
- `77858` bytes (for `chain_id="exrp_1440002-1"`)


## LATEST BLOCK HEIGHT
### Metric: `cometbft_consensus_latest_block_height`

**Description:**  
Indicates the latest confirmed block height in the blockchain.

**Example Value:**  
- `14413293` blocks (for `chain_id="exrp_1440002-1"`)

### Metric: `cometbft_consensus_height`

**Description:**  
Represents the current height of the blockchain.

**Example Value:**  
- `14413294` blocks (for `chain_id="exrp_1440002-1"`)

---

## TRANSACTIONS
### Metric: `cometbft_consensus_num_txs`

**Description:**  
Indicates the number of transactions included in the latest block.

**Example Value:**  
- `1` transaction in the last block.

---

## DUPLICATE EVENTS
### Metric: `cometbft_consensus_duplicate_block_part`

**Description:**  
Indicates the number of times the node received a duplicate block part.

**Example Value:**  
- `34` duplicate block parts detected.

### Metric: `cometbft_consensus_duplicate_vote`

**Description:**  
Represents the number of times a duplicate vote was received.

**Example Value:**  
- `3022` duplicate votes received.

---

## LATE VOTES
### Metric: `cometbft_consensus_late_votes`

**Description:**  
Stores the number of votes received by this node that correspond to earlier heights and rounds than the node is currently in.

**Value Interpretation:**  

| Vote Type | Description |
|-----------------|-------------------------------------------|
| `precommit` | Late precommit votes received. |
| `prevote`   | Late prevote votes received. |

**Example Values:**  

| `chain_id="exrp_1440002-1"` | `vote_type` | Value |
|----------------------|-------------|--------|
| exrp_1440002-1 | `precommit` | `1752` |
| exrp_1440002-1 | `prevote` | `149` |

---

## PROPOSALS
### Metric: `cometbft_consensus_proposal_receive_count`

**Description:**  
Represents the total number of proposals received by this node since process start. The metric is annotated by the status of the proposal from the application, either **accepted** or **rejected**.

**Example Value:**  
- `18` proposals accepted.

## ROUND VOTING POWER PERCENT
### Metric: `cometbft_consensus_round_voting_power_percent`

**Description:**  
Represents the percentage of the total voting power received in a round. The value starts at `0` for each round and increases towards `1.0` as additional voting power is observed.  

**Value Interpretation:**  

| Vote Type | Description |
|-----------------|-------------------------------------------|
| `precommit` | Percentage of voting power received in precommit phase. |
| `prevote`   | Percentage of voting power received in prevote phase. |

**Example Values:**  

| `chain_id="exrp_1440002-1"` | `vote_type` | Value |
|----------------------|-------------|--------|
| exrp_1440002-1 | `precommit` | `2.03%` |
| exrp_1440002-1 | `prevote` | `2.79%` |

---

## NUMBER OF ROUNDS
### Metric: `cometbft_consensus_rounds`

**Description:**  
Indicates the number of rounds the consensus process has gone through.

**Example Value:**  
- `1` round completed.

## TOTAL TRANSACTIONS
### Metric: `cometbft_consensus_total_txs`

**Description:**  
Indicates the total number of transactions processed in the blockchain.

**Example Value:**  
- `2` total transactions recorded.

---

## VALIDATORS
### Metric: `cometbft_consensus_validators`

**Description:**  
Represents the total number of validators participating in the consensus process.

**Example Value:**  
- `34` validators.

### Metric: `cometbft_consensus_validators_power`

**Description:**  
Indicates the total voting power of all validators combined.

**Example Value:**  
- `34` total voting power.

---

## MEMPOOL METRICS
### Metric: `cometbft_mempool_active_outbound_connections`

**Description:**  
Represents the number of active outbound connections used for gossiping transactions (experimental feature).

**Example Value:**  
- `7` active outbound connections.

### Metric: `cometbft_mempool_size`

**Description:**  
Indicates the number of uncommitted transactions currently in the mempool.

**Example Value:**  
- `0` transactions in mempool.

### Metric: `cometbft_mempool_size_bytes`

**Description:**  
Represents the total size of the mempool in bytes.

**Example Value:**  
- `0` bytes in mempool.

## NETWORK PEERS
### Metric: `cometbft_p2p_peers`

**Description:**  
Indicates the number of peers currently connected to the node.

**Example Value:**  
- `7` peers connected.

---

## BLOCK PROCESSING TIME
### Metric: `cometbft_state_block_processing_time`

**Description:**  
Represents the time spent processing `FinalizeBlock`. This metric is recorded as a histogram, showing the number of blocks processed within different time ranges.

**Value Interpretation:**  

| Processing Time (seconds) | Number of Blocks Processed |
|--------------------------|----------------------------|
| `≤ 1`   | 0  |
| `≤ 11`  | 1  |
| `≤ 21`  | 13 |
| `≤ 31`  | 18 |
| `≤ 41`  | 19 |
| `≤ 51`  | 19 |
| `≤ 61`  | 19 |
| `≤ 71`  | 19 |
| `≤ 81`  | 19 |
| `≤ 91`  | 19 |
| `+Inf`  | 19 |

**Additional Metrics:**
- **Total Processing Time:** `391.85` seconds.
- **Total Blocks Processed:** `19`.

---

## CONSENSUS PARAMETER UPDATES
### Metric: `cometbft_state_consensus_param_updates`

**Description:**  
Represents the number of consensus parameter updates returned by the application since the process started.

**Example Value:**  
- `19` updates.

## CPU USAGE
### Metric: `process_cpu_seconds_total`

**Description:**  
Represents the total user and system CPU time spent by the process in seconds.

**Example Value:**  
- `25.81` seconds.

---

## FILE DESCRIPTORS
### Metric: `process_max_fds`

**Description:**  
Indicates the maximum number of file descriptors that the process can open.

**Example Value:**  
- `65535` file descriptors.

### Metric: `process_open_fds`

**Description:**  
Represents the number of file descriptors currently open by the process.

**Example Value:**  
- `119` open file descriptors.

---

## NETWORK USAGE
### Metric: `process_network_receive_bytes_total`

**Description:**  
Indicates the total number of bytes received by the process over the network.

**Example Value:**  
- `8.88 TB` (`8.888071873671e+12` bytes).

### Metric: `process_network_transmit_bytes_total`

**Description:**  
Indicates the total number of bytes sent by the process over the network.

**Example Value:**  
- `2.83 TB` (`2.83214477635e+12` bytes).

---

## MEMORY USAGE
### Metric: `process_resident_memory_bytes`

**Description:**  
Represents the resident memory size (RAM) currently used by the process in bytes.

**Example Value:**  
- `294.38 MB` (`2.94375424e+08` bytes).

### Metric: `process_virtual_memory_bytes`

**Description:**  
Indicates the total virtual memory size used by the process in bytes.

**Example Value:**  
- `3.73 GB` (`3.730767872e+09` bytes).

### Metric: `process_virtual_memory_max_bytes`

**Description:**  
Indicates the maximum amount of virtual memory available to the process in bytes.

**Example Value:**  
- `18.44 EB` (`1.8446744073709552e+19` bytes).

---

## PROCESS START TIME
### Metric: `process_start_time_seconds`

**Description:**  
Represents the start time of the process since the Unix epoch, in seconds.

**Example Value:**  
- `1.73901610886e+09` (approximately `2025-02-08 00:21:48 UTC`).








---

## How to Use These Metrics
- **Monitor synchronization status:** Use `cometbft_blocksync_syncing` to check if a node is syncing.
- **Check block size trends:** Analyze `cometbft_consensus_block_size_bytes` to monitor storage growth.
- **Detect byzantine behavior:** Use `cometbft_consensus_byzantine_validators` and `cometbft_consensus_byzantine_validators_power` to check for malicious validators.
- **Observe blockchain growth:** Monitor `cometbft_consensus_chain_size_bytes`, `cometbft_consensus_height`, and `cometbft_consensus_latest_block_height`.
- **Identify network inefficiencies:** Track `cometbft_consensus_duplicate_block_part` and `cometbft_consensus_duplicate_vote` to detect excessive duplicate messages.
- **Analyze validator participation:** Use `cometbft_consensus_missing_validators` and `cometbft_consensus_missing_validators_power` to detect absent validators.
- **Evaluate proposal processing:** `cometbft_consensus_proposal_receive_count` can indicate how well the network handles proposals.

---

### Example Prometheus Output
```plaintext
# HELP cometbft_consensus_late_votes LateVotes stores the number of votes that were received by this node that correspond to earlier heights and rounds than this node is currently in.
# TYPE cometbft_consensus_late_votes counter
cometbft_consensus_late_votes{chain_id="exrp_1440002-1",vote_type="precommit"} 1752
cometbft_consensus_late_votes{chain_id="exrp_1440002-1",vote_type="prevote"} 149

# HELP cometbft_consensus_latest_block_height The latest block height.
# TYPE cometbft_consensus_latest_block_height gauge
cometbft_consensus_latest_block_height{chain_id="exrp_1440002-1"} 1.4413293e+07

# HELP cometbft_consensus_proposal_receive_count ProposalReceiveCount is the total number of proposals received by this node since process start.
# TYPE cometbft_consensus_proposal_receive_count counter
cometbft_consensus_proposal_receive_count{chain_id="exrp_1440002-1",status="accepted"} 18
