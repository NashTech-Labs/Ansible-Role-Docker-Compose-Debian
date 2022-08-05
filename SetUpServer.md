## Step 1 — Logging in as root
```bash
  $ ssh root@your_server_ip
```
## Step 2 — Creating a New User
```bash
  $ adduser username
```
## Step 3 — Granting Administrative Privileges
```bash
  $ usermod -aG sudo username
```
## Step 4 — Setting Up a Basic Firewall
```bash
  $ ufw allow OpenSSH
  $ ufw enable
  $ ufw status
  Status: active
  To                         Action      From
  --                         ------      ----
  OpenSSH                    ALLOW       Anywhere
  OpenSSH (v6)               ALLOW       Anywhere (v6)
```