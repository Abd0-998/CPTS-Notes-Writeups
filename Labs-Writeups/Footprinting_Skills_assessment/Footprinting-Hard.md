## Given data 

## step by step 

- as usual i started with the nmap scan 

```
sudo nmap -sC -sV -p- 10.129.202.20

Starting Nmap 7.95 ( https://nmap.org ) at 2026-08-07 20:11 EDT
Nmap scan report for 10.129.202.20
Host is up (0.051s latency).
Not shown: 65530 closed tcp ports (reset)
PORT    STATE SERVICE  VERSION
22/tcp  open  ssh      OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 3f:4c:8f:10:f1:ae:be:cd:31:24:7c:a1:4e:ab:84:6d (RSA)
|   256 7b:30:37:67:50:b9:ad:91:c0:8f:f7:02:78:3b:7c:02 (ECDSA)
|_  256 88:9e:0e:07:fe:ca:d0:5c:60:ab:cf:10:99:cd:6c:a7 (ED25519)
110/tcp open  pop3     Dovecot pop3d
|_pop3-capabilities: STLS AUTH-RESP-CODE PIPELINING RESP-CODES UIDL SASL(PLAIN) CAPA TOP USER
| ssl-cert: Subject: commonName=NIXHARD
| Subject Alternative Name: DNS:NIXHARD
| Not valid before: 2021-11-10T01:30:25
|_Not valid after:  2031-11-08T01:30:25
|_ssl-date: TLS randomness does not represent time
143/tcp open  imap     Dovecot imapd (Ubuntu)
|_imap-capabilities: IDLE have LITERAL+ LOGIN-REFERRALS listed ENABLE STARTTLS capabilities Pre-login more OK SASL-IR post-login AUTH=PLAINA0001 IMAP4rev1 ID
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=NIXHARD
| Subject Alternative Name: DNS:NIXHARD
| Not valid before: 2021-11-10T01:30:25
|_Not valid after:  2031-11-08T01:30:25
993/tcp open  ssl/imap Dovecot imapd (Ubuntu)
| ssl-cert: Subject: commonName=NIXHARD
| Subject Alternative Name: DNS:NIXHARD
| Not valid before: 2021-11-10T01:30:25
|_Not valid after:  2031-11-08T01:30:25
|_ssl-date: TLS randomness does not represent time
|_imap-capabilities: Pre-login LITERAL+ have LOGIN-REFERRALS ENABLE IDLE listed capabilities more OK SASL-IR post-login AUTH=PLAINA0001 IMAP4rev1 ID
995/tcp open  ssl/pop3 Dovecot pop3d
|_pop3-capabilities: RESP-CODES SASL(PLAIN) USER UIDL PIPELINING AUTH-RESP-CODE TOP CAPA
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=NIXHARD
| Subject Alternative Name: DNS:NIXHARD
| Not valid before: 2021-11-10T01:30:25
|_Not valid after:  2031-11-08T01:30:25
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 56.64 seconds
```

- the nmap reveals the mail services : pop3 , imap and there SSL based versions and ssh
- after trying the possible attacks that i could do on these protocols which are password bruteforcing and password reuse and all of them failed , i decided to scan the target again with the flag -sU to scan UDP ports 
- 


