#### Using Netsh.exe to Port Forward

```cmd-session
C:\Windows\system32> netsh.exe interface portproxy add v4tov4 listenport=8080 listenaddress=10.129.15.150 connectport=3389 connectaddress=172.16.5.25
```
#### Connecting to the Internal Host through the Proxy
```shell
xfreedrp /v:10.129.42.198:8080 /u:victor /p:pass@123
```
