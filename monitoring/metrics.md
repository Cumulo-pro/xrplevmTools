# Fuel Consensus System Metrics Documentation

## SYNC STATUS
### Metric: `cometbft_blocksync_syncing`

**Description:**  
A metric that indicates whether a node in the Fuel consensus system is syncing blocks. When this metric has a value of **1**, it means that the node is actively syncing blocks. Conversely, a value of **0** indicates that the node is not currently syncing blocks.

Block synchronization is the process by which a node updates its copy of the blockchain by downloading the most recent blocks from other nodes in the network.

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

Block gossiping is a key mechanism in the consensus process where nodes share block data with each other to ensure everyone has the latest transactions and block information.

**Value Interpretation:**

| Labels | Description |
|------------------------|----------------------------------------------|
| `matches_current="true"`  | Number of block parts received that belong to the block the node is assembling. |
| `matches_current="false"` | Number of block parts received that do not belong to the block the node is assembling. |

**Example Values:**  

| `chain_id="exrp_1440002-1"` | `matches_current` | Value |
|----------------------|----------------|--------|
| exrp_1440002-1 | `"false"` | `1` |
| exrp_1440002-1 | `"true"` | `53` |

---

## How to Use These Metrics
- **Monitoring block synchronization:**  
  Use `cometbft_blocksync_syncing` to check if a node is actively syncing or fully caught up.  
- **Analyzing block gossiping efficiency:**  
  `cometbft_consensus_block_gossip_parts_received` helps determine how well the node is receiving relevant block data, which can indicate network health and efficiency.

---

### Example Prometheus Output
```plaintext
# HELP cometbft_blocksync_syncing Whether or not a node is block syncing. 1 if yes, 0 if no.
# TYPE cometbft_blocksync_syncing gauge
cometbft_blocksync_syncing{chain_id="exrp_1440002-1"} 0

# HELP cometbft_consensus_block_gossip_parts_received Number of block parts received by the node, separated by whether the part was relevant to the block the node is trying to gather or not.
# TYPE cometbft_consensus_block_gossip_parts_received counter
cometbft_consensus_block_gossip_parts_received{chain_id="exrp_1440002-1",matches_current="false"} 1
cometbft_consensus_block_gossip_parts_received{chain_id="exrp_1440002-1",matches_current="true"} 53
