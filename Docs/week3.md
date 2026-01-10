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

### 4.1 Nginx Installation & Verification
The web server was installed and confirmed as active. Initial monitoring shows an extremely low memory footprint, significantly lower than the initial 50MB estimate.

<img width="937" height="402" alt="image" src="https://github.com/user-attachments/assets/2a2add01-617c-40fd-a957-9564d473ece3" />

Nginx used significantly less memory than I expected. This is because the server is currrently idle with no active web traffic.

### 4.2 MariaDB Installation & Verification
The database server was installed to provide back-end functionality. It is currently running in its default configuration, ready for performance tuning.

<img width="924" height="578" alt="image" src="https://github.com/user-attachments/assets/02c3bfbe-39f2-413c-bcc5-67f2bef29002" />

MariaDB used slightly less memory than I estimated.

### 4.3 Performance Testing Baseline

<img width="983" height="87" alt="image" src="https://github.com/user-attachments/assets/5be5bf3e-29c0-414a-9fec-05d89b06342f" />

Overall Usage: The system is utilizing 1.2Gi of RAM, leaving 2.5Gi available for future security tools.

Process Breakdown: Using ps aux, I confirmed that MariaDB (mariadbd) is one of the top consumers at 2.6% of total memory, while Nginx remains negligible.

Efficiency: Swap usage is at 0B, confirming no disk-thrashing is occurring.

---

## 5. 


