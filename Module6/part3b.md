# Linux Networking & Services Interview Handbook
# Part 3B – SCP, SFTP, FTP, vsftpd & Rsync

---

# Table of Contents

1. SCP
2. SFTP
3. FTP
4. vsftpd
5. Rsync
6. Production Scenarios
7. Troubleshooting
8. Best Practices
9. Commands to Remember
10. Interview Questions
11. Quick Revision

---

# 1. SCP (Secure Copy)

## Definition

SCP (Secure Copy Protocol) is a command-line utility used to securely transfer files and directories between two Linux systems over SSH.

Unlike FTP, SCP encrypts both authentication and file data using SSH.

Default Port:

```bash
22
```

---

## Why do we use SCP?

- Secure file transfer
- Backup configuration files
- Deploy application packages
- Copy logs from production servers
- Transfer scripts between servers

---

## Syntax

Copy local file to remote server

```bash
scp file.txt user@server:/home/user/
```

Copy remote file to local machine

```bash
scp user@server:/home/user/file.txt .
```

Copy directory

```bash
scp -r project user@server:/opt/
```

Specify SSH port

```bash
scp -P 2222 file.txt user@server:/tmp/
```

---

## Production Example

Deploy a new application package:

```bash
scp app.war deploy@192.168.1.50:/opt/tomcat/webapps/
```

---

## Interview Answer

> SCP is a secure file transfer utility that uses SSH for encryption. It is mainly used to copy files between Linux servers. Since it uses SSH, authentication and transferred data are encrypted, making it much more secure than traditional FTP.

---

# 2. SFTP (SSH File Transfer Protocol)

## Definition

SFTP is a secure file transfer protocol that runs over SSH.

Unlike SCP, SFTP provides an interactive session similar to an FTP client.

---

## Why use SFTP?

- Upload files
- Download files
- Rename files
- Delete files
- Browse remote directories

---

## Connect

```bash
sftp user@server
```

---

## Common Commands

Display remote directory

```bash
pwd
```

Display local directory

```bash
lpwd
```

Upload file

```bash
put file.txt
```

Download file

```bash
get file.txt
```

List files

```bash
ls
```

Exit

```bash
exit
```

---

## SCP vs SFTP

| SCP | SFTP |
|------|------|
| Simple file copy | Interactive session |
| Faster | More features |
| No directory browsing | Directory browsing |
| Uses SSH | Uses SSH |

---

## Interview Question

### Which one do you prefer?

Answer:

For simple copying, I prefer SCP.

For managing multiple files and browsing directories, I prefer SFTP.

---

# 3. FTP (File Transfer Protocol)

## Definition

FTP is a protocol used for transferring files between systems.

Default Ports

```text
21 Control Connection

20 Data Connection
```

---

## Disadvantages

FTP sends

- Username
- Password
- Data

without encryption.

For production systems, SFTP or SCP should be preferred.

---

## Common FTP Commands

Connect

```bash
ftp server-ip
```

Upload

```bash
put file.txt
```

Download

```bash
get file.txt
```

Exit

```bash
bye
```

---

## Interview Answer

> FTP is an older file transfer protocol that communicates over ports 20 and 21. Since it does not encrypt authentication or data, it is considered insecure. Modern environments typically use SFTP or SCP instead.

---

# 4. vsftpd

## Definition

vsftpd stands for

Very Secure FTP Daemon.

It is one of the most commonly used FTP servers on Linux.

---

## Installation

RHEL

```bash
sudo yum install vsftpd
```

---

## Start Service

```bash
sudo systemctl enable vsftpd

sudo systemctl start vsftpd
```

---

## Check Status

```bash
systemctl status vsftpd
```

---

## Configuration File

```text
/etc/vsftpd/vsftpd.conf
```

---

## Common Configuration Options

Enable local users

```text
local_enable=YES
```

Enable uploads

```text
write_enable=YES
```

Anonymous access

```text
anonymous_enable=NO
```

---

## Restart Service

```bash
sudo systemctl restart vsftpd
```

---

## Verify Listening Port

```bash
ss -tulpn | grep 21
```

---

## Production Scenario

Developers cannot upload release packages.

Check:

- vsftpd service
- Firewall
- SELinux
- Configuration
- User permissions

---

# 5. Rsync

## Definition

Rsync (Remote Synchronization) is a fast and efficient utility used to synchronize files and directories between systems.

Unlike SCP, rsync transfers only changed data, making it ideal for backups and large file synchronization.

---

## Why use Rsync?

- Incremental backup
- Faster transfers
- Synchronize directories
- Preserve permissions
- Resume interrupted transfers

---

## Syntax

Local copy

```bash
rsync -av source/ destination/
```

Remote copy

```bash
rsync -av source/ user@server:/backup/
```

Using SSH

```bash
rsync -av -e ssh project/ user@server:/opt/project/
```

Delete removed files

```bash
rsync -av --delete source/ backup/
```

---

## Important Options

Archive Mode

```bash
-a
```

Verbose

```bash
-v
```

Compression

```bash
-z
```

Progress

```bash
--progress
```

Delete removed files

```bash
--delete
```

---

## Production Example

Synchronize application logs every night

```bash
rsync -avz /var/log user@backup:/backup/logs
```

---

## Interview Answer

> Rsync is used to synchronize files efficiently between servers. Unlike SCP, it transfers only the modified portions of files, reducing bandwidth and transfer time. It is commonly used for backups, disaster recovery, and deployment.

---

# 6. Production Troubleshooting

## Problem

Cannot SSH

Check

```bash
systemctl status sshd
```

```bash
ss -tulpn | grep 22
```

```bash
journalctl -u sshd
```

---

## Problem

SCP Permission Denied

Verify

```bash
ls -ld ~/.ssh
```

```bash
chmod 700 ~/.ssh
```

```bash
chmod 600 ~/.ssh/authorized_keys
```

---

## Problem

FTP Not Working

Check

```bash
systemctl status vsftpd
```

Firewall

```bash
firewall-cmd --list-all
```

Port

```bash
ss -tulpn | grep 21
```

---

## Problem

Rsync Slow

Possible reasons

- Low bandwidth
- Large number of small files
- No compression

Solution

```bash
rsync -avz
```

---

# 7. Best Practices

- Use SSH keys instead of passwords.
- Disable root SSH login.
- Prefer SCP or SFTP over FTP.
- Keep OpenSSH updated.
- Restrict SSH access to authorized users.
- Use rsync for backups instead of repeated SCP copies.
- Monitor SSH authentication logs regularly.
- Disable anonymous FTP unless explicitly required.
- Regularly back up configuration files before making changes.

---

# 8. Commands to Remember

SSH

```bash
ssh user@host
```

Generate key

```bash
ssh-keygen
```

Copy key

```bash
ssh-copy-id user@host
```

SCP upload

```bash
scp file user@host:/tmp/
```

SCP download

```bash
scp user@host:/tmp/file .
```

SFTP

```bash
sftp user@host
```

FTP

```bash
ftp server
```

Start vsftpd

```bash
systemctl start vsftpd
```

Restart vsftpd

```bash
systemctl restart vsftpd
```

Rsync

```bash
rsync -avz source destination
```

---

# 9. Top Interview Questions

### What is SSH?

### Difference between SSH and Telnet?

### Difference between SCP and SFTP?

### Difference between SCP and Rsync?

### Why is FTP considered insecure?

### What is vsftpd?

### Where is vsftpd configuration stored?

### Which port does SSH use?

### Which ports does FTP use?

### How do you troubleshoot SSH connection failures?

### How do you enable key-based authentication?

### How do you copy a file securely between two Linux servers?

### How do you synchronize directories between servers?

### Why is Rsync preferred for backups?

### How do you secure remote access in production?

---

# 10. Quick Revision

| Utility | Purpose | Default Port |
|----------|---------|--------------|
| SSH | Secure remote login | 22 |
| SCP | Secure file copy | 22 |
| SFTP | Secure interactive file transfer | 22 |
| FTP | Plain-text file transfer | 20/21 |
| vsftpd | FTP Server | 21 |
| Rsync | Incremental synchronization | SSH (22) or daemon (873) |

---

## Interview Tips

- Always explain **why** a tool is used, not just its syntax.
- Mention **security**: prefer SSH-based tools over FTP.
- Use **production examples** (deployments, backups, log collection) to demonstrate practical experience.
- When discussing troubleshooting, explain a **step-by-step approach** (service → port → firewall → logs → permissions).
- Highlight **best practices**, such as key-based authentication, disabling root login, and using `rsync` for efficient synchronization.

---

## End of Part 3 – Complete ✅
