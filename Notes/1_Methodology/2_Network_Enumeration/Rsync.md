```
sudo nmap -sV -p 873 127.0.0.1
```
    
Scans the default Rsync port (873) using Nmap with version detection.
        
```
nc -nv 127.0.0.1 873
```
    
Probes the Rsync service using Netcat to discover accessible shares.
        
```
rsync -av --list-only rsync://127.0.0.1/dev
```
    
Enumerates the contents of a specific open share (e.g., `dev`) by listing the files without downloading them.
        
```
rsync -av rsync://127.0.0.1/dev
```
    
Syncs and pulls down all files from the target Rsync share to the local attack host.
        
```
rsync [...] -e ssh` or `-e "ssh -p2222"
```
    
These flags can be added to Rsync commands if Rsync is configured to use SSH for secure file transfers, specifying alternate ports if necessary.
