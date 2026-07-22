# Module 5 - Linux System Administration (Memory Sheet)

## 1. vi Editor

Remember:

- i → Insert
- Esc → Exit Insert Mode
- :w → Save
- :q → Quit
- :wq → Save & Quit
- :q! → Quit without saving
- dd → Delete line
- yy → Copy line
- p → Paste
- /word → Search
- :%s/old/new/g → Replace all

Interview:
Difference between vi and vim?

vi is the original editor.
vim is an enhanced version with syntax highlighting, plugins, undo history, and more features.

---

## 2. User Management

Create user

```bash
useradd john
passwd john
```

Modify user

```bash
usermod
```

Delete user

```bash
userdel -r john
```

Create group

```bash
groupadd devops
```

Add user to group

```bash
usermod -aG devops john
```

Important Files

```
/etc/passwd

/etc/shadow

/etc/group
```

Remember

passwd

User information

shadow

Encrypted password

group

Group information

---

## 3. Sudo

Switch user

```bash
su username
```

Login shell

```bash
su -
```

Run as root

```bash
sudo command
```

Edit sudo file

```bash
visudo
```

Configuration file

```
/etc/sudoers
```

---

## 4. Monitor Users

Current users

```bash
who
```

Who is doing what

```bash
w
```

Last login

```bash
last
```

Current user

```bash
whoami
```

User information

```bash
id
```

---

## 5. System Utility Commands

Current date

```bash
date
```

System uptime

```bash
uptime
```

Hostname

```bash
hostname
```

Kernel information

```bash
uname -a
```

Command location

```bash
which
```

Calendar

```bash
cal
```

Calculator

```bash
bc
```

---

## uptime

Example

```
11:18:23 up 83 days, 18:29, 4 users, load average: 0.16 0.03 0.01
```

Remember

Current Time

↓

11:18:23

Server Uptime

↓

83 days

Logged-in Users

↓

4 users

Load Average

↓

1 minute

5 minutes

15 minutes

Interview

High load average means

CPU is busy

Processes waiting

Investigate using

```
top

ps

free

```

---

## 6. Process Commands

Show process

```bash
ps
```

All processes

```bash
ps -ef
```

Detailed

```bash
ps aux
```

Kill process

```bash
kill PID
```

Force kill

```bash
kill -9 PID
```

Background

```bash
bg
```

Foreground

```bash
fg
```

List jobs

```bash
jobs
```

---

## 7. Signals

Normal termination

```
SIGTERM (15)
```

Force kill

```
SIGKILL (9)
```

CTRL+C

```
SIGINT
```

CTRL+Z

```
SIGTSTP
```

Continue process

```
SIGCONT
```

Pause process

```
SIGSTOP
```

Restart daemon

```
SIGHUP
```

Remember

TERM

↓

Graceful

KILL

↓

Force

---

## 8. top Command

Start

```bash
top
```

Remember

Row 1

Time

Uptime

Users

Load Average

Row 2

Running

Sleeping

Zombie

Row 3

CPU

Row 4

RAM

Row 5

Swap

Columns

PID

USER

CPU%

MEM%

TIME

COMMAND

Interview

Zombie

↓

Dead process

Running

↓

Executing

Sleeping

↓

Waiting

---

## 9. Crontab

Edit

```bash
crontab -e
```

List

```bash
crontab -l
```

Delete

```bash
crontab -r
```

Format

```
* * * * * command
```

Remember

Minute

Hour

Day

Month

Weekday

Command

---

## 10. CTRL Keys

CTRL+C

↓

Kill

CTRL+Z

↓

Suspend

CTRL+D

↓

Exit

CTRL+U

↓

Delete current line

CTRL+S

↓

Pause terminal

CTRL+Q

↓

Resume terminal

---

## 11. Recover Root Password

Remember

GRUB

↓

Edit

↓

Change

```
ro

↓

rw init=/sysroot/bin/sh
```

↓

Ctrl+X

↓

```
chroot /sysroot
```

↓

```
passwd root
```

↓

```
exit
```

↓

```
reboot
```

---

# 30 Commands to Remember

```
vi
:wq
:q!

useradd
usermod
userdel

groupadd
groupdel

passwd

su
sudo
visudo

who
w
last
id

date
uptime
hostname
uname
which

ps
top
kill
jobs
bg
fg

crontab

whoami
```

---

# Interview Tip

If asked **"My Linux server is slow, what will you check?"**, answer in this order:

```bash
uptime          # Check load average
top             # Check CPU and memory
free -m         # Check RAM usage
df -h           # Check disk space
ps aux --sort=-%cpu | head
journalctl -xe  # Check system logs (or /var/log/messages)
```

This troubleshooting flow demonstrates practical Linux administration rather than just command knowledge.
