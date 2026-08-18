# Linux Basics — Tasks 01–13

This module covers fundamental Linux administration and DevOps concepts through practical command-line exercises.

The tasks were completed in a Linux/WSL environment, with an Ubuntu Server used later for SSH, SCP, and SFTP practice.

---

# Module Objectives

The main objectives of this module were to learn:

* Linux boot process
* File and directory management
* Storage and disk management
* `cp`, `scp`, and `rsync`
* Linux permissions and ownership
* Bash scripting
* Nano editor
* Cron jobs
* `find`
* `grep`
* Text manipulation
* Processes and performance monitoring
* SSH, SCP, and SFTP
* Practical troubleshooting methodology

---

# Directory Structure

```text
module-1-linux-basics/
│
├── 01-boot-process/
├── 02-file-operations/
├── 03-storage/
├── 04-cp-scp-rsync/
├── 05-permissions/
├── 06-bash-scripting/
├── 07-nano/
├── 08-cron/
├── 09-find/
├── 10-grep/
├── 11-processes/
├── 12-ssh-sftp/
└── 13-troubleshooting-challenge/
```

---

# Task 01 — Linux Boot Process

## Objective

Understand what happens from powering on a Linux system until the user receives a login prompt.

## Topics Covered

* BIOS/UEFI
* Bootloader
* GRUB
* Linux kernel
* Initramfs
* `systemd`
* PID 1
* Services
* Login
* `dmesg`
* `journalctl`
* `systemctl`

## Boot Flow

```text
Power On
   ↓
BIOS / UEFI
   ↓
GRUB
   ↓
Linux Kernel
   ↓
Initramfs
   ↓
systemd (PID 1)
   ↓
System Services
   ↓
Login
```

## Investigation

Commands used included:

```bash
uname -a
dmesg
dmesg | grep -Ei "disk|storage|nvme|scsi"
dmesg | grep -Ei "network|eth|net|veth"
ip link
systemctl get-default
systemctl list-units --type=service
journalctl
```

The WSL environment was also identified as WSL2 with a Microsoft-provided Linux kernel.

---

# Task 02 — File and Directory Operations

## Objective

Learn how to create, remove, copy, move, and inspect files and directories.

## Commands Covered

```bash
pwd
ls
ls -l
mkdir
mkdir -p
touch
nano
cp
cp -r
mv
rm
rm -r
```

Examples:

```bash
mkdir dir1 dir2 dir3
touch file1.txt file2.txt file3.txt
cp file1.txt dir1/
mv file2.txt dir2/
cp -r dir1 dir3/
```

## Important Concepts

* Absolute paths
* Relative paths
* Files
* Directories
* Recursive operations
* File deletion

---

# Task 03 — Storage

## Objective

Understand Linux storage, filesystems, partitions, and disk usage.

## Important Commands

```bash
lsblk
df -h
du -sh
mount
findmnt
```

These commands were used to inspect:

* Block devices
* Filesystems
* Mounted filesystems
* Available disk space
* Directory sizes

---

# Task 04 — cp, scp and rsync

## Objective

Understand different methods of copying and synchronizing files.

## `cp`

Used for local file and directory copying.

```bash
cp file1.txt file2.txt
cp -r source/ destination/
```

## `scp`

Used to securely copy files between systems through SSH.

```bash
scp file.txt user@server:/path/
```

## `rsync`

Used to efficiently synchronize files and directories.

```bash
rsync -av source/ destination/
```

Remote synchronization can use SSH:

```bash
rsync -avz source/ user@server:/destination/
```

## Key Difference

```text
cp
→ local copying

scp
→ secure remote copying

rsync
→ efficient synchronization
```

`rsync` can transfer only changed data during subsequent synchronizations, making it particularly useful for backups and deployments.

---

# Task 05 — Linux Permissions

## Objective

Understand Linux file permissions, ownership, and special permissions.

## Concepts

* User/owner
* Group
* Others
* Read
* Write
* Execute
* `chmod`
* `chown`
* `chgrp`
* SUID
* SGID

Permissions can be viewed using:

```bash
ls -l
```

Example:

```text
-rwxr-xr--
```

The permissions are divided into:

```text
owner | group | others
```

Permission values:

```text
r = 4
w = 2
x = 1
```

Example:

```bash
chmod 755 script.sh
```

means:

```text
Owner  → rwx
Group  → r-x
Others → r-x
```

Ownership:

```bash
chown user file
chgrp group file
```

Special permissions such as SUID and SGID were also studied.

---

# Task 06 — Bash Scripting

## Objective

Learn basic Bash programming and automate Linux operations.

## Topics

* Variables
* Arguments
* Conditions
* `if`
* `case`
* Loops
* File tests
* Permissions
* Exit status
* `$1`
* `$EUID`

Example:

```bash
#!/bin/bash

if [ -d "$1" ]
then
    echo "Directory exists"
else
    echo "Directory does not exist"
fi
```

## File Tests

Examples:

```bash
-d
-f
-r
-w
-x
```

## Root Check

```bash
if [ "$EUID" -eq 0 ]
then
    echo "Running as root"
fi
```

## Loops

`for` loops were used to process multiple files.

---

# Task 07 — Nano

## Objective

Learn to create and edit files using the Nano terminal editor.

## Important Nano Commands

```text
Ctrl + O → Save
Ctrl + X → Exit
Ctrl + W → Search
Ctrl + K → Cut line
Ctrl + U → Paste
Ctrl + G → Help
```

Nano was used to create and edit scripts, documentation, and configuration files.

---

# Task 08 — Cron Jobs

## Objective

Understand scheduled tasks using cron.

## Important Commands

```bash
crontab -l
crontab -e
systemctl status cron
journalctl -u cron
```

A backup script was created using `tar` and `gzip`.

Example:

```bash
backup_file=$1
current_date=$(date +%Y-%m-%d)
tar -czvf backup/$current_date.tar.gz "$backup_file"
```

A test cron job was configured to run every minute.

```text
* * * * * /path/to/backup.sh /path/to/backup-source
```

## Cron Flow

```text
cron daemon
     ↓
scheduled time
     ↓
backup.sh
     ↓
tar
     ↓
compressed backup
```

The cron logs were inspected using:

```bash
journalctl -u cron
```

This confirmed that cron was executing the scheduled job.

---

# Task 09 — find

## Objective

Learn how to locate files based on different conditions.

## Search by Extension

```bash
find . -name "*.log"
```

## Recently Modified Files

```bash
find . -mtime -1
```

## Files Larger Than a Size

```bash
find . -size +1M
```

## Older Files

```bash
find . -mtime +7
```

## Search by Permissions

```bash
find . -perm 644
```

## Search by Name

```bash
find . -name "file1.txt"
```

## Search by Owner

```bash
find . -user afrooz
```

## `-exec`

`-exec` allows another command to be performed on the files found by `find`.

Example:

```bash
find . -name "*.log" -exec ls -l {} \;
```

## Safety

Deletion exercises were restricted to disposable files inside:

```text
09-find/test-delete/
```

Automated deletion commands were not run against system directories such as:

```text
/
/home
/var
```

## Important Lesson

Always inspect the output of `find` before combining it with an automatic deletion command.

---

# Task 10 — Text Manipulation with grep

## Objective

Learn how to search and process text using `grep`, `awk`, `sed`, and `cut`.

The sample log contained:

* INFO messages
* WARNING messages
* ERROR messages
* IP addresses
* Dates
* Numbers

## Basic Search

```bash
grep "ERROR" sample.log
```

## Case-Insensitive Search

```bash
grep -i "error" sample.log
```

## Line Numbers

```bash
grep -n "ERROR" sample.log
```

## Count Matches

```bash
grep -c "ERROR" sample.log
```

## Lines Beginning With a Number

```bash
grep "^[0-9]" sample.log
```

## Exclude a Pattern

```bash
grep -vi "error" sample.log
```

## Recursive Search

```bash
grep -r "ERROR" logs/
```

## awk

Example:

```bash
grep "ERROR" sample.log | awk '{print $NF}'
```

`awk` was used to extract a field from matching log entries.

## cut

`cut` can extract fields or character ranges from structured text.

Example:

```bash
echo "ERROR:Database failure" | cut -d ":" -f 1
```

## Pipe

The pipe:

```text
|
```

passes the standard output of one command into the standard input of another command.

Example:

```bash
grep "ERROR" sample.log | awk '{print $NF}'
```

## Script Exercise

A `log_checker.sh` script was created to check for keywords such as:

```text
ERROR
WARNING
```

and display alert messages when they were found.

---

# Task 11 — Processes and Performance Monitoring

## Objective

Understand Linux processes, process IDs, process states, CPU/memory usage, signals, and process priority.

## Important Commands

```bash
ps
top
htop
pgrep
kill
pkill
nice
renice
```

## Test Process

A safe process was started:

```bash
sleep 300 &
```

The `&` runs the command in the background.

The shell returned a PID.

## Inspect a Process

```bash
ps -p 695 -f
```

Detailed process information was displayed.

Another useful command was:

```bash
ps -p 695 -o pid,ppid,user,%cpu,%mem,stat,cmd
```

This displayed:

* PID
* Parent PID
* User
* CPU usage
* Memory usage
* Process state
* Command

## Multiple Processes

Several safe test processes were started:

```bash
sleep 300 &
sleep 300 &
sleep 300 &
```

Their PIDs were found with:

```bash
pgrep sleep
```

They were terminated using:

```bash
pkill sleep
```

A specific process was terminated using:

```bash
kill PID
```

## Process Priority

A process was started with a modified nice value:

```bash
nice -n 10 sleep 300 &
```

Priority was checked with:

```bash
ps -p PID -o pid,ni,pri,stat,cmd
```

The nice value was changed with:

```bash
renice 15 -p PID
```

## Key Concepts

### PID

Process ID — a unique number assigned to a running process.

### PPID

Parent Process ID — identifies the process that created the current process.

### Zombie

A terminated child process whose parent has not yet collected its exit status.

### SIGTERM

Requests graceful termination.

### SIGKILL

Immediately terminates a process and cannot normally be caught or handled by the process.

### Why not `kill -9` first?

SIGKILL does not give the application an opportunity to clean up resources. SIGTERM should normally be tried first.

---

# Task 12 — SSH, SCP and SFTP

## Objective

Learn secure remote access and secure file transfer between Linux systems.

Two Linux environments were used:

```text
WSL Ubuntu
      │
      │ SSH / SCP / SFTP
      ▼
Ubuntu Server
scp-test
10.0.110.100
```

## SSH

SSH stands for:

**Secure Shell**

It is used for secure remote access.

Default port:

```text
22
```

## SSH Client

The client initiates the connection:

```bash
ssh root@10.0.110.100
```

## SSH Server

The server runs:

```text
sshd
```

The service was checked with:

```bash
systemctl status ssh
```


## SSH Authentication

Password authentication was demonstrated.

SSH key authentication was also configured.

Key generation:

```bash
ssh-keygen -t ed25519
```

This creates:

```text
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub
```

### Private Key

```text
id_ed25519
```

Must remain secret.

### Public Key

```text
id_ed25519.pub
```

Can be installed on the server.

## Install Public Key

```bash
ssh-copy-id root@10.0.110.100
```

The public key is stored on the server in:

```text
~/.ssh/authorized_keys
```

## known_hosts

The client stores known SSH server identities in:

```text
~/.ssh/known_hosts
```

Remember:

```text
authorized_keys
→ server
→ who is allowed to log in?

known_hosts
→ client
→ which servers have I previously connected to?
```

## SCP

SCP securely copies files using SSH.

Upload:

```bash
scp file.txt root@10.0.110.100:/root/
```

Download:

```bash
scp root@10.0.110.100:/root/file.txt .
```

## SFTP

SFTP provides interactive secure file transfer.

Connect:

```bash
sftp root@10.0.110.100
```

Common commands:

```text
ls
pwd
cd
put
get
exit
```

## FTP vs SFTP

| Feature        | FTP                        | SFTP                 |
| -------------- | -------------------------- | -------------------- |
| Default port   | 21                         | 22                   |
| Encryption     | Not provided by plain FTP  | SSH encryption       |
| Security       | Weak on untrusted networks | Secure               |
| Authentication | Username/password          | Password or SSH keys |
| Technology     | FTP                        | SSH                  |

Plain FTP is generally unsuitable for sensitive information over an untrusted network because communication is not encrypted by default.

---

# Task 13 — Troubleshooting Challenge

## Scenario

A Bash backup script was working yesterday.

Today:

```text
Cron appears to run
        ↓
No backup is created
```

The objective is not necessarily to immediately find the fault.

The objective is to demonstrate a **logical troubleshooting process**.

---

# Troubleshooting Methodology

## 1. Check the Cron Job

```bash
crontab -l
```

Verify:

* The job still exists.
* The schedule is correct.
* The script path is correct.
* Arguments are correct.

---

# 2. Check Paths

Verify that the script exists:

```bash
ls -l /path/to/backup.sh
```

Verify the source directory:

```bash
ls -ld /path/to/backup-source
```

Verify the backup destination:

```bash
ls -ld /path/to/backup
```

Incorrect paths are a common cause of cron failures.

---

# 3. Check Permissions

Check the script:

```bash
ls -l backup.sh
```

Make sure it is executable.

For example:

```text
-rwxr-xr-x
```

Also verify that the cron user can read the source and write to the backup destination.

---

# 4. Manually Execute the Script

Run the same command used by cron:

```bash
/path/to/backup.sh /path/to/backup-source
```

This helps separate:

```text
Script problem
      vs
Cron problem
```

If the script fails manually, investigate the script itself.

If it succeeds manually but fails through cron, investigate the cron environment.

---

# 5. Check the Exit Code

Immediately after execution:

```bash
echo $?
```

Interpretation:

```text
0       → successful execution
non-zero → failure/exception reported
```

A non-zero exit code does not necessarily mean the script did not execute. It means the command reported an unsuccessful result.

---



# 6. Check Disk Space

Check available disk space:

```bash
df -h
```

Also check inode availability:

```bash
df -i
```

A full filesystem can prevent a backup from being created even when the script itself is correct.

---


# Key Linux Concepts Learned

Throughout the module, the following concepts were practiced:

```text
Linux filesystem
        ↓
Files & directories
        ↓
Permissions & ownership
        ↓
Processes
        ↓
Bash scripting
        ↓
Cron automation
        ↓
File searching
        ↓
Text processing
        ↓
Remote access
        ↓
Secure file transfer
        ↓
Troubleshooting
```

---

# Important Commands Reference

## Navigation

```bash
pwd
ls
cd
```

## Files

```bash
touch
cp
mv
rm
mkdir
```

## Permissions

```bash
chmod
chown
chgrp
```

## Storage

```bash
lsblk
df
du
mount
```

## Processes

```bash
ps
top
htop
pgrep
kill
pkill
nice
renice
```

## Searching

```bash
find
grep
```

## Text Processing

```bash
awk
sed
cut
```

## Scheduling

```bash
crontab
systemctl
journalctl
```

## Remote Access

```bash
ssh
scp
sftp
rsync
```

## Networking

```bash
ip
ping
ss
```

---

# Overall Learning Outcome

After completing Tasks 01–13, the module provided practical exposure to fundamental Linux administration and DevOps operations.

The progression was:

```text
01  Boot Process
 ↓
02  File Operations
 ↓
03  Storage
 ↓
04  cp / scp / rsync
 ↓
05  Permissions
 ↓
06  Bash Scripting
 ↓
07  Nano
 ↓
08  Cron
 ↓
09  find
 ↓
10  grep & Text Manipulation
 ↓
11  Processes & Performance
 ↓
12  SSH / SCP / SFTP
 ↓
13  Troubleshooting
```

The final troubleshooting challenge brought the concepts together by requiring a systematic approach involving **cron, Bash scripts, permissions, paths, environment variables, logs, exit codes, and system resources**.

## Completion

```text
Tasks completed: 01–13
Module: Linux Basics
Focus: Linux Administration & DevOps Fundamentals
Environment: WSL Ubuntu + Ubuntu Server
Status: Completed
```
