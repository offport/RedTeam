## Banner Grabbing

Telnet

    telnet  <IP> <PORT>
    
wget

    # The -q will suppress the normal output, and the -S parameter will print the headers sent by the HTTP server, which also works for FTP servers.
    
    wget  <IP> -q -S
    
cURL

    curl -s -I  <IP> | grep -e "Server: "
    
NMAP

    nmap -sV -p <PORT> --script=banner <IP>
    
Netcat

    nc -v  <IP> <PORT>
