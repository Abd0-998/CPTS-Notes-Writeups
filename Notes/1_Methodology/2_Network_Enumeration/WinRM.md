```
nmap -sV -sC 10.129.201.248 -p5985,5986 --disable-arp-ping -n
```
    
Scans the default WinRM ports for HTTP (5985) and HTTPS (5986) to detect the service version and gather basic header information.
        
```
evil-winrm -i 10.129.201.248 -u Cry0l1t3 -p P455w0rD!
```
    
Uses the `evil-winrm` tool to establish a remote command-line session (PowerShell) on the target host using valid credentials.
