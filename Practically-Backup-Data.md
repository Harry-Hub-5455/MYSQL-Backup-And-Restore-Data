Author Name- Harry Malusare

**Note: By default, mysqldump command does not dump the information_schema database, performance_schema, and MySQL Cluster ndbinfo database.
**

If you want to include the information_schema tables, you must explicitly specify the name of the database in the mysqldump command, also include the —skip-lock-tables option.

There are lots of options and features that can be used with mysqldump. You can view the complete list of options here. I am going to some of the basic features. Following is the syntax of the mysqldump utility.

**mysqldump -u [user name] –p [password] [options] [database_name] [tablename] > [dumpfilename.sql]
**

**The parameters are as following:**


-u [user_name]: It is a username to connect to the MySQL server. To generate the backup using mysqldump, ‘Select‘ to dump the tables, ‘Show View‘ for views, ‘Trigger‘ for the triggers. If you are not using —single-transaction option, then ‘Lock Tables‘ privileges must be granted to the user
-p [password]: The valid password of the MySQL user

[option]: The configuration option to customize the backup

[database name]: Name of the database that you want to take backup

[table name]: This is an optional parameter. If you want to take the backup specific tables, then you can specify the names in the command
“<” OR ”>”: This character indicates whether we are generating the backup of the database or restoring the database. You can use “>” to generate the backup and “<” to restore the backup

[dumpfilename.sql]: Path and name of the backup file. As I mentioned, we can generate the backup in XML, delimited text, or SQL file so we can provide the extension of the file accordingly
Generate the backup of a single database

For example, you want to generate the backup of the single database, run the following command. The command will generate the backup of the “sakila” database with structure and data in the sakila_20200424.sql file.

**mysqldump -u root -p sakila > C:\MySQLBackup\sakila_20200424.sql**


When you run this command, it prompts for the password. Provide the appropriate password.

See the following image:


![generate-backup-of-database](https://user-images.githubusercontent.com/72725035/214862066-af5801db-7d10-4330-847d-7212f0cec4aa.png)



Once backup generated successfully, let us open the backup file to view the content of the backup file. Open the backup location and double-click on the “sakila_20200424.sql” file.





![content-of-backup-file](https://user-images.githubusercontent.com/72725035/214862251-d650a129-dd1c-4817-8675-d76d493bafb6.png)




As you can see in the above image, the backup file contains the various T-SQL statements that can be used to re-create the objects.



Generate the backup of multiple databases or all the databases:

For example, you want to generate a backup of more than one database. You must add the —databases option in the mysqldump command. The following command will generate the backup of “sakila” and “employees” database with structure and data.


**mysqldump -u root -p --databases sakila employees > C:\MySQLBackup\sakila_employees_20200424.sql**


See the following image:

![backup-multiple-database](https://user-images.githubusercontent.com/72725035/214862752-f27000c7-db6b-44b3-9c2a-860e36714fd8.png)

Similarly, if you want to generate the backup of all the databases, you must use –all-databases option in the mysqldump command. The following command will generate the backup of all databases within MySQL Server.


**mysqldump -u root -p --all-databases > C:\MySQLBackup\all_databases_20200424.sql
**


See the following image:


![generating-backup-of-all-databases](https://user-images.githubusercontent.com/72725035/214862941-aeab7d94-f6b2-4234-b39a-c61ac17ab612.png)

Generate the backup of database structure
If you want to generate the backup of the database structure, then you must use the –no-data option in the mysqldump command. The following command generates the backup of the database structure of the sakila database.

**mysqldump -u root -p --no-data sakila > C:\MySQLBackup\sakila_objects_definition_20200424.sql
**


See the following image:

![generate-the-backup-of-database-structure](https://user-images.githubusercontent.com/72725035/214863227-69802118-d840-4011-9535-edc82446466a.png)


Generate the backup of a specific table
If you want to generate the backup of a specific table, then you must specify the name of the tables after the name of the database. The following command generates the backup of the actor table of the sakila database.


**mysqldump -u root -p sakila actor payment > C:\MySQLBackup\actor_payment_table_20200424.sql**


If you want to generate the backup of more than one tables, than you must separate the names of the tables with space, the following command generates the backup of actor and payment table of sakila database.

![generate-the-backup-of-table](https://user-images.githubusercontent.com/72725035/214863541-e93a4ab4-e41c-410b-aeac-bd0747f00905.png)



Generate the backup of database data

If you want to generate the backup of the data without the database structure, then you must use the –no-create-info option in the mysqldump command. The following command generates the backup of data of the sakila database.

**mysqldump -u root -p sakila --no-create-info > C:\MySQLBackup\sakila_data_only_20200424.sql**


See the following image:

![generating-backup-of-data-only](https://user-images.githubusercontent.com/72725035/214863693-6490cc1a-e423-42a9-b47e-1917f8577708.png)


Let us view the content of the backup file.


![content-of-backup-file-1](https://user-images.githubusercontent.com/72725035/214863852-ed3cae3b-7b31-422a-8287-6eedc70ac875.png)


As you can see in the above screenshot, the backup file contains the various T-SQL statements that can be used to insert data in the tables.

**Restore the MySQL Database:**

Restoring a MySQL database using mysqldump is simple. To restore the database, you must create an empty database. First, let us drop and recreate the sakila database by executing the following command.


mysql> drop database sakila;

Query OK, 24 rows affected (0.35 sec)
mysql> create database sakila;
Query OK, 1 row affected (0.01 sec)
MySQL>

When you restore the database, instead of using mysqldump, you must use mysql; otherwise, the mysqldump will not generate the schema and the data. Execute the following command to restore the sakila database:

**mysql -u root -p sakila < C:\MySQLBackup\sakila_20200424.sql**


Once command executes successfully, execute the following command to verify that all objects have been created on the sakila database.

mysql> use sakila;
Database changed
mysql> show tables;

See the following image:


![content-of-database-which-is-restored](https://user-images.githubusercontent.com/72725035/214866138-e26dabe7-f8ed-45cf-b9af-3f64f14493d2.png)


**Restore a specific table in the database:**

For instance, someone dropped a table from the database. Instead of restoring the entire database, we can restore the dropped table from the available backup. To demonstrate, drop the actor table from the sakila database by executing the following command on the MySQL command-line tool.


mysql> use sakila;
Database changed
mysql> drop table actor;


**To restore the actor table, perform the following step by step process.**


**Step 1 :**


Create a dummy database named sakila_dummy and restore the backup of the sakila database on it. Following is the command.

mysql> create database sakila_dummy;
mysql> use sakila_dummy;
mysql> source C:\MySQLBackup\sakila_20200424.sql


**Step 2:**


Backup the actor table to sakila_dummy_actor_20200424.sql file. Following is the command

**C:\Users\Nisarg> mysqldump -u root -p sakila_dummy actor > C:\MySQLBackup\sakila_dummy_actor_20200424.sql**


**Step 3:**


Restore the actor table from the “sakila_dummy_actor_20200424.sql” file. Following is the command on the MySQL command-line tool.


**mysql> source C:\MySQLBackup\sakila_dummy_actor_20200424.sql**


Execute the following command to verify the table has been restored successfully.

mysql> use sakila;
Database changed
mysql> show tables;


See the following image:


![a-table-have-been-restored](https://user-images.githubusercontent.com/72725035/214866768-886a338a-ada1-4742-b32c-8143cadbba1f.png)
