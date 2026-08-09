## Basic Syntax 

```
nc <Host-ip> <port>      // Open a TCP connection
nc -u <Host-ip> <port>   // Open a UDP connection
nc -l <port>             // Listen on a local port
nc -h                    // Show help and options
man nc                   // Read the local Netcat manual
```

## Connect and Listen

```
nc example.com 80             // Connect to TCP port 80 
nc -l 5555                    // Listen on port 5555
nc server.example.com 5555    // Connect to a listening host
nc -v host 22                 // Connect with verbose output
nc -n 192.168.1.10 22         // Skip DNS lookups
```