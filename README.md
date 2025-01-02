# Practical Linux (Ubuntu)

Linux is the most useful UNIX based OS. These are some useful information that can significantly enhance your productivity and efficiency in your job.

## Folder Structure

Understanding Folder Structure is very important. Without understanding it you can't use Linux effectively.

If follows the Filesystem Hierarchy Standard (FHS). The `/` is called Root Directory and this is the parent directory and every other directory is a child of the root directory.

| **Directory** | **Description**                                                             |
| ------------- | --------------------------------------------------------------------------- |
| `/`           | Root directory.                                                             |
| `/bin`        | Essential binaries (e.g., `ls`, `cp`) available to all users.               |
| `/sbin`       | Essential system administration binaries (e.g., `ifconfig`, `reboot`).      |
| `/etc`        | Configuration files for the system and installed applications.              |
| `/usr`        | User programs, libraries, and documentation (e.g., `/usr/bin`, `/usr/lib`). |
| `/lib`        | Essential shared libraries for binaries in `/bin` and `/sbin`.              |
| `/var`        | Variable data files (e.g., logs, cache, spools).                            |
| `/tmp`        | Temporary files.                                                            |
| `/home`       | User home directories.                                                      |
| `/proc`       | Virtual filesystem providing process and kernel information.                |
| `/dev`        | Device files (e.g., `/dev/sda` for a hard drive).                           |
| `/opt`        | Optional software packages and add-ons.                                     |
| `/root`       | Home directory for the root user.                                           |
| `/boot`       | Files required for booting the system (e.g., kernel, GRUB).                 |
| `/media`      | Mount points for removable media (e.g., USB drives, DVDs).                  |
| `/mnt`        | Temporary mount points for system administrators.                           |
| `/run`        | Runtime data for processes since the last boot.                             |

- If you are in / (means root directory).

```
ubuntu@ip-192-168-0-1:/$ ls
bin   dev  home  lib32  libx32      media  opt   root  sbin  srv  tmp  var
boot  etc  lib   lib64  lost+found  mnt    proc  run   snap  sys  usr
```

- If you are in /home directory.

```
ubuntu@ip-192-168-0-1:/home$ ls
ubuntu
```

### Recommandations

- Your web server's (NGINX, Apache) configuration should stay inside `/etc`.
- It is recommended when deploying on a server using web servers like NGINX or Apache, put your Application code like Node(.js), React(.js) or Vue(.js) inside `/var/www`.
- If you are using Let’s Encrypt to get free SSL certificates, your configuration should stay inside `/etc/ssl/certs/` directory.

## Basic Commands

### pwd

It will let you know the folder you're currently in.

```
ubuntu@ip-192-168-0-1:~$ pwd
/home/ubuntu
```

### ls

It will list down all files and directories in the current working directory.

```
ubuntu@ip-192-168-0-1:/var$ ls
backups  crash  local  log   opt  snap   tmp
cache    lib    lock   mail  run  spool  www
```

Use `ls -a` to view hidden files.

### mkdir

Used to create a directory/folder.

```
ubuntu@ip-192-168-0-1:~$ ls
confidentials info.txt
ubuntu@ip-192-168-0-1:~$ mkdir user-data
ubuntu@ip-192-168-0-1:~$ ls
confidentials info.txt user-data
```

user-data is the new folder.

### rm -rf [directoryName]

```
ubuntu@ip-192-168-0-1:~$ ls
confidentials info.txt user-data
ubuntu@ip-192-168-0-1:~$ rm -rf user-data
ubuntu@ip-192-168-0-1:~$ ls
confidentials info.txt
```

### cat

It is used to view a particular file or more than one file.

```
ubuntu@ip-192-168-0-1:~$ cat info.txt
Hello World
```

If you want to view this with line number.

```
ubuntu@ip-192-168-0-1:~$ cat -n info.txt
1 Hello World
```

If you want to view more than one file.

```
ubuntu@ip-192-168-0-1:~$ cat info.txt result.txt
Hello World
This is result content
```

### touch

Used to create a file.

```
ubuntu@ip-192-168-0-1:~$ touch info.txt
ubuntu@ip-192-168-0-1:~$ ls
info.txt
```

### rm [filename]

Used to delete a file.

```
ubuntu@ip-192-168-0-1:~$ ls
info.txt result.txt
ubuntu@ip-192-168-0-1:~$ rm info.txt
ubuntu@ip-192-168-0-1:~$ ls
result.txt
```

(Deleting a file requires permission, which we will discuss in the next section.)

### cp

Used to copy a file content to a different file.

```
ubuntu@ip-192-168-0-1:~$ cp info_1.txt info_2.txt
```

The content of info_1.txt is duplicated into info_2.txt.

### mv

Used to moves or rename a file or folder.

```
ubuntu@ip-192-168-0-1:~$ ls
info_1.txt
ubuntu@ip-192-168-0-1:~$ mv info_1.txt info_2.txt
ubuntu@ip-192-168-0-1:~$ ls
info_2.txt
```

### \*

In Linux it is a wildcard character. Used with `ls`, `cp`, `rm`, and `find` to match files or directories with names that fit a specific pattern.

```
ubuntu@ip-192-168-0-1:~$ ls *.txt
info_1.txt info_2.txt
```

It lists only files with the (.txt) extension in the current directory.

## Advance Commands

### ls -l

It will display the following information for each file in the current directory:

- Permissions
- Number of hardlinks
- File owner
- File group
- File size
- Modification time
- Filename

```
ubuntu@ip-192-168-0-1:/var/www/server$ ls -l
-rw-r--r--   1 root root  1241 Aug 14 05:23 app.js
drwxr-xr-x   2 root root  4096 Aug 14 05:23 config
drwxr-xr-x  11 root root  4096 Sep 17 11:59 controllers
....(more)
```

For `-rw-r--r--   1 root root  1241 Aug 14 05:23 app.js`

- -: Indicates `app.js` is a regular file.
- rw-: Permissions for the file owner (read r, write w, no execute -).
- r--: Permissions for the group (read r, no write -, no execute -).
- r--: Permissions for others (read r, no write -, no execute -).
- 1: Indicates the number of hard links to the file.
- root: Indicates who owns the file. In that case, root.
- root: Indicates the group that own the file. In that case, it means "root(group)".
- 1241: Indicates the size of the file in bytes.
- "Aug 14 05:23": Indicates last modification date and time of the file.
- "app.js": Name of the file.

For directory it will start with 'd' symbol.

### chown

Used to change the owner and(/or) group for a particular file or directory.

```
// Change owner:
ubuntu@ip-192-168-0-1:/var/www/server$ sudo chown lahin app.js
ubuntu@ip-192-168-0-1:/var/www/server$ ls -l app.js
-rw-r--r-- 1 lahin root 1241 Aug 14 05:23 app.js
```

```
// Change group:
ubuntu@ip-192-168-0-1:/var/www/server$ sudo chown :developers app.js
ubuntu@ip-192-168-0-1:/var/www/server$ ls -l app.js
-rw-r--r-- 1 lahin developers 1241 Aug 14 05:23 app.js
```

### chmod

Used to change the mode. Linux permission numbering system follows,

- For read; r = 4
- For write; w = 2
- For execute; e = 1

We have `-rw-r--r-- 1 lahin root 1241 Aug 14 05:23 app.js`. We want to give the execute privilege to others.

```
ubuntu@ip-192-168-0-1:/var/www/server$ sudo chmod 645 app.js
ubuntu@ip-192-168-0-1:/var/www/server$ ls -l app.js
-rw-r--r-x 1 root root 1241 Aug 14 05:23 app.js
```

As we ran this `sudo chmod 645 app.js` the 645 means

- For owner we have 6, r = 4 and write = 2; (4 + 2) is equal to 6
- For group we have 4, r = 4; means 4
- For others we have 5, r = 4 and execute = 1; (4 + 1) is equal to 5

### whoami

Displays the username of the currently logged-in user.

```
ubuntu@ip-192-168-0-1:~$ whoami
ubuntu
```

### who

Displays all logged-in users and their session details.

```
ubuntu@ip-192-168-0-1:~$ who
ubuntu   pts/0        2025-01-02 04:35 (120.112.118.220)
```

### history

Displays entire history of commands run in the terminal.

### nano [filename]

Opens a text editor in the terminal to edit a file.

### head -[numberOfLine] [filename]

Used to get the file's content based on the number of line given.

```
ubuntu@ip-192-168-0-1:/var/www/server$ head -3 app.js
const express = require("express");
const cors = require("cors");
const morgan = require("morgan");
```

### grep

Used to get the matching rows for a perticular word or sentence in a file.

```
ubuntu@ip-192-168-0-1:/var/www/server$ grep 'require("express")' app.js
const express = require("express");
```

### cmp

Used to compare two files byte by byte.

```
ubuntu@ip-192-168-0-1:/var/www/server$ cmp info1.txt info2.txt
info1.txt info2.txt differ: byte 12, line 1
```

That means line 1 is not identical.

### diff

Used to compare two files line by line and display the differences between them.

```
ubuntu@ip-192-168-0-1:/var/www/server$ diff -u info1.txt info2.txt
--- info1.txt	2024-12-27 10:41:24.248772500 +0000
+++ info2.txt	2024-12-27 10:41:58.995875405 +0000
@@ -1,2 +1,2 @@
-My name is Lahin.
-I love designing systems.
+My name is John.
+I like Javascript.
```

### find

Used to search for files and directories within a given path.

```
ubuntu@ip-192-168-0-1:~$ find /var/www -name app.js
/var/www/server/app.js
```

### alias

Used to create a shorthand for a longer command or sequence of commands, making them easier and quicker to execute.

```
ubuntu@ip-192-168-0-1:/var/www/server$ alias l="ls -l"
ubuntu@ip-192-168-0-1:/var/www/server$ l
-rw-r--r-- 1 lahin developers 1241 Aug 14 05:23 app.js
drwxr-xr-x   2 root root  4096 Aug 14 05:23 config
drwxr-xr-x  11 root root  4096 Sep 17 11:59 controllers
...(more)
```

You can also delete an alias

```
ubuntu@ip-192-168-0-1:/var/www/server$ unalias l
```

## How to Connect from your Laptop to a Linux Cloud Instance with SSH

Secure Shell (SSH) is a protocol that allows you to securely connect to a remote server. It's a common method for accessing Linux cloud instances.

### How SSH works

Lets say you want to connect from your pc to an instance via SSH. Generally SSH uses port 22 for communication. You mush ensure this port is not blocked by the firewall.

In your laptop you must ensure SSH Client.

SSH key-based authentication is more secure than password-based authentication. We have Private key and Public key.

You can't access to the instance unless you have the instance's Private Key. Typically it is (.)pem extension file.

And the remote instance should have your public key. During the connection, the instance uses this public key to verify the private key sent by the client.

#### How can you verify remote instance have client's public key

The remote instance must have your public key in its `~/.ssh/authorized_keys` file. Generally if it's your first time connecting to the remote instance, most probably it automatically copies your public key to the authorized_keys file.

If you are using AWS EC2 instance during the instance launch, AWS sets up the public key in the `~/.ssh/authorized_keys` file of the default user on the instance.

## User Management

It is one of the most important concept. You should/must know about this. Essential for SECURITY perspective.

### Create a new user

```
ubuntu@ip-192-168-0-1:/$ sudo adduser [username]
```

This will ask you for a password and additional user details.
