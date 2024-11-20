# Notes

The certificates are issued instantly, no waiting.

Subdomains require separate certificates.

```
domain.com
app.domain.com
admin.domain.com
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
sudo ls /etc/letsencrypt/live/domain.com
```

# Issue SSL

**IMPORTANT: MUST DISABLE NGINX! CERTBOT RUNS ITS OWN INSTANCE.**

**Issue certificate, before modifying nginx.conf.**

```bash
# Stop all nginx processes
sudo killall nginx

# Get SSL certificate
sudo certbot certonly --nginx -d subdomain.domain.com

# Start listening to port 80
sudo service nginx start
```

**Wildcard**

```bash
sudo certbot certonly --manual --preferred-challenges=dns --server https://acme-v02.api.letsencrypt.org/directory --agree-tos -d *.domain.com
```

Before continuing, it will ask you to:

```txt
Please deploy a DNS TXT record under the name:

_acme-challenge.domain.com.

with the following value:

cz4pZ2VYGypLyjBMrpDR8Af4JrRhhZhQ1Vtjj0tSwv4
```

# Renew SSL

**IMPORTANT: MUST DISABLE NGINX.**

```bash
# Stop all nginx processes
sudo killall nginx

# Renew bulk
sudo certbot renew

# Renew individual
sudo certbot renew --cert-name domain.com

# Start listening to port 80
sudo service nginx start
```

# Errors

```
sudo killall -9 nginx
sudo service nginx start
```

# Nginx configuration

**IMPORTANT!**

Other sites without SSL will be broken i.e. they will resolve to the SSL one due to the redirect rule. Browsers force SSL so the only solution is to add it to everything.

**A permanent redirect (code 301) gets cached by the browser.** If you had a 301 redirect for `app.domain.com` to `domain.com` in the past then **the browser will not check again** but use the already cached redirect and visit the target directly.

Check with an incognito window to make sure that existing caches will not be used. **Clear browser cache** to fix this.

```nginx
events {}

http {
    include mime.types;

    server {
        listen 80;
         # IMPORTANT: Add CNAME record for www > domain.com
        # www.domain.com won't work without this
        server_name domain.com www.domain.com;
        return 301 https://domain.com$request_uri;
    }

    server {
        listen 443 ssl;
        server_name domain.com;

        ssl_certificate /etc/letsencrypt/live/domain.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/domain.com/privkey.pem;

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
sudo service nginx restart

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
