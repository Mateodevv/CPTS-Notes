#### Using enum4linux

```shell-session
Tachipy@htb[/htb]$ enum4linux -U 172.16.5.5  | grep "user:" | cut -f2 -d"[" | cut -f1 -d"]"
```
#### Using rpcclient
```shell-session
Tachipy@htb[/htb]$ rpcclient -U "" -N 172.16.5.5

rpcclient $> enumdomusers 
user:[administrator] rid:[0x1f4]
user:[guest] rid:[0x1f5]
user:[krbtgt] rid:[0x1f6]
user:[lab_adm] rid:[0x3e9]
user:[htb-student] rid:[0x457]
user:[avazquez] rid:[0x458]
```
#### Using CrackMapExec --users Flag
```shell-session
Tachipy@htb[/htb]$ crackmapexec smb 172.16.5.5 --users
```
#### Using ldapsearch
```shell-session
Tachipy@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(&(objectclass=user))"  | grep sAMAccountName: | cut -f2 -d" "
```
#### Using windapsearch
```shell-session
Tachipy@htb[/htb]$ ./windapsearch.py --dc-ip 172.16.5.5 -u "" -U
```
#### Kerbrute User Enumeration
```shell-session
Tachipy@htb[/htb]$  kerbrute userenum -d inlanefreight.local --dc 172.16.5.5 /opt/jsmith.txt 
```