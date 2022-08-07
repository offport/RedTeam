## SMTP - Cheatsheet

The Simple Mail Transport Protocol (SMTP) supports several interesting commands, such as VRFY and EXPN. 

A VRFY request asks the server to verify an email address, while EXPN asks the server for the membership of a mailing list. These can often be abused to verify existing users on a mail server, which is useful information during a penetration test. Consider this example:

```
nc -nv 192.168.56.101 25   

(UNKNOWN) [192.168.56.101] 25 (smtp) open
220 metasploitable.localdomain ESMTP Postfix (Ubuntu)
VRFY root
252 2.0.0 root
VRFY msfadmin
252 2.0.0 msfadmin
VRFY FRED
550 5.1.1 <FRED>: Recipient address rejected: User unknown in local recipient table

```

Same above command in python:

```
#!/usr/bin/python

import socket 
import sys

if len(sys.argv) != 2:  
	print "Usage: vrfy.py <username>" 
	sys.exit(0)

# Create a Socket  
s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Connect to the Server  
connect = s.connect(('10.11.1.217',25))

# Receive the banner 

banner = s.recv(1024)

print banner

# VRFY a user  
s.send('VRFY ' + sys.argv[1] + '\r\n') 

result = s.recv(1024)

print result

# Close the socket 

s.close()
```

---

#### Metasploit Modules
Use a wordlist to enumerate users that are present on the remote system.
	
	use auxiliary/scanner/smtp/smtp_enum

Discover version

	use auxiliary/scanner/smtp/smtp_version

---

#### DMARC
https://seanthegeek.net/459/demystifying-dmarc/

---

#### References:
- Metasploit Modules https://www.offensive-security.com/metasploit-unleashed/scanner-smtp-auxiliary-modules/
