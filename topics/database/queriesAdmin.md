# Users

```sql
-- Create user
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';

-- Give access to certain areas. In this case, it's for everything as *.* stands for dbName.tableName i.e. all of them.
GRANT ALL PRIVILEGES ON * . * TO 'user'@'localhost';

-- Fix errors
ALTER USER 'user'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'

-- Reload the privileges.
FLUSH PRIVILEGES;

-- List all users.
SELECT user FROM mysql.user;

-- Show current user.
SELECT CURRENT_USER();

-- Delete user.
DROP USER 'user'@'localhost';
```

## Permissions

**Options:** ALL PRIVILEGES, CREATE, DROP, DELETE, INSERT, SELECT, UPDATE, GRANT OPTION (User can give permissions).

```sql
-- Give a specific permission, for a specific table.
GRANT permission ON dbName.tableName TO '<user>'@'localhost';

-- Remove a permission.
REVOKE permission ON dbName.tableName FROM '<user>'@'localhost';
```

## Change password

```sql
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
FLUSH PRIVILEGES;

ALTER USER 'root'@'localhost' IDENTIFIED WITH caching_sha2_password BY 'MyNewPass';
FLUSH PRIVILEGES;
```
