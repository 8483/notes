# nginx

It only knows about static content. While it can serve static HTML, for dynamically generated HTML, it has to offload requests to other processes in order to generate HTML. Ex. User requests a PHP file, and nginx sends it to an interpreter, which then returns the HTML, which is sent back by nginx.

Nginx communicates with the PHP interpreter via a **unix socket** file. Nginx communicates with the website visitor via a **TCP socket** file.  

To access a guest (VM) nginx from a host, set a port forwarding in Virtualbox with name `nginx`, host port `80`, guest port `80`.

## Install
`add-apt-repository ppa:nginx/stable` - Add 3rd party repo.  
`apt-get update` - Refresh packages list.  
`apt-get install nginx` - Install the web server.  

## Configuration

`/usr/share/nginx/html/` has the index.html file.  

`nginx -t` - After changes, check the syntax and run a test to make sure the web-server will work.  

`systemctl reload nginx` - Load the changes because the server is running with the old config file.    

`/etc/nginx/nginx.conf` is the main server configuration file. For each site we run, a separate file is needed, unless we want to run just one site.

Specific site configurations must be placed in the `/etc/nginx/conf.d/` folder. These override any `nginx.conf` settings. These are loaded by the `include /etc/nginx/conf.d/*.conf;` line.  

`http {}` block - For all incoming http requests, use these settings.

## Caching

Reusing page renders after they have been loaded once. Instead of everyone making a request, running a script, fetching data from the database, rendering a page and sending a response, only the first one does the work, the results is cached in a database, and everyone that wants the same is simply given the results from the database.

## Proxy vs Reverse Proxy

**Normal:** Client > Server  
**Proxy:** Client > Proxy > Server

Used to hide the origin of the request.  

**Normal:** Client > App  
**Reverse Proxy:** Client > Server > App

Used to prevent direct access to app i.e. exploit bad code.  

To run any Linux server on port 80, you need to be running as root. If you want to run node directly on port 80, that means you have to run node as root. If your application has any security flaws, it will become significantly compounded by the fact that it has access to do *anything it wants*. For example, if you write something that accidentally allows the user to read arbitrary files, now they can read files from other server user's accounts.  

If you use nginx in front of node, you can run your node as a limited user on port 3000 (or whatever) and then have nginx run as root on port 80, forwarding requests to port 3000. Now the node application is limited to the user permissions you give it.  

It's always better to use nginx as a proxy for a node.js server; nginx can proxy to a number of node backends and should any of them die can fail-over automatically with the benefit that, if there is an issue with the node interpreter (for instance while upgrading) and it stops responding, it can serve a fall-back HTML page.

Additionally, you shouldn't be using node.js for serving "static" files such as images, js/css files, etc. Use node.js for the complex stuff and let nginx take care of the things it's good at - serving files from the disk or from a cache.
