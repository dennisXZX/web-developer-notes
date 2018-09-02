## Linux

#### Fundamentals

Technically speaking, Linux is a kernel, not an operating system. Linux distribution refers to a Linux kernel plus additional software. [DistroWatch](https://distrowatch.com/) has a thorough list of Linux distrubtion.

Another lesser-known family of operaing system is BSD, one of the best known BSD derivatives is [FreeBSD](https://www.freebsd.org/). macOS is also a closed source descendant of the BSD family.

- `mkdir -p /data/db` create nested directories using the `-p` flag
- `cd -` navigate to the previous working directory
- `cd ~` navigate to the home directory
- `cat error-log.txt` display a file
- `echo $PATH` display command search path, each path is separated by a colon
- `man commandName` get documentation for a command. Use `g` to jump to the top and `G` to the bottom on the documentation page.
- `which commandName` to locate the command being executed

#### Change file/folder ownership

`sudo chown <username> /data/db` to change the owner of the folder `/data/db`. You can get your username using the command `whoami`.

#### Execute the last command with sudo

```shell
[dennis@ops-server4 ~]$ cat /etc/sudoers|tail -3
cat: /etc/sudoers: Permission denied
[dennis@ops-server4 ~]$ sudo !!
sudo cat /etc/sudoers|tail -3
```

#### View running processes

The easiest way to find out what processes are running on your server is to run the `top` command.

Find out all the Node.js related process by `ps aux | grep node`.

#### curl

- get all the content of a website

```shell
curl https://domain.com
```

- save the output of the URL to a file

```shell
curl -o api/posts.json https://domain.com/apiData.json
```

- download files

```shell
// download file.zip and save it to the current working directory 
curl https://domain.com/file.zip -O

// download file.zip to the current working directory and rename it as archive.zip
curl -o archive.zip https://domain.com/file.zip -O

// download multiple files simultaneously
curl -O https://domain.com/file.zip -O https://domain.com/file2.zip

// download file securely via SSH
curl -u user sftp://server.domain.com/path/to/file
```

- get HTTP header info

```shell
curl -I http://domain.com
```

#### Show how long a command takes to run

```shell
time yarn webpack
```

#### List files in a tree structure

For MacOS, you need to install a the tree package first `brew install tree`.

```shell
tree -F
```

#### Edit hosts file

Launch Terminal and run `sudo vim /etc/hosts` to edit hosts file.

The `hosts` file is an operating system file that maps hostnames to IP addresses.

#### Exploring the system

There are some common `ls` Options 

- `-a`, list all files, including hidden files
- `-l`, list in long format
- `-F`, reveal file types, `/` for directory, `@` for symbolic link and `*` for executable
- `-r`, list in reverse order
- `-S`, sort results by file size
- `-t`, sort results by modification time
- `-latr`, display most recently changed files at the bottom

If you need to work on a file name with space, you can use quote to get around it.

```shell
ls -l 'my notes.txt'
```

When you use the `-l` format, we can see the access rights to the file. 

```shell
drwxrwxrwx  18 dennis.x  staff    612 13 Jul 14:09 books
-rwxr-xr--   1 dennis.x  staff   8398  5 Jul 11:06 ei-activity-view.css
```

For `drwxrwxrwx`, the first character indicates the file type, `d` represents a directory while a `-` indicates a regular file.

- The first three symbols `rwx` mean the file's owner may read, write, or execute
- The second three symbols `r-x` mean anyone in the file's group may read or execute this file, but not write to it
- The last three symbols `r--` mean anyone at all may read this file, but not write to it or execute its contents as a process

### Change permissions for files or directories

You can change the permission for a file using `chmod` command.

```shell
chmod 400 myfile
```

The three number represents the group of User, Group and Other.

Here is how the permission is calculated.

- 4 stands for "read",
- 2 stands for "write",
- 1 stands for "execute", and
- 0 stands for "no permission."

So 7 is the combination of permissions 4+2+1 (read, write, and execute), 5 is 4+0+1 (read, no write, and execute), and 4 is 4+0+0 (read, no write, and no execute).

![permission table](./images/linux_permission_table.png)
