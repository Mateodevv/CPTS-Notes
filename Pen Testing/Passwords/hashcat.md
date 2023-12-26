#### Cracking NT Hashes with hashcat
```shell-session
sudo hashcat -m 1000 hashestocrack.txt /usr/share/wordlists/rockyou.txt
```

#### Hashcat - Cracking Unshadowed Hashes

  Hashcat - Cracking Unshadowed Hashes

```shell-session
Tachipy@htb[/htb]$ hashcat -m 1800 -a 0 /tmp/unshadowed.hashes rockyou.txt -o /tmp/unshadowed.cracked
```

#### Hashcat - Cracking MD5 Hashes

```shell-session
Tachipy@htb[/htb]$ hashcat -m 500 -a 0 md5-hashes.list rockyou.txt
```