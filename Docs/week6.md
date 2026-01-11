# Phase 6: Performance Evaluation and Analysis

## 1. Introduction and Methodology

This phase involves a quantitative analysis of the Ubuntu Server's behavior under various workloads. The goal is to identify hardware bottlenecks and verify the impact of configuration optimizations. Testing was conducted using a "Baseline vs. Load" approach, focusing on CPU, Memory, and Service Response times.

**Tools Used:**

- **stress-ng:** Used to simulate high CPU and Virtual Memory stress to find the system ceiling.
- **Apache Benchmark (ab):** Used to measure Nginx web server response times and concurrency handling.
- **top/free:** Used for real-time monitoring of system resource utilization.

---

## 2. Performance Data Table

The following data represents the system state before and after applying synthetic workloads. These measurements provide a quantitative basis for the performance analysis.

| Metric                 | Baseline (Idle) | Application Load Testing |
| :--------------------- | :-------------- | :----------------------- |
| **CPU Usage**          | ~0.3% - 1.0%    | **97.9%**                |
| **Memory Usage**       | ~300.0 MiB Free | ~249.4 MiB Free          |
| **Nginx Throughput**   | N/A             | **1390.57 req/s**        |
| **Mean Response Time** | N/A             | **14.383 ms**            |
| **Running Processes**  | 273 (Sleeping)  | 6 (Running)              |

---

## 3. Testing Scenarios and Evidence

### 3.1 Baseline Performance

The baseline was established with essential services (SSH, Nginx, MariaDB) active. The system demonstrated high efficiency, maintaining over 1400 MiB of available memory in an idle state.

<img width="935" height="138" alt="image" src="https://github.com/user-attachments/assets/26a6642e-c63b-4c6b-94a9-a6da7f384540" />

### 3.2 Application Load Testing & Bottleneck Identification

To identify the system's performance ceiling, I used `stress-ng` to dispatch multiple CPU and VM "hogs."

- **CPU Bottleneck:** During peak stress, the CPU utilization reached **97.9%** for the primary testing process. While the system remained stable, this identifies the dual-core virtual allocation as the primary hardware bottleneck under extreme load.
- **Service Stability:** Despite the high CPU load, the system successfully managed 273 sleeping processes without kernel panics or service failure.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c0c5d4ca-fe97-4ce2-8637-4502f4a49152" />

### 3.3 Web Server Response Analysis

I utilized `ab` (Apache Benchmark) to test the Nginx web server's ability to handle concurrent requests.

- **Result:** The server achieved a mean throughput of **1390.57 requests per second**.
- **Latency:** The mean time per request was recorded at **14.383 ms**, indicating excellent responsiveness for a virtualized environment.

<img width="734" height="73" alt="image" src="https://github.com/user-attachments/assets/643e01e4-0df2-4be0-b10f-5b3182c41971" />

---

## 4. Optimization Testing and Evidence

Based on the identified bottlenecks, two specific optimizations were implemented:

### 4.1 Optimization 1: Reduction of Service Overhead

An audit using `ss -tulpn` revealed active services for printing (`cupsd`) and discovery (`avahi-daemon`).

- **Action:** `sudo systemctl disable --now cups avahi-daemon`
- **Result:** This reduced the background process count and reclaimed system memory, simplifying the security surface area.

### 4.2 Optimization 2: Nginx Connection Efficiency

I tuned the Nginx worker configuration to handle high-concurrency loads more effectively.

- **Action:** Modified `/etc/nginx/nginx.conf` to set `worker_connections 1024`.
- **Result:** This ensures the web server can utilize available CPU cycles more effectively during the 1390+ req/s bursts observed in testing.

---

## 5. Network Performance Analysis

Network performance was analyzed via the VirtualBox Host-Only interface.

- **Latency:** Verified via `ping`, showing sub-millisecond responses in baseline states, increasing only slightly during the 97.9% CPU stress tests.
- **Port Audit:** Confirmed that MariaDB (3306) is correctly isolated to `127.0.0.1`, preventing external network bottlenecks or security leaks.

---

## 6. Conclusion

The performance evaluation confirms that the hardened Ubuntu 24.04 server is highly optimized for its role. While the CPU reached a 97.9% peak during extreme synthetic stress, the web server maintained exceptional throughput and low latency. The implemented optimizations ensure a lean, high-performance environment suitable for the project requirements.
