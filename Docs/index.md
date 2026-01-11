# Technical Journal: Linux Server Configuration, Security, and Performance

## Project Overview

This technical journal documents the deployment, hardening, and evaluation of a headless Ubuntu 24.04 LTS server. Following professional remote administration standards, all system management is performed via a Command Line Interface (CLI) over SSH from a separate workstation.

A core focus of this project is sustainability. Data centres currently consume approximately 1% of global electricity, with projections reaching 3-8% by 2030. This project demonstrates how optimised OS configurations can reduce server energy consumption by 15-30% through improved resource utilisation and the elimination of unnecessary graphical interfaces.

---

## Technical Journal: Table of Contents

### Phase 1: System Planning and Distribution Selection (Week 1)

Documentation of the dual-system architecture, selection of the Ubuntu distribution, and initial network configuration.

- [Go to Week 1](./week1.md)

### Phase 2: Security Planning and Testing Methodology (Week 2)

Development of a security baseline checklist, threat model, and performance testing approach.

- [Go to Week 2](./week2.md)

### Phase 3: Application Selection for Performance Testing (Week 3)

Selection of applications representing diverse workload types (CPU, RAM, I/O) and establishment of resource profiles.

- [Go to Week 3](./week3.md)

### Phase 4: Initial System Configuration & Security Implementation (Week 4)

Implementation of foundational security controls, including key-based SSH authentication and firewall rules.

- [Go to Week 4](./week4.md)

### Phase 5: Advanced Security and Monitoring Infrastructure (Week 5)

Deployment of AppArmor, Fail2Ban, and custom automation scripts for server-side verification and remote monitoring.

- [Go to Week 5](./week5.md)

### Phase 6: Performance Evaluation and Analysis (Week 6)

Detailed analysis of operating system behaviour under load and identification of performance bottlenecks.

- [Go to Week 6](./week6.md)

### Phase 7: Security Audit and System Evaluation (Week 7)

Final system audit using Lynis and Nmap to verify the security posture and overall configuration.

- [Go to Week 7](./week7.md)

---

## Technical Specifications Summary

- **Server OS:** Ubuntu 24.04 LTS (Headless)
- **Administration:** Remote CLI via SSH
- **Architecture:** Dual-system (Workstation to Server)
- **Security Goal:** Achieve Lynis security score >80
