
Check for Subdomains tru Certifactes
```bash
curl -s https://crt.sh/\?q\=inlanefreight.com\&output\=json | jq . | grep name | cut -d":" -f2 | grep -v "CN=" | cut -d'"' -f2 | awk '{gsub(/\\n/,"\n");}1;' | sort -u
```


Check DNS Records
```bash
dig any inlanefreight.com
```

Find AWS Buckets in Google

```google
intext: {company} inurl:amazonaws.com
```
Find Azure:
```google
intext: {company} inurl:blob.core.windows.net
```
