# Fuel Consensus System Metrics Documentation

## SYNC STATUS
### Metric: `cometbft_blocksync_syncing`

**Description:**  
A metric that indicates whether a node in the Fuel consensus system is syncing blocks. When this metric has a value of **1**, it means that the node is actively syncing blocks. Conversely, a value of **0** indicates that the node is not currently syncing blocks.

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

## CHAIN METRICS
### Metric: `cometbft_consensus_chain_size_bytes`

**Description:**  
Represents the total size of the blockchain in bytes.

**Example Value:**  
- `77858` bytes (for `chain_id="exrp_1440002-1"`)

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
