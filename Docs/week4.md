# Week 4: Initial System Configuration & Security Implementation

## Introduction
Following the successful installation of services in Phase 3, Phase 4 focuses on hardening the Ubuntu Server environment. In a production setting, a server exposed to a network is vulnerable to brute-force attacks and unauthorized access. To mitigate these risks, I have implemented a multi-layered security strategy including privilege management, cryptographic authentication, and network isolation via a "Default Deny" firewall policy.

---

## 1. User & Privilege Management

To adhere to the principle of "Least Privilege" and ensure accountability, I created a non-root administrative user. This ensures that the server is not managed using the default system account, which is a key security recommendation for Linux systems.

### 1.1 Create Non-Root User
I created a new user account specifically for administrative tasks. During this process, I provided identifying information to ensure the account is properly labeled in the system metadata.

```
sudo adduser benedict
```

### 1.2 Privilege Escalation (Sudo Group)
By default, new users do not have administrative power. I added the account to the sudo group to allow it to execute commands with elevated privileges when required.

```
sudo usermod -aG sudo benedict
```

<img width="628" height="69" alt="image" src="https://github.com/user-attachments/assets/171fd7b1-f5b6-42a9-8862-b67be5a060ab" />

---

## 2. Configure SSH with key-based authentication

SSH key-based authentication replaces password logins with cryptographic keys.
This improves security by preventing brute-force password attacks and ensuring only trusted devices can access the server.

Below is the complete process I used to configure SSH key-based authentication between my Windows workstation and my Ubuntu Server VM.

### 2.1 Generate an SSH Key Pair (Windows)

On my Windows machine, I generated an SSH key pair using PowerShell:
```
ssh-keygen -t ed25519
```
This created two files in my Windows .ssh directory:

- `id_ed25519` (private key)
- `id_ed25519.pub` (public key)

Directory:
```
C:\Users\Benedict Faraday\.ssh
```

<img width="770" height="191" alt="image" src="https://github.com/user-attachments/assets/ec00c8b9-50e6-4b6c-a2a4-ceec7d94e0b1" />

### 2.2 Transfer the Public Key to the Server

I used SCP to securely copy the public key to my Ubuntu VM:
```
scp "C:\Users\Benedict Faraday\.ssh\id_ed25519.pub" vboxuser@192.168.56.101:~/id_ed25519.pub
```

<img width="1479" height="58" alt="image" src="https://github.com/user-attachments/assets/2a4c31dd-aaa6-4aec-87d6-807dcd48f68d" />

### 2.3 Install the Public Key on the Server

Inside the Ubuntu VM (after SSH login):

```
mkdir -p ~/.ssh
chmod 700 ~/.ssh
cat ~/id_ed25519.pub > ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys
```

I verified the key installation with:

```
cat ~/.ssh/authorized_keys
```

### 2.4 Test Passwordless Login
I exited the session and reconnected:

``` Powershell
ssh vboxuser@192.168.56.101
```

SSH logged in without a password, confirming key-based authentication worked.

<img width="805" height="419" alt="image" src="https://github.com/user-attachments/assets/12e5eed9-1ae0-4ad0-a124-6824d92a2965" />

---

## 3. SSH Daemon Hardening

Setting up keys is only effective if password authentication is disabled. I modified the SSH configuration to ensure the server refuses password attempts entirely, preventing brute-force attacks.

### 3.1 Configuration Changes

I edited ``` /etc/ssh/sshd_config ``` to enforce the following security parameters:

| Parameter | Default/Previous State | Hardened State | Security Purpose |
| :--- | :--- | :--- | :--- |
| **PermitRootLogin** | `prohibit-password` | `no` | Disables direct root access to prevent brute-force attacks on the superuser account. |
| **PasswordAuthentication** | `yes` | `no` | Forces the server to only accept cryptographic keys and reject all password attempts. |

I used the following command to verify the active configuration:
```
sudo sshd -T | grep -E "passwordauthentication|permitrootlogin"
```

<img width="1019" height="91" alt="image" src="https://github.com/user-attachments/assets/91aaabf3-85c7-46b6-94fd-233ed425559c" />

---

## 4. Configure a firewall permitting SSH from one specific workstation only

The goal of this task was to secure the Ubuntu Server VM by allowing SSH access **only** from my workstation.  
This prevents any other device or network from accessing the server remotely.

### 4.1 Identify Workstation IP Address

On my workstation, I used ```ipconfig``` to check my VirtualBox Host-Only Adapter IP

<img width="823" height="286" alt="image" src="https://github.com/user-attachments/assets/5af98441-63c1-402a-9941-e201cadeca87" />

I renamed the adapter so that it would be easy for me to find it.

This is the only IP allowed to access SSH.

### 4.2 Implementation & Ruleset

I configured the Uncomplicated Firewall (UFW) with a "Default Deny" incoming policy. I then created specific exceptions to allow administrative access from my workstation and public access to the web services established in Phase 3.

| Rule Name | Action | From (Source) | Port / Protocol | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **SSH Management** | `ALLOW` | `192.168.56.1` | `22/tcp` | Restricts remote management to my specific workstation IP only. |
| **Nginx Full** | `ALLOW` | `Anywhere` | `80, 443/tcp` | Permits HTTP and HTTPS traffic for the web server to be public. |
| **Default Deny** | `DENY` | `Anywhere` | `All Ports` | Ensures any traffic not explicitly allowed is blocked by default. |

**Commands executed:**
```
sudo ufw default deny incoming
sudo ufw allow from 192.168.56.1 to any port 22
sudo ufw allow 'Nginx Full'
sudo ufw enable
```

### 4.3 Verification of Active Security

To confirm the firewall was correctly filtering traffic, I queried the status of the UFW service to ensure the rules were applied to the running system.

```
sudo ufw status verbose
```

<img width="742" height="306" alt="image" src="https://github.com/user-attachments/assets/2715091e-f67c-40e7-ae9d-5bd30580b4e4" />

---

## 5. Technical Reflection

### 5.1 Security vs. Usability
Implementing these controls creates a "Zero Trust" environment. While it significantly increases security, it introduces a "lockout risk" if the private key is lost or the workstation IP changes. In professional practice, this is balanced by using redundant backup keys or secure console access.

### 5.2 Sustainability Link
By filtering unauthorized traffic at the firewall level, the system saves CPU cycles that would otherwise be spent processing thousands of failed bot-login attempts. This reduces the energy consumption of the VM, supporting the efficiency goals mentioned in the Masanet et al. [3] study.

---

## 6. Conclusion
Phase 4 successfully transitioned the server from a default, vulnerable state to a hardened configuration. By disabling password authentication and restricting network access to a single trusted IP, the attack surface has been minimized. The server is now ready for final demonstration.

---

## References

1. **Masanet, E., Shehabi, A., Ramakrishnan, L., Liang, J., Fan, X., Lei, Z., & Koomey, J. (2020).** *Recalibrating global data center energy-use estimates.* Science, 367(6481), 984-986. [Online]. Available: https://doi.org/10.1126/science.aba3758 (Accessed 10 Jan 2026).
2. **Canonical Group.** (2024). *Ubuntu Server Documentation: Security - Firewall Configuration (UFW).* [Online]. Available: https://ubuntu.com/server/docs/security-firewalls (Accessed 10 Jan 2026).
3. **DigitalOcean Community.** (2023). *SSH Essentials: Working with SSH Servers, Clients, and Keys.* [Online]. Available: https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys (Accessed 10 Jan 2026).
4. **NIST (National Institute of Standards and Technology).** (2008). *Guide to General Server Security (Special Publication 800-123).* [Online]. Available: https://csrc.nist.gov/publications/detail/sp/800-123/final (Accessed 10 Jan 2026).
