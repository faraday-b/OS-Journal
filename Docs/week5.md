# Phase 5: Advanced Security and Monitoring Infrastructure

## Introduction

Phase 5 marks the transition from manual hardening to the implementation of an advanced security and monitoring framework. The objective of this phase is to establish a proactive defense-in-depth posture by integrating automated auditing, Mandatory Access Control (MAC), and intrusion prevention systems. Furthermore, this phase introduces custom automation scripts designed to ensure configuration consistency and remote performance visibility, effectively moving the server into a managed "Audited Infrastructure" state.

---

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

## 2. Mandatory Access Control (MAC)

To restrict program capabilities and prevent unauthorized data access, I verified the implementation of **AppArmor**. This provides an extra layer of security by limiting the resources processes can access, regardless of user permissions.

- **Implementation:** I confirmed that the AppArmor service is active and that security profiles are in `enforce mode`.
- **Verification:** The following screenshot confirms that the profiles are loaded and actively enforcing restrictions on system binaries to prevent unauthorized process execution or lateral movement.

<img width="437" height="115" alt="AppArmor Status" src="https://github.com/user-attachments/assets/9ab033d6-010f-48b9-be85-8e7179c60438" />

---

## 3. Automation & Intrusion Detection

### 3.1 Automatic Security Updates

To ensure long-term sustainability and protection against zero-day vulnerabilities, I configured the `unattended-upgrades` package. This automates the installation of critical security patches, ensuring the system remains protected without requiring manual administrative intervention.

<img width="733" height="68" alt="Unattended Upgrades Config" src="https://github.com/user-attachments/assets/19ddc9eb-b5df-4331-a397-0554f135438f" />

### 3.2 Fail2Ban Intrusion Detection

To mitigate brute-force risks, I implemented **Fail2Ban**. This service monitors system logs (such as `/var/log/auth.log`) for repeated failed login attempts. Upon detection of malicious patterns, it automatically updates firewall rules to ban the offending IP addresses.

<img width="765" height="232" alt="Fail2Ban Status" src="https://github.com/user-attachments/assets/dc6cd4cb-ed76-4b58-a39f-b33eabaa0990" />

---

## 4. Automation Scripts

### 4.1 Security Baseline Script (`security-baseline.sh`)

I developed a server-side Bash script to verify the hardened configuration at any given time. This script programmatically checks for the presence of the legal banner (MOTD), SSH port settings, and the status of Fail2Ban to ensure no configuration drift has occurred.

<img width="564" height="225" alt="Security Baseline Script" src="https://github.com/user-attachments/assets/1319b3a4-949d-41b8-acc4-808146d85716" />

### 4.2 Remote Monitoring Script (`monitor-server.sh`)

I created a PowerShell script to be executed from the workstation. It connects via SSH to collect real-time performance metrics—specifically CPU load, RAM utilization, and System Uptime—ensuring the server remains efficient and responsive under the established security load.

<img width="1025" height="187" alt="Remote Monitoring Script" src="https://github.com/user-attachments/assets/c03a452a-8869-4265-9d92-3090d0b65c27" />

---

## 5. Technical Reflection & Sustainability

This phase successfully transitioned the server from a functional state to a monitored and audited environment. By combining automated auditing (Lynis) with active defense tools like Fail2Ban and AppArmor, I have established a high-integrity infrastructure.

Automating security updates reduces the manual burden on administrators and ensures a consistent security posture. Furthermore, protecting the server from brute-force traffic with Fail2Ban prevents wasted CPU cycles on malicious requests, which directly supports the goal of energy-efficient and sustainable server operations. The implementation of remote monitoring provides the data necessary to maintain this balance between high-level security and optimal system performance.
