## Basic NFS Commands
###### Display the NFS exports on the target server
```
$ showmount -e $target 
```
###### Mount an NFS export to a local directory
```
$ mount -t nfs $target:/export /mnt/nfs
```
###### Mount an NFS export in read-only mode
```
$ mount -t nfs -o ro $target:/export /mnt/nfs
```
###### Unmount the NFS share
```
$ umount /mnt/nfs
```
###### Creates an empty local directory to serve as the mount
```
$ mkdir target-NFS
```
###### Mounts the root directory to your local `target-NFS`
```
$ sudo mount -t nfs 10.129.14.128:/ ./target-NFS/ -o nolock
```
###### Unmounts the NFS share from your local system
```
sudo umount ./target-NFS
```
## File System Enumeration 
###### Displays the directory and file structure of the mounted NFS share
```
$ tree .
```
###### Lists the files and folders inside the share
```
$ ls -l mnt/nfs/
```
###### Lists the files and folders but displays the numerical UIDs and GUIDs instead
```
$ ls -n mnt/nfs/
```



## Config Analysis & Service Management
###### Reads the main NFS configuration file
```
$ cat /etc/exports
```
###### sharing the `/mnt/nfs` folder
```
$ echo '/mnt/nfs 10.129.14.0/24(sync,no_subtree_check)' >> /etc/exports
```
###### Restarts the NFS service
```
$ systemctl restart nfs-kernel-server
```
###### Displays the currently active exported NFS shares
```
$ exportfs
```

## Dangerous Settings
| **Option**       | **Description**                                                                                                      |
| ---------------- | -------------------------------------------------------------------------------------------------------------------- |
| `rw`             | Read and write permissions.                                                                                          |
| `insecure`       | Ports above 1024 will be used.                                                                                       |
| `nohide`         | If another file system was mounted below an exported directory, this directory is exported by its own exports entry. |
| `no_root_squash` | All files created by root are kept with the UID/GID 0.                                                               |
## Footprinting & Enumeration 
###### Scans the essential NFS ports (TCP 111 for rpcbind and TCP 2049 for NFS)
```
sudo nmap 10.129.14.128 -p111,2049 -sV -sC
```
###### Runs all Nmap scripts starting with "nfs" against the target ports
```
sudo nmap --script nfs* 10.129.14.128 -sV -p111,2049
```

