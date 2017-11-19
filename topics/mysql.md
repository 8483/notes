# MySQL

## Install MySQL

1. `sudo apt-get install mysql-server`.
2. `sudo mysql_secure_installation`
    - Change root password to more secure.
    - Remove anonymous users.
    - Disable remote root login. Root should only connect via `localhost`.
    - Remove test database and access to it.
    - Reload privilege tables.
3. `systemctl restart mysql` or `sudo service mysql start`  

# Command line

[Digital Ocean tutorial](https://www.digitalocean.com/community/tutorials/a-basic-mysql-tutorial)  
Commands are **not** case sensitive, but table names are. **All commands must end with** `;`.  

`mysql -u root -p` - Log into MySQL as root.  
`;` - Execute/End current command.  
`ENTER` - Starts a new line. `;` is expected.  

## Users
`CREATE USER '<user>'@'localhost' IDENTIFIED BY '<password>';` - Create user.  
`GRANT ALL PRIVILEGES ON * . * TO '<user>'@'localhost';` - Give access to certain areas. In this case, it's for everything as `*.*` stands for `<database>.<table>` i.e. all of them.  
`FLUSH PRIVILEGES;` - Reload the privileges.  

` DROP USER '<user>'@'localhost';`

`SELECT USER();` - List all users.  
`SELECT CURRENT_USER();` - Show current user.  

`mysql -u <user> -p` - Log in as new user.  

#### Specific permissions

`GRANT <permission> ON <database>.<table> TO '<user>'@'localhost';` - Give a specific permission, for a specific table.  
`REVOKE <permission> ON <database>.<table> FROM '<user>'@'localhost';` - Remove a permission.  

**Permissions:** ALL PRIVILEGES, CREATE, DROP, DELETE, INSERT, SELECT, UPDATE, GRANT OPTION (Can give permissions).   


## Database
`SHOW DATABASES;` - List databases.  
`SELECT database();` - Show current database.  
`USE <database>;` - Select a database.  

`CREATE DATABASE <database>;` - Create a database.  
`DROP DATABASE <database>;` - Delete a database.  

## Tables
`SHOW TABLES;` - List all tables.  
`DESCRIBE <table>;` - Display columns and types.  

`SHOW CREATE TABLE <table>` - Shows the query that creates the table.  

`CREATE TABLE <table> (column1 DATATYPE, column2 DATATYPE)` - Create a table.  
```sql
CREATE TABLE Users (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT, -- Important
    name VARCHAR(255),
    pass VARCHAR(255)
);
```  

`ALTER TABLE <table> ADD <column> DATATYPE;` - Add a column at end.  
`ALTER TABLE <table> ADD <column> DATATYPE AFTER <column>;` - Add a column at certain location.  
`ALTER TABLE <table> DROP <column>;` - Delete a column.  

`DROP TABLE <table>` - Delete a table.  

## Foreign Keys

They are used for **data integrity** i.e. they prevent entering values that don't exist in the linked table (gives an error).  

A FOREIGN KEY is a field in one table that refers to the PRIMARY KEY in another table.  

The table containing the foreign key is called the child table, and the table containing the candidate key is called the referenced or parent table.  

`ALTER TABLE <table1> ADD FOREIGN KEY (<id_column>) REFERENCES <table2>(<id_column>);` - Adds a foreign key.  

`ALTER TABLE <table> DROP FOREIGN KEY FK_<column>;` - Removes a foreign key.  

`SHOW CREATE TABLE <table>` - Includes the foreign key creation.  

```sql
SELECT -- List foreign keys
    TABLE_NAME,
    COLUMN_NAME,
    CONSTRAINT_NAME,
    REFERENCED_TABLE_NAME,
    REFERENCED_COLUMN_NAME
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE
    REFERENCED_TABLE_NAME = 'My_Table';
```

`SELECT TABLE_NAME, COLUMN_NAME, CONSTRAINT_NAME, REFERENCED_TABLE_NAME, REFERENCED_COLUMN_NAME FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE WHERE REFERENCED_TABLE_NAME = 'Categories';` - The same in one line.  

## Records
`SELECT * FROM <table>` - Retrieve data.  

`INSERT INTO <table> (column1, column2) VALUES (value1, value2);` - Insert a record.  
```sql
INSERT INTO Users ('id', 'name', 'pass')
VALUES (NULL, 'john', 'abc123');
```

`UPDATE <table> SET <column> = <value> WHERE <criteria>` - Update a record.  
```sql
UPDATE Users SET name = 'Mike' WHERE id = `1`; -- John's id.
```
`DELETE FROM <table> WHERE <column> = <value>;` - Delete a record.  

# Backup

To do this, we use the `mysqldump` command which creates a file with the SQL statements necessary to re -create the database. Use `--databases` in order to have `CREATE TABLE IF NOT EXIST` included in the dump.  

`mysqldump --add-drop-table --databases [database] > backup.sql`

`mysqldump --databases [database] > backup.sql` - Simplest command.  
`mysqldump --databases [database1] [database2] > backup.sql` - Multiple databases.  
`mysqldump --all-databases > backup.sql` - Everything.  

`mysqldump -p --databases [database] > backup.sql` - Prompt for password.  

**options:**
- `--add-drop-table` - add a DROP TABLE statement before each CREATE TABLE.  
- `--no-data` - Only database structure, without contents.   

## Restore
If the dump was created without using `--databases`, then the database must be manually created before restoring. Also, the database must be specified with `mysql [database] < backup.sql`. Otherwise, just use:  

`mysql < backup.sql` - Restore a database.  
`mysql -p < backup.sql` - Prompt for password.  

`gzip -d < backup.sql.gz | mysql` - From a compressed file.    

If the database already exists and we want to restore it, we can use:  
`mysql [database] < backup.sql`

# Naming Convention

- **Singular form**. Both tables and columns.
- Always lowercase.
- Use underscores for spaces.
- Avoid using only `id`. Use `user_id`.
- No CamelCase, abreviations or prefixes.

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

`sudo apt-get install mysqltuner`  

It's a utility used to find out what could be done in order to optimize MySQL for the hardware and workload.  

It needs a bit of data to work properly, so a period should pass before running it, as it looks for usage patterns.  

To run it, just use `mysqltuner`.
