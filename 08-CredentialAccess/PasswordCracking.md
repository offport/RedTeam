## Cracking Windows Hashes

John


	john –format=LM –wordlist=/root/usr/share/john/password_john.txt hash.txt

example:
	aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d
	
	
	
## Cracking Linux Hashes - /etc/shadow


### Method 1

-  Two files needed
	1. /etc/passwd
	2. /etc/shadow

in Kali

- Combine the two files
	
		sudo unshadow /etc/passwd /etc/shadow > ~/Desktop/hashes.txt
		
- Crack

		sudo hashcat -m 1800 hashes.txt wordlist.txt --force

### Method 2

- Get the hash - *The hash is everything between the first and second colon*


```
	nano /etc/shadow
	root:$6$Tb/euwmK$OXA.dwMeOAcopwBl68boTG5zi65wIHsc84OWAIye5VITLLtVlaXvRDJXET..it8r.jbrlpfZeMdwD3B0fGxJI0:17298:0:99999:7:::
```

	
- John

		john --wordlist=/home/kali/Desktop/rockyou.txt hash.txt  
	
	- Show previously cracked hashes

			john --show hash.txt
			
- Hashcat
	
		hashcat -m 1800 -a 0 hash.txt /home/kali/Desktop/rockyou.txt
	
