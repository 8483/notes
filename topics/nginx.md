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

`/usr/share/nginx/html/` has the index.html file.

`nginx -t` - After changes, check the syntax and run a test to make sure the web-server will work.

`systemctl reload nginx` - Load the changes because the server is running with the old config file. Otherwise use `service nginx reload`.

`/etc/nginx/nginx.conf` is the main server configuration file. For each site we run, a separate file is needed, unless we want to run just one site.

Specific site configurations must be placed in the `/etc/nginx/conf.d/` folder. These override any `nginx.conf` settings. These are loaded by the default `include /etc/nginx/conf.d/*.conf;` line in the main .conf file.

`http {}` block - For all incoming http requests, use these settings.

## Command Line

The default location nginx looks in for the configuration file is `/etc/nginx/nginx.conf`, but you can pass in an arbitrary path with the `-c` flag. Ex. `nginx -c /usr/local/nginx/conf`.

```Bash
nginx -c <path> -t   # Test configuration at absolute path
nginx -c <path>      # Start with a custom configuration.
nginx -s <signal>    # Stop or reload.
```

## Terminology

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

## Simplest server

```nginx
events {}                            # Needed to be a valid conf file.

http {
    include mime.types;              # Recognize filetypes.
    server {
        listen 80;
        server_name 127.0.0.1;      # Should be domain name.
        root /sites/template;       # Match URIs to files here.
    }
}
```

After that, reload with `systemctl reload nginx`.

## Static server with custom configuration

**nginx.conf**

```nginx
events {}

http {
    include /etc/nginx/mime.types; # Path needs to be absolute.
    server {
        listen 8080;
        root /home/user/app/public; # index.html is inside public.
    }
}
```

**Useful commands**

```bash
# Check if nginx is running.
systemctl status nginx

# Start nginx if not.
systemctl start nginx

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

## Location Blocks

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

## Serving Files

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

#### Matching URIs to Location Blocks

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

#### try_files

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

## Logging

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

## SSL/HTTPS

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

#### Let's Encrypt

Certbot is used for generating certificates and automating their renewal.

# Proxy vs Reverse Proxy

**Normal:** Client > Server  
**Proxy:** Client > Proxy > Server

Used to hide the origin of the request.

**Normal:** Client > App  
**Reverse Proxy:** Client > Server > App

Used to prevent direct access to app i.e. exploit bad code.

To run any Linux server on port 80, you need to be running as root. If you want to run node directly on port 80, that means you have to run node as root. If your application has any security flaws, it will become significantly compounded by the fact that it has access to do _anything it wants_. For example, if you write something that accidentally allows the user to read arbitrary files, now they can read files from other server user's accounts.

If you use nginx in front of node, you can run your node as a limited user on port 3000 (or whatever) and then have nginx run as root on port 80, forwarding requests to port 3000. Now the node application is limited to the user permissions you give it.

It's always better to use nginx as a proxy for a node.js server; nginx can proxy to a number of node backends and should any of them die can fail-over automatically with the benefit that, if there is an issue with the node interpreter (for instance while upgrading) and it stops responding, it can serve a fall-back HTML page.

Additionally, you shouldn't be using node.js for serving "static" files such as images, js/css files, etc. Use node.js for the complex stuff and let nginx take care of the things it's good at - serving files from the disk or from a cache.

## Node Reverse Proxy

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

#### Important

**Any changes made to** `nginx.conf` **need to be reloaded with** `sudo nginx -s reload` **for them to work!!!**

It is crucial to note the usage of the **trailing slash**. This is called a **proxy path**. Unless we specify one, nginx assumes the original request path i.e. location. It is **recommended** to **use** the proxy path / trailing slash.

[Stack Overflow explanation.](https://stackoverflow.com/questions/32542282/how-do-i-rewrite-urls-in-a-proxy-response-in-nginx)

```nginx
location /foo/ {
    proxy_pass http://localhost:3000/;
}
```

**With** trailing slash, the request for `http://localhost:3000/foo/bar/baz` will be proxied to `http://localhost:3000/bar/baz`. Basically, `/foo/` gets replaced by `/` to change the request path from `/foo/bar/baz` to `/bar/baz`.

```nginx
location /foo {
    proxy_pass http://localhost:3000;
}
```

**Without** trailing slash, the same request will be proxied to `http://localhost:3000/foo/bar/baz`. Basically, the full original request path gets passed on without changes.

For example, a request to `http://localhost:8000/app/users` (with the config below) would return all users because the request made to the server is actually `http://localhost:3000/api/users`.

```nginx
server {
    listen 8000;

    location /app/ {
        proxy_pass 'http://localhost:3000/api/users';
    }
}
```

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

# PHP Interpreter Setup

Nginx only understands static files. It doesn't know how to handle code. For that, we use an interpreter (sits behind the web-server), which communicates with nginx via a socket file.

1. `apt-get install php-mysql php-fpm`. - Install the interpreter.
    - **FastCGI Process Manager** is used to create a pool of PHP interpreter processes waiting to deal with web-server requests. Without this, there would be only one process which would handle requests one by one, resulting in long wait times.
2. `apt-get install php-json php-xmlrpc php-curl php-gd php-xml` - Install extra extensions needed for Wordpress.
3. `mkdir /var/run/php-fpm` - Create directory for php-fpm sockets (webserver -> PHP)
4. `ls /etc/php/7.0/fpm/pool.d` - Check if the pool configurations directory exists. If not, create it.
5. Configure `/etc/php/7.0/fpm/php-fpm.conf`:
    - Make a backup copy.
    - Add **just** the following inside:
        ```
        [global]
        pid = /run/php-fpm.pid
        error_log = /var/log/php-fpm.log
        include = /etc/php/fpm/pool.d/*.conf
        ```
6. Create a default pool configuration in `/etc/php/7.0/fpm/pool.d/www.conf`:
    - Make a backup copy.
    - Add **just** the following inside to define a private socket between nginx and the interpreter i.e. www-data from nginx communicates with www-data from fpm:
        ```
        [default]
        security.limit_extensions = .php
        listen = /var/run/php/yourserverhostname.sock
        listen.owner = www-data
        listen.group = www-data
        listen.mode = 0660
        user = www-data
        group = www-data
        pm = dynamic
        pm.max_children = 75
        pm.start_servers = 8
        pm.min_spare_servers = 5
        pm.max_spare_servers = 20
        pm.max_requests = 500
        ```
7. Edit the top level php.ini file in `/etc/php/7.0/fpm/php.ini`:
    - Make a backup copy.
    - Add **just** the following inside:
        ```
        [PHP]
        engine = On
        short_open_tag = Off
        asp_tags = Off
        precision = 14
        output_buffering = 4096
        zlib.output_compression = Off
        implicit_flush = Off
        unserialize_callback_func =
        serialize_precision = 17
        disable_functions =
        disable_classes =
        zend.enable_gc = On
        expose_php = Off
        max_execution_time = 30
        max_input_time = 60
        memory_limit = 128M
        error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT
        display_errors = Off
        display_startup_errors = Off
        log_errors = On
        log_errors_max_len = 1024
        ignore_repeated_errors = Off
        ignore_repeated_source = Off
        report_memleaks = On
        track_errors = Off
        html_errors = On
        variables_order = "GPCS"
        request_order = "GP"
        register_argc_argv = Off
        auto_globals_jit = On
        post_max_size = 8M
        auto_prepend_file =
        auto_append_file =
        default_mimetype = "text/html"
        default_charset = "UTF-8"
        doc_root =
        user_dir =
        enable_dl = Off
        cgi.fix_pathinfo=0
        file_uploads = On
        upload_max_filesize = 25M
        max_file_uploads = 20
        allow_url_fopen = On
        allow_url_include = Off
        default_socket_timeout = 60
        [CLI Server]
        cli_server.color = On
        [Date]
        [filter]
        [iconv]
        [intl]
        [sqlite]
        [sqlite3]
        [Pcre]
        [Pdo]
        [Pdo_mysql]
        pdo_mysql.cache_size = 2000
        pdo_mysql.default_socket=
        [Phar]
        [mail function]
        SMTP = localhost
        smtp_port = 25
        mail.add_x_header = On
        [SQL]
        sql.safe_mode = Off
        [ODBC]
        odbc.allow_persistent = On
        odbc.check_persistent = On
        odbc.max_persistent = -1
        odbc.max_links = -1
        odbc.defaultlrl = 4096
        odbc.defaultbinmode = 1
        [Interbase]
        ibase.allow_persistent = 1
        ibase.max_persistent = -1
        ibase.max_links = -1
        ibase.timestampformat = "%Y-%m-%d %H:%M:%S"
        ibase.dateformat = "%Y-%m-%d"
        ibase.timeformat = "%H:%M:%S"
        [MySQL]
        mysql.allow_local_infile = On
        mysql.allow_persistent = On
        mysql.cache_size = 2000
        mysql.max_persistent = -1
        mysql.max_links = -1
        mysql.default_port =
        mysql.default_socket =
        mysql.default_host =
        mysql.default_user =
        mysql.default_password =
        mysql.connect_timeout = 60
        mysql.trace_mode = Off
        [MySQLi]
        mysqli.max_persistent = -1
        mysqli.allow_persistent = On
        mysqli.max_links = -1
        mysqli.cache_size = 2000
        mysqli.default_port = 3306
        mysqli.default_socket =
        mysqli.default_host =
        mysqli.default_user =
        mysqli.default_pw =
        mysqli.reconnect = Off
        [mysqlnd]
        mysqlnd.collect_statistics = On
        mysqlnd.collect_memory_statistics = Off
        [OCI8]
        [PostgreSQL]
        pgsql.allow_persistent = On
        pgsql.auto_reset_persistent = Off
        pgsql.max_persistent = -1
        pgsql.max_links = -1
        pgsql.ignore_notice = 0
        pgsql.log_notice = 0
        [Sybase-CT]
        sybct.allow_persistent = On
        sybct.max_persistent = -1
        sybct.max_links = -1
        sybct.min_server_severity = 10
        sybct.min_client_severity = 10
        [bcmath]
        bcmath.scale = 0
        [browscap]
        [Session]
        session.save_handler = files
        session.use_strict_mode = 0
        session.use_cookies = 1
        session.use_only_cookies = 1
        session.name = PHPSESSID
        session.auto_start = 0
        session.cookie_lifetime = 0
        session.cookie_path = /
        session.cookie_domain =
        session.cookie_httponly =
        session.serialize_handler = php
        session.gc_probability = 1
        session.gc_divisor = 1000
        session.gc_maxlifetime = 1440
        session.referer_check =
        session.cache_limiter = nocache
        session.cache_expire = 180
        session.use_trans_sid = 0
        session.hash_function = 0
        session.hash_bits_per_character = 5
        url_rewriter.tags = "a=href,area=href,frame=src,input=src,form=fakeentry"
        [MSSQL]
        mssql.allow_persistent = On
        mssql.max_persistent = -1
        mssql.max_links = -1
        mssql.min_error_severity = 10
        mssql.min_message_severity = 10
        mssql.compatibility_mode = Off
        mssql.secure_connection = Off
        [Assertion]
        [COM]
        [mbstring]
        [gd]
        [exif]
        [Tidy]
        tidy.clean_output = Off
        [soap]
        soap.wsdl_cache_enabled=1
        soap.wsdl_cache_dir="/tmp"
        soap.wsdl_cache_ttl=86400
        soap.wsdl_cache_limit = 5
        [sysvshm]
        [ldap]
        ldap.max_links = -1
        [dba]
        [opcache]
        [curl]
        [openssl]
        ```
8. `cgi.fix_pathinfo=0` - **Make sure this is set!**
