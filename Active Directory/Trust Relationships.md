## Enumerating Trust Relationships

#### Using Get-ADTrust
```powershell-session
PS C:\htb> Import-Module activedirectory
PS C:\htb> Get-ADTrust -Filter *
```
#### Checking for Existing Trusts using Get-DomainTrust
```powershell-session
PS C:\htb> Get-DomainTrust 
```
#### Using Get-DomainTrustMapping
```powershell-session
PS C:\htb> Get-DomainTrustMapping
```
#### Checking Users in the Child Domain using Get-DomainUser
```powershell-session
PS C:\htb> Get-DomainUser -Domain LOGISTICS.INLANEFREIGHT.LOCAL | select SamAccountName
```
#### Using netdom to query domain trust

```cmd-session
C:\htb> netdom query /domain:inlanefreight.local trust
```
#### Using netdom to query domain controllers

```cmd-session
C:\htb> netdom query /domain:inlanefreight.local dc
```
#### Using netdom to query workstations and servers

```cmd-session
C:\htb> netdom query /domain:inlanefreight.local workstation
```