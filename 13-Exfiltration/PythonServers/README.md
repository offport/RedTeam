### Create certificate

Create a certificate sp you can use SSL

	openssl req -new -x509 -keyout server.pem -out server.pem -days 365 -nodes


### Start the server

Usage:

	python simple-https-server.py 4433 admin:password
	
	
	
### Upload files to server

**WIndows**

`.\upload.ps1 <https://ip:port/> <admin:password> <file (in current dir)> `
	
`./upload.ps1 https://192.168.46.1:4433/ admin:password file_to_upload`

**Linux and MAC**

`curl -F 'file=@filename.txt' -k https://192.168.46.1:4433/ -u admin:password`