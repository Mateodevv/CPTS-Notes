#### Installing sshuttle

  Installing sshuttle

```shell-session
Tachipy@htb[/htb]$ sudo apt-get install sshuttle
```
#### Running sshuttle

  Running sshuttle

```shell-session
Tachipy@htb[/htb]$ sudo sshuttle -r ubuntu@10.129.202.64 172.16.5.0/23 -v 
```

#### Traffic Routing through iptables Routes

  Traffic Routing through iptables Routes

```shell-session
Tachipy@htb[/htb]$ nmap -v -sV -p3389 172.16.5.19 -A -Pn
```