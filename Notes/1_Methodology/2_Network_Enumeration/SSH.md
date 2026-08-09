## Basic Connection 

```
ssh user@host            // Connect to host 
ssh host                 // Connect with current username
ssh -p 2222 user@host    // Connect on custom port
ssh -v user@host         // Verbose mode (debug)
ssh -q user@host         // Quiet mode     
```

## Connection Options 

```
-p port          // Custom port
-i keyfile       // Identity file (private key)
-o option=value  // Set Config option 
-F configfile    // Custom config file
-J jumphost      // Jump through host (ProxyJump)
-X               // Enable X11 forwarding
```

## Footprinting and enumeration

```
cat /etc/ssh/sshd_config | grep -v "#" | sed -r '/^\s*$/d'
```
    
Reads the OpenSSH server configuration file (`sshd_config`), filtering out commented and empty lines. This is useful for identifying dangerous settings like `PasswordAuthentication yes`, `PermitEmptyPasswords yes`, or `PermitRootLogin yes`.
        
```
git clone [https://github.com/jtesta/ssh-audit.git](https://github.com/jtesta/ssh-audit.git) && cd ssh-audit
```
    
Clones and navigates into the `ssh-audit` tool directory.
        
```
./ssh-audit.py 10.129.14.132
```
    
Fingerprints the SSH server to check client-side and server-side configurations. It shows general information, the OpenSSH version, and which encryption algorithms are still used.
        
```
ssh -v cry0l1t3@10.129.14.132
```
    
Initiates an SSH connection with verbose output (`-v`). The detailed output can provide important information, such as which authentication methods the server accepts.
        
```
ssh -v cry0l1t3@10.129.14.132 -o PreferredAuthentications=password
```
    
Connects to the target SSH server while specifying the preferred authentication method as `password`. This is useful for forcing password prompts during potential brute-force attacks.
