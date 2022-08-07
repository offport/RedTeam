
## Passive reconnaissance 
_Passive reconnaissance_ is an attempt to gather information about targeted computers and networks without actually communicating with them.

The goal of this phase is to gather information such as:
1. DNS Info
2. IPs (to scan)
3. Technologies used and versions (useful to find vulnerabilities)
4. Emails, email formats, phones numbers (useful for phishing and brute force)
5. Sensitive files available online
6. Vulnerabilities (maybe)

---
### 1. Google https://www.google.com/

A *Google dork* is a search string that uses advanced search operators to find information that is not readily available on a website.  Google dorking/Google_ hacking, can return information that is difficult to locate through simple search queries.

 - Google Hacking Database https://www.exploit-db.com/google-hacking-database
- Basic Operators https://en.wikipedia.org/wiki/Google_hacking
- Publicly exposed documents
  `site:www.example.com ext:doc | ext:docx | ext:odt | ext:rtf | ext:sxw | ext:psw | ext:ppt | ext:pptx | ext:pps | ext:csv`
  
- File Type
  `site:www.example.com filtype:pdf`
  
- Directory listing vulnerabilities
  `site:www.example.com intitle:index.of`
  
- Configuration files exposed
  `site:www.example.com ext:xml | ext:conf | ext:cnf | ext:reg | ext:inf | ext:rdp | ext:cfg | ext:txt | ext:ora | ext:ini | ext:env`

- Database files exposed
  `site:www.example.com ext:sql | ext:dbf | ext:mdb`
  
- Log files exposed
  `site:www.example.com ext:log`
  
- Backup and old files
  `site:www.example.com ext:bkf | ext:bkp | ext:bak | ext:old | ext:backup`
  
- Login pages
  `site:www.example.com inurl:login | inurl:signin | intitle:Login | intitle:"sign in" | inurl:auth`
  
- SQL errors
  `site:www.example.com intext:"sql syntax near" | intext:"syntax error has occurred" | intext:"incorrect syntax near" | intext:"unexpected end of SQL command" | intext:"Warning: mysql_connect()" | intext:"Warning: mysql_query()" | intext:"Warning: pg_connect()"`
  
- PHP errors / warnings
 `site:www.example.com "PHP Parse error" | "PHP Warning" | "PHP Error"`
 
- phpinfo()
 `site:www.example.com ext:php intitle:phpinfo "published by the PHP Group"`
 
- Search Pastebin.com / pasting sites
 `site:pastebin.com | site:paste2.org | site:pastehtml.com | site:slexy.org | site:snipplr.com | site:snipt.net | site:textsnip.com | site:bitpaste.app | site:justpaste.it | site:heypasteit.com | site:hastebin.com | site:dpaste.org | site:dpaste.com | site:codepad.org | site:jsitor.com | site:codepen.io | site:jsfiddle.net | site:dotnetfiddle.net | site:phpfiddle.org | site:ide.geeksforgeeks.org | site:repl.it | site:ideone.com | site:paste.debian.net | site:paste.org | site:paste.org.ru | site:codebeautify.org  | site:codeshare.io | site:trello.com "www.example.com"`
 
- Search Github.com and Gitlab.com
 `site:github.com | site:gitlab.com "www.example.com"`
 
- Search Stackoverflow.com
 `site:stackoverflow.com "www.example.com"`
 
- Signup pages
`site:www.example.com inurl:signup | inurl:register | intitle:Signup`

---

### 2- Shodan https://www.shodan.io/
Shodan is a search engine for Internet-connected devices. Shodan can tell you things like what web server (and version) or how many anonymous FTP servers exist in a particular location, and what make and model the device may be.

Shodan can be used for banner-grabbing.

*regsteration is needed to user filters*

---

### 3- WHOIS-ICANN Lookup https://lookup.icann.org/
Every year, millions of individuals, businesses, organizations and governments register domain names. Each one must provide identifying and contact information which may include: name, address, email, phone number, and administrative and technical contacts. This information is often referred to as "WHOIS data."

WHOIS simply works by typing the domain name in the search bar.

WHOIS is also available as a command-line tool. (Available on Kali-Linux by default)

	`whois www.example.com`

---

### 4- Maltego
*Available on Kali-Linux by default*

*Maltego* is an open source intelligence (OSINT) and graphical link analysis tool for gathering and connecting information for investigative tasks.
Maltego is too large to explain in a few a paragraph. However, a separate review will be done.

---

### 5- Netcraft https://searchdns.netcraft.com/

An engine used for finding subdomains, old IP addresses, owner and technology information, 

---

### 6- Job Posting Ex: LinkedIn https://www.linkedin.com/

By looking at IT job posting, you can determine what technologies a company uses. Fore example: type of servers, networking devices, frameworks, firewalls, programming languages..

---

### 7- Wayback Machine https://archive.org/web/
The wayback machine is an archive of the entire internet. Basically they go to every website and they crawl it while taking screenshots and logging the data to a database. These endpoints can then be queried to pull down every path the site has ever crawled.

The wayback machine has a boat load of information. Before you decide to crawl a website yourself check out the wayback machine. It might save you a lot of time and effort by using other peoples crawled data. Once you get the data start looking for interesting files and GET parameters that might be vulnerable.

---

### 8- DNSDumpster https://dnsdumpster.com/
DNSdumpster.com is a FREE domain research tool that can discover hosts related to a domain. DNSDumpster is one of my favorite tools. You can download csv reports and see a graphical representation of results.

---

### 9- NMMapper https://www.nmmapper.com/sys/tools/subdomainfinder/
Online Subdomain finder using Sublist3r, DNScan, Anubis, Amass, Nmap-dns-brute.nse, Lepus, Findomain, Censys

---

### 10- DNSRecon 
*Available on Kali-Linux by default*
	`dnsrecon –d yourdomain.com`

---

### 11- Immuni Web https://www.immuniweb.com/ssl/
In addition to finding subdomains, this tool supports:
-   Web Server SSL Test
-   Email Server SSL Test
-   SSL Certificate Test
-   PCI DSS, HIPAA & NIST Test

---

### 12- The Harvester
	theHarvester -d megacorpone.com -b google
---

### 13- Password Dumps

	https://github.com/hmaverickadams/breach-parse
	
	https://haveibeenpwned.com/

---

### Finally, OSINT Framework https://osintframework.com/

OSINT framework focused on gathering information from free tools or resources. The intention is to help people find free OSINT resources. Some of the sites included might require registration or offer more data for $$$, but you should be able to get at least a portion of the available information for no cost.

