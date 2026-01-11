# Phase 1: System Planning and Distribution Selection

## 1. System Architecture Diagram

A high-level overview of the network topology and system interaction between the host workstation and the virtualized Ubuntu server.

<img width="490" height="114" alt="systemdiagram drawio" src="https://github.com/user-attachments/assets/bcb7a075-15b3-4885-918b-464744394f82" />

---

## 2. Distribution Selection Justification

The selection process focused on balancing hardware compatibility with long-term stability.

I chose **Ubuntu 24.04 LTS** over alternatives like Fedora for several critical reasons:

- **Hardware Performance:** Initial testing with Fedora resulted in performance degradation and stuttering on my host hardware. Ubuntu ran significantly smoother without requiring extensive driver adjustments.
- **Lifecycle Support:** The Long-Term Support (LTS) lifecycle ensures five years of security patches, which is essential for a stable server environment.
- **Package Management:** The `apt` package manager provides a robust and familiar ecosystem for the upcoming hardening and optimization phases.

---

## 3. Workstation Configuration Decision

I have selected **Option B**, utilizing my host machine as the primary workstation.

**Justification:**
By leveraging the host machine for administration rather than a second VM, I am preserving critical hardware resources (CPU and RAM). This ensures that the server VM has access to maximum available performance, maintaining high availability and system stability during intensive security hardening tasks.

---

## 4. Network Configuration Documentation

Detailed documentation of the VirtualBox network adapters and the established IP addressing scheme for internal and external communication.

| Deliverable                          | Screenshot Evidence                                                                                      |
| :----------------------------------- | :------------------------------------------------------------------------------------------------------- |
| **VirtualBox Adapter 1 (NAT)**       | <img width="699" src="https://github.com/user-attachments/assets/4eb131e7-fbe4-4764-be28-797d85486cff">  |
| **VirtualBox Adapter 2 (Host-Only)** | <img width="698" src="https://github.com/user-attachments/assets/92b3abab-cf9e-4ad2-be4f-38b3fca69fb7">  |
| **Server VM IP Address**             | <img width="1223" src="https://github.com/user-attachments/assets/0294af88-783d-4dfc-8a2f-48526e83644c"> |
| **Workstation Host IP Address**      | <img width="821" src="https://github.com/user-attachments/assets/8d73d3e5-7c52-4410-8a6a-566483339ee1">  |

---

## 5. CLI System Specifications

The following CLI outputs verify the kernel version, resource allocation, storage, and distribution details of the deployed server.

| Command                         | Screenshot Evidence                                                                                     |
| :------------------------------ | :------------------------------------------------------------------------------------------------------ |
| **uname -a** (Kernel Info)      | <img width="400" src="https://github.com/user-attachments/assets/b40f5f4d-b0dd-4ec6-ab62-99af303f4d76"> |
| **free -m** (RAM Allocation)    | <img width="400" src="https://github.com/user-attachments/assets/a8a25714-166a-4535-8347-4e286cce2d3c"> |
| **df -h** (Disk Usage)          | <img width="400" src="https://github.com/user-attachments/assets/58198e78-fe98-4d14-9235-afb93b09c970"> |
| **ip addr** (Interface Config)  | <img width="400" src="https://github.com/user-attachments/assets/4afac84a-3058-486c-b1d3-7f1085b155d6"> |
| **lsb_release -a** (OS Version) | <img width="400" src="https://github.com/user-attachments/assets/64a16178-96ca-4beb-ac78-d5c2f17029a0"> |
