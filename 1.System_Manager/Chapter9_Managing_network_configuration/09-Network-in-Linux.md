# Chapter 9: Managing Linux Network Configuration

## Chapter 9.1: Networking in Linux

Using Ip command

Networking Linux

- RHEL (Alma): Network manager

- OpenSuSe: wicked

- ubuntu: netplan

Hostname Resolution:

- dig

- /etc/nsswitch.conf	/configuration file

- DNS entries

Monitor network traffic

note: with different distribution of LINUX you have specific tools to manage network. so for easily managing linux network, you should use ip command

## Chapter 9.2: Ip command

common command of ip command using on Alma

```
ip n	// like arp cache

ip r	/route table

ip a show eth0 
```

On ubuntu:

```
netplan ip release esnp3	//dhcp release

```
all of network defined file residen underneath /etc/netplan folder

you can try with new configuration for a period of time `netplan try`, after that every thing will turn to normal as before. when you happy with new configuration you can apply with 

`netplan apply`

## Chapter 9.5: Using networkmanager in RHEL

- networkmanager

using ``nmcli`` command to show netowrk connection

in case you want to modify in backend, just edit on file ``/etc/sysconfig/network-scripts/ifcfg-enp0s8``

In case you want to modify on both backedn and frontend, just using ``nmcli`` command 

```
nmcli connection modify System enp0s8 ipv4.address 192.168.101.12/24, 172.16.8.19/16
nmcli connection up System\ enps08
nmcli connection down 

```

common command

`` ip a s ``	//show ip address

`` ip a d 172.16.x.x/16 dev name_of_interface``		//delete ip address

## Chapter 9.6: Managing correct hosts order

`ss -ntl` //checking port open using socket

the order hosts reference is local host file then dns system

you can edit the above order in file ``/etc/nsswitch`` file

note: `!t` it means last command begin with t letter

## Chapter 9.7: Dig command in Linux

Dig command used to diagnose dns issues.

## Chapter 9.8: Managing DNS Name server 

you can specify the DNS server using file

`vi +/NETCONFIG_DNS_STATIC_SERVERS /etc/sysconfig/networkconfig `

after edit the above file you should udpate using this command

`netconfig update -f`





