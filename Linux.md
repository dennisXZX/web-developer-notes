## Linux

#### Fundamentals

Technically speaking, Linux is a kernel, not an operating system. Linux distribution refers to a Linux kernel plus additional software. [DistroWatch](https://distrowatch.com/) has a thorough list of Linux distrubtion.

Another lesser-known family of operaing system is BSD, one of the best known BSD derivatives is [FreeBSD](https://www.freebsd.org/). macOS is also a closed source descendant of the BSD family.

Linux usually comes with a [package management tool](https://www.digitalocean.com/community/tutorials/package-management-basics-apt-yum-dnf-pkg) for finding and installing software.

- `mkdir -p /data/db` create nested directories using the `-p` flag
- `mkdir .storybook src` create two folders named `.storybook` and `src`
- `cat error-log.txt` display a file in one go, you can use `-n` flag to show line number
- `less error-log.txt` display a file page by page (press `b` for prevous page and `/` to search)
- `echo $PATH` display command search path, each path is separated by a colon
- `man commandName` get documentation for a command. `man -k "list directory"` to search something in manual. Use `g` to jump to the top and `G` to the bottom on the documentation page.
- `which commandName` locate the command being executed

- `head fileName` look at the first 10 lines of the file
- `tail fileName` look at the last 10 lines of the file, you can add the `-f` flag to follow the file change

- `find . -iname 'npm*' -ls` find files and directories start with 'npm' in current and sub-directories, ignoring case
- `find /home -name '*.jpg'` find all .jpg files in the /home and sub-directories
- `find . -type d` find all directiories in the current folder
- `find dist/ -name '*.built.js' -delete` find all built.js file in the dist folder and delete them
- `find images/ -name '*.png' -exec pngquant {} \;` find all png in the images folder and run pngquant on each matched file

- `cp -r source_directory destination_directory` recursively copy directory
- `mv source_file destination_file` move or rename a file or directory

#### Find out what program is using a specific port

- find out what program is using port 8080 `lsof -i :8080`
- kill the program `kill -9 <PID>`

#### .bash_profile (MacOX) / .bashrc (Linux)

You can put commands in `~/.bash_profile` file which will be run each time when you start your shell program.

```shell
// add alias
alias g="cd ~/Github/"
alias ..="cd .."
alias ...="cd ../../"

// add new path '/Users/bin' to the $PATH
// the best practice is to add new path after $PATH
PATH=$PATH:/Users/bin
```

You can generate a custom prompt using [.bashrc generator](http://bashrcgenerator.com/).

#### Redirection operator

- `ls -al > listings` re-direct the result of command `ls -al` file "listings" instead of your screen
- `echo "node_modules" >> .gitignore` add more content to an existing file instead of overwriting it
- `nc locallost 5000 < post.txt` send the text in post.txt to `netcat` command

#### Wildcards

- `*` match zero or more characters
- `?` match exactly one character
- `[]` match any of the characters included between the brackets. It match exactly one character
- `[!]` match any of the characters NOT included between the brackets. It match exactly one character
- `[a-g]` create a range of characters
- `[[:alpha:]]`, `[[:alnum:]]`, `[[:digit:]]`, `[[:lower:]]`, `[[:upper:]]`, `[[:space:]]` are pre-defined wildcards

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

#### grep

`grep --color -n -C 1 'download' package.json` search the word 'download' in package.json

`grep -i "boo" /etc/passwd` search passwd file for the word "boo", ignoring word case

`grep -i "shell" dictionary.txt | less` search dictionary.txt for the word "shell", ignoring word case and pipe the result to `less` command

`grep -r "192.168.1.5" /etc/` search all files under /etc folder for "192.168.1.5"

`grep -w 'word1|word2' /path/to/file` search word1 or word2 exact match in a file

`grep -c 'hello' /path/to/file` count how many times the word 'hello' appear in the file

#### curl

- get all the content of a website

```shell
curl 'https://google.com'

// use -L flag to follow redirection
curl -L 'https://google.com'
```

```shell
// save the output of the URL to a file
curl -o api/posts.json 'https://domain.com/apiData.json'

// save the html page to a file named google.html
curl -o google.html -L 'http://google.com'
```

- download files

```shell
// download file.zip and save it to the current working directory 
curl -O https://domain.com/file.zip

// download multiple files simultaneously
curl -O https://domain.com/file.zip https://domain.com/file2.zip

// download file.zip to the current working directory and rename it as archive.zip
curl -o archive.zip https://domain.com/file.zip

// download file securely via SSH
curl -u user:pass -O ftp://server.domain.com/path/to/file
```

- get HTTP header info

```shell
curl -i http://domain.com
```

- send a POST request

```shell
// -H to set a header
// -X to set a request
// -d to set the data to be sent
curl -H "X-First-Name: Dennis" -X POST http://localhost:5000 -d title=whatever -d date=12321231 -d body='hello world'
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
- `-h`, make the size human readable
- `-latr`, display most recently changed files at the bottom
- `ls /etc | wc -l`, display the number of file and folder in /etc folder

If you need to work on a file name with space, you can use quote to get around it.

```shell
ls -l 'my notes.txt'
```

When you use the `-l` format, we can see the permissions for files and directories. 

```shell
drwxrwxrwx  18 dennis.x  staff    612 13 Jul 14:09 books
-rwxr-xr--   1 dennis.x  staff   8398  5 Jul 11:06 ei-activity-view.css
```

For `drwxrwxrwx`, the first character indicates the file type, `d` represents a directory, `-` indicates a regular file and `l` refers to a symbolic link.

- The first three symbols `rwx` mean the file's owner may read, write, or execute
- The second three symbols `r-x` mean anyone in the file's group may read or execute this file, but not write to it
- The last three symbols `r--` mean anyone at all may read this file, but not write to it or execute its contents as a process

### Change permissions for files or directories

You can change the permission for a file using `chmod` symbolic notation.

```shell
// 'u' represents User
// 'g' represents Group
// 'o' represents Other
// 'a' represents All

// add read, write and execute permission to User group
chmod u+rwx myfile.txt

// add read permission to User, and remove execute permission from Group
chmod u+r,g-x myfile.txt

// set read, write, execute permission to User
// set read, execute permission to Group
// set no permission to Other
chmod u=rwx,g=rx,o= myfile.txt
```

On the other hand, you can also use numeric notation.

```shell
// set read, write, execute permission to User
chmod 700 myfile.txt
```

The three number represents the group of User, Group and Other.

Here is how the permission is calculated.

- 4 stands for "read"
- 2 stands for "write"
- 1 stands for "execute"
- 0 stands for "no permission"

So 7 is the combination of permissions 4+2+1 (read, write, and execute), 5 is 4+0+1 (read, no write, and execute), and 4 is 4+0+0 (read, no write, and no execute).

One thing to keep in mind, in any situation you tempt to set 777, consider changing the group instead of permission on a file.

```shell
// list all the groups available
groups

// change myfile.txt to newGroupName
chgrp newGroupName myfile.txt
```

![permission table](./images/linux_permission_table.png)

You can change default permission by using `umask` command.

```shell
// list the current default permission in symbolic notation
umask -S

// change default permission to u=rwx,g=rwx,o=
umask 007
```
