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

**To check if user is part of Administrators group**

The easiest way of determine whether we have local admin access (and therefore we’re part of the local Administrators group) to a remote machine is to attempt to list the content of the C: drive using the following command:

For a single machine

`dir \\<ip or hostname>\C$`

`powershell.exe Get-WMIObject -Class win32_operatingsystem -Computername <ip or hostname>`

For multiple machines

PowerView’s cmdlet Find-LocalAdminAccess that will simply output the machines where the current user have admin privileges.

**To check if user is part of Remote Management Users**

`Invoke-Command -Computername <ip or hostname> -ScriptBlock {whoami}`

**To check if user is part of Remote Desktop Users**

Attempt to RDP. Simple!



**Windows Lateral Movement** https://riccardoancarani.github.io/2019-10-04-lateral-movement-megaprimer/
