## Password Nginx site with Base Authentication

### Install apache2-utils

```
sudo apt install apache2-utils
```


### Set password for specific user 

```
htpasswd -c /etc/nginx/.htpasswd <username>
```

We can verify the password by ``` cat /etc/nginx/.htpasswd```

### Edit the configuration part of nginx

```
sudo vi /etc/nginx/sites-enabled/default
```
```
location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
                auth_basic "admin";
                auth_basic_user_file /etc/nginx/.htpasswd;

        }

```

### Restart the nginx 

```
sudo systemctl restart nginx.service 
```
