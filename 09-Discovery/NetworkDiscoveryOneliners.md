## bash

`for ip in $(cat SMB-HOSTS-WithOpenShares1.txt); do echo $ip && ping -c 1 $ip | grep -o "0 received\|1 received" ; done > pinger.txt`


## cmd

`(for /L %a IN (1,1,254) DO ping /n 1 /w 3 192.168.2.%a) | find "Reply" > ping_only_replies.txt`

### or even simpler (less noisy)
`arp -a`

## Powershell

`write-host "Ping Sweep!"; $FirstThreeOctets = Read-Host -Prompt 'First Three Octets (for example: 127.0.0)'; $FirstIP = Read-Host -Prompt 'Start IP (for example: 1)'; $LastIP = Read-Host -Prompt 'End IP (for example: 254)'; $FirstIP..$LastIP | foreach-object { (new-object System.Net.Networkinformation.Ping).Send($FirstThreeOctets + '.' + $_,150) } | where-object {$_.Status -eq 'success'} | select Address; Write-Host 'Done!'`

## Fast NMap Host Discovery

`nmap -sn -T5 --min-parallelism 100 192.168.0.0/mask`
