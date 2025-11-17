# Metrics Explained

This document provides a brief overview of the key metrics collected by the Linux Node integration. These explanations are designed to be accessible for both technical and non-technical audiences.

---

## CPU Metrics

### CPU Usage (%)
Shows how much of your processor's capacity is being used. Think of it like measuring how hard your computer's brain is working.

**What's normal:** 
- 0-30% = Light usage (browsing, basic tasks)
- 30-70% = Moderate usage (multiple applications, some processing)
- 70-100% = Heavy usage (intensive tasks, video editing, compilation)

**When to worry:** Sustained 100% usage over long periods may indicate performance bottlenecks.

### Load Average (1m, 5m, 15m)
Represents the average number of processes waiting to use the CPU over 1, 5, and 15-minute periods.

**Rule of thumb:** Compare the load to your CPU core count. On a 4-core system:
- Load of 4.0 = CPU is fully utilized but not overloaded
- Load of 8.0+ = System is overloaded, processes are waiting

<!-- **Learn more:** [Understanding Linux load averages](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-linux-node/) -->

### Per-Core CPU Usage
Shows individual CPU core utilization. Helps identify if workload is balanced across all cores or concentrated on specific cores.

---

## Memory Metrics

### Memory Usage (%)
Shows what percentage of your RAM is being used by applications and the system.

**What you'll see:**
- **Memory used:** Actively used by running applications
- **Memory cached:** Data stored for faster access (not wasted!)
- **Memory buffers:** Temporary storage for disk operations
- **Memory free:** Completely unused RAM
- **Memory available:** Free + reclaimable cache (what's actually available)

**Important:** High cache usage is normal and good - Linux uses spare RAM to speed up operations. Focus on "Memory available" rather than "Memory free."

**When to worry:** When "Memory available" drops very low and swap usage increases significantly.

<!-- **Learn more:** [Linux memory management explained](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-linux-node/)
 -->
### Swap Usage
Swap is disk space used as "overflow" when RAM is full. It's much slower than RAM.

**What's normal:** 
- Small swap usage (< 10%) is fine
- Occasional swapping during peak loads is expected

**When to worry:** Heavy, constant swap usage (swapping in/out) indicates you need more RAM.

---

## Disk Metrics

### Disk Reads/Writes
Measures how much data is being read from or written to your storage devices.

**What it shows:** Activity on each disk/partition. Spikes are normal during backups, updates, or large file operations.

**When to worry:** Constant high I/O with slow system performance may indicate:
- Failing disk hardware
- Application writing excessive logs
- Insufficient storage performance for workload

### Disk Space Usage
Shows how full your storage devices are for each mount point.

**What to monitor:**
- **/ (root):** Main system partition - should stay below 80%
- **/boot:** Boot files - rarely changes
- **/backup-** : Your backup partitions
- Other mount points as relevant to your setup

**When to worry:** 
- Above 90% on any partition = take action soon
- Above 95% = critical, clean up immediately
- 100% = system problems guaranteed

<!-- **Learn more:** [Monitoring disk usage](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-linux-node/) -->

---

## Network Metrics

### Network Traffic (Received/Transmitted)
Measures data flowing in and out of each network interface in bytes per second.

**Your interfaces:**
- **lo (loopback):** Internal system communication
- **wlp2s0:** Your WiFi adapter
- **tailscale0:** Your Tailscale VPN interface

**What's normal:** Varies widely based on usage. Background traffic of a few KB/s is typical. Large transfers show as spikes.

### Network Errors and Dropped Packets
Indicates problems with network transmission.

**What to monitor:**
- **Errors:** Malformed packets, hardware issues
- **Dropped packets:** Network congestion, buffer overflows

**When to worry:** Any consistent errors or drops indicate network problems. Occasional drops during heavy load are normal.

<!-- **Learn more:** [Network monitoring basics](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-linux-node/) -->

---

## System Overview

### Uptime
How long the system has been running since the last reboot. Tracked in days and hours.

### Hostname
The name of your server on the network.

### Kernel Version
The version of the Linux kernel running your system. This changes with system updates.

### OS
Your operating system and version (Ubuntu 25.10 in your case).

---

## Time Synchronization

### NTP Status
Shows whether your system clock is synchronized with internet time servers.

**What to monitor:** Green/healthy status means time is synchronized correctly. This is important for:
- Log timestamps
- Security certificates
- Scheduled tasks
- Distributed systems

### Time Synchronized Drift
Shows how far off your system clock drifts from the reference time source.

**What's normal:** Small drift (milliseconds to a few seconds) is expected and automatically corrected by NTP.

**When to worry:** Large, persistent drift may indicate:
- NTP service not running
- Firewall blocking NTP
- Hardware clock issues

---

## Additional Resources

- **Official Grafana Docs:** [Linux Node Integration Reference](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-linux-node/)
- **Grafana Cloud:** [Monitor Infrastructure Overview](https://grafana.com/docs/grafana-cloud/monitor-infrastructure/)
- **Understanding Metrics:** [Prometheus Metric Types](https://prometheus.io/docs/concepts/metric_types/)

---

## Quick Reference: What to Watch

| Metric | Green Zone | Yellow Zone | Red Zone |
|--------|------------|-------------|----------|
| **CPU Usage** | 0-70% | 70-90% | 90-100% sustained |
| **Memory Available** | > 20% | 10-20% | < 10% |
| **Disk Space** | < 80% | 80-90% | > 90% |
| **Load Average** | < CPU cores | 1-2x CPU cores | > 2x CPU cores |
| **Network Errors** | 0 | Occasional | Frequent |

---

**Note:** These thresholds are general guidelines. Your specific workload may have different "normal" ranges. Use these metrics to establish your baseline, then monitor for significant deviations.