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

Lynis was utilized to perform an in-depth security scan of the operating system to establish a hardening baseline and identify vulnerabilities.

- **Initial Lynis Score (Week 5):** 59
- **Baseline Score (Phase 7 Start):** 60 (Improved after Phase 6 service cleanup)
- **Final Hardened Score:** 61
- **Remediation Actions Implemented:**
  - **Malware Detection:** Installed `chkrootkit` to fulfill the requirement for automated rootkit scanning.
  - **Banner Implementation:** Added a legal warning to `/etc/issue` to provide notice of authorized access only.
  - **Audit Tools:** Installed `apt-show-versions` to improve package security tracking.
  - **Service Purge:** Fully removed `cups` and `avahi-daemon` packages to permanently close legacy ports.

<img width="527" height="130" alt="image" src="https://github.com/user-attachments/assets/c4b5d525-0844-4c6b-9f0a-58b36ca9775e" />

### 2.2 SSH Security Verification

SSH is the primary gateway for remote administration and requires strict access controls.

- **Verification Command:** `sudo sshd -T | grep -E "permitrootlogin|passwordauthentication|port"`
- **Result:** The audit confirms that `permitrootlogin` is set to `no` and `passwordauthentication` is set to `no`. This ensures that only key-based authentication is permitted, significantly reducing the risk of credential-based brute-force attacks.

<img width="1076" height="141" alt="image" src="https://github.com/user-attachments/assets/d25d9994-3efe-4e8b-9f62-d10e9ca948d7" />

---

## 3. Network Security Testing (Nmap)

A network-based assessment was conducted to identify the "outside-in" view of the server’s attack surface.

- **Scan Type:** Service detection and versioning (`nmap -sV`).
- **Results:** Only the necessary ports (22 for SSH, 80 for HTTP) are reachable. All other ports are filtered by the UFW firewall, effectively hiding internal services like MariaDB and Postfix from external probes.

<img width="925" height="378" alt="image" src="https://github.com/user-attachments/assets/f794c815-862a-4432-b560-85792a9440e5" />

---

## 4. Service Inventory and Justification

The following table justifies every active listening service on the system as of the final audit.

| Service             | Port | Protocol | Justification                                            |
| :------------------ | :--- | :------- | :------------------------------------------------------- |
| **Nginx**           | 80   | TCP      | Primary Web Server for application delivery.             |
| **sshd**            | 22   | TCP      | Secure remote administration.                            |
| **MariaDB**         | 3306 | TCP      | Database backend (Isolated to 127.0.0.1).                |
| **Postfix**         | 25   | TCP      | Local mailer for system alerts (Configured: Local Only). |
| **Systemd-resolve** | 53   | UDP/TCP  | Local DNS resolution for system services.                |

<img width="1887" height="327" alt="image" src="https://github.com/user-attachments/assets/b7a9d611-1af2-43d6-ad65-dfc9765b66ed" />

---

## 5. Access Control Verification

- **UFW Status:** The Uncomplicated Firewall is active and follows a strict "deny by default" incoming policy. Only explicitly allowed traffic (SSH/HTTP) is permitted.
- **User Privileges:** Verified that `benedict` is the only non-root user with `sudo` permissions, ensuring no unauthorized privilege escalation paths exist.

<img width="745" height="279" alt="image" src="https://github.com/user-attachments/assets/72349a99-81d9-4ab3-92b3-cd7eea0f9432" />

---

## 6. Remaining Risk Assessment

Despite comprehensive hardening, the following risks remain and are acknowledged for future remediation:

1. **Unencrypted Traffic (HTTP):** Currently using port 80. Risk: Data in transit is not encrypted. _Mitigation: Future implementation of SSL/TLS on port 443._
2. **Brute Force Sensitivity:** SSH remains on the standard port 22. _Mitigation: Implementation of Fail2Ban to block persistent failed login attempts._
3. **SMTP Exposure:** Postfix was installed as a dependency for security tools.
   - **Action:** To mitigate risk, it was configured as "Local Only" during the package configuration phase to prevent external relaying.

---

## 7. Final Conclusion

The Phase 7 audit confirms that the server is successfully hardened. By increasing the hardening index from 59 to 61 and verifying the network posture with Nmap and manual SSH audits, the system is now prepared for production use. The combination of Phase 6 performance tuning and Phase 7 security auditing ensures a fast, stable, and secure Ubuntu 24.04 environment.
