# Notes

The certificates are issued instantly, no waiting.

Subdomains require separate certificates.

```
example.com
app.example.com
admin.example.com
```

All three require different certificates.

You need to issue and renew the individually.

# Install Certbot

**The Certbot packages on your system come with a cron job or systemd timer that will renew your certificates automatically before they expire.** You will not need to run Certbot again, unless you change your configuration.

Guide for `nginx` on `ubuntu`.

```bash
# Install certbot
sudo snap install --classic certbot

# Link global command to installed one
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

# Check certificates

```bash
# Check all
sudo certbot certificates

# Location of certificate - Might require sudo -i
sudo ls /etc/letsencrypt/live/example.com
```

# Issue SSL

**IMPORTANT: Must disable nginx!**

**Issue certificate, before modifying nginx.conf.**

```bash
# Stop listening to port 80
sudo systemctl stop nginx

# Stop all processes
sudo killall nginx

# Get SSL certificate
sudo certbot certonly --nginx -d subdomain.domain.com

# Start listening to port 80
sudo systemctl start nginx
```

# Renew SSL

**IMPORTANT: Must disable nginx**

```bash
# Stop listening to port 80
systemctl stop nginx

# Stop all processes
sudo killall nginx

# Renew bulk
sudo certbot renew

# Renew individual
sudo certbot renew --cert-name example.com

# Start listening to port 80
systemctl start nginx
```

# Errors

```
sudo systemctl stop nginx
sudo killall -9 nginx
sudo systemctl start nginx
```

# Nginx configuration

**IMPORTANT!**

Other sites without SSL will be broken i.e. they will resolve to the SSL one due to the redirect rule. Browsers force SSL so the only solution is to add it to everything.

**A permanent redirect (code 301) gets cached by the browser.** If you had a 301 redirect for `app.example.com` to `example.com` in the past then **the browser will not check again** but use the already cached redirect and visit the target directly.

Check with an incognito window to make sure that existing caches will not be used. **Clear browser cache** to fix this.

```nginx
events {}

http {
    include mime.types;

    server {
        listen 80;
        server_name example.com;
        return 301 https://example.com$request_uri;
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
