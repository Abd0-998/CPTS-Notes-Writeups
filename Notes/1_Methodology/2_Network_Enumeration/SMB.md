## Tools 
### netexec 

###### installation with pipx
```
$ sudo apt install pipx git
$ pipx ensurepath
$ pipx install git+https://github.com/Pennyw0rth/NetExec
```
###### basic installation 
```
apt update
apt install netexec
```

### smbclient
###### installation 
```
apt install smbclient
```

### Impacket Example Scripts
- smbclient.py
- lookupsids.py
- samrdump.py
```
$ pipx install impacket
```

## Default Configuration

###### Samba Config file 
```
$ cat /etc/samba/smb.conf | grep -v "#\|\;"
```
###### Restarts the SMB service daemon
```
sudo systemctl restart smbd
```

| **Setting**                    | **Description**                                                       |
| ------------------------------ | --------------------------------------------------------------------- |
| `[sharename]`                  | The name of the network share.                                        |
| `workgroup = WORKGROUP/DOMAIN` | Workgroup that will appear when clients query.                        |
| `path = /path/here/`           | The directory to which user is to be given access.                    |
| `server string = STRING`       | The string that will show up when a connection is initiated.          |
| `unix password sync = yes`     | Synchronize the UNIX password with the SMB password?                  |
| `usershare allow guests = yes` | Allow non-authenticated users to access defined share?                |
| `map to guest = bad user`      | What to do when a user login request doesn't match a valid UNIX user? |
| `browseable = yes`             | Should this share be shown in the list of available shares?           |
| `guest ok = yes`               | Allow connecting to the service without using a password?             |
| `read only = yes`              | Allow users to read files only?                                       |
| `create mask = 0700`           | What permissions need to be set for newly created files?              |
## Footprinting & Enumeration 
### Nmap Scanning
###### service and default scripts
```
$ sudo nmap 10.129.14.128 -sV -sC -p139,445
```
### Service Interaction (smbclient)
###### Lists the available shares on the target server
```
$ smbclient -N -L //10.129.14.128
```
###### Connects directly to a specific share
```
$ smbclient //10.129.14.128/notes
```
######  Lists all the possible commands
```
smb: \> help
```
###### Display the files and folders
```
smb: \> ls
```

###### Download a specific file
```
smb: \> get <filename>
```

###### Execute local system commands
```
!<cmd>
```

### Administrative Status Checking 
###### samba status
```
$ smbstatus

# Checks current connections to the Samba server, displaying the Samba version, who is connected, from which host, and to which share.
```

### RPC Enumeration (rpcclient)

###### Connects to the target via RPC (anonymous session)
```
rpcclient -U "" 10.129.14.128
```
###### RPC shell queries 
```
rpcclient $> srvinfo                   // Server information.
rpcclient $> enumdomains               // Enumerate all domains in the network
rpcclient $> querydominfo              // domain, server, and user information 
rpcclient $> netshareenumall           // Enumerates all available shares.
rpcclient $> netsharegetinfo <share>   // Information about a specific share.
rpcclient $> enumdomusers              // Enumerates all domain users
rpcclient $> queryuser <RID>           // Information about a specific user
rpcclient $> querygroup <RID>          // Information about a specific group 
```

### Brute Forcing RIDs
```
$ for i in $(seq 500 1100);do rpcclient -N -U "" 10.129.14.128 -c "queryuser 0x$(printf '%x\n' $i)" | grep "User Name\|user_rid\|group_rid" && echo "";done
```

### Automated tools 
###### samrdump
```
samrdump.py 10.129.14.128

# used as an alternative to `rpcclient` to enumerate users, domains, and groups
```

###### smbmap
```
smbmab -H 10.129.14.128

# Automates the mapping of open SMB shares and lists the permissions the current user has for each share
```

###### crackmapexec
```
crackmapexec smb 10.129.14.128 --shares -u '' -p ''

# enumerate SMB shares and their permissions using null/blank credentials.
```

###### Enum4linux-ng 
```
## install 
$ git clone [https://github.com/cddmp/enum4linux-ng.git](https://github.com/cddmp/enum4linux-ng.git)

## cd to the dir
cd enum4linux-ng

## install requirements 
pip3 install -r requirements.txt

## Runs Enum4Linux-ng with the `-A` (All) flag to automate a massive amount of SMB and RPC queries

./enum4linux-ng.py 10.129.14.128 -A

```