# Nginx

It only knows about static content. While it can serve static HTML, for dynamically generated HTML, it has to offload requests to other processes in order to generate HTML. Ex. User requests a PHP file, and nginx sends it to an interpreter, which then returns the HTML, which is sent back by nginx.

Nginx communicates with the PHP interpreter via a **unix socket** file. Nginx communicates with the website visitor via a **TCP socket** file.

To access a guest (VM) nginx from a host, set a port forwarding in Virtualbox with name `nginx`, host port `80`, guest port `80`.

# Install

```bash
apt-get update # Refresh packages list.
apt-get install nginx # Install the web server.
```

If we want to add modules, we need to compile nginx from source and add them during the compilation phase.

# Configuration

`/usr/share/nginx/html/` has the default `index.html` file.

`/etc/nginx/nginx.conf` is the main server configuration file. You can configure many sites in this file

You can also create a separate configuration for each, which should be placed in the `/etc/nginx/conf.d/` folder. These override any `nginx.conf` settings. These are loaded by the default `include /etc/nginx/conf.d/*.conf;` line in the main .conf file.

# Commands

```bash
# Check if the configuration works
nginx -t

# Reload changes made to nginx.conf
systemctl reload nginx

# Check if nginx is running.
systemctl status nginx

# Start nginx.
systemctl start nginx

# Stop nginx.
systemctl stop nginx

# Reload nginx.
systemctl restart nginx

# Start a server with this custom configuration.
sudo nginx -c /home/user/elm-seed/nginx.conf

# Reload custom configuration after changes.
sudo nginx -s reload

# Show nginx processes.
ps -ef | grep nginx

# Kill a server with a PID 2847.
kill -9 2847
```

The default location nginx looks in for the configuration file is `/etc/nginx/nginx.conf`, but you can pass in an arbitrary path with the `-c` flag. Ex. `nginx -c /usr/local/nginx/conf`.

```Bash
nginx -c <path> -t   # Test configuration at absolute path
nginx -c <path>      # Start with a custom configuration.
nginx -s <signal>    # Stop or reload.
```

# Terminology

`Blocks` - Sections to which directives are applied. Also called context or scope. They can be nested. The most important ones are `main`, `http`, `server` and `location` (for matching URI locations on incoming requests to parent server context)

`Vhost` - Virtual host. Defined in the `http` block.

`Directives` - Specific configuration options composed of an option name and option value. Ex. `Listen 80`. There are 4 types:

-   **Standard** directive. Defined once, unless turned off elsewhere. `gzip on`.
-   **Array** directive. Can be defined multiple times with different flags. `access_log logs/access.log` vs `access_log logs/access_notice.log notice;` in the same context.
-   **Action** directive. Perform an action when hit `rewrite`, `return`.
-   **try_files** directive. `try_files $uri index.html =404` tries finding a file and if not found, server index.html. If that also fails, send a 404 error.

```nginx
server {               # Block start
    listen 80;         # Directive
}                      # Block end
```

## mime.types

If this file is not included in the `http` block, all the different files will be served as simple text files and thus won't be rendered. Intead of writing all cases like `http { text/html html; text/css css;}` we can simply include a list of them with `http { include mime.types; }`.

# Simplest server

```nginx
events {}                                 # Needed to be a valid conf file.

http {
    include mime.types;                   # Recognize filetypes.

    server {
        listen 80;
        server_name 123.456.789.255;      # Should be domain name.

        root /home/folder;                # Match URIs to files here.
        index index.html;                 # index is inside folder
    }
}
```

After that, reload with `systemctl reload nginx`.

```nginx
events {}                                         # Needed to be a valid conf file.

http {
    include mime.types;                           # Recognize filetypes.

    server {
        listen 80;
        server_name 123.456.789.255;              # Should be domain name.

        location /route {
            proxy_pass 'http://127.0.0.1:8080';   # http://123.456.789.255:8080
        }
}
```

App is available at `http://123.456.789.255:8080`.

# Real life server

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

# Location Blocks

The most used context block in any nginx configuration, which defines the behavior of specific URIs or requests. Think of them as intercepting a request based on its value and doing something else than serving to a client.

```nginx
events {}

http {
    include mime.types;
    server {
        listen 80;
        server_name 127.0.0.1;
        root /sites/template;

        location /example {
            return 200 "Hello from location block!";
        }
    }
}
```

### Serving Files

```nginx
events {}

http {
    include mime.types;
    server {
        listen 80;
        # The files are available at domain.com/files/file.ext
        # The location is appended to the specified root path for the full location.
        location /files/ {
            # The files need to be inside /home/username/files/
            root /home/username;
            try_files $uri $uri/ =404;
        }
    }
}
```

### Matching URIs to Location Blocks

In order of importance. Matching occurs for everything past the value. `location /greet` will match `/greetings/something`

1. `=` - Exact match.
2. `^~` - Preferential prefix. Same as standard (prefix), but more important than regex.
3. `~` or `~*` - Regex case sensitive or insensitive.
4. `no modifier` - Prefix standard match.

Example for `/greet`:

1. `= /greet` - Match **only** /greet.
2. `^~` - Override the standard match if it also happens.
3. `~* /greet[0-9]` - Match /greet123, /greet456 etc.
4. `no modifier` - Match /greet, /greeting etc.

### try_files

The example below sets up a location outside of the regular server root. When someone visits `domain/downloads/file` they would get the wanted file.

```nginx
events {}

http {
    include mime.types;
    server {
        listen 80;
        server_name 127.0.0.1;
        root /sites/template;

        location /downloads {
            root /sites;
            try_files #uri =404;
        }
    }
}
```

# SSL/HTTPS

After purchasing an SSL certificate, two files are obtained.

-   `.crt` certificate file.
-   `.key` certificate key file.

We place these files into `/etc/nginx/ssl/` and use this configuration in the server/domain block.

```nginx
server {
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/nginx.crt;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;
}
```

SSL/HTTPS uses the `443` port vs the standard HTTP `80`.

Certbot is used for generating **Let's Encrypt** certificates and automating their renewal.

# Proxy vs Reverse Proxy

**Proxy: Hide the origin of the request, by inserting servers in-between.**

```
Normal:   Client -----------------------> Server
Proxy:    Client ---> Server (proxy) ---> Server
```

**Reverse proxy: Prevent direct access to app i.e. exploit bad code.**

```
Normal:          Client -----------------------> App
Reverse Proxy:   Client ---> Server (proxy) ---> App
```

To run any Linux server on port 80, you need to be running as root. If you want to run node directly on port 80, that means you have to run node as root. If your application has any security flaws, it will become significantly compounded by the fact that it has access to do _anything it wants_. For example, if you write something that accidentally allows the user to read arbitrary files, now they can read files from other server user's accounts.

If you use nginx in front of node, you can run your node as a limited user on port `3000` (or whatever) and then have nginx run as root on port `80`, forwarding requests to port `3000`. Now the node application is limited to the user permissions you give it.

It's always better to use nginx as a proxy for a node.js server; nginx can proxy to a number of node backends and should any of them die can fail-over automatically with the benefit that, if there is an issue with the node interpreter (for instance while upgrading) and it stops responding, it can serve a fall-back HTML page.

> **You shouldn't be using node.js for serving "static" files such as images, js/css files, etc.**

Use node.js for the complex stuff and let nginx take care of the things it's good at - serving files from the disk or from a cache.

# Reverse Proxy

**IMPORTANT:** **Any changes made to** `nginx.conf` **need to be reloaded with** `sudo nginx -s reload` **for them to work!!!**

Any request made to the server on the `80` port is forwarded to `http://localhost:3000`, as if it was made directly there.

```nginx
events {}

http {
    server {
        listen 80;

        location / {
            proxy_pass 'http://localhost:3000/';
        }
    }
}
```

## **Trailing slash /**

[Stack Overflow explanation.](https://stackoverflow.com/questions/32542282/how-do-i-rewrite-urls-in-a-proxy-response-in-nginx)

It is crucial to note the usage of the **trailing slash**. This is called a **proxy path**. Unless we specify one, nginx assumes the original request path i.e. location.

**It is recommended to use the proxy path `/` trailing slash.**

### **WITH slash**

**With** trailing slash, the request for `http://localhost:3000/foo/bar/baz` will be proxied to `http://localhost:3000/bar/baz`. Basically, `/foo/` gets replaced by `/` to change the request path from `/foo/bar/baz` to `/bar/baz`.

```nginx
# http://localhost:3000/foo/bar/baz

location /foo/ {
    proxy_pass http://localhost:3000/;
}

# http://localhost:3000/bar/baz
```

For example, a request to `http://localhost:8000/app/users` (with the config below) would return all users because the request made to the server is actually `http://localhost:3000/api/users`.

```nginx
# http://localhost:8000/app/users
server {
    listen 8000;

    location /app/ {
        proxy_pass 'http://localhost:3000/api/users';
    }
}

# http://localhost:3000/api/users
```

### **WITHOUT slash**

**Without** trailing slash, the same request will be proxied to `http://localhost:3000/foo/bar/baz`. Basically, the full original request path gets passed on without changes.

```nginx
# http://localhost:3000/foo/bar/baz

location /foo {
    proxy_pass http://localhost:3000;
}

# http://localhost:3000/foo/bar/baz
```

# Logging

There are two types of logs by default, located in `/var/log/nginx`.

1. **Error** logs for anything that went wrong.
2. **Access** logs for all requests made.

If we want specific logs for specific context, we can do that by using `error_log /var/log/nginx/log_name.log debug;`.

Also, we can disable logs for certain locations by using `access_log off` or `error_log off`. This is useful for saving resources, ex. for all .css requests.

## Inheritance

Http > Server > Location

It only works for standard and array directives. It doesn't work for action and try_files directives as the stop the flow and are immediately executed.

## Testing

`curl -I http://127.0.0.1/css/bootstrap.css` - Request the response headers of a resource.

# Optimization

Further reading on these topics:

-   Expires headers. Client side caching.
-   Gzip. Compression.
-   FastCGI cache. Cache backend responses.
-   Limiting. Prevent DDoS.
-   GeoIP. Location of requests and limits.
-   HTTP2. One client-server connection vs 10+ individual requests.

# Security

Here are some of the steps for hardening an nginx server:

-   Remove unused modules. Can only be done while compiling from source.
-   Turn off server tokens with `server_tokens off;`. This hides the nginx version.
-   Set buffer sizes to prevent buffer overflow attacks.
-   Block user agents. Prevents indexing and scraping.
-   Configure X-frame-options. Tells when it's ok to server to an iframe.

# Caching

Reusing page renders after they have been loaded once. Instead of everyone making a request, running a script, fetching data from the database, rendering a page and sending a response, only the first one does the work, the results is cached in a database (for a certain amount of time), and everyone that wants the same is simply given the results from the database. **Useful for static content, not so much for dynamic i.e. web apps.**

Caching is also possible on the client side, by specifying certain HTTP headers (ETags, Last-Modified).

POST requests should not be cached.

Caching problems can be identified by appending a parameter at the end of the URL which bypasses the caching. `www.example.com/posts/1?test`

# Compression

Using `gzip` can greatly reduce the traffic by compressing text files (not images) like HTML, CSS, JS, XML, which use verbose bloated formats.

Done in `/etc/nginx/nginx.conf` (trickles down to all site-specific configs in `/etc/nginx/conf.d/SITEFILE`)

`gzip on` - Enable compression.  
`gzip_min_length 512` - Only compress files bigger than 512 bytes.  
`gzip_types text/plain application/javascript text/html text/css;` - Formats to compress.  
`gzip_vary` - Sends vary header.

It's important to use the `Vary` option in order to prevent compressed and non-compressed responses from interfering with each other i.e. two browsers download gzipped content, while only one supports it, resulting in only it being cached, making that response unusable to other non-supporting browsers.
