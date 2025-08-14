# Lab Sheet — Deploying an Application on a DigitalOcean Droplet (with Verification)
**Version:** 2025-08-14  
**Goal:** Provision a secure Linux VM (Droplet), harden access, and prepare it for application deployment — in a platform-agnostic way (not tied to any specific framework).

---

## Submission Requirements (What to Hand In)
- **Screenshots** listed under each step (exactly as requested).
- **Reflection (2–3 lines per step):** Describe **what** you did, **why** it’s necessary, and the **benefit**.
- Submit as a single PDF or a zipped folder of images + a Markdown/Doc file with your reflections.

> Replace all placeholders like `<ip_address_of_your_droplet>`, `<your_email@example.com>`, and `<your_ip_address>` with real values.

---

## 1) Create a New Droplet (Can skip if you are already done)
Provision a new Linux VM (e.g., Ubuntu 22.04 LTS) on DigitalOcean. Note its **public IP**.

### Commands/Actions
- Create droplet in DigitalOcean control panel
- Record: IP, region, image, and size

### Screenshot to Capture
- **DigitalOcean Droplet details page** showing the droplet name, public IP, image, and region.

### Reflection (2–3 lines)
- What was done?  
- Why this step is needed?  
- Benefit to the deployment process?

---

## 2) Connect to the Droplet via SSH/Password (as root, first login)
```bash
ssh root@<ip_address_of_your_droplet>
```

### Screenshot to Capture
- Terminal session **after** you successfully log in as `root` (the welcome banner or prompt).

### Reflection (2–3 lines)
- What was done?  
- Why SSH access is important?  
- Benefit for remote administration?

---

## 3) Update Software Packages
```bash
apt-get update && apt-get upgrade
```

### Screenshot to Capture
- Terminal output **showing completed package updates** (the last few lines are sufficient).

### Reflection (2–3 lines)
- What was done?  
- Why updates matter?  
- Benefit regarding security and stability?

---

## 4) Set the Hostname
```bash
hostnamectl set-hostname my-server
hostname   # verify
```

### Screenshot to Capture
- Output of `hostname` **showing the new hostname**.

### Reflection (2–3 lines)
- What was done?  
- Why set a hostname?  
- Benefit for identification and management?

---

## 5) Update `/etc/hosts`
```bash
vi /etc/hosts
# Add a line like:
# <ip_address_of_your_droplet> my-server
```

### Screenshot to Capture
- A **`cat /etc/hosts`** output showing your new line mapping IP → hostname.

### Reflection (2–3 lines)
- What was done?  
- Why map IP to hostname?  
- Benefit for local name resolution and tooling?

---

## 6) Create a Non-Root User with Sudo
```bash
adduser newadmin
adduser newadmin sudo

exit
ssh newadmin@<ip_address_of_your_droplet>
```

### Screenshot to Capture
- Terminal **prompt** showing you’re logged in as `newadmin` (e.g., `newadmin@my-server:~$`)
- Output of:
```bash
id
groups
```
(You can run both as `newadmin` to show group membership including `sudo`.)

### Reflection (2–3 lines)
- What was done?  
- Why avoid using root directly?  
- Benefit for principle of least privilege and auditability?

---

## 7) Set Up SSH Key Authentication
**On the remote server:**
```bash
mkdir -p ~/.ssh
```

**On the local machine:**
```bash
ssh-keygen -b 4096 -t rsa -C "<your_email@example.com>"
scp ~/.ssh/id_rsa.pub newadmin@<ip_address_of_your_droplet>:~/.ssh/authorized_keys
```

**On the remote server:**
```bash
ls -la ~/.ssh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

**Login test (passwordless):**
```bash
ssh newadmin@<ip_address_of_your_droplet>
```

### Screenshot to Capture
- `ls -la ~/.ssh` output (showing `authorized_keys` present and permissions)
- Successful passwordless login as `newadmin`

> **Privacy tip:** If you open `authorized_keys`, truncate or blur most of the key for submission.

### Reflection (2–3 lines)
- What was done?  
- Why use key-based auth?  
- Benefit for security and convenience?

---

## 8) Harden SSH (Disable Root & Password Auth)
Edit SSH config:
```bash
sudo vi /etc/ssh/sshd_config
# Set:
# PermitRootLogin no
# PasswordAuthentication no
```

Restart SSH:
```bash
sudo systemctl restart ssh
```

Validate effective config:
```bash
sudo sshd -T | grep -E 'permitrootlogin|passwordauthentication'
sudo grep -r "PasswordAuthentication" /etc/ssh/sshd_config.d/
```

(Optional) Negative test from local:
```bash
ssh -o PreferredAuthentications=password root@<ip_address_of_your_droplet>
# Expect failure if password auth and root login are disabled
```

### Screenshot to Capture
- Output of `sshd -T | grep ...` showing `permitrootlogin no` and `passwordauthentication no`
- (Optional) Failed password-login attempt proof (with sensitive details redacted)

### Reflection (2–3 lines)
- What was done?  
- Why disable root/password logins?  
- Benefit regarding attack surface reduction?

---

## 9) Configure Firewall (UFW)
Install and configure:
```bash
sudo apt-get install ufw

sudo ufw default allow outgoing
sudo ufw default deny incoming

sudo ufw allow ssh
sudo ufw allow 8000

sudo ufw enable
sudo ufw status verbose
```

### Screenshot to Capture
- `sudo ufw status verbose` **showing**:
  - Default incoming: **deny**
  - Default outgoing: **allow**
  - Open ports for **OpenSSH** and **8000**

### Reflection (2–3 lines)
- What was done?  
- Why apply least-privilege network policy?  
- Benefit for reducing exposure and complying with best practices?

---

## 10) Install & Configure Fail2Ban
Install:
```bash
sudo apt install fail2ban
```

(Reference files will be in `/etc/fail2ban/`)

Copy base configs and edit:
```bash
cd /etc/fail2ban
sudo cp fail2ban.conf fail2ban.local
sudo cp jail.conf jail.local
sudo vi jail.local
```

Under `[sshd]`, ensure:
```
[sshd]
enabled = true
port = ssh
filter = sshd
logpath = %(sshd_log)s
maxretry = 3
bantime = 24h
findtime = 10m
ignoreip = 127.0.0.1/8 ::1 <your_ip_address>
```

Restart & enable:
```bash
sudo systemctl restart fail2ban
sudo systemctl status fail2ban
sudo systemctl enable --now fail2ban
```

Check logs / status:
```bash
sudo tail -f /var/log/fail2ban.log
# In another terminal:
sudo fail2ban-client status sshd
```

### Screenshot to Capture
- `sudo systemctl status fail2ban` showing **active (running)**
- `sudo fail2ban-client status sshd` showing jail status and any banned IPs (if testing)

### Reflection (2–3 lines)
- What was done?  
- Why use Fail2Ban?  
- Benefit in defending brute-force attacks?

---

## Final Self‑Check (Optional, Recommended)
- [ ] Logging in as **root** is blocked.  
- [ ] **Password** authentication is disabled.  
- [ ] **UFW** is enabled with only necessary ports open.  
- [ ] **Fail2Ban** is active and monitoring `sshd`.  
- [ ] You can log in as **`newadmin`** using **SSH keys**.  
- [ ] Hostname and `/etc/hosts` mapping are correct.

---

## Tips for Clean Submissions
- Blur/redact **private keys**, secrets, and full IPs if required by policy.
- Keep terminal fonts large enough to read.
- Create a PDF file and send to "samadhivkcom@gmail.com" or "SLaksahan@innodata.com".

**End of Lab.**
