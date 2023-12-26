|   |   |
|---|---|
|**Privilege Escalation**||
|`./linpeas.sh`|Run `linpeas` script to enumerate remote server|
|`sudo -l`|List available `sudo` privileges|
|`sudo -u user /bin/echo Hello World!`|Run a command with `sudo`|
|`sudo su -`|Switch to root user (if we have access to `sudo su`)|
|`sudo su user -`|Switch to a user (if we have access to `sudo su`)|
|`ssh-keygen -f key`|Create a new SSH key|
|`echo "ssh-rsa AAAAB...SNIP...M= user@parrot" >> /root/.ssh/authorized_keys`|Add the generated public key to the user|
|`ssh root@10.10.10.10 -i key`|SSH to the server with the generated private key|
