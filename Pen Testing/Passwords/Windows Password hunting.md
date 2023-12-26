
#### Running Lazagne All

  Running Lazagne All

```cmd-session
C:\Users\bob\Desktop> start lazagne.exe all
```
#### Using findstr

We can also use [findstr](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/findstr) to search from patterns across many types of files. Keeping in mind common key terms, we can use variations of this command to discover credentials on a Windows target:

  Using findstr

```cmd-session
C:\> findstr /SIM /C:"password" *.txt *.ini *.cfg *.config *.xml *.git *.ps1 *.yml
```
