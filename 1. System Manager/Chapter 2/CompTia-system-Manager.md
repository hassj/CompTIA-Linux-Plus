# CHAPTER 2: MANAGING LINUX FILES
## Working with linux command

- See metadata of file

`ls -l $(tty)`

Explaination:
> Column 1: file type
>
> Column 2: file mode
>
> Column 3: hard link counts - number of name (how many names we are point into a files)
>
> Column 4: user ownership
> 
> Column 5: group ownership
>
> Column 6: file size in bytes>
> Column 7: last modified time 
> column 8: file name itself

Note: FHS = consistency

## Understanding timestamps

- Moving to old previous working directory on linux

` cd $OLDPWD` or `cd -`

- Display file or file system status include time of modified, access, change...

`stat file_name`

`stat -c %x file_name`	// access time
`stat -c %y file_name`	// Modify
`stat -c %z file_name`	// Change 

Note
> Access: Last read
>
> Modify: data last modified
>
> Change: Metadata last change

- Change both access and modified times, change will always update

`touch file_naem`

`touch -a file_name`	// changes access time
`touch -m file_name`	// changes modify time

- Copy file with preserve permison, ownership and timestamps with --archive mode

`cp -a source_file dest_file`

## Stream editor sed

```
# sudo sshd -T | grep passwordauthentication
# sudo sed -Ei 's/(passwordauthentication) no/\1 yes/' /etc/ssh/sshd_config
# systemctl restart sshd
```

## Understanding remote Copy


`find /var/www/html -name '*.html' -exec cp {} docs/ \;`
> by default option of find command is -print
>
> {} represent for all files found
>
> \; indicate that the end of subcommand instead of special character in bash. using it without "\" will treat as special character in bash shell.

whilst scp is good for copying single file to remote system, rsync is better equipped to copy complete directory structures and keep them up to data.

`#rsync -ave ssh source_folder des_folder`
> -a: archive mode
>
> -v: verbose
> 
> -e: external shell, specific remote shell to use

## Using git as remote VCS

you can initialize git server on a bare system instead of use public vcs such as github, gitlabt, etc...

on server

`git init --bare`

on client

` git clone user@server_ip/project`

or push code to the server

`git config --global user.email "abc@example.com"`
`git config --global user.name "abc"`
` git push origin master`

On opensuse distribution must install git-core package for using git

`sudo zypper in -y git-core`

# CHAPTER 3: BACKUP AND COMPRESSION FILE IN LINUX


