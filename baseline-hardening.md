# Linux Server Baseline Hardening Standard Guide

Fresh server, first day. Do these in order. Every single time.
Update all packages first
Harden SSH 
Set up user accounts with proper permissions
Configure firewall, only open ports that actually need
Set up log monitoring
Configure automatic security updates
Disable unnecessary services

---

## 1st Update Everything First

Before anything else, patch the system. You don't want to harden a server that's already vulnerable.

Debian:
```bash
sudo apt update && sudo apt upgrade -y
```

RH:
``` bash
sudo dnf update -y
```

Then reboot:

```bash
sudo reboot
```

remark: new servers often ship with outdated packages. attackers knew default packages versions and their vulnerabilities.

---

## 2nd Create a sudo user or adding to sudo/wheel

never to work as root directly. create a normal user and give it sudo access. or wheel in RH world

Debian:
```bash
adduser yourname
usermod -aG sudo yourname
```

RH:
```bash
adduser yourname
usermod -aG wheel yourname
```

remarks: if you are always root, one mistake destroys everything. sudo gives you a safety net and an audit trail.

---

## 3rd Hardening SSH

this is the first front door. lock it properly.

first, back up the config:
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

generate your key pair on your (host) local machine (not the server):
```bash
ssh-keygen -t ed25519
```

copy the public key to the server:
```bash
ssh-copy-id yourname@server_IP
```

edit the SSH config on the server:
```bash
sudo nano /etc/ssh/sshd_config
```

set these values (uncomment if needed, remove the sharp sign # ):
```bash
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
AuthenticationMethods publickey
```

on RH, also check the override files:
```bash
sudo ls /etc/ssh/sshd_config.d/
```

remove the Anaconda root login override if it exists:
```bash
sudo rm /etc/ssh/sshd_config.d/01-permitrootlogin.conf
```

reload SSH without closing your current session:
```bash
sudo systemctl reload sshd
```

test from a new host terminal before closing the existing session:
```bash
ssh yourname@server_IP
```

only close the original session after confirming the new one works.

remarks: bots hammer port 22 constantly. keys are mathematically impossible to brute force. Passwords are not.

---

## 4th Configure the firewall

only open ports thay actually need. Close everything else.

Debian (ufw):
```bash
sudo apt install ufw
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status
```

RH(firewalld):
```bash
sudo systemctl start firewalld
sudo systemctl enable firewalld
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

if running a web server, also open:
```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

remarks: every open port is a potential attack surfaces. if a service doesnt need to be reachable from outside, it shouldnt be.

---

## 5th Secure Shared Memory

shared memory can be exploited to attack running services.

edit fstab:
```bash
sudo nano /etc/fstab
```

add this line at the bottom:
```bash
tmpfs /run/shm tmpfs defaults,noexec,nosuid 0 0
```

remarks: prevents attackers from executing malicious code through shared memory.

---

## 6th Disable unused services

every running service is a potential vulnerabilities. Disable what doesnt need.

check whats been running:
```bash
sudo systemctl list-units --type=service --state=running
```

disabled a service thaty don't need:
```bash
sudo systemctl stop servicename
sudo systemctl disable servicename
```

common ones to considers disabling if not needed:
```bash
bluetooth.service
cups.service (printing)
avahi-daemon.service (network discovery)
```

remarks: less attack surface means less risks. if a service isnt running, it cant be exploited.

---

## 7th Set up automatic security updates

people forget. automation dont.

Debian:
```bash
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades
```

RH:
```bash
sudo dnf install dnf-automatic
sudo nano /etc/dnf/automatic.conf
```

Set this in the config:
```bash
apply_updates = yes
```

enable and start it:
```bash
sudo systemctl enable dnf-automatic.timer
sudo systemctl start dnf-automatic.timer
```

remarks: most breaches happen through known vulnerabilities that already have patches available. automatic updates fixes that window.

---

## 8th Lock down file permissions

check that critical system files are owned by root and not writable by others.

```bash
sudo chmod 644 /etc/passwd
sudo chmod 640 /etc/shadow
sudo chmod 644 /etc/group
sudo chmod 600 /etc/ssh/sshd_config
```

remarks: If an attacker can write to /etc/passwd or /etc/shadow, they can create their own root account.

---

## 9th Set up basic log monitoring

logs tell everything. start watching them from day one.

watch authentication logs live:

Debian:
```bash
sudo tail -f /var/log/auth.log
```

RHEL:
```bash
sudo tail -f /var/log/secure
```

check for failed login attempts:

Debian:
```bash
sudo grep "Failed password" /var/log/auth.log
```

RHEL:
```bash
sudo grep "Failed password" /var/log/secure
```

count how many failed attempts:

Debian:
```bash
sudo grep -c "Failed password" /var/log/auth.log
```

remarks: if you see thousands of failed login attempts from the same IP, someone is actively trying to get in. you need to know that and note down that.

---

## 10th Verify everything is working

after all steps, do a final check.

check disk space is healthy:
```bash
df -h
```

checks no unexpected ports are open:
```bash
ss -tunlp
```

checks running services look normal:
```bash
sudo systemctl list-units --type=service --state=running
```

checked SSH is properly configureds:
```bash
sudo sshd -t
```
Note : he -t flag stands for Test Mode. last command tests the sshd_config for syntax errors without restarting anything. clean output means no errors. to use with nginx, it'll beocme like
```bash
sudo nginx -t
```
---

## Quick reference checklist

```bash
System fully updated and rebooted
Sudo user created, root login disabled
SSH keys set up, password auth disabled
Firewall configured, only needed ports open
Shared memory secured
Unnecessary services disabled
Automatic security updates enabled
Critical file permissions locked down
Log monitoring in place
Final double checking passed
```

---

## important rules to never forget

always back up config files before editing:
```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.bak
```

***always test SSH from a new terminal before closing your existing session.

***always read logs before making changes.

***always understand what a command does before running it.

***the server will tell exactly what is wrong if you know how to listen to it.

---
