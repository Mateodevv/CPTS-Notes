|   |   |
|---|---|
|**Web Enumeration**||
|`gobuster dir -u http://10.10.10.121/ -w /usr/share/dirb/wordlists/common.txt`|Run a directory scan on a website|
|`gobuster dns -d inlanefreight.com -w /usr/share/SecLists/Discovery/DNS/namelist.txt`|Run a sub-domain scan on a website|
|`curl -IL https://www.inlanefreight.com`|Grab website banner|
|`whatweb 10.10.10.121`|List details about the webserver/certificates|
|`curl 10.10.10.121/robots.txt`|List potential directories in `robots.txt`|
|`ctrl+U`|View page source (in Firefox)|
