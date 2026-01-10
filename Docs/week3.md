# Phase 3: Application Selection for Performance Testing

## 1. Selected Services

I have selected the following lightweight services to ensure the server remains stable on my 8GB RAM host machine:

* **Nginx Web Server:** Chosen for its event-driven architecture which handles high concurrency with very low memory overhead.
* **MariaDB Database:** Chosen as a robust relational database that can be tuned specifically for low-resource environments.

---

## 2. Justification and Resource Planning

### Why these applications?
The primary goal is to simulate a functional web environment without causing "swapping" on the host laptop. Nginx and MariaDB are industry standards that are highly compatible with the **UFW** and **Fail2ban** security measures planned in Phase 2.

### Resource Allocation Table
| Component | Status | Estimated RAM Usage |
| :--- | :--- | :--- |
| **Ubuntu Base OS** | Running | 600 MB |
| **Nginx** | Proposed | 50 - 100 MB |
| **MariaDB** | Proposed | 150 - 250 MB |
| **Total VM Load** | **Target** | **~1.0 GB** |

---

## 3. Installation Strategy

I will use the `apt` package manager to install these services. To keep the system lean, I will:
1.  Install only the core packages (no recommended "bloat" packages).
2.  Disable the services from starting automatically until the security hardening in Phase 4 is complete.
3.  Monitor the impact on the Host-Only network latency during the initial setup.

---

## 4. Implementation Evidence
Following the strategy above, I successfully installed and verified the services via SSH.

### 4.1: Nginx Installation & Verification
The web server was installed and confirmed as active. Initial monitoring shows an extremely low memory footprint, significantly lower than the initial 50MB estimate.

<img width="937" height="402" alt="image" src="https://github.com/user-attachments/assets/2a2add01-617c-40fd-a957-9564d473ece3" />

Nginx used significantly less memory than I expected. This is because the server is currrently idle with no active web traffic.

### 4.2: MariaDB Installation & Verification
The database server was installed to provide back-end functionality. It is currently running in its default configuration, ready for performance tuning.

<img width="924" height="578" alt="image" src="https://github.com/user-attachments/assets/02c3bfbe-39f2-413c-bcc5-67f2bef29002" />

MariaDB used slightly less memory than I estimated.

### 4.3 Monitoring Strategy & Performance Baseline

To conclude Phase 3, I have established a resource consumption baseline. [cite_start]This satisfies the requirement for a monitoring strategy by explaining the measurement approach used to evaluate system health before the security hardening in Phase 4 and Phase 5[cite: 49]. 

#### Measurement Approach
[cite_start]All monitoring is performed via SSH from the workstation to adhere to the project's administrative constraints[cite: 16, 60]. I am using a dual-layered approach:
1. [cite_start]**System-Level:** Utilizing `free -h` to monitor total RAM and Swap usage to prevent host machine instability[cite: 37].
2. [cite_start]**Process-Level:** Utilizing `ps aux` and `systemctl status` to isolate the resource footprint of Nginx and MariaDB individually[cite: 4, 131].

#### Baseline Evidence Table
The following data represents the "clean" state of the server with both selected applications running in their default configurations.

| Metric | CLI Command | Evidence | Result |
| :--- | :--- | :--- | :--- |
| **System Memory** | `free -h` | <img width="983" alt="image" src="https://github.com/user-attachments/assets/5be5bf3e-29c0-414a-9fec-05d89b06342f" /> | **1.2Gi Used**; 2.5Gi Available. No swap usage detected, ensuring host stability. |
| **Process RAM %** | `ps aux --sort=-%mem \| head -n 6` | <img width="1362" alt="image" src="https://github.com/user-attachments/assets/bd922c9d-ad17-4e63-92cf-1460e837d157" /> | **MariaDB** is the top consumer at **2.6%** MEM; Nginx usage is negligible. |
| **Nginx Status** | `systemctl status nginx` | <img width="937" alt="image" src="https://github.com/user-attachments/assets/2a2add01-617c-40fd-a957-9564d473ece3" /> | Service is **active (running)**; utilizing only **3.7MB** of RAM. |
| **MariaDB Status** | `systemctl status mariadb` | <img width="924" alt="image" src="https://github.com/user-attachments/assets/02c3bfbe-39f2-413c-bcc5-67f2bef29002" /> | Service is **active (running)**; utilizing **78.6MB** of RAM. |

---

#### Technical Reflection & Trade-off Analysis
* **Quantitative Success:** The actual total memory usage of **1.2Gi** aligns with the initial resource planning to keep the VM load around 1.0 GB. 
* **Optimized Selection:** Nginx's footprint of 3.7MB confirms the "event-driven architecture" justification from Section 1, providing a high-performance web server with nearly zero idle overhead.
* **Security Buffer:** With **2.5Gi of RAM available**, there is significant overhead to implement the mandatory security controls in the next phases, such as **UFW** and **Fail2ban**, without risking a system crash or performance bottleneck.

### Phase 3 Conclusion
The applications were successfully installed and baseline metrics recorded via SSH. [cite_start]This quantitative data proves that the Ubuntu server is a stable, optimized environment ready for the Phase 4 security implementation[cite: 12, 50].

## References

[1] University of Roehampton, "CMPN202 Operating Systems Assessment Brief," 2025. [Online]. Available: [Insert link to your Moodle document]. Accessed: Jan. 10, 2026.

[2] Nginx Documentation, "Nginx Beginner's Guide," 2026. [Online]. Available: https://nginx.org/en/docs/beginners_guide.html. Accessed: Jan. 10, 2026.

[3] MariaDB Foundation, "MariaDB Knowledge Base - systemd," 2026. [Online]. Available: https://mariadb.com/kb/en/library/systemd/. Accessed: Jan. 10, 2026.

[4] Ubuntu Documentation, "The Free Command," 2026. [Online]. Available: https://manpages.ubuntu.com/manpages/noble/man1/free.1.html. Accessed: Jan. 10, 2026.

[5] E. Masanet, A. Shehabi, N. Lei, S. Smith, and J. Koomey, "Recalibrating global data center energy-use estimates," Science, vol. 367, no. 6481, pp. 984-986, 2020. [Online]. Available: https://www.science.org/doi/10.1126/science.aba3758. Accessed: Jan. 10, 2026.

---

## AI Acknowledgement

[cite_start]In accordance with the University of Roehampton's AI guidance[cite: 220, 222], I acknowledge the use of Gemini (a Large Language Model) in the development of this technical journal. 

**Nature of AI Use:**
* **Technical Guidance:** Assistance with specific command-line syntax for `systemctl`, `ps aux`, and `ufw`.
* **Formatting:** Support in structuring the Technical Journal using Markdown and creating comparative tables for performance data.
* **Troubleshooting:** Identifying methods to interpret memory usage (e.g., distinguishing between 'Used' and 'Available' RAM in Linux).

**Student Verification:**
All terminal commands were manually executed by me via SSH on the Ubuntu Server VM. All screenshots and quantitative data presented in this journal represent my own primary work and local system environment.
