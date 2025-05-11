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

### Get public ip address

```
ubuntu@ip-192-168-0-1:~$ curl -4 ifconfig.me
```

### Get private ip address

```
ubuntu@ip-192-168-0-1:~$ ip a
```

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

#### What should be done if there is no public key available for a particular user?

If there is no Public Key available for a perticular user inside authorized_keys file, then the user can't access the instance.

You need to generate the public key in your local machine.

```
ssh-keygen -t rsa -b 4096 -C "muhammad.lahin@gmail.com"
```

- It will create a new RSA key pair with 4096-bit encryption.
- The -C flag associates the key with the email provided, which helps with identification.
- By default, the private key (id_rsa) and public key (id_rsa.pub) are stored in the .ssh directory of your home folder.

This will create a new file inside, location `/Users/mizanurlahin/.ssh/id_rsa.pub`.

Copy the content.

Run this,

```
cat /Users/mizanurlahin/.ssh/id_rsa.pub | ssh username@server_ip 'mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys && chmod 700 ~/.ssh'
```

This will put the public key inside authorized_keys file. Now the instance have the public key of the user.

Let's consider another example,

You have two AWS Lightsail instances, both residing in the same Availability Zone. Instance A needs to connect to Instance B via SSH.

From Instance A you can't just type, `ssh ubuntu@{ip_of_instance_b}`

To enable SSH access, you must configure Instance A's public key in the authorized_keys file of Instance B.

- Step 1 - Generate an SSH Key Pair (From Instance A): `ssh-keygen -t rsa -b 4096`

- Step 2 - Copy the Public Key: `cat ~/.ssh/id_rsa.pub`. This will display the public key. Copy it.

- Step 3 - Add the Public Key to authorized_keys of Instance B: `nano ~/.ssh/authorized_keys`, paste the public key of Instance A.

- Step 4 - Set Proper Permissions inside Instance B:

```
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

## User Management

It is one of the most important concept. You should/must know about this. Essential for SECURITY perspective.

### Create a new user

```
ubuntu@ip-192-168-0-1:/$ sudo adduser [username]
```

This will ask you for a password and additional user details.

### Change user's password

```
ubuntu@ip-192-168-0-1:/$ sudo passwd ubuntu
```

It asks for your current password (since you're not root). Then it prompts you to enter a new password for the ubuntu user. It asks again to confirm the new password. If both entries match and meet password policy requirements, the password will be updated.

### Add user to a particular group

```
ubuntu@ip-192-168-0-1:/$ sudo usermod -aG [groupname] [username]
```

### Delete a specific user

```
root@ip-192-168-0-1:/$ deluser ubuntu
```

It will delete the ubuntu user account from the system.

## Copying one or more files from Local to Remote Server or Two Remote Servers

It is used to securely transfer files or directories between systems via SSH (Secure Shell).

### Copy a file from local to remote host

```
scp info.txt [remote_host_username]@[remote_host_ip]:/path/to/destination/
```

Here, `info.txt` is the actual file that needs to be transferred. `remote_host_username` and `remote_host_ip` refer to the username and IP address of the remote host. `/path/to/destination/` is the destination path where the file(info.txt) will be copied.

### Copy a file from remote host to local

```
scp [remote_host_username]@[remote_host_ip]:/path/to/info.txt /local/destination/
```

Here, `remote_host_username` and `remote_host_ip` refer to the username and IP address of the remote host. `/path/to/info.txt` is the path with file attached, that needs to be transferred. `/local/destination/` is the local system directory where the file(info.txt) will be copied.

## Process Management

It refers to the controlling and monitoring of processes running on a Linux system. It involves starting, stopping, pausing, resuming, managing priorities, and monitoring system resources utilized by processes to ensure efficient and stable operation.

### Show all active processes

```
ubuntu@ip-192-168-0-1:/$ ps
```

You will see these information.

- PID -> The unique identifier, we call it Process ID.
- TTY -> The terminal (or pseudo-terminal) associated with the process.
- TIME -> The total CPU time the process has used since it started.
- CMD -> The command or the name of the executable that started this process.

### Show all processes

```
ubuntu@ip-192-168-0-1:/$ ps aux
```

For example, when you run `pm2 start app.js`, a process starts in the background. You can find this process using the ps command. Running `ps aux | grep pm2` will provide information such as:

- USER: The username that owns the process.
- PID: The Process ID, a unique number identifying the process.
- %CPU: The percentage of CPU the process is currently using.
- %MEM: The percentage of memory the process is currently using.
  and more.

### Real time view of all running process

Provides a dynamic, real-time view of running processes, showing CPU and memory usage.

```
ubuntu@ip-192-168-0-1:/$ top
```

### Kill a process

```
ubuntu@ip-192-168-0-1:/$ kill {PID}
```

This sends a SIGTERM signal, asking the process to terminate gracefully. But if you want an immediate termination.

```
ubuntu@ip-192-168-0-1:/$ kill -9 {PID}
```

## Swap Memory

It is a space or area on a disk that is used when the amount of physical RAM is full. When the system runs out of RAM, it swaps out less frequently used memory pages to the Swap Memory, freeing up RAM for more critical tasks.

```
ubuntu@ip-192-168-0-1:/$ free -h
```

You will see,

- Memory usage
- Swap usage

## Disk Space Usage

Shows available and used disk space on all mounted filesystems.

```
ubuntu@ip-192-168-0-1:/$ df -h
```

## PING - Network Troubleshooting

`ping` is a very useful command so that we can troubleshoot network connectivity.

Let's say from your system want to check connectivity of google website. You can either put ip address of google or directly the actual domain.

```
mizanurlahin@Mizanurs-MacBook-Pro ~ % ping google.com
PING google.com (142.250.182.142): 56 data bytes
64 bytes from 142.250.182.142: icmp_seq=0 ttl=59 time=31.100 ms
64 bytes from 142.250.182.142: icmp_seq=1 ttl=59 time=35.991 ms
64 bytes from 142.250.182.142: icmp_seq=2 ttl=59 time=29.149 ms
64 bytes from 142.250.182.142: icmp_seq=3 ttl=59 time=36.358 ms
64 bytes from 142.250.182.142: icmp_seq=4 ttl=59 time=35.116 ms
^C
--- google.com ping statistics ---
5 packets transmitted, 5 packets received, 0.0% packet loss
round-trip min/avg/max/stddev = 29.149/33.543/36.358/2.887 ms
```

As you can see 0% packet loss that means we can successfully make connection from our system to google.

if you try to use 192.0.2.1, which is a reserved ip for demo,

```
mizanurlahin@Mizanurs-MacBook-Pro ~ % ping 192.0.2.1
PING 192.0.2.1 (192.0.2.1): 56 data bytes
Request timeout for icmp_seq 0
Request timeout for icmp_seq 1
Request timeout for icmp_seq 2
Request timeout for icmp_seq 3
Request timeout for icmp_seq 4
^C
--- 192.0.2.1 ping statistics ---
6 packets transmitted, 0 packets received, 100.0% packet loss
```

You will get 100.0% packet loss.

### Why the ping command can't connect to a network?

- Firewall: If there is a firewall on the destination network and it doesn't have an inbound rule allowing traffic from your network, then the connection will be blocked and the ping will fail.

You also need to check the outbound rules of your system.

```
ubuntu@ip-192-168-0-1:/$ sudo ufw status
Status: inactive
```

**Status: inactive** it means the Firewall is not currently active — so none of its inbound or outbound rules are being enforced.

- Internet Connection: If either your system or the destination has no internet connection or is experiencing network issues, the `ping` command won't be able to reach the destination and will fail.

- ICMP: Some servers are configured to silently disable the ICMP requests (common for security reasons), meaning ping will not respond even if the host is up and reachable.

- DNS Resolution: If you’re pinging a domain name and there’s an issue with DNS, your system might not be able to resolve the IP address. Example, if your system can’t reach a DNS server (due to misconfiguration or no internet) or the DNS server is down.

## SS - Network Troubleshooting

ss is used to display information about sockets — things like open ports, listening services, established connections etc.

Let's say you are using Node.js and you are serving this with NGINX.

To check about the information of NGINX

```
ubuntu@ip-192-168-0-1:/$ sudo ss -tulpn | grep nginx
```

Let's understand each part of the command,

- **ss**: lists all open sockets.
- **-tulpn**
  - **-t**: TCP
  - **-u**: UDP
  - **-l**: Listening
  - **-p**: Show process info
  - **-n**: Don't resolve names in DNS
- **grep nginx**: filters for lines related to Nginx.

Output can be,

```
tcp    LISTEN   0       128          0.0.0.0:80         0.0.0.0:*     users:(("nginx",pid=1234,fd=6))
tcp    LISTEN   0       128          [::]:80            [::]:80       users:(("nginx",pid=1234,fd=7))
```

Means NGINX is accepting connections on port 80 from **any IPv4 and IPV6 address**.

### Do you want to block this NGINX access from the outside?

Open `/etc/nginx/sites-available/default` and replace `listen 80;` to `listen 127.0.0.1:80;`.

Now restart the NGINX, `sudo systemctl restart nginx`

## Excute a script after intence rebooted

Let's say your instance suddenly got rebooted. You were using pm2. So pm2 stopped.

Let's write a (.sh) script that will start the pm2 service after the instance rebooted.

Create a file called start_script.sh inside `/usr/local/bin/initial/` (I created initial directory)

```
#!/bin/bash

# Full system PATH
export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

# Start Server
cd /var/www/my_project/server/ || {
  echo "$(date) - Failed to access Server directory" >> /var/log/my_project-startup.log
  exit 1
}

sudo pm2 start npm --name server -- run dev

# Start Client
cd /var/www/my_project/client/ || {
  echo "$(date) - Failed to access Client directory" >> /var/log/my_project-startup.log
  exit 1
}
sudo pm2 start ecosystem.config.cjs

# Save process list (no restart needed)
sudo pm2 save --force
```

Make sure you make the file executable,

```
sudo chmod +x /usr/local/bin/initial/start_script.sh
```

Run, `sudo crontab -e`.

And paste,

```
@reboot /usr/local/bin/initial/start_script.sh >> /var/log/my_project-startup.log 2>&1
```

**`sudo crontab -e` edits the root user’s crontab, so any @reboot or other cron jobs added there will run as root.**

You can test it by removing all pm2 services, `pm2 delete all`

Run, `sudo reboot`

Then you can re-enter the instance and check.
