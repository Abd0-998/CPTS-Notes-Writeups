```
sudo nmap -sV -p 512,513,514 10.0.17.2
```
    
Scans the standard R-services TCP ports: 512 (rexec), 513 (rlogin), and 514 (rsh/rcp).
        
```
cat /etc/hosts.equiv
```
    
Reads the global configuration file that contains a list of trusted hosts and users for R-services.
        
```
cat .rhosts
```
    
Reads the per-user configuration file for trusted hosts. Misconfigurations here (like using the `+` wildcard) can allow attackers to authenticate without credentials.
        
```
rlogin 10.0.17.2 -l htb-student
```
    
Attempts to log in to the remote host under a specific user account (`htb-student`), which can succeed without credentials if the `.rhosts` file is misconfigured.
        
```
rwho
```
    
Lists all interactive sessions on the local network by sending requests to UDP port 513. This helps identify which users are authenticated to which hosts.
        
```
rusers -al 10.0.17.5
```
    
Provides a more detailed account of all logged-in users over the network compared to `rwho`. It displays the username, hostname, TTY, login time, idle time, and the remote host they logged in from.
