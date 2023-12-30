
#### Enumerating the Password Policy - from Linux - Credentialed


```shell-session
Tachipy@htb[/htb]$ crackmapexec smb 172.16.5.5 -u avazquez -p Password123 --pass-pol
```

#### Using rpcclient

  Using rpcclient

```shell-session
Tachipy@htb[/htb]$ rpcclient -U "" -N 172.16.5.5
```
#### Obtaining the Password Policy using rpcclient

  Obtaining the Password Policy using rpcclient

```shell-session
rpcclient $> querydominfo

Domain:		INLANEFREIGHT
Server:		
Comment:	
Total Users:	3650
Total Groups:	0
Total Aliases:	37
Sequence No:	1
Force Logoff:	-1
Domain Server State:	0x1
Server Role:	ROLE_DOMAIN_PDC
Unknown 3:	0x1
rpcclient $> getdompwinfo
min_password_length: 8
password_properties: 0x00000001
	DOMAIN_PASSWORD_COMPLEX
```
#### Using enum4linux

```shell-session
Tachipy@htb[/htb]$ enum4linux -P 172.16.5.5
```
#### Using enum4linux-ng

```shell-session
Tachipy@htb[/htb]$ enum4linux-ng -P 172.16.5.5 -oA ilfreight
```
#### Enumerating Null Session - from Windows

It is less common to do this type of null session attack from Windows, but we could use the command `net use \\host\ipc$ "" /u:""` to establish a null session from a windows machine and confirm if we can perform more of this type of attack.

#### Establish a null session from windows

  Establish a null session from windows

```cmd-session
C:\htb> net use \\DC01\ipc$ "" /u:""
The command completed successfully.
```

We can also use a username/password combination to attempt to connect. Let's see some common errors when trying to authenticate:

#### Error: Account is Disabled

  Error: Account is Disabled

```cmd-session
C:\htb> net use \\DC01\ipc$ "" /u:guest
System error 1331 has occurred.

This user can't sign in because this account is currently disabled.
```

#### Error: Password is Incorrect

  Error: Password is Incorrect

```cmd-session
C:\htb> net use \\DC01\ipc$ "password" /u:guest
System error 1326 has occurred.

The user name or password is incorrect.
```

#### Error: Account is locked out (Password Policy)

  Error: Account is locked out (Password Policy)

```cmd-session
C:\htb> net use \\DC01\ipc$ "password" /u:guest
System error 1909 has occurred.

The referenced account is currently locked out and may not be logged on to.
```
#### Enumerating the Password Policy - from Linux - LDAP Anonymous Bind
#### Using ldapsearch

```shell-session
Tachipy@htb[/htb]$ ldapsearch -h 172.16.5.5 -x -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "*" | grep -m 1 -B 10 pwdHistoryLength

forceLogoff: -9223372036854775808
lockoutDuration: -18000000000
lockOutObservationWindow: -18000000000
lockoutThreshold: 5
maxPwdAge: -9223372036854775808
minPwdAge: -864000000000
minPwdLength: 8
modifiedCountAtLastProm: 0
nextRid: 1002
pwdProperties: 1
pwdHistoryLength: 24
```
#### Enumerating the Password Policy - from Windows
#### Using net.exe

```cmd-session
C:\htb> net accounts
```
#### Using PowerView

  Using PowerView

```powershell-session
PS C:\htb> import-module .\PowerView.ps1
PS C:\htb> Get-DomainPolicy
```