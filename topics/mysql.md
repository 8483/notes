# Best practices

### Naming Convention

-   **Singular form**. Both tables and columns.
-   Be consistent! Doesn't matter if you use camelCase or snake_case. Use whatever the front-end uses.
-   Avoid abbreviations or prefixes.
-   Use unique names that cannot collude with SQL/RDBMS reserved words (avoid name, order, percent...) **or** use a trailing underscore.
-   Do not use the table name followd by “id” (e.g. client_id) as your PK. id is more than enough and everyone will understand.
-   Never use capital letters in your table or field names. Ever.

### Performance

-   Always foreign key to IDs, rather than column values.
-   Try to stick to `where` clauses on indexed columns, instead of `like`.
-   Don't go crazy with `joins`.
-   Don't use varchar(255). Try to use the lowest number possible.

# RAM usage

SQL engines will consume as much memory as you will allow.

The reason for this is that the engine caches the data in RAM, so that it can access if faster than it could if it needed to read the data from the disk every time a user needed it.

# Install MySQL

```bash
sudo apt install mysql-server

sudo mysql_secure_installation
# Change root password to more secure.
# Remove anonymous users.
# Disable remote root login. Root should only connect via `localhost`.
# Remove test database and access to it.
# Reload privilege tables.

sudo /etc/init.d/mysql start
# sudo service mysql start
# systemctl restart mysql
```

### Remove MySQL

```bash
sudo apt remove --purge mysql*
sudo apt purge mysql*
sudo apt autoremove
sudo apt autoclean
sudo apt remove dbconfig-mysql
```

# Status

```bash
sudo systemctl status mysql
sudo systemctl start mysql

sudo /etc/init.d/mysql status
sudo /etc/init.d/mysql start
```

# Login

```bash
# Log into MySQL as root, with password.
sudo mysql -u root -p
```

# Run script

```bash
# Run global script
mysql -u user -p < db.sql

# Run database specific script
mysql -u user -p db_name < db.sql

# Output results
mysql -u user -p db_name < db.sql > /tmp/output.txt
```

# Command line

[Digital Ocean tutorial](https://www.digitalocean.com/community/tutorials/a-basic-mysql-tutorial)

Commands are **not** case sensitive, but table names are. **All commands must end with** `;`.

`;` - Execute/End current command.  
`ENTER` - Starts a new line. `;` is expected.

# Users

```sql
-- Create user
CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';

-- Give access to certain areas. In this case, it's for everything as *.* stands for dbName.tableName i.e. all of them.
GRANT ALL PRIVILEGES ON * . * TO 'user'@'localhost';

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

# Database

```sql
SHOW DATABASES;              -- List databases.
SELECT database();           -- Show current database.
USE dbName;                 -- Select a database.

CREATE DATABASE dbName;     -- Create a database.
DROP DATABASE dbName;       -- Delete a database.
```

# Tables

### Create

```sql
SHOW TABLES;                 -- List all tables.
DESCRIBE tableName;         -- Display columns and types.

-- Shows the query that creates the table.
SHOW CREATE TABLE tableName;

-- Create a table.
CREATE TABLE tableName (column1 DATATYPE, column2 DATATYPE);

-- Example
CREATE TABLE user (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT, -- Important
    name VARCHAR(255),
    pass VARCHAR(255)
);
```

### Modify

```sql
-- Rename a table.
RENAME TABLE tableName1 TO tableName2;

-- Change a column's datatype.
ALTER TABLE tablename MODIFY columnname DATATYPE;

-- Add a column at end.
ALTER TABLE tableName ADD columnName DATATYPE;

-- Add a column at certain location.
ALTER TABLE tableName ADD columnName DATATYPE AFTER columnName;

-- Delete a column.
ALTER TABLE tableName DROP columnName;

-- Change a column name. Has to be backticks.
ALTER TABLE tableName CHANGE `oldcolname` `newcolname` datatype(length);

-- Reset AUTO_INCREMENT id. For this to work, the table must be empty.
ALTER TABLE table AUTO_INCREMENT = 1;
```

### Delete

Be **VERY** careful with this one. **ALWAYS** select first, delete second.

```sql
DROP TABLE tableName;
```

## Foreign Keys

They are used for **data integrity** i.e. they prevent entering values that don't exist in the linked table (gives an error).

A FOREIGN KEY is a field in one table that refers to the PRIMARY KEY in another table.

The table containing the foreign key is called the child table, and the table containing the candidate key is called the referenced or parent table.

```sql
-- Foreign key definition during table creation.
CREATE TABLE user (
    id INT NOT NULL PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255),
    pass VARCHAR(255),
    phoneId INT,
    FOREIGN KEY (phoneId) REFERENCES phone(id)
);

-- Add foreign key.
ALTER TABLE table1 ADD FOREIGN KEY (idColumn) REFERENCES table2(idColumn);

-- Remove foreign key.
ALTER TABLE tableName DROP FOREIGN KEY FK_columnName;
```

```sql
-- List foreign keys.
SELECT
    tableName,
    COLUMNNAME,
    CONSTRAINT_NAME,
    REFERENCED_tableName,
    REFERENCED_COLUMNNAME
FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE
WHERE
    REFERENCED_tableName = 'my_table';

-- One-liner.
SELECT tableName, COLUMNNAME, CONSTRAINT_NAME, REFERENCED_tableName, REFERENCED_COLUMNNAME FROM INFORMATION_SCHEMA.KEY_COLUMN_USAGE WHERE REFERENCED_tableName = 'my_table';
```

# Records

```sql
-- Get records.
SELECT * FROM tableName;

-- Add record.
INSERT INTO tableName (column1, column2) VALUES (value1, value2);

-- Example
INSERT INTO user (id, name, pass)
VALUES (NULL, 'john', 'abc123');

-- Modify record.
UPDATE tableName SET columnName = 'value' WHERE criteria;

-- Example
UPDATE user SET name = 'Mike' WHERE id = 1;

-- Delete record.
DELETE FROM tableName WHERE columnName = <value>;
```

### INSERT ON DUPLICATE KEY UPDATE

Insert if `id` doesn't exit. Else, update the data at said `id`.

```sql
+----+--------------+
| id | columnName   |
+----+--------------+
|  1 | foo          |
|  2 | bar          |
|  3 | baz          |
+----+--------------+

INSERT INTO table (id, columnName)
VALUES
    (1, 'qux'),
    (null, 'hex')
ON DUPLICATE KEY UPDATE
    columnName = values(columnName);

+----+--------------+
| id | columnName   |
+----+--------------+
|  1 | qux          | -- Value updated
|  2 | bar          |
|  3 | baz          |
|  4 | hex          | -- Inserted row
+----+--------------+
```

# Size

```sql
-- Check the size of all the databases
SELECT
	table_schema,
	ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) "Size (MB)"
FROM information_schema.TABLES
GROUP BY table_schema;

-- Check the size of all the tables in a database
SELECT
	  table_name,
    ROUND(((data_length + index_length) / 1024 / 1024), 2) "Size (MB)"
FROM information_schema.TABLES
WHERE table_schema = "DATABASE_NAME" -- DATABASE_NAME
ORDER BY (data_length + index_length) DESC;
```

# Timezone

1. Add this line in `/etc/mysql/my.cnf`

```bash
[mysqld]
default-time-zone = "+01:00"
```

2. Restart the server

```
sudo service mysql restart
```

# Backup

To do this, we use the `mysqldump` command which creates a file with the SQL statements necessary to re-create the database.

With `--all-databases` or `--databases`, mysqldump writes `CREATE DATABASE` and `USE` statements prior to the dump output for each database, in order to insure the data goes to the right database.

```bash
# Dump just the data, without creating the database
mysqldump dbName > backup.sql

mysqldump --add-drop-table --databases dbName > backup.sql

# Simplest command.
mysqldump --databases dbName > backup.sql

# Multiple databases.
mysqldump --databases db1 db2 > backup.sql

# Everything.
mysqldump --all-databases > backup.sql

# Prompt for password.
mysqldump -p --databases dbName > backup.sql
```

Options

```bash
# Add DROP DATABASE statement before each CREATE DATABASE statement
--add-drop-database

# Add DROP TABLE statement before each CREATE TABLE statement
--add-drop-table

# Only database structure, without contents.
--no-data
```

## Automated

```bash
# backup database - backup-db-name-20200903-032043.sql
mysqldump -u root --password="db-pass" --databases db-name > /home/user/backups/"backup-db-name-$(date +%Y%m%d-%H%M%S).sql"

# Edit crontabs i.e Add a cron job that runs as root. Opens editor.
sudo crontab -e

# backup database every day at 23:59
59 23 * * * mysqldump -u root --password="db-pass" --databases db-name > /home/user/backups/"backup-db-name-$(date +%Y%m%d-%H%M%S).sql"

# backup ALL databases every day at 23:59
59 23 * * * mysqldump -u root --password="db-pass" --all-databases > /home/user/backups/"backup-all-$(date +%Y%m%d-%H%M%S).sql"
```

# Restore

If the dump was created without using `--databases`, then the database must be manually created before restoring.

Also, the database must be specified with `mysql dbName < backup.sql`.

Otherwise, just use:

```bash
# Restore a database.
mysql < backup.sql

# Prompt for password.
mysql -p < backup.sql

# From a compressed file.
gzip -d < backup.sql.gz | mysql

# If the database already exists and we want to restore it.
mysql dbName < backup.sql
```

# Transfer

Copy data from one database into another.

```
mysqldump -u root -p'password' source_db | mysql -u root -p'password' target_db
```

# Import/Export

**Import from remote to local**

```bash
remotepath="/remote/path/backups/"
localpath="/local/path/backups/"

filename="dbname-$(date +%Y%m%d-%H%M%S)-import"

echo "backup: $filename"

echo "creating backup at remote server..."
ssh user@123.456.789.255 "mysqldump -u root --password="pass" --databases dbname > $remotepath$filename"\
\
&& echo "downloading backup from remotepath to local..." \
&& rsync -av user@123.456.789.255:$remotepath$filename $localpath \
\
&& echo "importing backup into local mysql..." \
&& mysql -u root --password="pass" < $localpath$filename
```

**Export from local to remote**

```bash
remotepath="/remote/path/backups/"
localpath="/local/path/backups/"

filename="dbname-$(date +%Y%m%d-%H%M%S)-export"

echo "backup: $filename"

echo "creating local backup..."
mysqldump -u root --password="" dbname > $localpath$filename \
\
&& echo "transfering backup from local to remotepath..." \
&& rsync -av -e 'ssh' $localpath$filename user@123.456.789.255:$remotepath \
\
&& echo "importing transfered backup into remote mysql..." \
&& ssh user@123.456.789.255 "mysql -u root --password="pass" dbname < $remotepath$filename" \
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
sudo apt install mysqltuner
```

It's a utility used to find out what could be done in order to optimize MySQL for the hardware and workload.

It needs a bit of data to work properly, so a period should pass before running it, as it looks for usage patterns.

To run it, just use `mysqltuner`.

# Dynamic Columns / Pivot

```sql
SET @sql = NULL;
SET @textColumns = NULL;
SET @numberColumns = NULL;
SET @dateColumns = NULL;

SELECT
  GROUP_CONCAT(
	DISTINCT CONCAT('max(IF(f.uid = ''', f.uid, ''', dt.value, NULL))', f.uid)
  ) INTO @textColumns
FROM dataText dt
	left join field f on f.id = dt.fieldId
    left join entity e on e.id = f.entityId
where e.uid = 'XirQSpRrPP';

SELECT
  GROUP_CONCAT(
	DISTINCT CONCAT('max(IF(f.uid = ''', f.uid, ''', dn.value, NULL))', f.uid)
  ) INTO @numberColumns
FROM dataNumber dn
	left join field f on f.id = dn.fieldId
    left join entity e on e.id = f.entityId
where e.uid = 'XirQSpRrPP';

SELECT
  GROUP_CONCAT(
	DISTINCT CONCAT('max(IF(f.uid = ''', f.uid, ''', dd.value, NULL))', f.uid)
  ) INTO @dateColumns
FROM dataDate dd
	left join field f on f.id = dd.fieldId
    left join entity e on e.id = f.entityId
where e.uid = 'XirQSpRrPP';

SET @sql = CONCAT('
	select
		r.id,
		r.uid,
		', @textColumns, ',
		', @numberColumns, ',
        ', @dateColumns, '
   from record r
		left join entity e on e.id = r.entityId
		left join field f on f.entityId = e.id
		left join dataText dt on dt.recordId = r.id
		left join dataNumber dn on dn.recordId = r.id
        left join dataDate dd on dd.recordId = r.id
	where e.uid = ''XirQSpRrPP''
	group by
		r.id,
		r.uid
');

PREPARE stmt FROM @sql;
EXECUTE stmt;
DEALLOCATE PREPARE stmt;
```
