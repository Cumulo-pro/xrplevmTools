`# XRPL EVM Benchmarking Tool by Cumulo

📡 Live Metrics Endpoint: [http://51.79.78.121:9200/metrics](http://51.79.78.121:9200/metrics)  
🔗 Grafana [Snapshot](https://snapshots.raintank.io/dashboard/snapshot/8IGNlPIAeHnqN1EVIHrL8gaRjvXAJ7Jg?orgId=0&refresh=5s&from=now-24h&to=now) dashboard  
🔗 Grafana [Dashboard](https://github.com/Cumulo-pro/xrplevmTools/blob/main/Benchmarking%20Tool/XRPL%20EVM%20Benchmarking%20Tool%20by%20Cumulo-1744652994792.json)

## 🧠 Project Objective

Measure the technical performance of the XRPL EVM Testnet through an automated benchmarking script that:
- Sends real EVM-compatible transactions
- Records inclusion time (latency)
- Measures gas used per transaction
- Calculates averages, success rates, and standard deviation
- Exposes all metrics to Prometheus for Grafana dashboards

## 🧹 Motivation

XRPL EVM is a sidechain of the XRP Ledger enabling EVM-compatible smart contracts. This benchmarking tool helps assess:
- RPC reliability and latency
- Transaction inclusion speed
- Gas usage per transaction
- Core metrics for DApp developers, node providers, and validators

## 📆 Tech Stack
- Node.js`
- ethers.js
- dotenv
- prom-client
- Hardhat (for contract context)

## 📊 Key Metrics

| Metric | Description |
|--------|-------------|
| `xrpl_tx_count` | Total benchmark transactions sent |
| `xrpl_avg_latency_ms` | Average latency from sending to inclusion (in ms) |
| `xrpl_avg_gas_used` | Average gas used across benchmarked txs |
| `evm_tx_success_rate` | Success rate of transactions (0–1) |
| `evm_tx_exec_time_stddev` | Latency standard deviation |
| `xrpl_tx_latency_histogram_ms` | Distribution of tx latencies (histogram buckets) |
| `xrpl_wallet_balance_eth` | ETH balance of wallet at start of run |
| `xrpl_avg_fee_paid` | Average fee paid per tx |
| `xrpl_benchmark_duration_ms` | Duration of benchmark run (summary) |

## 🛠️ Project Structure
```
xrpl-benchmark/
├── scripts/
│   ├── benchmark.js
│   └── multi_rpc_benchmark.js
├── exporter.js
├── .env
├── hardhat.config.js
├── package.json
```

## 🚫 Estimated Gas Costs
- Avg. gas per tx: ~21,000
- At 10–20 gwei = ~0.0002 XRP
- 10 tx per 5 min = ~2.8 XRP/day

## 💥 Stress Test Configuration

To run a configurable stress test, update your `.env`:
```env
RPC_URL=http://json-rpc.xrpl.cumulo.com.es:8542
PRIVATE_KEY=<your_testnet_private_key>
TX_COUNT=100  # Change to 500 or 1000 for high stress
```

| Stress Level | TX_COUNT | Description |
|--------------|----------|-------------|
| 🟢 Low       | 10–50    | Benchmark default or light load |
| 🟡 Medium    | 100–250  | Sustained load to test latency and throughput |
| 🔴 High      | 500–1000 | Strong stress, likely to surface RPC or network limits |
| 🧨 Extreme   | 2000+    | Maximum capacity test, use with caution |

**Tips:**
- Monitor `evm_tx_success_rate` and `xrpl_tx_latency_histogram_ms` for signs of overload.
- Combine with `multi_rpc_benchmark.js` to test across multiple endpoints.
- Tune `setInterval(runBenchmark, ...)` to adjust execution frequency.

