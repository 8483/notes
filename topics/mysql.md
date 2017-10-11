# MySQL

## Terminology

`Model` holds all the chemas. There can be many modes, and each can have many schemas.

`Schema` is the overall design of the database which defines the tables, number of columns, foreign keys... This rarely changed, if at all...  

`EER Diagram` is a visual representation of a schema with boxes for tables and lines for table relations.  

`Database` is an instance of a schema. It's also where the data lives.  

A database is created by forward engineering a schema. An existing database is expanded with the new schema objects, but it does not alter the existing ones.  

To overwrite them, the tables need to be dropped first, which is an option during the forward engineering.

`Meta-data` is data about the database i.e. where the schema is stored.  

## Creating a Database in MySQL Workbench

1. Create a localhost connection as root on port 3306.
2. Create a model **AND** name it.  
3. Forward Engineer the model in the localhost connection.  
4. Find the created database and populate it.
