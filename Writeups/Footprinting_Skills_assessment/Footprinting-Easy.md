## Given Info

We were commissioned by the company `Inlanefreight Ltd` to test three different servers in their internal network. The company uses many different services, and the IT security department felt that a penetration test was necessary to gain insight into their overall security posture.

The first server is an internal DNS server that needs to be investigated. In particular, our client wants to know what information we can get out of these services and how this information could be used against its infrastructure. Our goal is to gather as much information as possible about the server and find ways to use that information against the company. However, our client has made it clear that it is forbidden to attack the services aggressively using exploits, as these services are in production.

Additionally, our teammates have found the following credentials "ceil:*****", and they pointed out that some of the company's employees were talking about SSH keys on a forum.

The administrators have stored a `flag.txt` file on this server to track our progress and measure success. Fully enumerate the target and submit the contents of this file as proof.


## Step by Step

- first i used nmap to figure out what services are running on this server

```
sudo nmap -sC -sV 10.129.42.195

Starting Nmap 7.95 ( https://nmap.org ) at 2026-08-01 08:35 HDT
Stats: 0:04:10 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 90.62% done; ETC: 08:39 (0:00:05 remaining)
Nmap scan report for 10.129.42.195
Host is up (0.25s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp?
| fingerprint-strings: 
|   GenericLines: 
|     220 ProFTPD Server (ftp.int.inlanefreight.htb) [10.129.42.195]
|     Invalid command: try being more creative
|_    Invalid command: try being more creative
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)

53/tcp   open  domain  ISC BIND 9.16.1 (Ubuntu Linux)
| dns-nsid: 
|_  bind.version: 9.16.1-Ubuntu
2121/tcp open  ftp
| fingerprint-strings: 
|   GenericLines: 
|     220 ProFTPD Server (Ceil's FTP) [10.129.42.195]
|     Invalid command: try being more creative
|_    Invalid command: try being more creative
2 services unrecognized despite returning data. If you know the service/version, please submit the following fingerprints at https://nmap.org/cgi-bin/submit.cgi?new-service :
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port21-TCP:V=7.95%I=7%D=8/1%Time=6A6E2E5D%P=x86_64-pc-linux-gnu%r(Gener
SF:icLines,9C,"220\x20ProFTPD\x20Server\x20\(ftp\.int\.inlanefreight\.htb\
SF:)\x20\[10\.129\.42\.195\]\r\n500\x20Invalid\x20command:\x20try\x20being
SF:\x20more\x20creative\r\n500\x20Invalid\x20command:\x20try\x20being\x20m
SF:ore\x20creative\r\n");
==============NEXT SERVICE FINGERPRINT (SUBMIT INDIVIDUALLY)==============
SF-Port2121-TCP:V=7.95%I=7%D=8/1%Time=6A6E2E5D%P=x86_64-pc-linux-gnu%r(Gen
SF:ericLines,8D,"220\x20ProFTPD\x20Server\x20\(Ceil's\x20FTP\)\x20\[10\.12
SF:9\.42\.195\]\r\n500\x20Invalid\x20command:\x20try\x20being\x20more\x20c
SF:reative\r\n500\x20Invalid\x20command:\x20try\x20being\x20more\x20creati
SF:ve\r\n");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 261.29 seconds
```

- port 21 ftp : the banner revealed to us a important subdomain name : ftp.int.inlanefreight.htb
- port 53 DNS : the service is bind which will help us to gain more info about the target network
- port 2121 : the banner reveals the name of the server ceil’s FTP
- The next step i used the given creds : ceil:***** to login into the FTP server :

```
$ftp 10.129.123.254 2121 
Connected to 10.129.123.254. 
220 ProFTPD Server (Ceil's FTP) [10.129.123.254]
Name (10.129.123.254:user): ceil 
331 Password required for ceil 
Password: 
230 User ceil logged in 
Remote system type is UNIX. 
Using binary mode to transfer files. 
ftp>
```

- then i listed all the directories in this server :

```
ftp> ls -la
229 Entering Extended Passive Mode (|||37838|)
150 Opening ASCII mode data connection for file list
drwxr-xr-x   4 ceil     ceil         4096 Nov 10  2021 .
drwxr-xr-x   4 ceil     ceil         4096 Nov 10  2021 ..
-rw-------   1 ceil     ceil          294 Nov 10  2021 .bash_history
-rw-r--r--   1 ceil     ceil          220 Nov 10  2021 .bash_logout
-rw-r--r--   1 ceil     ceil         3771 Nov 10  2021 .bashrc
drwx------   2 ceil     ceil         4096 Nov 10  2021 .cache
-rw-r--r--   1 ceil     ceil          807 Nov 10  2021 .profile
drwx------   2 ceil     ceil         4096 Nov 10  2021 .ssh
-rw-------   1 ceil     ceil          759 Nov 10  2021 .viminfo
226 Transfer complete
ftp>
```

- we can see the hidden directory called ssh , lets see what it have

```
ftp> cd .ssh
250 CWD command successful
ftp> ls -la
229 Entering Extended Passive Mode (|||63059|)
150 Opening ASCII mode data connection for file list
drwx------   2 ceil     ceil         4096 Nov 10  2021 .
drwxr-xr-x   4 ceil     ceil         4096 Nov 10  2021 ..
-rw-rw-r--   1 ceil     ceil          738 Nov 10  2021 authorized_keys
-rw-------   1 ceil     ceil         3381 Nov 10  2021 id_rsa
-rw-r--r--   1 ceil     ceil          738 Nov 10  2021 id_rsa.pub
226 Transfer complete
ftp>
```

- the next step is to get the id_rsa file to use it to ssh the server

```
ftp> get id_rsa
local: id_rsa remote: id_rsa
229 Entering Extended Passive Mode (|||36867|)
150 Opening BINARY mode data connection for id_rsa (3381 bytes)
100% |***********************************************************************|  3381       19.23 KiB/s    00:00 ETA
226 Transfer complete
3381 bytes received in 00:00 (6.77 KiB/s)
ftp>
```

- to use the ssh file we have to change the permissions of the ssh key to be able to use it

```
chmod 600 id_rsa
```

- then we can use this key to connect to the server

```
ssh -i id_rsa ceil@10.129.42.195
```

```
ceil@NIXEASY:~$ ls -la
total 36
drwxr-xr-x 4 ceil ceil 4096 Nov 10  2021 .
drwxr-xr-x 5 root root 4096 Nov 10  2021 ..
-rw------- 1 ceil ceil  294 Nov 10  2021 .bash_history
-rw-r--r-- 1 ceil ceil  220 Nov 10  2021 .bash_logout
-rw-r--r-- 1 ceil ceil 3771 Nov 10  2021 .bashrc
drwx------ 2 ceil ceil 4096 Nov 10  2021 .cache
-rw-r--r-- 1 ceil ceil  807 Nov 10  2021 .profile
drwx------ 2 ceil ceil 4096 Nov 10  2021 .ssh
-rw------- 1 ceil ceil  759 Nov 10  2021 .viminfo
ceil@NIXEASY:~$
```

- there was no directory containing the flag , so i used a simple bash command to find the flag

```
find / -type f -name flag.txt 2>/dev/null
```

```
ceil@NIXEASY:~$ find / -type f -name flag.txt 2>/dev/null
/home/flag/flag.txt
ceil@NIXEASY:~$ cd /home/flag
ceil@NIXEASY:/home/flag$ ls
flag.txt
ceil@NIXEASY:/home/flag$ cat flag.txt
HTB{}
```



