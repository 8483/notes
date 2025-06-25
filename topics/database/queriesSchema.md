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

# Foreign Keys

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

# Delete

Be **VERY** careful with this one. **ALWAYS** select first, delete second.

```sql
DROP TABLE tableName;
```
