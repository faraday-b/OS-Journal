# Week 1 - System Planning and Distribution Selection

## 1. System architecture diagram

<img width="490" height="114" alt="systemdiagram drawio" src="https://github.com/user-attachments/assets/bcb7a075-15b3-4885-918b-464744394f82" />

## 2. Distribution selection justification

I chose Ubuntu over the other distributions because when I tried to install Fedora, it did not run smoothly on my laptop despite making adjustments to the settings. Beyond performance, Ubuntu 24.04 LTS was selected for its long-term support (LTS) lifecycle and the robust apt package management system, which is ideal for a stable server environment.

## 3. Workstation configuration decision

By choosing Option B, I am leveraging the host machine as the workstation. This preserves hardware resources (CPU/RAM) for the server VM, ensuring high availability and preventing system instability during the hardening phases.

## 4. Network Configuration

| Text                       | Screenshots                                                                                              |
| :------------------------- | :------------------------------------------------------------------------------------------------------- |
| **VirtualBox Adapter 1**   | <img width="699" src="https://github.com/user-attachments/assets/4eb131e7-fbe4-4764-be28-797d85486cff">  |
| **VirtualBox Adapter 2**   | <img width="698" src="https://github.com/user-attachments/assets/92b3abab-cf9e-4ad2-be4f-38b3fca69fb7">  |
| **VirtualBox IP Address**  | <img width="1223" src="https://github.com/user-attachments/assets/0294af88-783d-4dfc-8a2f-48526e83644c"> |
| **Workstation IP Address** | <img width="821" src="https://github.com/user-attachments/assets/8d73d3e5-7c52-4410-8a6a-566483339ee1">  |

## 5. CLI system specifications

| Command         | Screenshot Evidence                                                                                     |
| :-------------- | :------------------------------------------------------------------------------------------------------ |
| **uname**       | <img width="400" src="https://github.com/user-attachments/assets/b40f5f4d-b0dd-4ec6-ab62-99af303f4d76"> |
| **free**        | <img width="400" src="https://github.com/user-attachments/assets/a8a25714-166a-4535-8347-4e286cce2d3c"> |
| **df -h**       | <img width="400" src="https://github.com/user-attachments/assets/58198e78-fe98-4d14-9235-afb93b09c970"> |
| **ip addr**     | <img width="400" src="https://github.com/user-attachments/assets/4afac84a-3058-486c-b1d3-7f1085b155d6"> |
| **lsb_release** | <img width="400" src="https://github.com/user-attachments/assets/64a16178-96ca-4beb-ac78-d5c2f17029a0"> |
