# Install Certbot

Guide for `nginx` on `ubuntu`. The certificates are issued instantly, no waiting.

```bash
# Install certbot
sudo wget https://dl.eff.org/certbot-auto -O /usr/sbin/certbot-auto
sudo chmod a+x /usr/sbin/certbot-auto

# nginx plugin
sudo apt install python-certbot-nginx
```

# Issue SSL

**Must disable nginx**

```bash
# Stop listening to port 80
systemctl stop nginx

# Get SSL certificate
sudo certbot-auto certonly --standalone -d example.com

# Check certificate - Might require sudo -i
ls /etc/letsencrypt/live/example.com

# Start listening to port 80
systemctl start nginx
```

# Renew SSL

**Must disable nginx**

```bash
# Stop listening to port 80
systemctl stop nginx

# test
sudo certbot renew --cert-name example.com --dry-run

#renew
sudo certbot renew --cert-name example.com

# Start listening to port 80
systemctl start nginx
```

# Nginx configuration

**IMPORTANT!** Other sites without SSL will broken i.e. they will resolve to the SSL one due to the redirect rule. Browsers force SSL so the only solution is to add it to everything.

```nginx
events {}

http {
    include mime.types;

    server {
        listen 80;
        server_name example.com;
        return 301 https://example.com$request_uri; # redirect to 443 SSL
    }

    server {
        listen 443 ssl;
        server_name example.com;
        ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

        location / {
            proxy_pass 'http://localhost:3000/';
        }
    }
}
```

# Automate Renewal

```bash
# Check nginx configuration
sudo nginx -t

# Restart nginx with new configuration
systemctl start nginx

# Schedule task
crontab -e

# At 02:00 auto-renew SSL certificate if required
0 2 * * * sudo /usr/sbin/certbot-auto -q renew --nginx
```

# Guide

The domain already needs to be specified in the nginx conf file.The certbot takes the domain from file only, no need to specify the domain.

1. First install Cetbot with

```
$ sudo apt-get install software-properties-common
$ sudo add-apt-repository ppa:certbot/certbot
$ sudo apt-get update
$ sudo apt-get install certbot
```

2. Install nginx plugin after installing certbot.

```
sudo apt install python-certbot-nginx
```

3. Now navigate to the nginx config file with

```
sudo nano /etc/nginx/nginx.conf
```

4. here go to the included files for sites enabled in http scope exp-

```
/etc/nginx/sites-enabled/
```

5. open default with

```
sudo vi default
```

6. Here change "server_name" to your domain name for 443 port. These are
   individual server blocks
7. If you need to add something on other port then it can be done in this file.
8. And now save the file
9. start the bash as admin with "sudo bash"
10. Now start certbot with nginx plugin. "certbot --nginx"
11. select the appropriate options and domain which will be listed automatically.

This works in Ubuntu 16.04 so I guess it would work on most other as well.

# Windows IIS

Windows ACME Simple (WACS)

https://github.com/win-acme/win-acme

-   Run `wacs.exe` as administrator
-   Follow guide
