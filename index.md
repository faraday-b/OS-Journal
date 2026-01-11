# Technical Journal: Linux Server Configuration, Security, and Performance

## Project Overview

This technical journal documents the deployment, hardening, and evaluation of a headless Ubuntu 24.04 LTS server. The project follows a dual-system architecture, where all administrative tasks are performed remotely from a workstation via SSH to simulate real-world data center management.

The primary objective is to create a secure, high-performance environment while adhering to sustainability goals. By optimizing resource utilization and implementing robust security controls—such as key-based authentication, firewall restrictions, and intrusion detection—this project demonstrates how to reduce the environmental impact of computing infrastructure through efficient OS configuration.

---

## Technical Journal: Table of Contents

### Phase 1: System Planning and Distribution Selection

Establishment of the dual-system architecture, selection of the Ubuntu distribution, and initial network configuration documentation.

- [Go to Week 1](./week1.md)

### Phase 2: Security Planning and Testing Methodology

Development of a security baseline checklist and a quantitative performance testing plan to evaluate system health.

- [Go to Week 2](./week2.md)

### Phase 3: Application Selection for Performance Testing

Selection of Nginx and MariaDB to represent network and I/O workloads, and establishment of a performance baseline.

- [Go to Week 3](./week3.md)

### Phase 4: Foundational Security Implementation

Configuration of SSH key-based authentication and UFW firewall rules to restrict access to the server.

- [Go to Week 4](./week4.md)

### Phase 5: Advanced Security and Monitoring

Implementation of Fail2Ban, AppArmor profiles, and custom automation scripts for security verification and remote monitoring.

- [Go to Week 5](./week5.md)

### Phase 6: Performance Evaluation and Analysis

Detailed load testing and analysis of OS behavior under different workloads, including resource optimization results.

- [Go to Week 6](./week6.md)

### Phase 7: Security Audit and Final Evaluation

A comprehensive system audit using Lynis and Nmap to verify the final security posture and Hardening Index.

- [Go to Week 7](./week7.md)

---

## Technical Specifications Summary

- **Server OS:** Ubuntu 24.04 LTS (Headless)
- **Workstation:** [Your Workstation Option, e.g., Windows 11 with PowerShell SSH]
- **Network Architecture:** VirtualBox Host-Only Network
- **Core Services:** Nginx Web Server, MariaDB Database
