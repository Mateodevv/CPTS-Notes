
## NGINX
#### Create a Directory to Handle Uploaded Files

  Create a Directory to Handle Uploaded Files

```shell-session
Tachipy@htb[/htb]$ sudo mkdir -p /var/www/uploads/SecretUploadDirectory
```

#### Change the Owner to www-data

  Change the Owner to www-data

```shell-session
Tachipy@htb[/htb]$ sudo chown -R www-data:www-data /var/www/uploads/SecretUploadDirectory
```

#### Create Nginx Configuration File

Create the Nginx configuration file by creating the file `/etc/nginx/sites-available/upload.conf` with the contents:

  Create Nginx Configuration File

```shell-session
server {
    listen 9001;
    
    location /SecretUploadDirectory/ {
        root    /var/www/uploads;
        dav_methods PUT;
    }
}
```

#### Symlink our Site to the sites-enabled Directory

  Symlink our Site to the sites-enabled Directory

```shell-session

Tachipy@htb[/htb]$ sudo ln -s /etc/nginx/sites-available/upload.conf /etc/nginx/sites-enabled/
```

#### Start Nginx

  Start Nginx

```shell-session
Tachipy@htb[/htb]$ sudo systemctl restart nginx.service
```
#### Upload File Using cURL

  Upload File Using cURL

```shell-session
Tachipy@htb[/htb]$ curl -T /etc/passwd http://localhost:9001/SecretUploadDirectory/users.txt
```

  Upload File Using cURL

```shell-session
Tachipy@htb[/htb]$ tail -1 /var/www/uploads/SecretUploadDirectory/users.txt 

user65:x:1000:1000:,,,:/home/user65:/bin/bash
```s