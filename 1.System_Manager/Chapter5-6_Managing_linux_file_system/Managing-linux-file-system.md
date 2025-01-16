# CHAPTER 5: Managing linux file system

## ext4

Working with ext4 file system

```
sudo -i
cryptsetup luksOpen /dev/loop1 my-data
dumpe2fs /dev/mapper/my-data  |grep -i "mount count"

set options when create file system or other can be set with tune2fs also

tune2fs -l /dev/mapper/my-data

tune2fs -L my-data /dev/mapper/my-data 	// set lable option for device

// you can mount to certain folder: 

mount -label=my-data /mnt/tempt

// After that you can use that disk 

// you can use tune2fs tool to see more detail infor of that ext4 file system.

```

## XFS

Big advance of XFS is defrag dis when its still running online

```
sudo -i

mkfs -t xfs /deve/loop12
xfs_info /dev/mapper/loop12	// same as tune2fs or btrfs filesystem show xfs_info wil show detail infor of xfs file system disk 

// the really cool thing of xfs file system is it can optimize or defrag space of disk while it is running or online.

xfs_fsr /mount-point

xfs_admin -L new_label /dev/loop3	// xfs_admin tool used to modify metadata of xfs file system disk 

```

## BTrfs

available on solarix system not available on redhat 
with btrfs you can add more than one disk to a mount point online. 

```
fallocate -l1G disk1
fallocate -l1G disk2
losetup -f --show disk1 		// show information of disk1
losetup -f --show disk2		// show information of disk2
mkfs -t btrfs /dev/loop0 	// right here disk1 mount to loop0
mkdir -p /mnt/sale-data
mount /dev/loop0 /mnt/sale-data
btrfs filesystem show /mnt/sale-data	//show information of /mnt/sale-data
btrfs device add /dev/loop1 /mnt/sale-data	// add more disk to /mnt/sale-data
df -h /mnt/sale-data					//checking capacity of /mnt/sale-data after add disk2


```

## Chapter 5.5: working with mount and unmount

` mount <dev> <mountpoint> / umount <dev> <mountpoint>`

you want to keep persistent mountpoint at every boot time. edit mount point int file system /etc/fstab

Mount by UUID will ensure the filesystem is uniquely identified

use ``blkid`` to determine UUID

show all mount ponit on system with ``mount`` or mount point of xfs type `` mount -t xfs`` or mount point of iso file `` mount -t iso``


# CHAPTER 6: Managing linux file system advance

Creating soft raid need "mdadm" tool
The configuration file of raid: ``/etc/mdadm.conf `` or ``/etc/mdadm/mdadm.conf``

- file sharing

`` nfs`` or the traditional unix file server

- cifs 
` 
## Chapter 6.6: Mirrored volumes using btrfs

On Open Suse, install mdadm as bellow: `zypper install -y btrfsprogs`

With btrfs you don't need partition and configuration of software raid. just using btrfs tool to create raids with raw disks.

`mkfs.btrfs -m raid1 -d raid1 /dev/loop0 /dev/loop2` then we have to mount soft disk to working directory

## Chapter 6.7: Understanding NFS

File sharing have 2 types: NFS (unix tradditional file server), CIFS (common internet file system - allow Linux share file to Windows user)

NFS version current is NFSv3 and above, NFSv2 is disabled on REL by default. NFSv4 only needing TCP port 2049 to be open.

### Configuring NFS in Alma linux

```
# nfsconf --set nfsd vers4 y
# nfsconf --set nfsd tcp y
# nfsconf --set nfsd udp n
# nfsconf --set nfsd vers4 n 
```

### Sharing directory using nfs

First of all, just sharing file using export tool by creating some .exprots file like this

```
vi /etc/exports.d/sales.exports

// Content like this: /data/sales 192.168.33.*(no_root_squash,rw, sync)

exportfs -r 	// no need to restart system, just reread the nfs file system 

exports 		// without option that used to show nfs setting

ip -4 a 		// show ip v4 information only.
```

Checking listening port on server using ss and netstat tools:

```
ss -ntl

netstat -plant
```

how to share and mount directory using nfs: 

on server ( installed nfs already) -> creating a .exports file with contetn /directory ip_v4(option) -> make sure that port and 2049 port allow on firewall
on client -> sudo mount server_ip:/directory_shared -> done

## Chapter 6.9: Working with Samba server

the process like this: Alma linux as the Samba server (create shares on this server, Create Samba User), Ubuntu as client (install CIFS  then connect to share from Ubuntu)

Edit samba server: 

```
vi /etc/samba/smb.conf

add the bottom of file the line:
[data]
	path = /data
```
after that just grant permission to user access to that directory by 
`pdbedit -a user_which_you_use`

checking with command : `ls -Zd /shared_folder`

in case selinux running and you need permission: 
`chcon -t samba_share_t -Rv /shared_folder`

on client you could mount that shared folder with command: 
`mount -o user=user_name,password=password //ip_server_share/shared_fodler /destination_folder`

## Chapter 6.10: Understanding Iscsi

on server (target) install targetcli package used to create share block on server

```
# targetcli 
/> ls 
/> backstore/fileio/ create targetdisk1 /root/tragetdisk1 1G		// Create backstore disk targetdisk1 1G 
/> iscsi/ create iqn.date.reverse_name_of_fqdn:san1					// Create target
/> ls
/ cd Iscsi/iqn.date.reverse_name_of_fqdn:san1/tpg1/
/.. /tpg1 > luns/ create /backstore/fileio/targetdisk1
/... /tpg1> acls/ create iqn.date.reverse_name_of_fqdn:san1
/... /tpg1> cd /
/> exit
```

On client (play as role of initiator)

```
sudo apt install open-iscsi
echo "InitiatorName=iqn.date.reverse_name_of_fqdn:san1" | sudo tee /etc/iscsi/initiatorName.iscsi
sudo systecmctl restart iscsi open-iscsi
sudo iscsiadm -m discovery -t sendtargets -p ip_of_target
sudo iscsiadm -m node --login
lsblk

```

## Chapter 6.11: Configuring the ISCSI target

