## Nmap Scanning & Footprinting
###### Scans the standard ports for POP3 (110), IMAP (143), IMAPS over TLS (993), and POP3S over TLS (995)
```
sudo nmap 10.129.14.128 -sV -p110,143,993,995 -sC
```
## Service interaction (cURL)
###### Connects to the secure IMAP service
```
curl -k 'imaps://10.129.14.128' --user user:p4ssw0rd
```
###### Uses the verbose flag (`-v`) during connection to reveal the TLS handshake
```
curl -k 'imaps://10.129.14.128' --user cry0l1t3:1234 -v
```

## Service Interaction (openSSL for Encrypted Ports)
###### Establishes a manual, encrypted TLS connection to the POP3S service
```
openssl s_client -connect 10.129.14.128:pop3s
```
###### Establishes a manual, encrypted TLS connection to the IMAPS service
```
openssl s_client -connect 10.129.14.128:imaps
```

## IMAP Shell Commands (Inside the IMAP Session)

- (Note: IMAP commands often require a sequence tag or identifier at the beginning, usually a number like `1` or a letter combination, to match requests with server responses).
###### Authenticates the user to the IMAP server
```
1 LOGIN username password
```
###### Lists all directories/mailboxes available to the user.
```
1 LIST "" *
```
###### Creates a new mailbox with the specified name.
```
1 CREATE "INBOX"
```
###### Deletes a specified mailbox
```
1 DELETE "INBOX"
```
###### Renames an existing mailbox.
```
1 RENAME "ToRead" "Important"
```
###### Returns a subset of mailbox names that the user has declared as active or subscribed.
```
1 LSUB "" *
```
###### Selects a specific mailbox so that the messages inside it can be accessed or read.
```
1 SELECT INBOX
```
###### Exits the currently selected mailbox.
```
1 FETCH <ID> all
```
###### Retrieves the data associated with a specific message in the selected mailbox using its ID.
```
1 FETCH <ID> all
```
###### Permanently removes all messages in the mailbox that have the "Deleted" flag set.
```
1 CLOSE
```
###### Closes the connection with the IMAP server.
```
1 LOGOUT
```


## POP3 Shell Commands (Inside the POP3 Session)

```
USER username
```
    
    - Submits the username for identification to the server.
        
```
PASS password
```
    
    - Authenticates the identified user using their password.
        
```
STAT
```
    
    - Requests the number of saved emails and their total size from the server.
        
```
LIST
```
    
    - Requests all emails, including their specific ID numbers and sizes.
        
```
RETR id
```
    
    - (reads) the contents of a specific email using its assigned ID number.
        
```
DELE id
```
    
    - Marks a specific email to be deleted from the server using its ID.
        
```
CAPA
```
    
    - Requests the server to display its capabilities and supported extensions.
        
```
RSET
```
    
    - Resets the session and clears any actions that haven't been committed yet.
        
```
QUIT
```
    
    - Closes connection with the POP3 server and commits any pending deletions.
