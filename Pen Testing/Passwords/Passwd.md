/etc/passwd
#### Passwd Format

|`cry0l1t3`|`:`|`x`|`:`|`1000`|`:`|`1000`|`:`|`cry0l1t3,,,`|`:`|`/home/cry0l1t3`|`:`|`/bin/bash`|
|---|---|---|---|---|---|---|---|---|---|---|---|---|
|Login name||Password info||UID||GUID||Full name/comments||Home directory||Shell|


Edit the 2 nd filede to "" to get no passwd

## Opasswd

The PAM library (`pam_unix.so`) can prevent reusing old passwords. The file where old passwords are stored is the `/etc/security/opasswd`. Administrator/root permissions are also required to read the file if the permissions for this file have not been changed manually.


#### Unshadow

  Unshadow

```shell-session
Tachipy@htb[/htb]$ sudo cp /etc/passwd /tmp/passwd.bak 
Tachipy@htb[/htb]$ sudo cp /etc/shadow /tmp/shadow.bak 
Tachipy@htb[/htb]$ unshadow /tmp/passwd.bak /tmp/shadow.bak > /tmp/unshadowed.hashes
```

#### Hashcat - Cracking Unshadowed Hashes

  Hashcat - Cracking Unshadowed Hashes

```shell-session
Tachipy@htb[/htb]$ hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```

#### Hashcat - Cracking MD5 Hashes

  Hashcat - Cracking MD5 Hashes

```shell-session
Tachipy@htb[/htb]$ cat md5-hashes.list

qNDkF0zJ3v8ylCOrKB0kt0
E9uMSmiQeRh4pAAgzuvkq1
```

  Hashcat - Cracking MD5 Hashes

```shell-session
Tachipy@htb[/htb]$ hashcat -m 500 -a 0 md5-hashes.list rockyou.txt
```