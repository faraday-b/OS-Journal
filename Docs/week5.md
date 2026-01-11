# Phase 5: Advanced Security and Monitoring Infrastructure

## 1. Initial Audit & Hardening Index

To evaluate the current security posture of the hardened Ubuntu server, I performed an automated security audit using **Lynis**. This tool analyzes the system configuration, kernel, and installed services against industry standards to provide a "Hardening Index" and a list of actionable improvements.

| Metric           | Initial Value |
| :--------------- | :-----------: |
| Hardening Index  |      59       |
| Tests Performed  |      253      |
| Warnings Found   |       2       |
| Suggestions Made |      39       |

<img width="960" alt="Lynis Audit Summary" src="https://github.com/user-attachments/assets/1314d4a2-7e42-4c25-8f79-53d79fea8ef3" />

---

## 2. Mandatory Access Control (Deliverable 1)

To restrict program capabilities and prevent unauthorized data access, I verified the implementation of **AppArmor**.

- **Implementation:** I confirmed that the AppArmor service is active and that security profiles are in `enforce mode`.
- **Verification:** The following screenshot confirms that the profiles are loaded and actively enforcing restrictions on system binaries to prevent unauthorized process execution.

<img width="437" height="115" alt="image" src="https://github.com/user-attachments/assets/9ab033d6-010f-48b9-be85-8e7179c60438" />

---

## 3. Automation & Intrusion Detection (Deliverables 2 & 3)

### 3.1 Automatic Security Updates

To ensure long-term sustainability and protection against vulnerabilities, I configured the `unattended-upgrades` package. This automates critical security patches without manual intervention.

<img width="733" height="68" alt="image" src="https://github.com/user-attachments/assets/19ddc9eb-b5df-4331-a397-0554f135438f" />

### 3.2 Fail2Ban Intrusion Detection

To mitigate brute-force risks, I implemented **Fail2Ban**. This service monitors system logs for repeated failed login attempts and automatically bans offending IP addresses at the firewall level.

<img width="765" height="232" alt="image" src="https://github.com/user-attachments/assets/dc6cd4cb-ed76-4b58-a39f-b33eabaa0990" />

---

## 4. Automation Scripts (Deliverables 4 & 5)

### 4.1 Security Baseline Script (`security-baseline.sh`)

I developed a server-side bash script to verify the hardened configuration. This script checks for the presence of the legal banner, SSH port settings, and the status of Fail2Ban to ensure no configuration drift has occurred.

<img width="564" height="225" alt="image" src="https://github.com/user-attachments/assets/1319b3a4-949d-41b8-acc4-808146d85716" />

### 4.2 Remote Monitoring Script (`monitor-server.sh`)

I created a PowerShell script to run from my workstation. It connects via SSH to collect real-time performance metrics (CPU, RAM, and Uptime), ensuring the server remains efficient and sustainable per **Masanet et al. (2020)**.

<img width="1025" height="187" alt="image" src="https://github.com/user-attachments/assets/c03a452a-8869-4265-9d92-3090d0b65c27" />

---

## 5. Technical Reflection & Sustainability

This phase successfully transitioned the server from a functional state to a monitored and audited environment. By combining automated auditing with active defense tools like Fail2Ban and AppArmor, I have moved the server to an "Audited Infrastructure." Automating security updates reduces the manual overhead for administrators, while Fail2Ban protects CPU resources from being wasted on malicious brute-force traffic, supporting energy-efficient data center operations.
