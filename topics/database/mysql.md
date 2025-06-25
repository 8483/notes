# Install MySQL

```bash
sudo apt install mysql-server;

sudo /etc/init.d/mysql start;
# sudo service mysql start
# systemctl restart mysql

sudo mysql_secure_installation; # Makes mysql more secure
# Change root password to more secure.
# Remove anonymous users.
# Disable remote root login. Root should only connect via `localhost`.
# Remove test database and access to it.
# Reload privilege tables.
```

### Remove MySQL

```bash
sudo apt remove --purge mysql*
sudo apt-get remove --purge mysql-server mysql-client mysql-common mysql-server-core-* mysql-client-core-*
sudo rm -rf /etc/mysql /var/lib/mysql
sudo rm -rf /var/log/mysql /var/log/mysql.*
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
