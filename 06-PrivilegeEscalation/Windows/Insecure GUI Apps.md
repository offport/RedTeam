# Insecure GUI Apps

If a GUI app that allows you to open file like "Internet Explorer" or "Paint" is running as a privileged user, you can exploit this to start cmd as SYSTEM

List the admin users

`net localgroup Administrators`

Look for application running as admin user

`tasklist /V | findstr <admin-username>`

Open the application in GUI and try to open cmd.exe from the GUI.

Example: Paint -> open -> C:\Windows\System32\cmd.exe