# Collection

The adversary is trying to gather data of interest to their goal.

## General Loots

**Linux**
- Hostname
  - `hostname`
- OS info
  - `uname -a`
- Network
  - `ifconfig -a`
- Users and Groups
  - `cat /etc/passwd`
  - `cut -d: -f1 /etc/passwd` or `cut -d: -f1 /etc/passwd`
  - `getent passwd`
- Processes
  - `ps aux`
- Installed software
  - `apt list --installed`
  - `dpkg -l`
  - check sudo `apt list -a sudo`
- Scheduled Tasks
  - `crontab -l`
  - services by a user `sudo crontab -u [username] -l`
  - `ls -la /etc/cron.hourly`
  - `ls -la /etc/cron.daily`
  - `ls -la /etc/cron.weekly`
  - `ls -la /etc/cron.monthly`
- File System
  - `tree`
  - `du -a`
- Services
  - `systemctl list-units --type=service` or `systemctl list-units --type=service --state=active`
  - `netstat -ltup`
  - `ss -ltup`
- After PrivEsc
  - Hashes `cat /etc/shadow`

**Windows**
- OS info
  - `systeminfo`
- Network
- Users and Groups
- Processes
- Installed software
- Scheduled Tasks
- File System
- Services
- After PrivEsc
  - Hashes

## Archive collected data after collection

**ZIP tools**
