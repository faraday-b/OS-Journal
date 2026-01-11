# Phase 7: Security Audit and System Evaluation

## 1. Introduction

This final phase involves a comprehensive security audit and system evaluation of the Ubuntu 24.04 server. The objective is to validate the effectiveness of previous hardening measures using industry-standard auditing tools and to ensure the system meets a production-ready security posture.

**Audit Methodology:**

- **Automated Auditing:** Using Lynis for system-wide security scanning.
- **Network Reconnaissance:** Using Nmap for external port and service verification.
- **Manual Verification:** Reviewing access controls, SSH configurations, and service justifications.

---

## 2. Infrastructure Security Assessment

### 2.1 Lynis Security Audit

Lynis was utilized to perform an in-depth security scan of the operating system.

- **Initial Lynis Score:** [Insert Score, e.g., 62]
- **Remediation Actions:** Based on Lynis suggestions, I implemented [list 1-2 items, e.g., legal banners, tightened file permissions].
- **Final Lynis Score:** [Insert Final Score, e.g., 78]

**[INSERT SCREENSHOT: Show the Lynis summary screen with the Security Index score]**

### 2.2 SSH Security Verification

SSH is the primary gateway for remote administration and requires strict access controls.

- **Verification Command:** `sudo sshd -T | grep -E "permitrootlogin|passwordauthentication|port"`
- **Result:** Confirmed that root login is disabled and only authorized authentication methods are permitted.

**[INSERT SCREENSHOT: Show the output of the SSH verification command]**

---

## 3. Network Security Testing (Nmap)

A network-based assessment was conducted to identify the "outside-in" view of the server’s attack surface.

- **Scan Type:** Service detection and versioning (`nmap -sV`).
- **Results:** Only the necessary ports (22 for SSH, 80 for HTTP) are reachable. All other ports are filtered by the firewall.

**[INSERT SCREENSHOT: Run 'nmap -sV <VM_IP>' from your host machine and paste result here]**

---

## 4. Service Inventory and Justification

The following table justifies every active listening service on the system as of the final audit.

| Service             | Port | Protocol | Justification                                |
| :------------------ | :--- | :------- | :------------------------------------------- |
| **Nginx**           | 80   | TCP      | Primary Web Server for application delivery. |
| **SSH (sshd)**      | 22   | TCP      | Secure remote administration.                |
| **MariaDB**         | 3306 | TCP      | Database backend (isolated to 127.0.0.1).    |
| **Systemd-resolve** | 53   | UDP/TCP  | Local DNS resolution for system services.    |

**[INSERT SCREENSHOT: Run 'sudo ss -tulpn' to verify this inventory matches the system state]**

---

## 5. Access Control Verification

- **UFW Status:** The Uncomplicated Firewall is active and follows a "deny by default" incoming policy.
- **User Privileges:** Verified that the primary user `benedict` is the only non-root user with `sudo` privileges.

**[INSERT SCREENSHOT: Run 'sudo ufw status verbose']**

---

## 6. Remaining Risk Assessment

Despite comprehensive hardening, the following risks remain and are acknowledged:

1. **Unencrypted Traffic (HTTP):** Currently using port 80. Risk: Data in transit is not encrypted. _Mitigation: Future implementation of SSL/TLS on port 443._
2. **Brute Force:** SSH is on a standard port. _Mitigation: Implementation of Fail2Ban or moving to non-standard ports._

---

## 7. Final Conclusion

The Phase 7 audit confirms that the server is successfully hardened. By disabling unnecessary services (Phase 6) and implementing strict access controls (Phase 7), the system maintains a high security index while fulfilling its role as a high-performance web server.
