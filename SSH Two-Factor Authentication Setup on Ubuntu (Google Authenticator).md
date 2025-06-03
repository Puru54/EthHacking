 

This guide helps you set up SSH two-factor authentication (2FA) using Google Authenticator on an Ubuntu VM.

*setup ubuntu vm server for this 
link: https://releases.ubuntu.com/24.04.2/ubuntu-24.04.2-live-server-amd64.iso 

*set up a live-server ubuntu vm as it comes with preinstalled sshd service 



---

## 📋 Prerequisites
- Ubuntu VM (20.04 or later recommended)
- SSH access and sudo privileges
- Installed SSH server
![[Pasted image 20250603131612.png]]
*	*make sure that sshd is active and running* 

---

##  Step 1: Install Google Authenticator PAM Module
```bash
sudo apt update
sudo apt install libpam-google-authenticator
```

---

##  Step 2: Configure Google Authenticator for Your User

*You will need to install and setup Google Authenticator in your phone 

Run the following command as the user who will use SSH:
```bash
google-authenticator
```
Answer the prompts:
- Choose **time-based tokens**
- Scan the QR code using an app like Google Authenticator or Authy
- Save the emergency codes displayed
- Answer **yes** to the configuration questions (recommended)
![[Pasted image 20250603132023.png]]

---

##  Step 3: Configure PAM for SSH
Edit the PAM configuration file for SSH:
```bash
sudo nano /etc/pam.d/sshd
```
Add this line near the top:
```bash
auth required pam_google_authenticator.so
```
![[Pasted image 20250603131838.png]]

---

##  Step 4: Configure SSH Daemon
Edit the SSH daemon config:
```bash
sudo nano /etc/ssh/sshd_config
```
Ensure the following lines are set:
```bash
ChallengeResponseAuthentication yes
UsePAM yes
AuthenticationMethods password,keyboard-interactive
```
> If you use public key authentication:
```bash
AuthenticationMethods publickey,keyboard-interactive
```
![[Pasted image 20250603131908.png]]

---

##  Step 5: Restart SSH Service
```bash
sudo systemctl restart ssh
```

---

##  Step 6: Test the Setup
From another system, connect via SSH:
```bash
ssh username@your-server-ip
```
You should be prompted for:
1. Your account password
2. Your TOTP code (from Google Authenticator)

---



##  Summary
| Step | Action |
|------|--------|
| 1 | Install PAM module |
| 2 | Configure Google Authenticator per user |
| 3 | Update PAM and sshd_config |
| 4 | Restart SSH |
| 5 | Test login |

---


