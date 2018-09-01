# Best practices

### Naming Convention

- **Singular form**. Both tables and columns.
- Always lowercase.
- Use underscores for spaces.
- No CamelCase, abreviations or prefixes.

### Performance

- Try to stick to `where` clauses on indexed columns.
- Don’t go crazy with `joins`.

# Install MySQL

```bash
sudo apt-get install mysql-server

sudo mysql_secure_installation
# Change root password to more secure.
# Remove anonymous users.
# Disable remote root login. Root should only connect via `localhost`.
# Remove test database and access to it.
# Reload privilege tables.

systemctl restart mysql
# sudo service mysql start  
```


# Command line

[Digital Ocean tutorial](https://www.digitalocean.com/community/tutorials/a-basic-mysql-tutorial)  
Commands are **not** case sensitive, but table names are. **All commands must end with** `;`.  

`mysql -u root -p` - Log into MySQL as root.  
`;` - Execute/End current command.  
`ENTER` - Starts a new line. `;` is expected.  

# Users
```bash
# Log in as new user. 
mysql -u user -p 
```

```sql
-- Create user
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';  

-- Give access to certain areas. In this case, it's for everything as *.* stands for db_name.table_name i.e. all of them.
GRANT ALL PRIVILEGES ON * . * TO 'user'@'localhost';

-- Reload the privileges.
FLUSH PRIVILEGES; 

-- List all users.
SELECT USER();

-- Show current user.  
SELECT CURRENT_USER();

-- Delete user.
DROP USER 'user'@'localhost';
```

## Permissions

**Options:** ALL PRIVILEGES, CREATE, DROP, DELETE, INSERT, SELECT, UPDATE, GRANT OPTION (User can give permissions). 

```sql
-- Give a specific permission, for a specific table. 
GRANT permission ON db_name.table_name TO '<user>'@'localhost';

-- Remove a permission.   
REVOKE permission ON db_name.table_name FROM '<user>'@'localhost';
```

## Change password
```sql 
ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass';
FLUSH PRIVILEGES;
```

# Database
```sql
SHOW DATABASES;              -- List databases.  
SELECT database();           -- Show current database.  
USE db_name;                 -- Select a database.  

CREATE DATABASE db_name;     -- Create a database.  
DROP DATABASE db_name;       -- Delete a database. 
```

 

## Tables

### Create
```sql
SHOW TABLES;                 -- List all tables.  
DESCRIBE table_name;         -- Display columns and types.  

-- Shows the query that creates the table.  
SHOW CREATE TABLE table_name; 

-- Create a table.  
CREATE TABLE table_name (column1 DATATYPE, column2 DATATYPE);

-- Example
CREATE TABLE user (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT, -- Important
    name VARCHAR(255),
    pass VARCHAR(255)
);
```  
### Modify

```sql
-- Add a column at end.  
ALTER TABLE table_name ADD column_name DATATYPE; 

-- Add a column at certain location.
ALTER TABLE table_name ADD column_name DATATYPE AFTER column_name;

-- Delete a column.
ALTER TABLE table_name DROP column_name;

-- Change a column name. Has to be backticks.
ALTER TABLE table_name CHANGE `oldcolname` `newcolname` datatype(length);

-- Reset AUTO_INCREMENT id. For this to work, the table must be empty.
ALTER TABLE table AUTO_INCREMENT = 1;
```
### Delete
Be **VERY** careful with this one. **ALWAYS** select first, delete second.

```sql  
DROP TABLE table_name;
```

## Foreign Keys

They are used for **data integrity** i.e. they prevent entering values that don't exist in the linked table (gives an error).  

A FOREIGN KEY is a field in one table that refers to the PRIMARY KEY in another table.  

The table containing the foreign key is called the child table, and the table containing the candidate key is called the referenced or parent table. 

```sql
-- Add foreign key. 
ALTER TABLE table1 ADD FOREIGN KEY (id_column) REFERENCES table2(id_column); 

-- Remove foreign key.  
ALTER TABLE table_name DROP FOREIGN KEY FK_column_name;

-- Foreign key definition during table creation.
SHOW CREATE TABLE table_name; 

-- List foreign keys.
SELECT
    TABLE_NAME,
    COLUMN_NAME,
    CONSTRAINT_NAME,
    REFERENCED_TABLE_NAME,
    REFERENCED_COLUMN_NAME
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE
    REFERENCED_TABLE_NAME = 'my_table';

-- One-liner.  
SELECT TABLE_NAME, COLUMN_NAME, CONSTRAINT_NAME, REFERENCED_TABLE_NAME, REFERENCED_COLUMN_NAME FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE WHERE REFERENCED_TABLE_NAME = 'my_table';
```


## Records
```sql
-- Get records. 
SELECT * FROM table_name; 

-- Add record. 
INSERT INTO table_name (column1, column2) VALUES (value1, value2);

-- Example
INSERT INTO user ('id', 'name', 'pass')
VALUES (NULL, 'john', 'abc123');

-- Modify record.  
UPDATE table_name SET column_name = 'value' WHERE criteria;

-- Example
UPDATE user SET name = 'Mike' WHERE id = 1;

-- Delete record.
DELETE FROM table_name WHERE column_name = <value>;
```

# Backup
To do this, we use the `mysqldump` command which creates a file with the SQL statements necessary to re -create the database. Use `--databases` in order to have `CREATE TABLE IF NOT EXIST` included in the dump.  

```bash
mysqldump --add-drop-table --databases db_name > backup.sql

# Simplest command.
mysqldump --databases db_name > backup.sql

# Multiple databases.
mysqldump --databases db1 db2 > backup.sql

# Everything.
mysqldump --all-databases > backup.sql 

# Prompt for password.
mysqldump -p --databases db_name > backup.sql 

# Options

# add a DROP TABLE statement before each CREATE TABLE.  
--add-drop-table

# Only database structure, without contents. 
--no-data  
```

## Restore
If the dump was created without using `--databases`, then the database must be manually created before restoring. Also, the database must be specified with `mysql db_name < backup.sql`. Otherwise, just use:  

```bash
# Restore a database.
mysql < backup.sql

# Prompt for password. 
mysql -p < backup.sql 

# From a compressed file.
gzip -d < backup.sql.gz | mysql    

# If the database already exists and we want to restore it.  
mysql db_name < backup.sql
```

# MySQL Workbench

## Creating a Database

1. Create a localhost connection as root on port 3306.
2. Create a model **AND** name it.  
3. Forward Engineer the model in the localhost connection.  
4. Find the created database and populate it.

## Terminology

`Model` holds all the schemas. There can be many modes, and each can have many schemas.

`Schema` is the overall design of the database which defines the tables, number of columns, foreign keys... This rarely changed, if at all...  

`EER Diagram` is a visual representation of a schema with boxes for tables and lines for table relations.  

`Database` is an instance of a schema. It's also where the data lives.  

A database is created by forward engineering a schema. An existing database is expanded with the new schema objects, but it does not alter the existing ones.  

To overwrite them, the tables need to be dropped first, which is an option during the forward engineering.

`Meta-data` is data about the database i.e. where the schema is stored.  

# Tuning

```bash 
sudo apt-get install mysqltuner
```  

It's a utility used to find out what could be done in order to optimize MySQL for the hardware and workload.  

It needs a bit of data to work properly, so a period should pass before running it, as it looks for usage patterns.  

To run it, just use `mysqltuner`.