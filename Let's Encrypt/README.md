## Secure Nginx with Let's Encrypt on Ubuntu 20.04

### Step 1 — Installing Certbot
The first step to using Let’s Encrypt to obtain an SSL certificate is to install the Certbot software on your server.

Install Certbot and it’s Nginx plugin with apt:

```
sudo apt install certbot python3-certbot-nginx
```
### Step 2 — Confirming Nginx’s Configuratio

To check, open the configuration file for your domain using nano or your favorite text editor:

```
sudo vi /etc/nginx/sites-available/default
```
Find the existing server_name line. It should look like this:

/etc/nginx/sites-available/default
```
server_name example.com www.example.com;
```

If it doesn’t, update it to match. Then save the file, quit your editor, and verify the syntax of your configuration edits:

```
sudo nginx -t
```
If you get an error, reopen the server block file and check for any typos or missing characters. Once your configuration file’s syntax is correct, reload Nginx to load the new configuration:
```
sudo systemctl reload nginx
```
### Step 3 — Obtaining an SSL Certificate

Certbot provides a variety of ways to obtain SSL certificates through plugins. The Nginx plugin will take care of reconfiguring Nginx and reloading the config whenever necessary. To use this plugin, type the following:

```
sudo certbot --nginx -d example.com -d www.example.com
```

### Step 4 — Verifying Certbot Auto-Renewal

Let’s Encrypt’s certificates are only valid for ninety days. This is to encourage users to automate their certificate renewal process. The certbot package we installed takes care of this for us by adding a systemd timer that will run twice a day and automatically renew any certificate that’s within thirty days of expiration.

You can query the status of the timer with systemctl:

```
sudo systemctl status certbot.timer
```
```
Output
● certbot.timer - Run certbot twice daily
     Loaded: loaded (/lib/systemd/system/certbot.timer; enabled; vendor preset: enabled)
     Active: active (waiting) since Mon 2020-05-04 20:04:36 UTC; 2 weeks 1 days ago
    Trigger: Thu 2020-05-21 05:22:32 UTC; 9h left
   Triggers: ● certbot.service
```
To test the renewal process, you can do a dry run with certbot:

```
sudo certbot renew --dry-run
```



### Configure NginX for Let's Encrypt SSL


```
server {

    server_name itsyndicate.org;

    listen 443 ssl;
    ssl on;

    ssl_certificate     /etc/letsencrypt/live/ssl.itsyndicate.org/fullchain.pem;

    ssl_certificate_key /etc/letsencrypt/live/ssl.itsyndicate.org/privkey.pem;

    root /var/www/html/;

    index index.php index.html index.htm;

    location ~ /.well-known {

        root /var/www/letsencrypt;

        allow all;

    }

}
```
