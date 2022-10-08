# Install SSL Certificate NGINX Server 


## Step 1: Combine All Certificates into a Single File

You should have received your SSL certificate via email in the form of a .zip file. Once you download and extract the file, you will see it consists of a server certificate, a root certificate, and an intermediate certificate.

The first step is to combine all three files into one.


You can do this manually, by copying and pasting the content of each file in a text editor and saving the new file under the name ```ssl-bundle.crt.```


a) If all three certificates are listed separately, use the command:
```
cat your_domain.crt intermediate.crt root.crt >> ssl-bundle.crt
```
b) If the intermediate certificates are in one bundle, run:
```
cat your_domain.crt your_domain.ca-bundle >> ssl-bundle.crt
```


## Step 2: Edit NGINX Configuration File

Next, configure the NGINX server block (AKA virtual host file) for your server.

If you don’t know the location of the file, run the command:

sudo find nginx.conf

Open the file to make the necessary modifications.

```
Start by specifying the server should listen to port 443: listen 443;
Make sure the server block includes the line: ssl on;
Define the path of the SSL certificate: ssl_certificate /etc/ssl/ssl-bundle.crt;
Specify the directory where the SSL Certificate Key is located: /path/to/your_private.key;
```


```
server {
listen 443;
ssl on;
ssl_certificate /etc/ssl/ssl-bundle.crt;
ssl_certificate_key /path/to/your_private.key;
root /path/to/webroot;
server_name your_domain.com;
}
access_log /var/log/nginx/nginx.vhost.access.log;
error_log /var/log/nginx/nginx.vhost.error.log;
location / {
root /var/www/;
root  /home/www/public_html/your.domain.com/public/;
index index.html;
}
}
```

## Step 4: Verify SSL Certificate
The best way to check you have successfully installed the SSL certificate on NGINX is to connect to your server via browser.

Open a browser of your choice and navigate to your domain using the https protocol:

```
https://your.domain.com
```
