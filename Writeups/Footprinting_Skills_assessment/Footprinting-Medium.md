## Given Data 

This second server is a server that everyone on the internal network has access to. In our discussion with our client, we pointed out that these servers are often one of the main targets for attackers and that this server should be added to the scope.

Our customer agreed to this and added this server to our scope. Here, too, the goal remains the same. We need to find out as much information as possible about this server and find ways to use it against the server itself. For the proof and protection of customer data, a user named `HTB` has been created. Accordingly, we need to obtain the credentials of this user as proof.

hint : In SQL Management Studio, we can edit the last 200 entries of the selected database and read the entries accordingly. We also need to keep in mind, that each Windows system has an Administrator account.

## step by step 

- first i started with scanning the server using nmap 

```
$ nmap -sC -sV -p- 10.129.202.41

Starting Nmap 7.95 ( https://nmap.org ) at 2026-08-07 18:59 EDT
Stats: 0:00:03 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 10.81% done; ETC: 18:59 (0:00:33 remaining)
Nmap scan report for 10.129.202.41
Host is up (0.052s latency).
Not shown: 65519 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
111/tcp   open  rpcbind       2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/tcp6  rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  2,3,4        111/udp6  rpcbind
|   100003  2,3         2049/udp   nfs
|   100003  2,3         2049/udp6  nfs
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100005  1,2,3       2049/tcp   mountd
|   100005  1,2,3       2049/tcp6  mountd
|   100005  1,2,3       2049/udp   mountd
|   100005  1,2,3       2049/udp6  mountd
|   100021  1,2,3,4     2049/tcp   nlockmgr
|   100021  1,2,3,4     2049/tcp6  nlockmgr
|   100021  1,2,3,4     2049/udp   nlockmgr
|   100021  1,2,3,4     2049/udp6  nlockmgr
|   100024  1           2049/tcp   status
|   100024  1           2049/tcp6  status
|   100024  1           2049/udp   status
|_  100024  1           2049/udp6  status
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
2049/tcp  open  nlockmgr      1-4 (RPC #100021)
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=WINMEDIUM
| Not valid before: 2026-08-06T22:34:22
|_Not valid after:  2027-02-05T22:34:22
| rdp-ntlm-info: 
|   Target_Name: WINMEDIUM
|   NetBIOS_Domain_Name: WINMEDIUM
|   NetBIOS_Computer_Name: WINMEDIUM
|   DNS_Domain_Name: WINMEDIUM
|   DNS_Computer_Name: WINMEDIUM
|   Product_Version: 10.0.17763
|_  System_Time: 2026-08-07T23:00:31+00:00
|_ssl-date: 2026-08-07T23:00:38+00:00; 0s from scanner time.
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49679/tcp open  msrpc         Microsoft Windows RPC
49680/tcp open  msrpc         Microsoft Windows RPC
49681/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-08-07T23:00:31
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 120.58 seconds

```

- the main services running on this server as we see are : SMB , MS-SQL , RDP , and NFS 
- lets enumerate the NFS to see what are the existing directories hosted on its shares
```
showmount -e 10.129.202.41
Export list for 10.129.202.41:
/TechSupport (everyone)
```

- there is a directory called TechSupport as we see , lets make a mount directory on our local machine to be able to navigate the shared dir files

```
mkdir Mount
sudo mount -t nfs 10.129.202.41:/TechSupport /home/htb-ac/Mount
```

- now when we cd to the mounted dir we can see the techsupport content 

```
cd /Mount
ls -la

total 72
        0 Nov 10  2021 ticket4238791283775.txt
-rwx------  1 nobody         nogroup            0 Nov 10  2021 ticket4238791283776.txt
-rwx------  1 nobody         nogroup            0 Nov 10  2021 ticket4238791283777.txt
-rwx------  1 nobody         nogroup            0 Nov 10  2021 ticket4238791283778.txt
-rwx------  1 nobody         nogroup            0 Nov 10  2021 ticket4238791283779.txt
-rwx------  1 nobody         nogroup            0 Nov 10  2021 ticket4238791283780.txt
-rwx------  1 nobody         nogroup            0 Nov 10  2021 ticket4238791283781.txt
-rwx------  1 nobody         nogroup         1305 Nov 10  2021 ticket4238791283782.txt
-rwx------  1 nobody         nogroup            0 Nov 10  2021
```

- as we can see the only file that has different size is : ticket4238791283781.txt

- when i cat the content of the file this what i have found : 

```
cat ticket4238791283782.txt
Conversation with InlaneFreight Ltd

Started on November 10, 2021 at 01:27 PM London time GMT (GMT+0200)
---
01:27 PM | Operator: Hello,. 
 
So what brings you here today?
01:27 PM | alex: hello
01:27 PM | Operator: Hey alex!
01:27 PM | Operator: What do you need help with?
01:36 PM | alex: I run into an issue with the web config file on the system for the smtp server. do you mind to take a look at the config?
01:38 PM | Operator: Of course
01:42 PM | alex: here it is:

 1smtp {
 2    host=smtp.web.dev.inlanefreight.htb
 3    #port=25
 4    ssl=true
 5    user="alex"
 6    password="[REDACTED FOR HTB POLICES]"
 7    from="alex.g@web.dev.inlanefreight.htb"
 8}
 9
10securesocial {
11    
12    onLoginGoTo=/
13    onLogoutGoTo=/login
14    ssl=false
15    
16    userpass {      
17    	withUserNameSupport=false
18    	sendWelcomeEmail=true
19    	enableGravatarSupport=true
20    	signupSkipLogin=true
21    	tokenDuration=60
22    	tokenDeleteInterval=5
23    	minimumPasswordLength=8
24    	enableTokenJob=true
25    	hasher=bcrypt
26	}
27
28     cookie {
29     #       name=id
30     #       path=/login
31     #       domain="10.129.2.59:9500"
32            httpOnly=true
33            makeTransient=false
34            absoluteTimeoutInMinutes=1440
35            idleTimeoutInMinutes=1440
36    }

```

- on the lines 5,4 we can see that there is a username : alex and password : [REDACTED FOR HTB POLICES]

- lets try to enumerate the SMB service using the creds we found 

```
smbclient //10.129.202.41/devshare -U alex
Password for [WORKGROUP\alex]:
Try "help" to get a list of possible commands.
smb: \> 
```

- as we can see we got access on the SMB service using the username and password we have found 

- lets see what files are on this server 

```
smb: \> ls 
  .                                   D        0  Wed Nov 10 11:12:22 
  ..                                  D        0  Wed Nov 10 11:12:22 
  important.txt                       A       16  Wed Nov 10 11:12:55 

 10328063 blocks of size 4096. 6100710 blocks available
```

- we found a file called important.txt 
- lets open it and see what does it have 

```
smb: \> get important.txt 
getting file \important.txt of size 16 as important.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \> !cat important.txt 
sa:[REDACTED FOR HTB POLICES] smb: \> 
```

- the file contains a username : sa and password : [REDACTED FOR HTB POLICES]

- the sa is the username of the system admin in the SSMS database in windows 

- lets try to connect to the windows server using RDP with the same password we have found 

- i used remmina tool with this creds : username : Administrator and password : [REDACTED FOR HTB POLICES]

<img width="1063" height="701" alt="image" src="https://github.com/user-attachments/assets/7cb2c22e-cabd-4376-a7dd-8e2ef3d0047d" />


- then i opened the SMSS and used windows authentication : WINMEDIUM

<img width="643" height="429" alt="image" src="https://github.com/user-attachments/assets/f53685c3-4201-4b88-8ed1-4d6d59a65afa" />


- then in the SMSS follow this path : Databases --> accounts --> Tables 

- then right-click on dbo.devsacc and choose : select top 200 Rows and search for the user called HTB and submit his password as the answer 

