# Phase 3: Application Selection for Performance Testing

## 1. Selected Services
I have selected the following lightweight services to ensure the server remains stable on my host machine while meeting the functional requirements of the project:

* **Nginx Web Server:** Chosen for its event-driven architecture, which allows it to handle high concurrency with minimal memory overhead compared to process-based servers like Apache.
* **MariaDB Database:** Selected as a robust, open-source relational database that is highly compatible with Ubuntu and can be tuned for low-resource environments.

---

## 2. Justification and Resource Planning

### Why these applications?
The primary goal is to simulate a functional web environment without causing "swapping" on the host laptop, which can lead to significant performance degradation. Nginx and MariaDB are industry standards that are highly compatible with the **UFW** and **Fail2ban** security measures planned for Phase 4 [1].

### Resource Allocation Table
| Component | Status | Estimated RAM | **Actual RAM (Verified)** |
| :--- | :--- | :--- | :--- |
| **Ubuntu Base OS** | Running | 600 MB | ~600-800 MB |
| **Nginx** | Running | 50 - 100 MB | **3.7 MB** |
| **MariaDB** | Running | 150 - 250 MB | **78.6 MB** |
| **Total VM Load** | **Target** | **~1.0 GB** | **1.2 GB (Total Used)** |

---

## 3. Installation Strategy
I utilized the `apt` package manager to install these services via SSH. To keep the system lean and sustainable, I followed these steps:
1. **Core Installation:** Installed only essential packages to reduce disk footprint and potential attack vectors.
2. **Service Management:** Verified the services were running using `systemctl` to ensure they were correctly integrated with the OS.
3. **Remote Administration:** Performed all tasks via SSH to simulate real-world server management [1].

---

## 4. Implementation Evidence
Following the strategy above, I successfully installed and verified the services.

### 4.1: Nginx Installation & Verification
The web server was installed and confirmed as active. Initial monitoring shows an extremely low memory footprint, significantly lower than the initial estimate due to the server being in an idle state.

<img width="937" alt="Nginx Status Evidence" src="https://github.com/user-attachments/assets/2a2add01-617c-40fd-a957-9564d473ece3" />

### 4.2: MariaDB Installation & Verification
The database server was installed to provide back-end functionality. It is currently running in its default configuration, utilizing approximately 78.6 MB of RAM.

<img width="924" alt="MariaDB Status Evidence" src="https://github.com/user-attachments/assets/02c3bfbe-39f2-413c-bcc5-67f2bef29002" />

### 4.3 Monitoring Strategy & Performance Baseline
To conclude Phase 3, I established a resource consumption baseline. This satisfies the requirement for a monitoring strategy by explaining the measurement approach used to evaluate system health before security hardening [1].

#### Measurement Approach
All monitoring is performed via SSH to adhere to project constraints [1]. I am using a dual-layered approach:
* **System-Level:** Utilizing `free -h` to monitor total RAM and Swap usage to prevent host machine instability.
* **Process-Level:** Utilizing `ps aux` and `systemctl status` to isolate the specific footprint of Nginx and MariaDB.

#### Baseline Evidence Table
| Metric | CLI Command | Evidence | Result |
| :--- | :--- | :--- | :--- |
| **System Memory** | `free -h` | <img width="400" alt="Free -h Output" src="https://github.com/user-attachments/assets/5be5bf3e-29c0-414a-9fec-05d89b06342f" /> | **1.2Gi Used**; 2.5Gi Available. No swap usage detected. |
| **Process RAM %** | `ps aux --sort=-%mem \| head -n 6` | <img width="400" alt="PS AUX Output" src="https://github.com/user-attachments/assets/bd922c9d-ad17-4e63-92cf-1460e837d157" /> | **MariaDB** is the top consumer at **2.6%** MEM. |
| **Nginx Status** | `systemctl status nginx` | <img width="400" alt="Nginx Status" src="https://github.com/user-attachments/assets/2a2add01-617c-40fd-a957-9564d473ece3" /> | Service is **active**; utilizing **3.7MB** RAM. |
| **MariaDB Status** | `systemctl status mariadb` | <img width="400" alt="MariaDB Status" src="https://github.com/user-attachments/assets/02c3bfbe-39f2-413c-bcc5-67f2bef29002" /> | Service is **active**; utilizing **78.6MB** RAM. |

---

#### Technical Reflection & Trade-off Analysis
* **Quantitative Success:** The total memory usage of **1.2Gi** aligns with the project's sustainability goals by minimizing resource waste on the host machine.
* **Performance Buffer:** With **2.5Gi of RAM available**, there is significant overhead to implement the mandatory security controls in Phase 4 without risking performance bottlenecks [1].

### Phase 3 Conclusion
The applications were successfully installed and baseline metrics recorded. This data proves the Ubuntu server is a stable, optimized environment ready for the Phase 4 security implementation.

---

## References
[1] University of Roehampton, "CMPN202 Operating Systems Assessment Brief," 2025. [Online].

[2] Nginx Documentation, "Nginx Beginner's Guide," 2026. [Online]. Available: https://nginx.org/en/docs/beginners_guide.html.

[3] MariaDB Foundation, "MariaDB Knowledge Base - systemd," 2026. [Online]. Available: https://mariadb.com/kb/en/library/systemd/.

[4] Ubuntu Documentation, "The Free Command," 2026. [Online]. Available: https://manpages.ubuntu.com/manpages/noble/man1/free.1.html.

[5] E. Masanet et al., "Recalibrating global data center energy-use estimates," Science, vol. 367, no. 6481, pp. 984-986, 2020. [Online]. Available: https://www.science.org/doi/10.1126/science.aba3758.

---

## AI Acknowledgement
In accordance with university guidance, I acknowledge the use of Gemini (Large Language Model) in developing this journal.

**Nature of AI Use:**
* **Technical Guidance:** Support with command syntax for `systemctl` and `ps aux`.
* **Formatting:** Assistance in structuring Markdown tables and the IEEE reference list.
* **Analysis:** Support in interpreting the "Available" vs "Used" RAM metrics in the Linux kernel.

**Student Verification:**
All terminal commands were manually executed by me via SSH on the Ubuntu Server VM. All screenshots and quantitative data represent my own primary implementation and verification work.
