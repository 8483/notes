It's a web server configuration. Nothing changes in the app code.

# Install

Guide for `nginx` on `ubuntu`. The certificates are issued instantly, no waiting.

``` bash
# Install certbot
sudo wget https://dl.eff.org/certbot-auto -O /usr/sbin/certbot-auto
sudo chmod a+x /usr/sbin/certbot-auto

# Stop listening to port 80
systemctl stop nginx

# Get SSL certificate
sudo certbot-auto certonly --standalone -d example.com

# Check certificate - Might require sudo -i
ls /etc/letsencrypt/live/example.com

# Start listening to port 80
systemctl start nginx
```

# Nginx configuration
```nginx
events {}

http {
    include mime.types;

    server {
        listen 80 default_server;
        server_name example.com;
        return 301 https://example.com$request_uri; # redirect to 443 SSL
    }

    server {
        listen 443 ssl default_server;
        server_name example.com;
        ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;

        location / {
            proxy_pass 'http://localhost:3000/';
        }
    }
}
```
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
