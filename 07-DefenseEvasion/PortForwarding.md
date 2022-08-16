# Port Forwarding

There are three types of SSH port forwarding:

Local Port Forwarding. - Forwards a connection from the client host to the SSH server host and then to the destination host port.
Remote Port Forwarding. - Forwards a port from the server host to the client host and then to the destination host port.
Dynamic Port Forwarding. - Creates a SOCKS proxy server that allows communication across a range of ports.


### SSH Commands

It requires to have an SSH server running on the controlled machine and a valid account. 

**Local port forwarding**

`ssh -N -L <port to forward to on the ssh server>:<IP of machine running the service>:<port service is running on> user@<IP of ssh server>`

**Remote port forwarding**

`ssh -N -R <port to forward to on the ssh server>:<IP of machine running the service>:<port service is running on> user@<IP of ssh server>`

**Dynamic port forwarding**

`ssh -D [LOCAL_IP:]LOCAL_PORT [USER@]SSH_SERVER`

### SSH Config

### Netcat

### PLink

### Metasploit


https://linuxize.com/post/how-to-setup-ssh-tunneling/#:~:text=Local%20Port%20Forwarding.,to%20the%20destination%20host%20port.
