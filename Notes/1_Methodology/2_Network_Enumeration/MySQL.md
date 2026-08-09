## Config Analysis & Installation (Local Testing)

###### Installs the MySQL server package
```
sudo apt install mysql-server -y
```
###### Reads the primary MySQL configuration file (`mysqld.cnf`)        
```
cat /etc/mysql/mysql.conf.d/mysqld.cnf | grep -v "#" | sed -r '/^\s*$/d'
```
## Nmap Scanning & Footprinting

###### Scans the default MySQL TCP port (3306) with version detection (`-sV`) and default scripts (`-sC`). By appending `--script mysql*`, it executes all MySQL-specific NSE scripts to aggressively enumerate valid usernames, check for empty passwords (e.g., root without a password), and extract server capabilities.
```
sudo nmap 10.129.14.128 -sV -sC -p3306 --script mysql*
```
## Service Interaction & Connection

###### Attempts to connect to a remote MySQL server as the `root` user without providing a password (used to manually verify if an empty password vulnerability actually exists).
```
mysql -u root -h 10.129.14.132
```
###### Connects to the remote MySQL server using a specific username and password. **Crucial:** There must be absolutely no space between the `-p` flag and the password itself.     
```
mysql -u <user> -p<password> -h <IP address>
```
## Database Enumeration (Inside the MySQL Shell)

###### Lists all available databases on the server (look for default ones like `mysql`, `sys`, and `information_schema`, as well as custom application databases).
```
show databases;
```
###### Queries the database engine to explicitly return the current MySQL/MariaDB version       
```
select version();
```
###### Selects and switches your active session to a specific database so you can interact with its tables.        
```
use <database>;
```
###### Displays all available tables within the currently selected database.
```
show tables;
```
###### Displays the structure, data types, and all column names for a specific table.        
```
show columns from <table>;
``` 
## Data Extraction (SQL Queries)

###### Dumps and displays all data (every row and every column) contained within the specified table.

```
select * from <table>;
```
###### Searches for and extracts specific data. It filters the table to return only the rows where the specified column exactly matches your target string.     
```
select * from <table> where <column> = "<string>";
```
###### A specific query example used to extract only the `host` and `unique_users` columns from the `host_summary` table (often found in the `sys` database).
        
```
select host, unique_users from host_summary;
```
