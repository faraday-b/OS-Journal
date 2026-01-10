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


## 4. Configure a firewall permitting SSH from one specific workstation only

The goal of this task was to secure the Ubuntu Server VM by allowing SSH access **only** from my workstation.  
This prevents any other device or network from accessing the server remotely.

---

### 4.1 Identify Workstation IP Address

On my Windows machine, I used the following command to check my VirtualBox Host-Only Adapter IP:

```
ipconfig
```

I renamed the adapter so that it would be easy for me to find it.

```
IPV4 Address: 192.168.56.1
```

This is the only IP allowed to access SSH.
