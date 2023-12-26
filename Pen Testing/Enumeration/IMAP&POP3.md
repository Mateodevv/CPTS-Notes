IMAP Port: 143, 110, 993, 995


|**Command**|**Description**|
|---|---|
|`curl -k 'imaps://<FQDN/IP>' --user <user>:<password>`|Log in to the IMAPS service using cURL.|
|`openssl s_client -connect <FQDN/IP>:imaps`|Connect to the IMAPS service.|
|`openssl s_client -connect <FQDN/IP>:pop3s`|Connect to the POP3s service.|


IMAP COMMANDS:

|**Command**|**Description**|
|---|---|
|` A LOGIN user password` |Log in to the IMAPS service|
|`A SELECT MAILBOX`| Select Mailbox |
|`openssl s_client -connect <FQDN/IP>:pop3s`|Connect to the POP3s service.|
