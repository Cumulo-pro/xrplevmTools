# 📊 XRPL EVM Benchmarking Tool by Cumulo  
## Technical Report – Benchmarking + Stress Test Stages 1 & 2

---

## 🧪 Baseline Benchmark – Stable Test

### 📋 Summary

| Metric                                 | Value      |
|----------------------------------------|------------|
| Total transactions                     | 5,760      |
| Average per interval                   | 10         |
| Std deviation per batch                | 0.0        |
| Transaction success rate               | 100%       |
| Average latency (estimated)            | ~1.5–2.9 s |
| Latency std deviation                  | 1.5 s      |

> ✅ Perfectly stable behavior, ideal for isolating latency performance without volume interference.

---

## 🔥 Stress Stage 1 – High Load with Network Stability

### 📋 Summary

| Metric               | Value        |
|----------------------|--------------|
| Total TXs            | 17,220       |
| Success Rate         | 100% ✅     |
| Avg latency          | 9.1 s        |
| Std deviation        | 2.38 s       |
| Avg gas used         | 21,000       |
| Avg fee              | 0.00577 XRP  |
| Wallet balance       | 77,318 XRP   |

### 📈 Latency Distribution

- ≤ 4,000 ms: 94 TX  
- ≤ 8,000 ms: 1,994 TX  
- ≤ 12,000 ms: 16,449 TX  
- > 16,000 ms: minimal

> 🔍 ~95% of transactions included between 4 and 12 seconds.

### 🚨 External Errors (not impacting benchmark)

| Error                  | Occurrences |
|------------------------|-------------|
| `ECONNREFUSED`         | ~30–40      |
| `EAI_AGAIN` (DNS)      | ~40–50      |
| `ECONNRESET`           | ~20         |
| `insufficient funds`   | 5–10        |
| `tx_already_in_mempool`| ~80+        |

> ⚠️ These did not affect benchmark metrics directly.

---

## 🔥 Stress Stage 2 – Extreme Load with Saturation

### 📋 Summary

| Metric               | Value         |
|----------------------|---------------|
| Total TXs sent       | 39,326        |
| Status entries       | 726,608       |
| Success Rate         | 0.2% ❌      |
| Failure Rate         | 99.8%         |
| Avg latency          | 7.4 s         |
| Std deviation        | 70.5 ms       |
| Avg gas used         | 21,000        |
| Avg fee              | 0.00578 XRP   |
| Wallet balance       | 66,277 XRP    |

### 🚨 Critical Errors

- `connect ECONNREFUSED`
- `connect ETIMEDOUT`
- `read ECONNRESET`
- `tx_already_in_mempool`
- `insufficient funds`

> 🔴 Indicates complete RPC saturation and infrastructure breakdown under load.

---

## 📊 Full Comparison

| Metric                    | Baseline Benchmark | Stress Stage 1 | Stress Stage 2     |
|---------------------------|--------------------|----------------|---------------------|
| TXs sent                  | 5,760              | 17,220         | 39,326              |
| Success rate              | 100% ✅           | 100% ✅       | 0.2% ❌            |
| Avg latency               | ~2s                | 9.1 s          | 7.4 s               |
| Latency std deviation     | ~1.5 s             | 2.38 s         | 70.5 ms             |
| RPC/network errors        | None               | Minor external | Critical saturation |
| Wallet funding            | Stable             | 77k XRP        | 66k XRP (low)       |

---

## ✅ Technical Conclusions

- XRPL EVM is stable and reliable under normal and moderate loads.
- The benchmarking tool by Cumulo is effective and accurate.
- Stress Stage 2 exposes critical limits in:
  - RPC availability and retry handling
  - Wallet gas balance
  - Nonce management and duplicate mempool entries

---

## 🛠️ Recommendations

- Introduce load balancing and fallback RPCs.
- Implement smarter nonce/retry logic to avoid duplication.
- Use wallet balances over 100k XRP for high-throughput tests.
- Add error tracking metrics: `rpc_failures_total`, `tx_retries_total`.
- Setup Prometheus alerts for latency, RPC errors, and resource exhaustion.

---

## 🚀 Final Evaluation

✅ Benchmarking passed under expected network conditions.  
⚠️ Stress test revealed bottlenecks, as expected under extreme throughput.  
🔧 Tool and setup are ready for continuous iteration and advanced monitoring.

---

© Cumulo – Validator & Infrastructure Tools for XRPL EVM
