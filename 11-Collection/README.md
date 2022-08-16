# Collection

The adversary is trying to gather data of interest to their goal.

## General data to collect after compromise

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
  - `systemctl list-unit-files --type service -all`
  - `systemctl list-units --type=service` or `systemctl list-units --type=service --state=active`
  - `netstat -ltup`
  - `ss -ltup`
- ARP Table
  - `arp -a`
- Hashes `cat /etc/shadow`

**Windows**
- Hostname
  - `hostname`
- OS info
  - `systeminfo`
- Patches
  - `wmic qfe list`
- Network
  - `ipconfig`
- Users and Groups
  - `net user`
  - Admin users `net localgroup Administrators`
- Processes
- Installed software
  - `wmic product get name`
- Scheduled Tasks
- File System
  - `tree C://`
- Services
  - Just names `net start`
  - More detailed `wmic service where (state="running") get caption, name, startmode, state`
  - With ports
- ARP Table
  - `arp -a`
- AV info
  - `wmic /namespace:\\root\securitycenter2 path antivirusproduct GET displayName, productState, pathToSignedProductExe`
- Hashes
  - Mimikatz
  - Check [Add Link](TODO)
- Clean logs
  - `wmic nteventlog where filename='system' call cleareventlog`
- Computer Status
	- Powershell `Get-MpComputerStatus`
	
## Archive collected data after collection

**ZIP tools**
