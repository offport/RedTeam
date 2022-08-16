# Port Forwarding

There are three types of SSH port forwarding:

Local Port Forwarding. - Forwards a connection from the client host to the SSH server host and then to the destination host port.
Remote Port Forwarding. - Forwards a port from the server host to the client host and then to the destination host port.
Dynamic Port Forwarding. - Creates a SOCKS proxy server that allows communication across a range of ports.

### SSHuttle

  - Project https://github.com/sshuttle/sshuttle
  - Documentation https://sshtunnel.readthedocs.io/

### SSH Commands

It requires to have an SSH server running on the controlled machine and a valid account. 

**Local port forwarding**

*accessing services on the internal network example*

`ssh -N -L <port to forward service to on local machine>:<IP of machine running the service>:<port service is running on> user@<IP of ssh server>`

**Remote port forwarding**

*downloading files from kali machine example*

`ssh -N -R <port to forward service to on local machine>:<IP of machine running the service>:<port service is running on> user@<IP of ssh server>`

**Dynamic port forwarding**

`ssh -D [LOCAL_IP:]LOCAL_PORT [USER@]SSH_SERVER`

### Netcat

Syntax 

`nc -lvk $LOCAL_ADDRESS $LOCAL_PORT -c "nc -v $REMOTE_ADDRESS $REMOTE_PORT"`

Make local host listen on 8080 and forward them to example.org:80 which is listening on 80

`ncat -l localhost 8080 --sh-exec "ncat example.org 80"`


Listen on port 8080 of the server, and when somebody tries to connect, establish a link with 127.0.0.1:22 (i.e.: SSH server).

`netcat -L 127.0.0.1:22 -p 8080 -vvv`

Listen on port 25000 of the server, and when somebody tries to connect, connect him to Google web server.

`netcat -L google.fr:80 -p 25000 -vvv`


### PLink

plink on windows target. ssh server on attacker machine.

`plink.exe -l root -pw mysecretpassword 192.168.0.101 -R 8080:127.0.0.1:8080`

### Metasploit

```
# Add port forward
portfwd add –l $LOCAL_PORT –p $REMOTE_PORT –r $REMOTE_ADDRESS

# List ports forwarded
portfwd list

# Delete port forwarded
portfwd delete –l $LOCAL_PORT –p $REMOTE_PORT –r $REMOTE_ADDRESS

# Remove all port forwarded
portfwd flush
```

### Putty GUI

https://phoenixnap.com/kb/ssh-port-forwarding


## Refernces

- https://www.thehacker.recipes/sys/pivoting/port-forwarding
- https://www.devkb.org/linux/115-TCP-tunnel-port-forwarding-using-Netcat
- https://linuxize.com/post/how-to-setup-ssh-tunneling/#:~:text=Local%20Port%20Forwarding.,to%20the%20destination%20host%20port.
