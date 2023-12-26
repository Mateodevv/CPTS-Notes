Ports: 111, 

#### Show Available NFS Shares

  Show Available NFS Shares

```shell-session
Tachipy@htb[/htb]$ showmount -e 10.129.14.128

Export list for 10.129.14.128:
/mnt/nfs 10.129.14.0/24
```

#### Mounting NFS Share

  Mounting NFS Share

```shell-session
Tachipy@htb[/htb]$ mkdir target-NFS
Tachipy@htb[/htb]$ sudo mount -t nfs 10.129.163.211:/ ./target-NFS/ -o nolock
Tachipy@htb[/htb]$ cd target-NFS
Tachipy@htb[/htb]$ tree .
```

#### List Contents with Usernames & Group Names

  List Contents with Usernames & Group Names

```shell-session
Tachipy@htb[/htb]$ ls -l mnt/nfs/

total 16
-rw-r--r-- 1 cry0l1t3 cry0l1t3 1872 Sep 25 00:55 cry0l1t3.priv
-rw-r--r-- 1 cry0l1t3 cry0l1t3  348 Sep 25 00:55 cry0l1t3.pub
-rw-r--r-- 1 root     root     1872 Sep 19 17:27 id_rsa
-rw-r--r-- 1 root     root      348 Sep 19 17:28 id_rsa.pub
-rw-r--r-- 1 root     root        0 Sep 19 17:22 nfs.share
```

#### List Contents with UIDs & GUIDs

  List Contents with UIDs & GUIDs

```shell-session
Tachipy@htb[/htb]$ ls -n mnt/nfs/

total 16
-rw-r--r-- 1 1000 1000 1872 Sep 25 00:55 cry0l1t3.priv
-rw-r--r-- 1 1000 1000  348 Sep 25 00:55 cry0l1t3.pub
-rw-r--r-- 1    0 1000 1221 Sep 19 18:21 backup.sh
-rw-r--r-- 1    0    0 1872 Sep 19 17:27 id_rsa
-rw-r--r-- 1    0    0  348 Sep 19 17:28 id_rsa.pub
-rw-r--r-- 1    0    0    0 Sep 19 17:22 nfs.share
```
#### Unmounting

  Unmounting

```shell-session
Tachipy@htb[/htb]$ cd ..
Tachipy@htb[/htb]$ sudo umount ./target-NFS
```
