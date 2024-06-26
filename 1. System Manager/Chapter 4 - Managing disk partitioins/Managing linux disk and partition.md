# Chapter 4: Managing linux file and partitions
Linux can also makes remote shared storage as disk space (SANS) or file system such as NFS, CIFS

- list storage (block) devices

`lsblk`

- List devices on pci 

`lspci`

`lscpu`

`lsusb`

## Chapter 4.2 Understanding disk files and working with disk file
virtual disks: 

- ISO: files are virtual disk with file system and data are added. these can be mounted directly to mount point

- Disk files: we can create new raw disk with dd or fallocate tools. these files are linked to devices with losetup

`dd if=/dev/zero of=/disk1.img bs=1M count=1000`

`fallocate -l 1G disk2.img`

- Using ``losetup`` command to link loop disk to devices

`sudo losetup /dev/loop<number> disk2.img`

or go straigh away with below command:

`sudo losetup -f --show disk<available.img`

## Chapter 4.3: Understanding disk partitions

### Partition labels
- MSDOS: supprot to 4 primary partitions with maximum of 2TB capacity

- GPT:  support to 128 primary partitions with maximum of 32EB

### Partitioning disks

- fdisk tools has been used traditionally. It is interactive and can not be scripted

- gparted: tool has been used with interactive and can be scripted

A completed disk can be used without disk partitions. 

```
sudo parted /dev/loop1 mklabel msdos mkpart primary 0% 50%
sudo parted /dev/loop1 mkpart primary 50% 100%
sudo parted /dev/loop1 print
```
- Inform the OS of partitions

when the hardware detected the partition table is read into OS. The command ``parted`` will also inform to OS the changes about partition added.
Adding a parttion to active disk need to inform to OS the changes. we can check the partitions table with ``-d `` 

`sudo partprobe -ds /dev/loop12`

result in ok, you can run above command withou -d flag to inform to OS know about the changes.
after that you can check the appearing of block devices which has created with command: ``lsblk``

### Encrypting partitions
Adding adititonal security layer, by encrypting disk partition before file system is added
for example bellows: 

```
sudo cryptsetup luksFormat --type luks2 /dev/loop12p3		// encrypted /dev/loop12p3 disk partition
sudo cryptsetup luksOpen /dev/loop12p3 example_name			// then open encrypted disk for use
sudo cryptsetup -v status /dev/mapper/example_name			// check status
sudo dd if=/dev/zero of=/dev/mapper/example_name status=progess	// write data to that disk Partition
sudo mkfs.ext4 /dev/mapper/example_name							// add file system 
sudo cryptsetup luksClose example_name						// close disk after use.
```

