### Hunting for SSH Keys

  Hunting for SSH Keys

```shell-session
cry0l1t3@unixclient:~$ grep -rnw "PRIVATE KEY" /* 2>/dev/null | grep ":1"
```

### Cracking encrypted ssh keys

#### Transforming into right format
```shell-session
Tachipy@htb[/htb]$ ssh2john.py SSH.private > ssh.hash
Tachipy@htb[/htb]$ cat ssh.hash 
```
#### Cracking SSH Keys
```shell-session
Tachipy@htb[/htb]$ john --wordlist=rockyou.txt ssh.hash
```



### Cracking documents
#### Cracking Microsoft Office Documents

```shell-session
Tachipy@htb[/htb]$ office2john.py Protected.docx > protected-docx.hash
Tachipy@htb[/htb]$ cat protected-docx.hash
```
```shell-session
Tachipy@htb[/htb]$ john --wordlist=rockyou.txt protected-docx.hash
```
```shell-session
Tachipy@htb[/htb]$ john protected-docx.hash --show
```
#### Cracking PDFs

  Cracking PDFs

```shell-session
Tachipy@htb[/htb]$ pdf2john.py PDF.pdf > pdf.hash
Tachipy@htb[/htb]$ cat pdf.hash 

PDF.pdf:$pdf$2*3*128*-1028*1*16*7e88...SNIP...bd2*32*a72092...SNIP...0000*32*c48f001fdc79a030d718df5dbbdaad81d1f6fedec4a7b5cd980d64139edfcb7e
```

  Cracking PDFs

```shell-session
Tachipy@htb[/htb]$ john --wordlist=rockyou.txt pdf.hash
```
## Protected Archives
#### Download All File Extensions



```shell-session
Tachipy@htb[/htb]$ curl -s https://fileinfo.com/filetypes/compressed | html2text | awk '{print tolower($1)}' | grep "\." | tee -a compressed_ext.txt
```
### Cracking ZIP

#### Using zip2john

```shell-session
Tachipy@htb[/htb]$ zip2john ZIP.zip > zip.hash
```
#### Viewing the Contents of zip.hash

  Viewing the Contents of zip.hash

```shell-session
Tachipy@htb[/htb]$ cat zip.hash 
```
#### Cracking the Hash with John

```shell-session
Tachipy@htb[/htb]$ john --wordlist=rockyou.txt zip.hash
```
#### Viewing the Cracked Hash

  Viewing the Cracked Hash

```shell-session
Tachipy@htb[/htb]$ john zip.hash --show
```
### Cracking OpenSSL Encrypted Archives

#### Using file



```shell-session
Tachipy@htb[/htb]$ file GZIP.gzip 
GZIP.gzip: openssl enc'd data with salted password
```
#### Using a for-loop to Display Extracted Contents

```shell-session
Tachipy@htb[/htb]$ for i in $(cat rockyou.txt);do openssl enc -aes-256-cbc -d -in GZIP.gzip -k $i 2>/dev/null| tar xz;done
```
## Cracking BitLocker Encrypted Drives

#### Using bitlocker2john


```shell-session
Tachipy@htb[/htb]$ bitlocker2john -i Backup.vhd > backup.hashes
Tachipy@htb[/htb]$ grep "bitlocker\$0" backup.hashes > backup.hash
Tachipy@htb[/htb]$ cat backup.hash
```
#### Using hashcat to Crack backup.hash

```shell-session
Tachipy@htb[/htb]$ hashcat -m 22100 backup.hash /opt/useful/seclists/Passwords/Leaked-Databases/rockyou.txt -o backup.cracked
```
#### Viewing the Cracked Hash

```shell-session
Tachipy@htb[/htb]$ cat backup.cracked 
```