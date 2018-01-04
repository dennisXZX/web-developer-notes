## Linux

#### Show how long a command takes to run

```
time yarn webpack
```

#### List files in a tree structure

For MacOS, you need to install a the tree package first `brew install tree`.

```
tree -F
```

#### Navigation in Linux

// go to the previous working directory
cd -

// go to the home directory
cd ~

#### Exploring the system

There are some common `ls` Options 

- -a, list all files, including hidden files
- -l, list in long format
- -F, indicate directories by placing a '/' at the end of each listed file
- -r, list in reverse order
- -S, sort results by file size
- -t, sort results by modification time

When you use the `-l` format, we can see the access rights to the file. 

```
drwxrwxrwx  18 dennis.x  staff    612 13 Jul 14:09 books
-rwxr-xr--   1 dennis.x  staff   8398  5 Jul 11:06 ei-activity-view.css
```

For `drwxrwxrwx`, the first character indicates the file type, `d` represents a directory while a `-` indicates a regular file.

- The first three symbols `rwx` mean the file's owner may read, write, or execute
- The second three symbols `r-x` mean anyone in the file's group may read or execute this file, but not write to it
- The last three symbols `r--` mean anyone at all may read this file, but not write to it or execute its contents as a process

### Change permissions for files or directories

You can change the permission for a file using `chmod` command.

```
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
