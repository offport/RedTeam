## Nmap Scan

`sudo nmap -sP -n xxx.xxx.xxx`

Another one

`sudo nmap -sP 172.31.201.0/24 | awk '/Nmap scan report for/{printf $5;}/MAC Address:/{print " => "$3;}' | sort`

  Expected output

```
172.31.201.80 => 00:50:56:AF:56:FB
172.31.201.97 => 00:26:73:78:51:42
server1.company.internal.local => 3C:D9:2B:70:BC:99
```

Find vendor 

  you could use the AA:BB:CC with the colons removed to identify a device from its vendor ID, for example.

```
$ grep -i '709E29' /usr/local/share/nmap/nmap-mac-prefixes 
709E29 Sony Interactive Entertainment
```

Check MAC address of the machine you want to spoof

`arping -c 1 192.168.x.x`

## MAC Spoofing

`macchanger -m XX:XX:XX:XX:XX:XX`


https://redteam.coffee/woot/nac-bypass-cheatsheet
