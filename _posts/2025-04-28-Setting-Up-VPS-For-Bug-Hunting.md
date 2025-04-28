---
title: "Setting Up VPS For Bug Bounty Hunting"
date: 2025-04-28
categories: [Bug-Bounty]
tags: [Bug-Bounty, VPS, tools]
---

### 🧠 Day - 1: Setting up a Ubuntu VPS for Bug Bounty Hunting

---

#### ✅ VPS Initial Setup

1. **Login to VPS via SSH as root**
   - (Use either SSH key or root password sent via email)

   ```bash
   ssh root@your_server_ip
   ```

2. **Change root password**
   - Use a secure password generator (e.g., [bitwarden](https://bitwarden.com/password-generator/)) and store it securely.

   ```bash
   passwd
   ```

3. **Create a new user with sudo privileges**

   ```bash
   adduser bugbounty
   usermod -aG sudo bugbounty
   ```

4. **Verify the new user**

   ```bash
   su - bugbounty
   ```

---

#### 🔐 SSH Hardening

Edit the SSH config for extra security:

```bash
sudo nano /etc/ssh/sshd_config
```

Update the config with the following:

```conf
LoginGraceTime 60
PermitRootLogin no
StrictModes yes
MaxAuthTries 3
MaxSessions 2
PubkeyAuthentication yes
PasswordAuthentication no
AllowUsers bugbounty
ChallengeResponseAuthentication no
UsePAM yes
ClientAliveInterval 300
ClientAliveCountMax 3
```

Restart SSH to apply changes:

```bash
sudo systemctl restart ssh
```

---

#### 🛡️ Fail2Ban (Optional for Extra Security)

1. **Create/edit Fail2Ban config:**

   ```bash
   sudo nano /etc/fail2ban/jail.local
   ```

   Add:

   ```conf
   [sshd]
   enabled = true
   port = 22
   maxretry = 5
   bantime = 3600
   findtime = 600
   ```

2. **Restart Fail2Ban:**

   ```bash
   sudo systemctl restart fail2ban
   ```

---

#### 🔧 System & Tools Setup

1. **Update and upgrade system**

   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. **Install Go**

   ```bash
   sudo apt install golang -y
   echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
   echo 'export GOPATH=$HOME/go' >> ~/.bashrc
   echo 'export PATH=$PATH:$GOPATH/bin' >> ~/.bashrc
   source ~/.bashrc
   ```

3. **Install pipx and Python venv**

   ```bash
   sudo apt install pipx python3-venv -y
   pipx ensurepath
   source ~/.bashrc
   ```

---

#### 🧰 Install Bug Bounty Tools

```bash
go install -v github.com/projectdiscovery/subfinder/v2/cmd/subfinder@latest
go install -v github.com/projectdiscovery/httpx/cmd/httpx@latest
sudo apt install nmap -y
sudo apt install sqlmap -y
go install -v github.com/projectdiscovery/naabu/v2/cmd/naabu@latest
go install -v github.com/ffuf/ffuf@latest
git clone https://github.com/maurosoria/dirsearch.git
go install -v github.com/LukaSikic/subzy@latest
go install -v github.com/projectdiscovery/katana/cmd/katana@latest
go install -v github.com/lc/gau/v2/cmd/gau@latest
go install -v github.com/tomnomnom/waybackurls@latest
go install -v github.com/tomnomnom/anew@latest
go install -v github.com/sa7mon/S3Scanner@latest
go install -v github.com/hahwul/dalfox/v2@latest
go install -v github.com/projectdiscovery/nuclei/v3/cmd/nuclei@latest
go install -v github.com/KathanP19/Gxss@latest
go install -v github.com/s0md3v/uro@latest
sudo apt install wpscan
go install -v github.com/projectdiscovery/dnsx/cmd/dnsx@latest
git clone https://github.com/s0md3v/Arjun.git
```

---

✅ **Day 1 Completed!** Your bug bounty lab environment is now ready to go.

