# MySQL

## Install MySQL

1. `sudo apt-get install mysql-server`.
2. `sudo mysql_secure_installation`
    - Change root password to more secure.
    - Remove anonymous users.
    - Disable remote root login. Root should only connect via `localhost`.
    - Remove test database and access to it.
    - Reload privilege tables.
3. `systemctl restart mysql`

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
