## DNS - Cheatsheet

**DNS enumeration** is the process of locating all the **DNS** servers and their corresponding records for an organization. 

A company may have both internal and external **DNS** servers that can yield information such as usernames, computer names, and IP addresses of potential target systems.

**- DNSEnum**  

		dnsenum zonetransfer.me
	
**- DNSRecon**  

- -t to specify the type of enumeration to perform (in this case a zone transfer).
- Result is a full dump of the Zone file of the domain.

		dnsrecon -d megacorpone.com -t axfr
		
- -D to specify a file name containing potential subdomain strings, and -t to specify the type of enumeration to perform (in this case brt for brute force):

		dnsrecon -d megacorpone.com -D ~/list.txt -t brt

- Common subdomain wordlists https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS

**- Sublis3r**

- Tool for finding subdomains https://github.com/aboul3la/Sublist3r