
## Setting up to Pivot
#### NMap Scan
```shell-session
nmap -sT -p22,3306 10.129.202.64
```

#### Local Port Forward SSH
```shell-session
Tachipy@htb[/htb]$ ssh -L 1234:localhost:3306 ubuntu@10.129.202.64
```
#### Confirming Port Forward with Nmap
```shell-session
Tachipy@htb[/htb]$ nmap -v -sV -p1234 localhost
```
#### Forwarding Multiple Ports
```shell-session
Tachipy@htb[/htb]$ ssh -L 1234:localhost:3306 -L 8080:localhost:80 ubuntu@10.129.202.64
```
#### Enabling Dynamic Port Forwarding with SSH
```shell-session
Tachipy@htb[/htb]$ ssh -D 9050 ubuntu@10.129.202.64
```
#### Checking /etc/proxychains.conf
```shell-session
Tachipy@htb[/htb]$ tail -4 /etc/proxychains.conf
```
#### Using Nmap with Proxychains (Check hosts)
```shell-session
Tachipy@htb[/htb]$ proxychains nmap -v -sn 172.16.5.1-200
```
#### Enumerating the Target through Proxychains and NMap
```shell-session
Tachipy@htb[/htb]$ proxychains nmap -v -Pn -sT 172.16.5.19
```
## Using Metasploit with Proxychains

```shell-session
Tachipy@htb[/htb]$ proxychains msfconsole
```
```shell-session
msf6 > search rdp_scanner

Matching Modules
================

   #  Name                               Disclosure Date  Rank    Check  Description
   -  ----                               ---------------  ----    -----  -----------
   0  auxiliary/scanner/rdp/rdp_scanner                   normal  No     Identify endpoints speaking the Remote Desktop Protocol (RDP)


Interact with a module by name or index. For example info 0, use 0 or use auxiliary/scanner/rdp/rdp_scanner

msf6 > use 0
msf6 auxiliary(scanner/rdp/rdp_scanner) > set rhosts 172.16.5.19
```
### Using xfreerdp with Proxychains
```shell-session
Tachipy@htb[/htb]$ proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```