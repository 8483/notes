# Memcached

It stores key-value pairs in memory for quick access. It doesn't persist data to disk as a database.

Can be run on a separate machine, but usually it's on the app server.

When an HTTP request hits the server, it first checks if memcached has the response stored and returns that. If not, it retireves the data from the database, generates the reposnse, and saves it in the cache for future use.

```bash
sudo apt-get install memcached
```

```bash
# key (string) - value (string)
set(key, value)
get(key)
delete(key)
```