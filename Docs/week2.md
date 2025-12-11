<!DOCTYPE html>

# Security Planning and Testing Methodology

## 1. Performance Testing Plan

The Performance Testing Plan defines the methodology for remotely evaluating the server's resource utilization and performance under controlled conditions. This plan is divided into three sections: Remote Monitoring Methodology, Baseline Performance Testing, and Application Load Testing.

### 1.1 Remote Monitoring Methodology

Since the server is configured as a headless system and all administration is conducted via SSH from the workstation, all performance data collection must be performed remotely.

* **Primary Tool:** A custom remote monitoring script, `monitor-server.sh`, will be developed and executed from the **workstation**. This script will establish an SSH connection to the server, execute the necessary performance commands, and collect the output for storage and analysis on the workstation.
* **Performance Metrics:** The script will be designed to collect data for all required metrics: CPU usage, Memory usage, Disk I/O performance, Network performance, System latency, and Service response times.
* **CLI Tools:** Standard Linux command-line utilities will be used to gather raw data directly from the server's kernel, including:
    * `vmstat` (Virtual Memory Statistics) for CPU and Memory.
    * `iostat` (Input/Output Statistics) for Disk I/O performance.
    * `netstat` or `ss` for network connection status.
    * `ping` or `iperf3` for network latency and throughput analysis.
* **Data Handling:** The collected data will be captured in a structured format (e.g., CSV or a simple data table) for subsequent processing. This data will be used in Phase 6 to create performance data tables and visualisations (charts/graphs).

### 1.2 Baseline Performance Testing Approach

Baseline testing is required to establish a control set of data, identifying the server's native resource consumption when running only essential operating system services in an idle state.

* **Purpose:** To measure the minimum CPU, RAM, and disk activity required for the Ubuntu server to function without any added application load. This acts as the **control group** for comparison against load testing.
* **Methodology:**
    1.  Ensure all non-essential services and applications are stopped.
    2.  Run the `monitor-server.sh` script over a defined period (e.g., 5-10 minutes) to capture average idle statistics.
* **Key Metrics:** Focus on CPU idle percentage, free memory capacity, and average disk I/O wait times.

### 1.3 Application Load Testing Approach

Load testing evaluates the system's performance and behavior under the stress of the chosen applications (which will be defined in Phase 3).

* **Purpose:** To identify performance bottlenecks and system limitations when the server is operating under heavy workload, corresponding to CPU-, RAM-, I/O-, and Network-intensive scenarios.
* **Methodology:**
    1.  For each application selected in Phase 3, a specific load will be applied (e.g., running `stress-ng`, initiating a large file transfer, or simulating multiple concurrent server requests).
    2.  The `monitor-server.sh` script will be executed simultaneously to continuously capture performance data while the load is active.
* **Analysis:** The data collected during load testing will be compared directly against the baseline data to quantify the resource impact of each application workload. This comparison will facilitate performance analysis and the identification of optimisation opportunities for Phase 6.
