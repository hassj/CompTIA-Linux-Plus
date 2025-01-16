# Chapter 8: Managing linux boot process

## Chapter 8.1: Booting the Linux kernel

grub: runing on tradditional partion MBR, 

ramdisk: is initramfs used to load acess to file system 

the first thing that grub need to do is load the ramdisk that is load the access to file system for things that is not built-in the kernel like drivers

after ramdisk is kernel will be loaded 


## Chapter 8.2: Working with GRUB default

file /etc/default/grub

```
GRUB_TIMEOUT=5	// TIME LOADING BY default

GRUB_DISTRIBUTOR="$(sed 's, release .$,,g'/etc/sytem-release)"
GRUB_DEFAULT=saved
GRUB_DISABLE_SUBMENU=true
GRUB_TERMINAL_OUTPUT="console"
GRUB_CMDLINE_LINUX="resum==UUID=new_iD rghb quiet"
GRUB_DISABLE_RECOVERY="true"
GRUB_ENABLE_BLSCFG=true
```

to config the grub setting edit on file ``/etc/default/grub``. After edit, you need make it effective by ``grub2-mkconfig > /etc/default/grub `` to generate new grub configuration file.

## Chapter 8.3: Grubby command

through /etc/default/grub you can pass all parameters to all entries of kernel like: resume (no need on server), rhgb (no one cares on a server), quiet (this is no one is going to see)

so you can change it

```
sudo -i
cat /boot/grub2/grubenv	// all env will be passsed to booting process
cat /boot/loader/entries/< ..> .conf // here you can edit all or individual entries
grubby --update-kernel=All --remove-args="rhgb quiet"	// remove parameters against booting process
grubby --update-kernel=All --args="rhgb quiet" 			//add parameters to booting process
grubby --update-=kernel=/boot/vmlinuz-$(uname -r) --remove-args="rhbg quiet"	// add parameter to specifiic entries

```

## Chapter 8.4: When the kernel panic

the case when system cant  mount the file system since booting initramfs cannot be loaded.
you need boot to rescure mode, then recover the initramfs base on current running version of kernel
`mkinitrd --force /boot/initramfs - $(uname -r).img $(uname -r)`

