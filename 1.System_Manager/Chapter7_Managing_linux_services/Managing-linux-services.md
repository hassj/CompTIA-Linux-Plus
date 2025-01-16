# CHAPTER 7: Managing linux service

## Chapter 7.1: Working with service in linux

### Linux services and process management

- systemd is parent process with id 1 (PID=1)

behind systemd we have ecosystem process like: systemctl, hostnamectl, localctl, timedatectl

- Services

chrony - systemd-timesync service have same function

cron have same function with systemd-timer units

- Process management

show process with ``ps`` and ``pgrep``

kill process with ``pkill``

we can change priority of process with ``nice`` and ``renice``

you can view overview the how busy of system with ``top``

the order starting of system os: boot loader -> kernel --(loading the ) -> systemd (parent proces)

Using systemctl to manage services

```
systemctl status chronyd 	// checking status of service
systemctl enable --now chronyd	//we can both start and enable a service with enable --now option
systemctl disable --now chronyd	// same function with enable --now
systemctl cat chronyd
systemctl edit chronyd --full		// you can cat or edit a configuration file of a service without know where is service
```

you can analyze the system and show how long the service take to booting with 

```
systemd analyze
systemd analyze blame 	// show the process booting longest
```

Other command

```
hostnamectl set-hostname
timedatectl set-timezone 'Europe/London'
```

## Chapter 7.2: Linux service management demo

` systemctl status ` // show all services are running on system being managed by systemd 

## Chapter 7.3: Understanding Service units

## Chapter 7.4: Working with Service units

`partprobe` command will infor to OS table about changes occur 

to keep persistent something setup after reboot OS, you should create systemd service unit to boot that setup when reboot OS

for example about losetup.service created

```
[Unit]
Description=Setup loop device
DefaultDependencies=no
Before=local-fs.target
After=systemd-udevd.service

[Services]
Type=oneshot
ExecStart=/sbin/losetup /dev/loop1 disk1
ExecStart=/sbin/partprobe /dev/loop1

[Install]
WantedBy=local-fs.target
```

to create that service unit, doing as following order

```
systemctl edit losetup.service --full --force	// Create service unit file name losetup.service
systemctl daemon-reload 	//infor to systemd know about the changes

```

## Chapter 7.5: time service in linux

note: 

`grep -vE '^(#|$}' /etc/chronyd.conf `	// # represent for the begining of the line, $ present for the end of empty line , -v inverse, -E enhance of regular express

chronyd will be installed on server, and systemd-timesyncd will be installed on client

## Chapter 7.6: Scheduling with cron

crond: jobs at regular intervals

atd: once off, or irregular jobs

timer units: no need for additional services

on ``/etc/cron/`` folder you will see cront.deny and crontab file is being defined for task 

## Chapter 7.7: Scheduling with at

Like tmux or cron service function 

## Chapter 7.11 Manage process priorities

using nice and renice to set priorities for pocesses. Default value run from -20 to 19 

only root permision could set renice down to -20.

