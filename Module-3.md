# Module 3 - Linux System Access & File System

## Complete Interview Handbook (Cheat Sheet + Questions + Answers)

> This handbook combines the module concepts, memory tricks, important
> commands, interview questions, answers, and scenarios into one
> document.

------------------------------------------------------------------------

# 1. Linux Basics

## What is Linux?

Linux is an open-source, Unix-like, multi-user and multitasking
operating system. It uses the Linux kernel to communicate between
applications and hardware. It is widely used for servers because it is
stable, secure, flexible and performs well.

### Remember

-   Open Source
-   Multi-user
-   Multitasking
-   Secure
-   Stable
-   CLI Oriented

### Interview Question

**Q: Tell me about Linux.**

**Answer:** Linux is an open-source operating system used primarily in
servers. It supports multiple users and multitasking. It is preferred in
enterprise environments because of its stability, security, flexibility
and strong command-line capabilities.

------------------------------------------------------------------------

# 2. Linux Architecture

``` text
Applications
      ↓
    Shell
      ↓
    Kernel
      ↓
   Hardware
```

### Interview Question

**Q: Explain Linux architecture.**

**Answer:** Applications communicate with the shell. The shell
interprets user commands and sends system calls to the kernel. The
kernel manages CPU, memory, storage, processes and hardware devices.

------------------------------------------------------------------------

# 3. Linux Kernel

## Definition

The kernel is the core component of Linux.

## Responsibilities

-   CPU Management
-   Memory Management
-   Process Scheduling
-   Device Management
-   File System Management

### Interview Question

**Q: What is the kernel?**

**Answer:** The Linux kernel is the core of the operating system. It
acts as a bridge between applications and hardware and manages CPU,
memory, devices, filesystems and processes.

------------------------------------------------------------------------

# 4. Linux vs Windows

  Linux               Windows
  ------------------- ---------------------
  Open Source         Licensed
  CLI                 GUI
  Better uptime       Frequent reboot
  Better automation   Microsoft ecosystem

### Interview Question

**Q: Why Linux over Windows?**

**Answer:** Linux is preferred for servers because it is stable, secure,
lightweight, open source and easier to automate using shell scripting.

------------------------------------------------------------------------

# 5. Root User

Root is the superuser with unrestricted access.

### Can do

-   Create users
-   Delete users
-   Install software
-   Modify configs
-   Shutdown system

### Interview Question

**Q: Why shouldn't you work as root?**

**Answer:** Because root has unrestricted permissions. An incorrect
command can damage the operating system. It is safer to use sudo
whenever possible.

------------------------------------------------------------------------

# 6. Linux Access

## Local

Console Login

## Remote

SSH PuTTY Windows SSH Client

``` bash
ssh user@server-ip
```

### Interview Question

**Q: What is SSH?**

**Answer:** SSH (Secure Shell) is an encrypted protocol used to securely
access Linux systems remotely over a network.

------------------------------------------------------------------------

# 7. Linux File System

## Definition

A filesystem is the method used by Linux to organize and store files on
a disk or partition.

## Everything is a File

Examples - Regular files - Directories - Hard disks - USB devices -
Printers

------------------------------------------------------------------------

# 8. Important Directories

  Directory   Purpose
  ----------- -----------------
  /           Root
  /etc        Configuration
  /home       User Data
  /usr        Programs
  /var        Logs
  /boot       Boot Files
  /tmp        Temporary Files
  /dev        Device Files

### Interview Question

**Q: What is stored in /etc?**

Configuration files.

**Q: What is /var?**

Logs, spool files and variable data.

------------------------------------------------------------------------

# 9. File Naming

Rules - Case Sensitive - Avoid spaces - Hidden files start with . - . =
Current Directory - .. = Parent Directory - \~ = Home Directory

------------------------------------------------------------------------

# 10. Password Management

``` bash
passwd
passwd username
```

Passwords are stored in

``` text
/etc/shadow
```

------------------------------------------------------------------------

# 11. find vs locate

## find

-   Live Search
-   Accurate
-   Slow

``` bash
find / -name file.txt
```

## locate

-   Uses Database
-   Fast
-   Requires updatedb

``` bash
locate file.txt
updatedb
```

### Interview Question

**Q: Difference between find and locate?**

**Answer:** find searches the live filesystem and is slower but
accurate. locate searches a prebuilt database and is faster, but it
depends on the updatedb database.

------------------------------------------------------------------------

# 12. Wildcards

  Symbol   Meaning
  -------- -------------------------
  \*       Zero or more characters
  ?        Exactly one character
  \[\]     Character range

Examples

``` bash
ls *.txt
ls file?.txt
ls [abc]*
```

------------------------------------------------------------------------

# 13. Hard Link vs Soft Link

## Hard Link

-   Same inode
-   Doesn't break if filename changes
-   Cannot cross filesystems

## Soft Link

-   Different inode
-   Shortcut
-   Breaks if original file removed
-   Can cross filesystems

### Interview Question

**Q: Difference between hard link and soft link?**

**Answer:** A hard link points to the inode and continues working if the
original filename changes. A soft link points to the pathname and
becomes broken if the original file is deleted or renamed.

------------------------------------------------------------------------

# 14. File Permissions

Read = 4

Write = 2

Execute = 1

Examples

7 = rwx

6 = rw-

5 = r-x

4 = r--

0 = ---

``` bash
chmod 764 file
```

Owner = rwx

Group = rw-

Others = r--

### Interview Question

**Q: Explain chmod 764.**

**Answer:** Owner has read, write and execute permissions. Group has
read and write permissions. Others have read-only permission.

------------------------------------------------------------------------

# 15. Commands to Remember

``` bash
pwd
ls -l
cd
find
locate
updatedb
chmod
passwd
ssh
whoami
```

------------------------------------------------------------------------

# 16. Top Interview Questions

1.  What is Linux?
2.  Explain Linux architecture.
3.  What is the kernel?
4.  Difference between Linux and Windows?
5.  What is the root user?
6.  Explain the Linux filesystem.
7.  What is SSH?
8.  Difference between hard link and soft link?
9.  Difference between find and locate?
10. Explain chmod 764.

------------------------------------------------------------------------

# 17. Scenario Questions

## Scenario 1

**Q:** You cannot find a file.

**Answer:** Use find for a live search. If using locate, ensure the
database is updated with updatedb.

------------------------------------------------------------------------

## Scenario 2

**Q:** You cannot SSH into a server.

**Answer:** Check: - Server reachable (ping) - SSH service running -
Firewall - Port 22 - Credentials - Network connectivity

------------------------------------------------------------------------

# 18. 5-Minute Revision

-   Linux = Open Source + Multi-user
-   Kernel = Core of OS
-   SSH = Secure Remote Login
-   Root = Superuser
-   Everything is a File
-   /etc = Config
-   /var = Logs
-   /home = User Data
-   find = Live Search
-   locate = Database Search
-   Hard Link = Same inode
-   Soft Link = Shortcut
-   764 = rwx rw- r--

------------------------------------------------------------------------

# Interview Answer Formula

For every definition answer:

1.  What is it?
2.  Why is it used?
3.  Real-world example.

This structure makes your answers concise, practical and suitable for an
L2 Linux Infrastructure interview.
