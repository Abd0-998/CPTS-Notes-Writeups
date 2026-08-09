# Basic Commands
## Connect and login 
| Command                                                                                     | Description                                 |
| ------------------------------------------------------------------------------------------- | ------------------------------------------- |
| [`ftp hostname`](https://linuxize.com/post/how-to-use-linux-ftp-command-to-transfer-files/) | Connect to server                           |
| `ftp 192.168.1.10`                                                                          | Connect by IP                               |
| `ftp hostname 21`                                                                           | Connect to a custom port                    |
| `user username`                                                                             | Log in with username (prompts for password) |
| `user username password`                                                                    | Log in with username and password           |
| `quit`                                                                                      | Quit session                                |
| `bye`                                                                                       | Quit session (alias for `quit`)             |
| ftp://username:password@X.X.X.X                                                             | Connect Using Web Browser                   |
## local and Remote Paths  

| Command           | Description                   |
| ----------------- | ----------------------------- |
| `pwd`             | Show remote working directory |
| `!pwd`            | Show local working directory  |
| `cd /remote/path` | Change remote directory       |
| `lcd /local/path` | Change local directory        |
| `ls`              | List remote files             |
| `!ls`             | List local files              |
## Download Files
| Command                    | Description                          |
| -------------------------- | ------------------------------------ |
| `get file.txt`             | Download one file                    |
| `get remote.txt local.txt` | Download and rename locally          |
| `mget *.log`               | Download multiple files              |
| `prompt`                   | Toggle interactive prompt for `mget` |
| `reget large.iso`          | Resume interrupted download          |
## Upload Files 
| Command                    | Description                      |
| -------------------------- | -------------------------------- |
| `put file.txt`             | Upload one file                  |
| `put local.txt remote.txt` | Upload with remote name          |
| `mput *.txt`               | Upload multiple files            |
| `append file.txt`          | Append local file to remote file |
## Remote File Management
| Command                  | Description                  |
| ------------------------ | ---------------------------- |
| `mkdir dirname`          | Create remote directory      |
| `rmdir dirname`          | Remove remote directory      |
| `delete file.txt`        | Delete remote file           |
| `mdelete *.tmp`          | Delete multiple remote files |
| `rename old.txt new.txt` | Rename remote file           |
| `size file.iso`          | Show remote file size        |
## Default Configuration

###### install vsFTPd 
```
$ sudo apt install vsftpd
```

###### vsFTPd Config File
```
$ cat /etc/vsftpd.conf | grep -v "#"
```

|**Setting**|**Description**|
|---|---|
|`listen=NO`|Run from inetd or as a standalone daemon?|
|`listen_ipv6=YES`|Listen on IPv6 ?|
|`anonymous_enable=NO`|Enable Anonymous access?|
|`local_enable=YES`|Allow local users to login?|
|`dirmessage_enable=YES`|Display active directory messages when users go into certain directories?|
|`use_localtime=YES`|Use local time?|
|`xferlog_enable=YES`|Activate logging of uploads/downloads?|
|`connect_from_port_20=YES`|Connect from port 20?|
|`secure_chroot_dir=/var/run/vsftpd/empty`|Name of an empty directory|
|`pam_service_name=vsftpd`|This string is the name of the PAM service vsftpd will use.|
|`rsa_cert_file=/etc/ssl/certs/ssl-cert-snakeoil.pem`|The last three options specify the location of the RSA certificate to use for SSL encrypted connections.|
|`rsa_private_key_file=/etc/ssl/private/ssl-cert-snakeoil.key`||
|`ssl_enable=NO`||
###### FTP users 
```
$ cat /etc/ftpusers

guest 
john 
kevin
```

## Dangerous Settings 

| **Setting**                    | **Description**                                                                    |
| ------------------------------ | ---------------------------------------------------------------------------------- |
| `anonymous_enable=YES`         | Allowing anonymous login?                                                          |
| `anon_upload_enable=YES`       | Allowing anonymous to upload files?                                                |
| `anon_mkdir_write_enable=YES`  | Allowing anonymous to create new directories?                                      |
| `no_anon_password=YES`         | Do not ask anonymous for password?                                                 |
| `anon_root=/home/username/ftp` | Directory for anonymous.                                                           |
| `write_enable=YES`             | Allow the usage of FTP commands: STOR, DELE, RNFR, RNTO, MKD, RMD, APPE, and SITE? |
###### Anonymous login 
```
$ ftp 10.129.14.136

Connected to 10.129.14.136.
220 "Welcome to the HTB Academy vsFTP service."
Name (10.129.14.136:cry0l1t3): anonymous

230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.

ftp>
```

## TFTP  

#### Using TFTP Client (Linux/Unix)

```
# Interactive mode
tftp target.com
tftp> get <filename>
tftp> put <localfile>
tftp> quit

# Direct command
tftp target.com <<EOF
get config.cfg
quit
EOF
status
verbose

# Specify port (if non-standard)
tftp -p 6969 target.com

```
# Enumeration and Footprinting

## Nmap Scanning

###### Update the Nmap Scripting Engine database
```
$ sudo nmap --script-updatedb
```
###### Search for all FTP scripts 
```
$ find / -type f -name ftp* 2>/dev/null | grep scripts
```
###### Aggressive , Version , default Scripts Scan 
```
$ sudo nmap -sV -p21 -sC -A <IP>
```
###### Packet Tracing 
```
$ sudo nmap -sV -p21 -sC -A <IP> --script-trace
```

## Service interaction
###### Connect to the target 
```
$ ftp <IP>
```
###### banner grabbing
```
$ nc -nv <IP> 21
```
###### Telnet as alternative to Netcat
```
$ telnet <IP> 21
```
###### openssl connection 
```
$ openssl s_client -connect <IP>:21 -starttls ftp
```
###### Bulk Downloads
```
wget -m --no-passive ftp://anonymous:anonymous@<IP>
```
## Client commands
```
ftp> status           // connection status
ftp> debug            // Enables debugging mode
ftp> trace            // Enables packet tracing
ftp> ls               // Lists the files and directories
ftp> ls -R            // Lists files recursively
ftp> get <filename>   // Downloads a specific file
ftp> put <filename>   // Uploads a file from your local machine
```


