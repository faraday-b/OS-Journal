# Phase 6: Performance Evaluation and Analysis

## 1. Introduction and Methodology

This phase involves a quantitative analysis of the Ubuntu Server's behavior under various workloads. The goal is to identify hardware bottlenecks and verify the impact of configuration optimizations. Testing was conducted using a "Baseline vs. Load" approach, focusing on CPU, Memory, and Service Response times.

**Tools Used:**

- **stress-ng:** Used to simulate high CPU and Virtual Memory stress.
- **Apache Benchmark (ab):** Used to measure Nginx web server response times.
- **top/free:** Used for real-time monitoring of system resource utilization.
- **iperf3:** Used to measure network throughput.

---

## 2. Performance Data Table

The following data represents the system state before and after applying synthetic workloads.

| Metric                 | Baseline (Idle)   | Application Load Testing |
| :--------------------- | :---------------- | :----------------------- |
| **CPU Usage**          | ~0.3% - 1.0%      | **97.7%**                |
| **Memory Usage**       | **1.5 GiB Avail** | **825 MiB Free**         |
| **Nginx Throughput**   | N/A               | **1390.57 req/s**        |
| **Mean Response Time** | N/A               | **14.383 ms**            |
| **Network Throughput** | N/A               | **7.62 Gbits/sec**       |

### 2.1 Performance Visualization

<img width="481" height="289" alt="image" src="https://github.com/user-attachments/assets/9ad779f9-ac9a-42b0-a10c-a6c3e17842ae" />

---

## 3. Testing Scenarios and Evidence

### 3.1 Baseline Performance

The system demonstrated high efficiency in an idle state, maintaining over 1.5 GiB of available memory.

<img width="935" height="138" alt="Baseline free -h" src="https://github.com/user-attachments/assets/26a6642e-c63b-4c6b-94a9-a6da7f384540" />

### 3.2 Application Load Testing & Bottleneck Identification

During peak stress, the CPU utilization reached **97.7%**. This confirms the dual-core virtual allocation is the primary hardware bottleneck under extreme load.

<img width="1920" height="1080" alt="Stress-ng Top Output" src="https://github.com/user-attachments/assets/c0c5d4ca-fe97-4ce2-8637-4502f4a49152" />

### 3.3 Web Server Response Analysis

The server achieved a throughput of **1390.57 req/s** with a mean latency of **14.383 ms**.

<img width="734" height="73" alt="Apache Benchmark Results" src="https://github.com/user-attachments/assets/643e01e4-0df2-4be0-b10f-5b3182c41971" />

---

## 4. Performance Optimization and Evidence

### 4.1 Optimization 1: Reduction of Service Overhead

**Action:** Audit and disablement of `cupsd` and `avahi-daemon`.
**Result:** Reclaimed resources. The "After" audit confirms these services are no longer listening on network ports.

<img width="914" height="261" alt="image" src="https://github.com/user-attachments/assets/1e2fb1d1-4c3b-4775-a6cd-4bf412173fa6" />

### 4.2 Optimization 2: Nginx Connection Efficiency

**Action:** Modified `/etc/nginx/nginx.conf` to set `worker_connections 1024`.
**Result:** Increased the ceiling for concurrent connections.

<img width="566" height="210" alt="image" src="https://github.com/user-attachments/assets/d1620da0-78f1-4878-ab81-68c0680abcc9" />

---

## 5. Network Performance Analysis

Internal throughput was measured using `iperf3`, achieving a bitrate of **7.62 Gbits/sec**.

<img width="953" height="462" alt="iperf3 results" src="https://github.com/user-attachments/assets/a2155a21-d675-4610-9481-0928374e737a" />

**Latency Evidence:**
Ping tests confirmed sub-millisecond responses, ensuring rapid communication between the web and database layers.

<img width="751" height="233" alt="image" src="https://github.com/user-attachments/assets/490ae6a9-bf54-4c61-bc8e-0ad140494493" />

---

## 6. Conclusion

The performance evaluation confirms that the hardened Ubuntu 24.04 server is highly optimized. While the CPU reached a **97.7%** peak under stress, the optimizations ensured the system remained stable with ample available memory (1.5 GiB).
