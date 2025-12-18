# Security Planning and Testing Methodology

## Task 1: Performance Testing Plan

### 1. Remote Monitoring Methodology
The Ubuntu server is a headless system with no graphical interface. This means that all performance monitoring must be conducted remotely from the workstation (my laptop) using an SSH connection. This prevents monitoring tools from consuming the server's limited resources.

* **Primary tool:** I will use `htop` for real time visualisation of CPU and RAM usage.
* **Data collection:** I will use `vmstat` and `iostat` to capture raw data into text files on the workstation for later analysis.
* **Metric focus:** I will prioritise tracking memory usage as my laptop only has 8GB of RAM.

---

### 2. Baseline Performance Testing
The baseline test establishes how the server performs in its "natural" state before any applications or security hardening are applied. This acts as the control group for the project.

* **Procedure:** I will boot the server and allow it to run in an idle state for 10 minutes.
* **Monitoring:** I will record the "resting" RAM consumption and CPU percentage every 60 seconds.
* **Goal:** To determine the minimum resource overhead required to keep the OS running.

---

### 3. Application Load Testing
Load testing evaluates how the system behaves under the stress of the applications I will choose in Phase 3. This is critical to ensure the 8GB RAM on my laptop can handle the workload.

* **Simulated Stress:** I will use the `stress-ng` utility to manually push the CPU to 50% and 80% capacity.
* **I/O Testing:** I will perform a large file transfer between the workstation and the server using `scp` to measure disk write speeds.
* **Success Criteria:** The server must remain responsive via SSH even under 80% load.

---

## Task 2: Security Configuration Checklist

### 1. Access Control and Authentication
* **Implement SSH Key-Pair Authentication:** Disable password-based logins to ensure only the workstation with the private key can access the server.
* **Disable Root Login:** Modify the SSH configuration to prevent direct root access.
* **Limit User Privileges:** Audit user groups to ensure the "least privilege" principle is applied.

### 2. Network Security and Firewall
* **Enable UFW (Uncomplicated Firewall):** Set a global "Deny" policy for all incoming traffic.
* **Port Restricting:** Only open the specific custom port required for SSH management.
* **IP Whitelisting:** Configure the firewall to specifically allow traffic only from the workstation's IP.

### 3. Intrusion Prevention and System Integrity
* **Install and Configure Fail2ban:** Set up automated monitoring of logs to ban brute-force behavior.
* **Enable Unattended Upgrades:** Configure the system to automatically install security patches.
* **AppArmor Profile Verification:** Ensure Mandatory Access Control (MAC) profiles are active.

### 4. Logging and Auditing
* **Centralized Log Review:** Establish a schedule for reviewing `auth.log` and `syslog`.
* **System Integrity Checks:** Use tools to verify that system binaries have not been tampered with.

---

## Task 3: Threat Model

### 1. Threat Identification Matrix

| Threat | CIA Triad Focus | Mitigation Strategy |
| :--- | :--- | :--- |
| **Brute-Force SSH Attacks** | Confidentiality | Disable password authentication and enforce SSH Key-Pairs. |
| **Unauthorized Privilege Escalation** | Integrity | Lock the root account and use a non-root user with sudo privileges. |
| **Service Downtime (DoS)** | Availability | Configure Fail2ban to ban malicious IPs and use UFW to limit traffic. |

---

### 2. Mitigation Justification

* **Confidentiality:** Since my server uses a Host-Only network, it is primarily at risk from devices on my local network. By disabling passwords, I ensure that even if an attacker knows my username, they cannot gain access without my unique physical private key.

* **Integrity:** To ensure the system remains untampered, I will disable direct root logins. This forces any administrative action to be logged through a specific user account, providing an audit trail of changes.

* **Availability:** With only 8GB of RAM on my host machine, the VM is vulnerable to resource exhaustion. Using Fail2ban ensures that the system automatically drops connections from aggressive sources before they consume enough memory to crash the server.
