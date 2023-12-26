
```cmd
FOR /L %i IN (1,1,254) DO ping -n 1 172.16.10.%i | FIND /i "Reply">>C:\ipaddresses.txt
```
