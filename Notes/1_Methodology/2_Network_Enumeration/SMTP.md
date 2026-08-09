## Nmap Scanning & Footprinting
###### Scans the standard SMTP port (25), running default scripts and version detection
```
sudo nmap 10.129.14.128 -sC -sV -p25
```
###### determine if the SMTP server is vulnerable to an "Open Relay" attack
```
sudo nmap 10.129.14.128 -p25 --script smtp-open-relay -v
```
## Service Interaction & Manual Enumeration
###### TCP connection to the SMTP server
```
$ telnet 10.129.14.128 25
```
###### Initializes the session and introduces your client to the server
```
$ HELO <hostname>
$ EHLO <hostname>
```
###### verify if a specific username exists on the system
```
$ VRFY <username>
```

## Sending an Email via CLI

- Once connected via Telnet, you can test for spoofing or open relays by manually sending an email using the following sequence :
###### Sets the sender's email address
```
MAIL FROM: <cry0l1t3@inlanefreight.htb>
```
###### Sets the recipient's email address.
```
RCPT TO: <mrb3n@inlanefreight.htb> NOTIFY=success,failure
```
###### Tells the server you are ready to type the actual email content
```
DATA
```
###### tells the server that you have finished typing the email data
```
.
```
###### Terminates the SMTP session.
```
QUIT
```

## Config Analysis
###### Reads the Postfix SMTP configuration file
```
cat /etc/postfix/main.cf | grep -v "#" | sed -r "/^\s*$/d"
```

- Dangerous Setting to look for:_ If you see `mynetworks = 0.0.0.0/0`
