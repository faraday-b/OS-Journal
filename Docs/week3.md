# Application Selection for Performance Testing

## Task 1: Application Selection

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
