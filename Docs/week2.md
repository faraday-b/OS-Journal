# Phase 2: Security Planning and Testing Methodology

## 1. Performance Testing Plan

This plan defines the remote monitoring methodology and testing approaches to ensure system stability on a host machine with limited hardware resources (8GB RAM).

### 1.1 Remote Monitoring Methodology

The Ubuntu server is a headless system. To minimize resource consumption on the guest OS, all monitoring is conducted remotely from the workstation via SSH.

- **Primary Tools:** Real-time visualization using `top` and `systemctl status`.
- **Data Collection:** Capturing system-wide metrics using `free -h` and `ps aux`.
- **Metric Focus:** Prioritizing RAM and CPU tracking to prevent host system instability.

### 1.2 Baseline Performance Testing (Control Group)

This establishes the "resting" state of the OS before any third-party applications or hardening scripts are applied.

- **Procedure:** Boot the server and maintain an idle state for 10 minutes.
- **Metrics:** Record baseline RAM consumption using the `free -h` utility.
- **Goal:** Establish a resource overhead baseline to measure the impact of future hardening.

### 1.3 Application Load Testing

Evaluates system behavior under the stress of the services selected in Phase 3 (Nginx and MariaDB).

- **Simulated Stress:** Monitor service responsiveness during high-intensity tasks like database indexing or concurrent web requests.
- **I/O Testing:** Utilize `scp` for file transfers to measure the impact of network and disk operations on CPU load.

---

## 2. Security Configuration Checklist

This checklist defines the security baseline across six critical domains to ensure a production-ready posture.

### 2.1 SSH Hardening & User Privilege Management

- [ ] **Disable Root Login:** Prevent direct administrative entry via SSH.
- [ ] **Non-Root User:** Establish a standard user with `sudo` privileges for all management.
- [ ] **Limit Auth Attempts:** Configure SSH to drop connections after multiple failed logins.

### 2.2 Firewall & Network Security

- [ ] **Default Deny Policy:** Configure UFW to block all incoming traffic by default.
- [ ] **Port Restriction:** Explicitly allow only necessary ports (e.g., SSH and HTTP).
- [ ] **IP Access Control:** Limit sensitive ports to authorized workstation IPs where applicable.

### 2.3 Mandatory Access Control & System Integrity

- [ ] **AppArmor Enforcement:** Verify that Mandatory Access Control (MAC) profiles are in "Enforce" mode.
- [ ] **Automatic Updates:** Enable `unattended-upgrades` for automated security patching.
- [ ] **Intrusion Prevention:** Deploy **Fail2Ban** to automate the banning of malicious IP addresses.

---

## 3. Threat Model

This model identifies specific security threats and aligns them with mitigation strategies based on the CIA Triad (Confidentiality, Integrity, Availability).

### 3.1 Threat Identification Matrix

| Threat                                | CIA Triad Focus | Mitigation Strategy                                                |
| :------------------------------------ | :-------------- | :----------------------------------------------------------------- |
| **Brute-Force SSH Attacks**           | Confidentiality | Change default SSH port and implement Fail2ban.                    |
| **Unauthorized Privilege Escalation** | Integrity       | Disable direct root logins and enforce `sudo` audit trails.        |
| **Resource Exhaustion (DoS)**         | Availability    | Use UFW rate-limiting and Fail2ban to drop aggressive connections. |

### 3.2 Mitigation Justification

- **Confidentiality:** Hardening SSH ensures that unauthorized users on the local network cannot access sensitive system data.
- **Integrity:** Forcing administrative actions through a non-root user provides an audit trail in `/var/log/auth.log` that can be verified during Lynis audits.
- **Availability:** With 8GB of host RAM, the VM is vulnerable to crashes; Fail2ban protects availability by dropping high-frequency connection attempts before they consume excessive CPU/RAM.
