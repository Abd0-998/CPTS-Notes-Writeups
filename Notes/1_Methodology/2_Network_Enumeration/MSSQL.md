## Nmap Scanning & Footprinting

```
sudo nmap --script ms-sql-info,ms-sql-empty-password,ms-sql-xp-cmdshell,ms-sql-config,ms-sql-ntlm-info,ms-sql-tables,ms-sql-hasdbaccess,ms-sql-dac,ms-sql-dump-hashes --script-args mssql.instance-port=1433,mssql.username=sa,mssql.password=,mssql.instance-name=MSSQLSERVER -sV -p 1433 10.129.201.248
```
###### A comprehensive Nmap scan targeting the default MSSQL port (1433). It runs multiple specialized NSE scripts to extract the hostname, instance name, software version, NTLM info, enabled named pipes, and tests for empty passwords on the default `sa` (system administrator) account.
## Metasploit Enumeration

```
msfconsole
```
    
###### Launches the Metasploit Framework.
        
```
use auxiliary/scanner/mssql/mssql_ping
```
    
###### Selects the `mssql_ping` auxiliary module, which broadcasts a request to discover MSSQL instances and extract detailed server information.
        
```
set rhosts 10.129.201.248
```
    
###### Sets the target IP address for the Metasploit scanner.
        
```
run
```
    
###### Executes the scanner to retrieve the ServerName, InstanceName, Version, TCP port, and Named Pipes (e.g., `\\SQL-01\pipe\sql\query`).

## Client Setup & Connection (Impacket)

```
locate mssqlclient
```
    
###### Searches your local Linux system for the path to Impacket's `mssqlclient.py` script, which is one of the most reliable command-line clients for pentesting MSSQL.
        
```
python3 mssqlclient.py Administrator@10.129.201.248 -windows-auth
```
    
###### Connects to the remote MSSQL server using Windows Authentication. This forces the underlying Windows OS/Active Directory to process the login request instead of native SQL authentication.

## Database Enumeration (Inside the T-SQL Shell)

```
select name from sys.databases
```

###### A Transact-SQL (T-SQL) query executed inside the `mssqlclient` shell to list all databases present on the server. Look for default system databases (`master`, `model`, `msdb`, `tempdb`) alongside custom application databases.

## Config Analysis & Dangerous Settings (Checklist)

_(Keep an eye out for these misconfigurations during enumeration or post-exploitation)_

- **Weak `sa` Credentials:** Admins often forget to disable or set strong passwords for the default `sa` (system administrator) account.
    
- **No Encryption:** MSSQL clients not enforcing encryption when connecting to the server, allowing traffic sniffing.
    
- **Self-Signed Certificates:** The use of self-signed certificates for encryption, which can be easily spoofed for Man-in-the-Middle (MitM) attacks.
    
- **Named Pipes Enabled:** Allows local or authenticated network access to the database engine via SMB/RPC, which can be leveraged for privilege escalation.
