# Practical Linux (Ubuntu)

Linux is the most useful UNIX based OS. These are some useful information that will help you greatly.

## Folder Structure

Understanding Folder Structure is very important. Without understanding it you can't use Linux effectively.

If follows the Filesystem Hierarchy Standard (FHS). The `/` is called Root Directory and this is the parent directory and every other directory is a child of the root directory.

| **Directory** | **Description**                                                                 |
|---------------|---------------------------------------------------------------------------------|
| `/`           | Root directory.                                                                 |
| `/bin`        | Essential binaries (e.g., `ls`, `cp`) available to all users.                   |
| `/sbin`       | Essential system administration binaries (e.g., `ifconfig`, `reboot`).          |
| `/etc`        | Configuration files for the system and installed applications.                  |
| `/usr`        | User programs, libraries, and documentation (e.g., `/usr/bin`, `/usr/lib`).     |
| `/lib`        | Essential shared libraries for binaries in `/bin` and `/sbin`.                  |
| `/var`        | Variable data files (e.g., logs, cache, spools).                                |
| `/tmp`        | Temporary files.                                                                |
| `/home`       | User home directories.                                                          |
| `/proc`       | Virtual filesystem providing process and kernel information.                    |
| `/dev`        | Device files (e.g., `/dev/sda` for a hard drive).                               |
| `/opt`        | Optional software packages and add-ons.                                         |
| `/root`       | Home directory for the root user.                                               |
| `/boot`       | Files required for booting the system (e.g., kernel, GRUB).                     |
| `/media`      | Mount points for removable media (e.g., USB drives, DVDs).                      |
| `/mnt`        | Temporary mount points for system administrators.                               |
| `/run`        | Runtime data for processes since the last boot.                                 |

* If you are in / (means root directory).

```
ubuntu@ip-192-168-0-1:/$ ls
bin   dev  home  lib32  libx32      media  opt   root  sbin  srv  tmp  var
boot  etc  lib   lib64  lost+found  mnt    proc  run   snap  sys  usr
```

* If you are in /home directory.

```
ubuntu@ip-192-168-0-1:/home$ ls
ubuntu
```

### Recommandations

* Your web server's (NGINX, Apache) configuration should stay inside `/etc`.
* It is recommended when deploying on a server using web servers like NGINX or Apache, put your Application code like Node(.js), React(.js) or Vue(.js) inside `/var/www`.
* If you are using Let’s Encrypt to get free SSL certificates, your configuration should stay inside `/etc/ssl/certs/` directory.

## Basic Commands

### pwd (print working directory)

It will let you know the folder you're currently in.

```
ubuntu@ip-192-168-0-1:~$ pwd
/home/ubuntu
```

### ls (list files and directories)

It will list down all files and directories in the current working directory.

```
ubuntu@ip-192-168-0-1:/var$ ls
backups  crash  local  log   opt  snap   tmp
cache    lib    lock   mail  run  spool  www
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

### rm {filename} - delete a file

```
ubuntu@ip-192-168-0-1:~$ rm info.txt
ubuntu@ip-192-168-0-1:~$ ls
result.txt
```

(Deleting a file requires permission, which we will discuss in the permissions section.)

More to continue...