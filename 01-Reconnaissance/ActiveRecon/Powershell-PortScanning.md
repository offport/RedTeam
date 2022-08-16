- https://www.sans.org/blog/pen-test-poster-white-board-powershell-built-in-port-scanner/


- Scan one host

`1..1024 | % {echo ((new-object Net.Sockets.TcpClient).Connect("10.0.0.100",$_)) "Port $_ is open!"} 2>$null`

- Scan a range

`foreach ($ip in 1..20) {Test-NetConnection -Port 80 -InformationLevel "Detailed" 192.168.1.$ip}`