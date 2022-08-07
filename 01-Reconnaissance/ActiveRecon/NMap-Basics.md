## Nmap - The Basics

### Quick scans
- Nmap comprehesive, aggressive, all ports, all scripts

	`nmap -sC -sV -p- -A -oA <output-filename> <IP>`

- Nmap fast scan
	`nmap -p- --min-rate 10000 -oA <output-filename> <IP>`
	
	
### More complete scans


### CTF and OSCP Style

Save IP of the targets using `alias`

```
export ip='192.168.231.11'
export myip='192.168.49.231'
```

Save the following as a bash script and run it

```bash
nmap -Pn -p- --min-rate 10000 -oN FastScanAllPorts $ip > /dev/null &
nmap -Pn -sC -sV -p- -oN FullScanAllPorts $ip > /dev/null &
nmap -Pn -F -oN Top100Ports $ip > /dev/null &
nmap -Pn -sC -sV -p- --top-ports 1000 -oN FullScanTop1000Ports $ip > /dev/null &
```

