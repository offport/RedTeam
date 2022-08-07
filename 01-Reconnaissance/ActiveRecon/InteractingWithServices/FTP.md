## FTP - Cheatsheet

- Banner grabbing

		nc -vn <IP> 21
		
- Install python FTP server package

		pip3 install pyftpdlib

- Start a python FTP server

		python3 -m pyftpdlib

- Install FTP in linux
	
		sudo apt-get install vsftpd
		
		service vsftpd start
		
- Enable anonymous login in /etc/vsftpd.conf

		anonymous_enable=YES
	
- Connect to FTP server

		ftp -p <ip> <port>
		
- Other command

	-   `USER username`
    
	-   `PASS password`
    
	-   `HELP` The server indicates which commands are supported or `help`
		
- Connect in browser

		ftp://127.0.0.1:2121/
		
		ftp://anonymous:password@127.0.0.1:2121/
		
- Anonymous credentials

		anonymous : anonymous 
		anonymous : 
		ftp : ftp
	
- Downloads and Upload in FTP
	
	https://tecadmin.net/download-upload-files-using-ftp-command-line/
	
	a. Download
	
		get <filename>
		
	b. Upload
	
		put <filename>
		
- Download using Wget

		wget -m ftp://anonymous:anonymous@10.10.10.98 #Donwload all
		wget -m --no-passive ftp://anonymous:anonymous@10.10.10.98 #Download all

- Metasploit Modules

	https://www.offensive-security.com/metasploit-unleashed/scanner-ftp-auxiliary-modules/
	
		use auxiliary/scanner/ftp/anonymous
		use auxiliary/scanner/ftp/ftp_login
		use auxiliary/scanner/ftp/ftp_version
	
	- Default credentials wordlist
	
```
curl -o FTPCredentialList.txt https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt
```
	
- Nmap scan for vulnerabilities

		nmap --script ftp-vuln-cve2010-4221 -p 21 <host>
		
		nmap --script ftp-brute -p 21 <host>
		
		nmap --script nmap-vulners -sV <host> -p 21

- Anonymous login attack

	Metasploit 
	`use auxiliary/scanner/ftp/anonymous`
	
	Nmap 
	`nmap --script ftp-anon.nse -p 21`
		
- Bruteforce attack

	https://null-byte.wonderhowto.com/how-to/brute-force-ftp-credentials-get-server-access-0208763/
	
	`ncrack -U usernames.txt -P passwords.txt <ip>:21 -v`
	
	`medusa -h <ip> -U usernames.txt -P passwords.txt -M ftp`
	
	`hydra -L usernames.txt -P passwords.txt ftp://<ip> -s 21`
	
	`nmap --script ftp-brute -p 21 <host>`
	
	Metasploit 
	`use auxiliary/scanner/ftp/ftp_login`
		
- Default credentials wordlist

	https://raw.githubusercontent.com/danielmiessler/SecLists/master/Passwords/Default-Credentials/ftp-betterdefaultpasslist.txt
	
- Guide

	https://steflan-security.com/ftp-enumeration-guide/
		
