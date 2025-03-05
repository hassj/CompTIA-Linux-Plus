# CHAPTER 10: MANAGING LINUX SYSTEM

## Chapter 10.1: MANAGING package in linux distributioin

## Chapter 10.2: MANAGING package in linux ubuntu

you see the content of the file in which defined where is package installed from

`cat /etc/apt/source.list`

`grep -vE ^(#|$) /etc/apt/sources.list`

in case you want to check the age of cache to see the time of udpated package using stat command 

`stat -c %z /var/lib/apt/periodic/update-success-stamp`

you can check the available package to update using command `apt --upgradable` then you can update with that command 

```
apt upgrade	// upgrade all available package

apt upgrade package_name 	// update specifi package

```

## Chapter 10.3: MANAGING package in linux Open suse

`ls /etc/zypp/repos.d`	// list all repo in which you can install software from on Open suse

## Chapter 10.4: MANAGING package in linux Rhel

`yum repolist`	// list all repo on Rhel

## Chapter 10.5 Understanding Installing from source

that means download compiled source code then customize as you want then install 

```
git clone 	// download source code from source

./configure 	// make configuration file
make 			//compile source code
make install 	//install package

```

## Chapter 10.7: Understanding Pythong virtual environment

```
sudo apt install python3-virtualenv
mkdir python
cd python
virtualenv ansible 		//named for environment which you want to create
source ansible/bin/active	//active the environment

```

Note: 

find package start and ending with ansible `apt search '^ansible$' `

