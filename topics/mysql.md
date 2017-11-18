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

## Command line
Commands are **not** case sensitive, but table names are. **All commands must end with** `;`.  

`mysql -p` - Log into MySQL.  
`;` - Execute/End current command.  
`ENTER` - Starts a new line. `;` is expected.  

#### Navigation
`SELECT database();` - Show current database.  
`USE <database>` - Select a database.  
`SHOW TABLES;` - List all tables.  
`DESCRIBE <table>` - Display columns and types.  

#### Creation
`CREATE DATABASE <database` - Create a database.  
`CREATE TABLE <table> (column1 datatype, column2 datatype)` - Create a table.  

#### Data
`INSERT INTO <table> (column1, column2) VALUES (value1, value2)` - Insert a record.  
`SELECT * FROM <table>` - Retrieve data.  

## Creating a Database in MySQL Workbench

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

## Tuning

`sudo apt-get install mysqltuner`  

It's a utility used to find out what could be done in order to optimize MySQL for the hardware and workload.  

It needs a bit of data to work properly, so a period should pass before running it, as it looks for usage patterns.  

To run it, just use `mysqltuner`.
