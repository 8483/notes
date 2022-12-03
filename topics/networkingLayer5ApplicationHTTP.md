# HTTP - Hypertext Transfer Protocol

![TEA](../pics/networking/http.jpg)

HTTP is implemented in browsers and web servers.

HTTP is needed so that communicating systems can understand each other.

Simple HTTP Request

```bash
# HTTP header
GET /posts/1 HTTP/1.1
Host: jsonplaceholder.typicode.com

# Manual request - Returns header and body
printf 'GET /posts/1 HTTP/1.1\r\nHost:jsonplaceholder.typicode.com\r\n\r\n' | nc jsonplaceholder.typicode.com 80

# Just the body
curl jsonplaceholder.typicode.com/posts/1
```

# DNS

It's basically a phonebook for IP addresses.

An `A Record` is matched with an IP address, so when someone looks for `www.google.com`, the A record is referenced and the IP address is sent back to the user.

The DNS resolver i.e. client code is built into the OS.

**CNAME** - Canonical Name i.e. alias for a domain.  
**AAAA** - IPv6 equivalent to an A record.  
**NS** - DNS Name server. The NS record for a particular domain specifies which DNS has the records.
