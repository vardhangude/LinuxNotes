# Module 5 – System Utility Commands
## Linux Interview Handbook (4+ Years DevOps/Linux Administrator)

> These commands are frequently used during Linux administration, production troubleshooting, and DevOps interviews.

---

# Table of Contents

1. date
2. uptime
3. hostname
4. uname
5. which
6. cal
7. bc
8. Production Scenarios
9. Interview Questions

---

# 1. date

## Interview Question

### What is the date command?

### Answer

The `date` command displays or sets the current system date and time.

Syntax

```bash
date
```

Example

```bash
$ date

Tue Jul 22 11:18:23 IST 2026
```

---

## Common Options

Display UTC time

```bash
date -u
```

Display custom format

```bash
date "+%d-%m-%Y"
```

Output

```
22-07-2026
```

Display only time

```bash
date "+%H:%M:%S"
```

---

## Production Usage

- Verify server time
- Check timestamp before deployments
- Compare application logs
- Verify NTP synchronization

---

## Interview Follow-up

### Why is system time important?

Incorrect system time can cause

- SSL certificate failures
- Authentication problems
- Incorrect log timestamps
- Cron jobs executing at the wrong time

---

# 2. uptime

## Interview Question

### Explain the uptime command.

### Answer

The `uptime` command shows

- Current system time
- How long the server has been running
- Number of logged-in users
- Load average for the last 1, 5, and 15 minutes :contentReference[oaicite:1]{index=1}

Example

```bash
$ uptime

11:18:23 up 83 days, 18:29, 4 users, load average: 0.16, 0.03, 0.01
```

---

## Explain Every Field

### Current Time

```
11:18:23
```

Current Linux system time.

---

### Uptime

```
up 83 days, 18:29
```

Server has been running continuously for

- 83 days
- 18 hours
- 29 minutes

High uptime generally indicates system stability, but administrators should also consider planned maintenance and kernel updates.

---

### Logged-in Users

```
4 users
```

Four users are currently logged into the system.

Verify users

```bash
who
```

or

```bash
w
```

---

### Load Average

```
0.16 0.03 0.01
```

These values represent the average system load during the last:

| Value | Time Period |
|--------|-------------|
| 0.16 | Last 1 minute |
| 0.03 | Last 5 minutes |
| 0.01 | Last 15 minutes |

Load average indicates how many processes are waiting for CPU or are currently running.

---

## Interview Question

### What is Load Average?

### Answer

Load average represents the average number of runnable or waiting processes.

Example

```
0.50
```

Means

Less than one process is waiting.

Server is healthy.

---

Example

```
4.50
```

On a 4-core server

CPU is almost fully utilized.

---

Example

```
12.30
```

On a 4-core server

The server is overloaded.

Processes are waiting for CPU resources.

---

## Production Scenario

Application team reports that the server is slow.

First command

```bash
uptime
```

Output

```
load average: 12.45 11.80 10.92
```

This indicates sustained high system load.

Next investigation steps:

```bash
top
```

```bash
ps aux --sort=-%cpu | head
```

```bash
free -m
```

```bash
iostat
```

---

## Useful Options

Pretty format

```bash
uptime -p
```

Example

```
up 83 days, 18 hours, 29 minutes
```

Show boot time

```bash
uptime -s
```

Example

```
2026-04-30 16:48:01
```

---

# 3. hostname

## Interview Question

### What is the hostname command?

### Answer

The `hostname` command displays or changes the system hostname. :contentReference[oaicite:2]{index=2}

Display hostname

```bash
hostname
```

Display IP

```bash
hostname -I
```

Display FQDN

```bash
hostname -f
```

---

## Production Usage

Used to verify

- Server identity
- DNS configuration
- Cluster nodes
- Kubernetes worker nodes

---

# 4. uname

## Interview Question

### Explain uname.

### Answer

`uname` displays Linux kernel and operating system information. :contentReference[oaicite:3]{index=3}

Display everything

```bash
uname -a
```

Kernel version

```bash
uname -r
```

Machine architecture

```bash
uname -m
```

Operating System

```bash
uname -o
```

---

## Production Usage

Frequently used after SSH login to verify

- Kernel version
- Architecture
- Operating system

---

# 5. which

## Interview Question

### What does the which command do?

### Answer

`which` shows the full path of an executable command. :contentReference[oaicite:4]{index=4}

Example

```bash
which python3
```

Output

```
/usr/bin/python3
```

List all matches

```bash
which -a python
```

---

## Production Usage

Useful for identifying which binary is executed when multiple versions are installed.

---

# 6. cal

## Interview Question

### Explain cal.

### Answer

Displays a calendar.

Current month

```bash
cal
```

Current year

```bash
cal 2026
```

---

## Production Usage

Helpful for planning maintenance windows and scheduled activities.

---

# 7. bc

## Interview Question

### What is bc?

### Answer

`bc` is a command-line calculator.

Example

```bash
bc
```

```
10+20
30
```

Floating-point calculation

```bash
echo "10/3" | bc -l
```

Output

```
3.333333333
```

---

## Production Usage

Used in shell scripts for arithmetic operations such as CPU, memory, or storage calculations.

---

# Best Practices

- Verify system time before troubleshooting.
- Check uptime after server reboot.
- Understand load average relative to CPU cores.
- Use `uname -a` after logging into an unfamiliar server.
- Use `which` to confirm executable paths before scripting.

---

# Top Interview Questions

1. Explain the `date` command.
2. Explain the `uptime` command.
3. What do the three load average values represent?
4. How do you interpret load average on a 4-core server?
5. Difference between uptime and boot time?
6. Explain `hostname`.
7. Difference between `hostname` and `hostname -I`.
8. Explain `uname -a`.
9. Difference between `uname -r` and `uname -a`.
10. Explain the `which` command.
11. What is `bc` used for?
12. How do you display the current year's calendar?
13. Which command would you use immediately after SSHing into a production server?
14. Which command helps identify the full path of an executable?
15. A server feels slow—what commands would you run first?

---

# Quick Revision

| Command | Purpose |
|----------|---------|
| `date` | Display or set system date/time |
| `uptime` | Show uptime and load average |
| `hostname` | Display system hostname |
| `hostname -I` | Show IP addresses |
| `uname -a` | Display all system information |
| `uname -r` | Display kernel release |
| `which` | Show executable path |
| `cal` | Display calendar |
| `bc` | Command-line calculator |

---

## Module Complete ✅
