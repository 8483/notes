# Server

-   Buy server.
-   Find out IP address.
-   Setup SSH public and private key for remote connections.
-   Setup the environment, same as development one.

# Interoperability

If you server connects to a database on another server, instead of using a VPN, ask the admins to add your server's IP address to their firewall.

# Deploy code

Use `rsync` to upload new files, and replace existing ones.

Deploy the front-end static files, as well as the server ones.

```
npm run build --prefix app/ \
&& rsync -av -e 'ssh' ./server ./app/dist --exclude 'server/node_modules' user@123.456.789.255:~/project/ \
&& ssh user@123.456.789.255 pm2 reload /home/user/project/server/server.js --name "project"
```

# Domain

If someone else owns the domain...

-   Tell them to create an A record for the domain/subdomain, which points to your server.

If you own it...

-   Find the nameservers of your server's host.
-   Configure the domain's nameservers i.e. which DNS to use for further configuration. This is done in your registrar's admin panel, or you can tell the admin to manually set them up. They look like this:

```
ns1.host.com
ns2.host.com
ns3.host.com
```

# DNS

-   Add the domain.
-   Create A record for the domain/subdomain, which points to your server.

# SSL

-   Add a letencrypt SSL certificate with certbot.

# Web server -nginx

-   Add the new configuration, which has SSL and a reverse proxy.

```nginx

events {}

http {
    include mime.types;

    server {
        listen 80;
        server_name subdomain.domain.com;
        return 301 https://subdomain.domain.com$request_uri;
    }

    server {
        listen 443 ssl;
        server_name subdomain.domain.com;
        ssl_certificate /etc/letsencrypt/live/subdomain.domain.com/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/subdomain.domain.com/privkey.pem;

        root /home/user/app/public;
        index index.html;

        location /auth {
            proxy_pass 'http://127.0.0.1:3000';
        }

        location /api {
            proxy_pass 'http://127.0.0.1:3000';
        }

        location /socket.io/ {
            proxy_pass 'http://127.0.0.1:3000';
        }
    }
}
```

# Start server script

Use daemons such as `pm2` to run the script continuously. You can also use `nodemon` for running it temporarily.

```
pm2 start server.js --name "project"
```

# Test

Visit the domain from multiple devices and be aware of caching, which might show something else than the real things.

# Email

Use a service like `migadu` or `mxroute` for low volume, and `mailgun` for high volume.
