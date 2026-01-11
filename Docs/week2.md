# Phase 2: Security Planning and Testing Methodology

## Task 1: Performance Testing Plan

### 1. Remote Monitoring Methodology

The Ubuntu server is a headless system with no graphical interface. This means that all performance monitoring must be conducted remotely from the workstation (my laptop) using an SSH connection. This prevents monitoring tools from consuming the server's limited resources.

- **Primary tools:** I will use `top` and `systemctl status` for real-time visualization of CPU and RAM usage.
- **Data collection:** I will use `free -h` and `ps aux` to capture system-wide and process-specific metrics directly from the CLI.
- **Metric focus:** I will prioritize tracking memory usage as my laptop only has 8GB of RAM.

---

### 2. Baseline Performance Testing

The baseline test establishes how the server performs in its "natural" state before any applications or security hardening are applied. This acts as the control group for the project.

- **Procedure:** I will boot the server and allow it to run in an idle state for 10 minutes.
- **Monitoring:** I will record the "resting" RAM consumption and CPU percentage using the `free -h` utility.
- **Goal:** To determine the minimum resource overhead required to keep the OS running before third-party services are installed.

---

### 3. Application Load Testing

Load testing evaluates how the system behaves under the stress of the applications selected in Phase 3 (Nginx and MariaDB). This is critical to ensure the 8GB RAM on my laptop can handle the workload.

- **Simulated Stress:** I will monitor service responsiveness during the installation and initial configuration of the database and web server.
- **I/O Testing:** I will perform file transfers between the workstation and the server using `scp` to measure how network and disk operations impact CPU load.
- **Success Criteria:** The server must remain responsive via SSH and maintain available memory overhead even under active service load.

---

## Task 2: Security Configuration Checklist

### 1. Access Control and Authentication

- **Implement SSH Hardening:** Modify the SSH configuration to limit authentication attempts and enforce security best practices.
- **Disable Root Login:** Prevent direct root access to the server to reduce the risk of unauthorized administrative entry.
- **Create Non-Root User:** Establish a standard user account with `sudo` privileges for daily management tasks.

### 2. Network Security and Firewall

- **Enable UFW (Uncomplicated Firewall):** Set a global "Deny" policy for all incoming traffic to minimize the attack surface.
- **Port Restricting:** Only open the specific custom port required for SSH management (Port 2222) and standard web traffic.
- **IP Management:** Configure the firewall to ensure only authorized traffic can reach sensitive system ports.

### 3. Intrusion Prevention and System Integrity

- **Install and Configure Fail2ban:** Set up automated monitoring of logs to identify and ban brute-force behavior at the firewall level.
- **Enable Unattended Upgrades:** Configure the system to automatically install security patches to ensure long-term sustainability.
- **AppArmor Profile Verification:** Ensure Mandatory Access Control (MAC) profiles are active and in "Enforce" mode.

### 4. Logging and Auditing

- **Log Review:** I will establish a schedule for reviewing `/var/log/auth.log` and `/var/log/syslog` to verify SSH access attempts and system health.
- **Integrity Verification:** I will utilize **Lynis** as an automated auditing tool to perform integrity checks and provide a hardening index score.

---

## Task 3: Threat Model

### 1. Threat Identification Matrix

| Threat                                | CIA Triad Focus | Mitigation Strategy                                                                |
| :------------------------------------ | :-------------- | :--------------------------------------------------------------------------------- |
| **Brute-Force SSH Attacks**           | Confidentiality | Change default SSH port and implement Fail2ban to ban malicious IPs.               |
| **Unauthorized Privilege Escalation** | Integrity       | Disable direct root logins and use a non-root user with sudo privileges.           |
| **Service Downtime (DoS)**            | Availability    | Use UFW to limit traffic and Fail2ban to drop connections from aggressive sources. |

---

### 2. Mitigation Justification

- **Confidentiality:** Since my server uses a Host-Only network, it is primarily at risk from devices on my local network. By hardening SSH and limiting authentication attempts, I ensure that unauthorized users cannot gain access to sensitive system data.

- **Integrity:** To ensure the system remains untampered, I will disable direct root logins. This forces any administrative action to be logged through a specific user account, providing an audit trail of changes that can be verified during Lynis audits.

- **Availability:** With only 8GB of RAM on my host machine, the VM is vulnerable to resource exhaustion. Using Fail2ban ensures that the system automatically drops connections from aggressive sources before they consume enough memory or CPU cycles to crash the server.
