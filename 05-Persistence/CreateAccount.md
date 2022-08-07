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
- Add account to admin group `net localgroup administrators <username> /add`
- Remove an account `net user <username> /delete`
- Change password of an account `net user <username> <newpassword>`
