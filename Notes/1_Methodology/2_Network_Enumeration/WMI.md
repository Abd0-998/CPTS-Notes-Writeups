```
wmiexec.py Cry0l1t3:"P455w0rD!"@10.129.201.248 "hostname"
```

- **Usage:** Uses the Impacket script `wmiexec.py` to authenticate and execute a command (`hostname`) on the remote Windows target. The initial connection is made over TCP port 135 before moving to a random high port for command execution.