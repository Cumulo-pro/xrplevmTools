## XRPL EVM Prometheus Metrics

### 🟢 Benchmark Metrics

| Metric Name | Type | Description |
|-------------|------|-------------|
| `xrpl_tx_count` | `counter` | Total number of transactions sent during the benchmark |
| `xrpl_avg_latency_ms` | `gauge` | Average time (ms) between sending and confirming transactions |
| `xrpl_avg_gas_used` | `gauge` | Average gas consumed per transaction |
| `evm_tx_success_rate` | `gauge` | Success ratio of transactions (range: 0 to 1) |
| `evm_tx_exec_time_stddev` | `gauge` | Standard deviation of execution latency (ms) |
| `xrpl_tx_failures_total` | `counter` | Total number of failed transactions |
| `xrpl_tx_status_total{status="success"}` | `counter` | Count of transactions grouped by status |
| `xrpl_tx_latency_histogram_ms` | `histogram` | Distribution of transaction confirmation latency in ms |

---

### 📟 Extended Benchmark Metrics

| Metric Name | Type | Description |
|-------------|------|-------------|
| `xrpl_wallet_balance_eth` | `gauge` | Wallet balance (in ETH) at benchmark start |
| `xrpl_avg_fee_paid` | `gauge` | Average transaction fee paid (in ETH) |
| `xrpl_benchmark_duration_ms` | `summary` | Total duration of the benchmark (in milliseconds) including quantiles |

---

### ⚙️ Node.js Process Metrics

| Metric Name | Type | Description |
|-------------|------|-------------|
| `process_cpu_user_seconds_total` | `counter` | Total CPU user time in seconds |
| `process_cpu_system_seconds_total` | `counter` | Total system CPU time in seconds |
| `process_cpu_seconds_total` | `counter` | Combined user and system CPU time |
| `process_start_time_seconds` | `gauge` | Time since the process started (epoch timestamp) |
| `process_resident_memory_bytes` | `gauge` | Resident memory used by process |
| `process_virtual_memory_bytes` | `gauge` | Virtual memory used by process |
| `process_heap_bytes` | `gauge` | Heap size allocated by Node.js |
| `process_open_fds` | `gauge` | Open file descriptors |
| `process_max_fds` | `gauge` | Maximum file descriptors allowed |

---

### 🧠 Node.js Heap Space Metrics

| Metric | Type | Description |
|--------|------|-------------|
| `nodejs_heap_space_size_total_bytes{space=*}` | `gauge` | Total heap space per zone (e.g. old, new, code) |
| `nodejs_heap_space_size_used_bytes{space=*}` | `gauge` | Used heap space per zone |
| `nodejs_heap_space_size_available_bytes{space=*}` | `gauge` | Available heap space per zone |
| `nodejs_external_memory_bytes` | `gauge` | External memory used by Node.js |

---

### 🔀 Node.js Event Loop Metrics

| Metric | Type | Description |
|--------|------|-------------|
| `nodejs_eventloop_lag_seconds` | `gauge` | Current event loop lag in seconds |
| `nodejs_eventloop_lag_{min,max,mean,stddev,p50,p90,p99}_seconds` | `gauge` | Distribution of event loop lag |

---

### 🔧 Node.js Runtime Details

| Metric | Type | Description |
|--------|------|-------------|
| `nodejs_version_info{version,major,minor,patch}` | `gauge` | Static version information |
| `nodejs_active_resources{type=*}` | `gauge` | Count of active async resources by type |
| `nodejs_active_handles{type=*}` | `gauge` | Active libuv handles by type |
| `nodejs_active_requests_total` | `gauge` | Number of active requests |

---

### 🗑️ Node.js Garbage Collection Metrics

| Metric | Type | Description |
|--------|------|-------------|
| `nodejs_gc_duration_seconds_bucket{kind=*}` | `histogram` | GC duration by type (minor, major, incremental) |
| `nodejs_gc_duration_seconds_sum{kind=*}` | `histogram` | Sum of GC durations |
| `nodejs_gc_duration_seconds_count{kind=*}` | `histogram` | Number of GC runs |

