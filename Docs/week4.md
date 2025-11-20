# Phase 4: Initial System Configuration & Security Implementation

1. Configure SSH with key-based authentication

SSH key-based authentication replaces password logins with cryptographic keys.
This improves security by preventing brute-force password attacks and ensuring only trusted devices can access the server.

Below is the complete process I used to configure SSH key-based authentication between my Windows workstation and my Ubuntu Server VM.

1.1 Generate an SSH Key Pair (Windows)

On my Windows machine, I generated an SSH key pair using PowerShell:
ssh-keygen -t ed25519

This created two files in my Windows .ssh directory:

id_ed25519 → the private key (kept securely on my workstation)

id_ed25519.pub → the public key (safe to upload to the server)

Location of the keys:
C:\Users\Benedict Faraday\.ssh

<img width="770" height="191" alt="image" src="https://github.com/user-attachments/assets/ec00c8b9-50e6-4b6c-a2a4-ceec7d94e0b1" />

1.2 Transfer the Public Key to the Server

I used SCP to securely copy the public key to my Ubuntu VM:

scp "C:\Users\Benedict Faraday\.ssh\id_ed25519.pub" vboxuser@192.168.56.101:~/id_ed25519.pub

(Host-Only Adapter IP)

<img width="1479" height="58" alt="image" src="https://github.com/user-attachments/assets/2a4c31dd-aaa6-4aec-87d6-807dcd48f68d" />







