# Linux System Task Notes

## 1. History & Process of Boot to Login

### Boot Process Overview
When a Linux system powers on, it goes through these stages in order:

**Stage 1: BIOS/UEFI**
- The firmware built into the motherboard runs first.
- It performs POST (Power-On Self Test) to check hardware (RAM, CPU, disks).
- BIOS is older/legacy, UEFI is modern and supports larger disks, faster boot, secure boot.
- After POST, BIOS/UEFI looks for a bootable device and hands control to the bootloader.

**Stage 2: GRUB (Bootloader)**
- GRUB (GRand Unified Bootloader) is loaded from the disk's boot sector (MBR) or EFI partition.
- It shows a menu (if configured) to choose OS or kernel version.
- GRUB loads the Linux kernel and initial ramdisk (initrd/initramfs) into memory.
- Config file location: `/boot/grub/grub.cfg` or `/etc/default/grub`.

**Stage 3: Kernel Initialization**
- The kernel decompresses itself and initializes core hardware (CPU, memory management, drivers).
- It mounts the initramfs (temporary root filesystem) to load necessary drivers.
- Then it mounts the real root filesystem (`/`) as defined in `/etc/fstab`.
- Finally, the kernel starts the first process, PID 1.

**Stage 4: systemd (init system)**
- systemd is PID 1, the first user-space process.
- It initializes all services, mounts filesystems, sets up networking.
- Runs targets (similar to old runlevels), e.g., `multi-user.target`, `graphical.target`.
- Starts background services (daemons) like SSH, cron, networking.

**Stage 5: Login**
- Once systemd reaches the login target, it starts a display manager (GUI) or a getty (text login) on TTYs.
- User enters username/password.
- PAM (Pluggable Authentication Modules) verifies credentials.
- On success, a shell (bash, zsh) or desktop session starts.

### Tracking Boot with dmesg and Logs
- `dmesg` shows kernel ring buffer messages — useful for hardware detection, driver loading, errors during boot.
  ```
  dmesg | less
  dmesg | grep -i error
  ```
- `journalctl -b` shows systemd boot logs for the current boot.
- `journalctl -b -1` shows logs from the previous boot.
- `systemd-analyze` shows how long boot took.
- `systemd-analyze blame` shows which services took the longest to start.
- Key stages to look for in dmesg: CPU detection, memory init, disk/filesystem mount, network driver load, USB detection.

---

## 2. Basic File Operations

### Creating Directories and Files
```bash
mkdir linux_basics
cd linux_basics
touch file1.txt file2.txt
nano file3.txt      # opens editor, type content, Ctrl+O to save, Ctrl+X to exit
```

### Moving Files
```bash
mkdir subdir
mv file1.txt subdir/
```
- `mv` moves or renames files. If source and destination are on the same filesystem, it's just a pointer change (fast).

### Copying Files
```bash
cp file2.txt subdir/
cp -r subdir subdir_backup   # -r for recursive (copy folders)
```

### Deleting Files and Directories
```bash
rm file2.txt          # delete a file
rm -r subdir_backup   # delete a folder and its contents
rmdir empty_folder    # only works on empty directories
```

### Soft Links vs Hard Links
```bash
ln -s file1.txt link_to_file1     # soft link
ln file1.txt hardlink_file1       # hard link
```
- **Soft link (symbolic link):** A pointer/shortcut to the original file path. If the original file is deleted, the link breaks (dangling link). Can link across filesystems and to directories.
- **Hard link:** Another name for the same inode (same actual data on disk). Deleting the original doesn't break it, since the data still exists as long as one hard link remains. Cannot link across filesystems or to directories.

### Compressing and Extracting
```bash
tar -cvf archive.tar linux_basics/        # create tar archive
gzip archive.tar                          # compress it -> archive.tar.gz

# or do both in one step:
tar -czvf archive.tar.gz linux_basics/

# extract:
tar -xzvf archive.tar.gz
```
- `-c` create, `-x` extract, `-z` gzip compression, `-v` verbose, `-f` filename.

---

## 3. cp vs scp vs rsync

### cp -r (local copy)
```bash
cp -r source_dir destination_dir
```
- Copies files/folders within the same system.

### scp (secure copy over network)
```bash
scp -r local_folder user@remote_host:/path/to/destination
scp user@remote_host:/path/to/file.txt ./local_folder
```
- Uses SSH for encrypted transfer between two systems.
- Simple but copies everything every time, even if files already exist and are unchanged.

### rsync (synchronize)
```bash
rsync -avz source_dir/ user@remote_host:/path/to/destination/
```
- `-a` archive mode (preserves permissions, timestamps, symlinks)
- `-v` verbose
- `-z` compress during transfer

**Why rsync is more efficient than scp for large directories:**
- rsync only transfers the differences (delta) between source and destination files, not the whole file again.
- It can resume interrupted transfers.
- scp always copies everything fully, even unchanged files, which wastes bandwidth and time on large datasets.

### rsync with --delete
```bash
rsync -avz --delete source_dir/ destination_dir/
```
- Makes destination an exact mirror of source — deletes any files in destination that no longer exist in source.
- Useful for backups where you want a clean, exact copy.

---

## 4. File Permissions and Ownership

### Understanding Permissions
Every file/directory has three permission sets: **owner**, **group**, **others**.
Each set has: **r** (read=4), **w** (write=2), **x** (execute=1).

```bash
touch myfile.txt
chmod 755 myfile.txt
```
- `755` = owner: rwx (7), group: r-x (5), others: r-x (5)
- Meaning: owner can read/write/execute, group and others can read/execute but not write.

Common combos:
- `644` = owner rw-, group r--, others r-- (typical for regular files)
- `755` = typical for scripts/executables/directories
- `700` = only owner has any access

### Changing Ownership
```bash
chown newuser myfile.txt          # change owner
chown newuser:newgroup myfile.txt # change owner and group
chgrp newgroup myfile.txt         # change group only
```

### Symbolic Notation
```bash
chmod u+r myfile.txt   # add read permission for user (owner)
chmod g-w myfile.txt   # remove write permission for group
chmod o+x myfile.txt   # add execute for others
chmod a+r myfile.txt   # add read for all (user, group, others)
```
- `u` = user/owner, `g` = group, `o` = others, `a` = all
- `+` add permission, `-` remove permission, `=` set exactly

### Making and Running a Script
```bash
nano myscript.sh
# add: #!/bin/bash
#      echo "Hello World"
chmod +x myscript.sh
./myscript.sh
```

### Special Permissions: setuid, setgid, sticky bit

**setuid (4xxx):** When set on an executable, it runs with the permissions of the file's owner, not the user running it.
```bash
chmod u+s /path/to/program
# example: /usr/bin/passwd has setuid so normal users can change passwords (which requires root access to /etc/shadow)
```

**setgid (2xxx):** On an executable, runs with group permissions of the file. On a directory, new files created inside inherit the directory's group.
```bash
chmod g+s /shared_folder
# useful for team folders where all files should belong to the same group automatically
```

**sticky bit (1xxx):** On a directory, only the file's owner (or root) can delete/rename files inside it, even if others have write access to the directory.
```bash
chmod +t /shared_folder
# /tmp is a classic example — everyone can write, but only file owners can delete their own files
```

---

## 5. Conditional Statements in Bash

### Check if Directory Exists
```bash
#!/bin/bash
DIR="/home/user/testdir"
if [ -d "$DIR" ]; then
    echo "Directory exists"
else
    echo "Directory not found"
fi
```

### Check File Permissions (readable, writable, executable)
```bash
#!/bin/bash
FILE="myfile.txt"
if [ -r "$FILE" ]; then echo "Readable"; fi
if [ -w "$FILE" ]; then echo "Writable"; fi
if [ -x "$FILE" ]; then echo "Executable"; fi
```

### Check if User is Root
```bash
#!/bin/bash
if [ "$EUID" -eq 0 ]; then
    echo "You are root"
else
    echo "You are a normal user"
fi
```
- `$EUID` is the effective user ID; root's EUID is always 0.

### Script with Command-Line Arguments
```bash
#!/bin/bash
if [ "$1" == "start" ]; then
    echo "Starting service..."
elif [ "$1" == "stop" ]; then
    echo "Stopping service..."
else
    echo "Usage: $0 {start|stop}"
fi
```
Run with: `./script.sh start`

### For Loop Over Files and Print Permissions
```bash
#!/bin/bash
for file in *; do
    if [ -e "$file" ]; then
        ls -l "$file"
    fi
done
```

---

## 6. Using nano Editor

### Basic Usage
```bash
nano myfile.txt
```

### Common Shortcuts
| Action | Shortcut |
|---|---|
| Save file | Ctrl+O then Enter |
| Exit | Ctrl+X |
| Search | Ctrl+W |
| Cut a line | Ctrl+K |
| Paste | Ctrl+U |
| Go to line number | Ctrl+_ (then type line number) |
| Select text | Alt+A (start mark), move cursor, then Ctrl+K to cut |

### Adding Comments in a Bash Script with nano
```bash
#!/bin/bash
# This script backs up a folder
# Author: Afrooz
# Date: created for practice

SOURCE="/home/user/data"   # folder to back up
DEST="/home/user/backup"   # backup destination

cp -r "$SOURCE" "$DEST"    # perform the copy
echo "Backup complete"     # confirmation message
```
- Comments start with `#` and are ignored by bash when running the script — useful for explaining logic.

---

## 7. Cronjob

### Backup Script
```bash
#!/bin/bash
# backup.sh - backs up and compresses a directory
SOURCE="/home/user/important_data"
DEST="/home/user/backups"
DATE=$(date +%Y-%m-%d)

mkdir -p "$DEST"
tar -czf "$DEST/backup_$DATE.tar.gz" "$SOURCE"
echo "Backup created: backup_$DATE.tar.gz"
```
```bash
chmod +x backup.sh
```

### Schedule Daily at Midnight
```bash
crontab -e
```
Add this line:
```
0 0 * * * /home/user/backup.sh
```
- Cron format: `minute hour day month weekday`
- `0 0 * * *` = at 00:00 every day.

### Verify Execution
```bash
grep CRON /var/log/syslog
# or on some systems:
journalctl -u cron
```

### Weekly Cron Job Emailing Process List
```
0 8 * * 1 ps aux | mail -s "Weekly Process List" you@example.com
```
- `0 8 * * 1` = 8:00 AM every Monday (1 = Monday in cron's weekday field).
- Requires `mailutils` or similar mail package installed and configured.

---

## 8. find Command

### Locate .log Files
```bash
find /var/log -name "*.log"
```

### Files Modified in Last 7 Days
```bash
find /path/to/dir -mtime -7
```
- `-mtime -7` means modified within the last 7 days (negative = less than).

### Find and Delete Files Larger Than 100MB
```bash
find /path/to/dir -type f -size +100M -exec rm {} \;
```
- `-size +100M` = larger than 100 megabytes.
- Always test with `-exec ls -lh {} \;` first before using `rm`, to confirm which files will be deleted.

### Delete Files Older Than 30 Days
```bash
find /path/to/dir -type f -mtime +30 -exec rm {} \;
```
- `{}` is replaced by each matched filename; `\;` ends the exec command.

### Find Files with Specific Permissions (e.g., 777)
```bash
find / -type f -perm 777
```
- Finds files where everyone has full read/write/execute — a common security check, since 777 files are a risk.

---

## 9. Text Manipulation using grep

### Search for a Word in a Log File
```bash
grep "error" /var/log/syslog
grep -i "error" /var/log/syslog   # case-insensitive
```

### Combine grep with find
```bash
find /var/log -name "*.log" -exec grep -l "error" {} \;
```
- Finds all `.log` files, then searches each for "error", printing filenames that match (`-l` = list filenames only).

### grep with Regex — Lines Starting with a Number
```bash
grep -E "^[0-9]" file.txt
```
- `^` means start of line, `[0-9]` matches any digit.

### Chain grep with awk or sed
```bash
grep "error" logfile.txt | awk '{print $1, $2}'   # print first two fields of matching lines
grep "error" logfile.txt | sed 's/error/ERROR/'    # replace text in matching lines
```

### Monitor Log File and Alert on Keywords
```bash
#!/bin/bash
LOGFILE="/var/log/syslog"
tail -Fn0 "$LOGFILE" | while read line; do
    if echo "$line" | grep -qi "error\|fail"; then
        echo "ALERT: $line"
        # could also send an email or notification here
    fi
done
```
- `tail -F` follows the file as it grows (like `tail -f` but also handles log rotation).

---

## 10. Process Monitoring

### htop
```bash
htop
```
- Interactive, color-coded view of CPU, memory usage, and all running processes.
- Install if missing: `sudo apt install htop`

### ps aux
```bash
ps aux
```
- Shows all running processes with details: user, PID, CPU%, MEM%, start time, command.
- Columns: USER, PID, %CPU, %MEM, VSZ, RSS, TTY, STAT, START, TIME, COMMAND.

### Find Processes for a Specific Program
```bash
ps aux | grep chrome
pgrep chrome
```

### Kill a Process by PID
```bash
kill 1234        # sends SIGTERM (graceful stop)
kill -9 1234      # sends SIGKILL (force stop)
```

### Kill All Processes for a Specific User
```bash
pkill -u username
```

### nice and renice (Process Priority)
```bash
nice -n 10 ./myscript.sh     # start a process with lower priority (higher niceness = lower priority)
renice -n -5 -p 1234          # change priority of already-running process (PID 1234)
```
- Priority range: -20 (highest priority) to 19 (lowest priority).
- Only root can set negative (higher priority) values.
- Lower niceness = more CPU time given to the process; useful to speed up important tasks or slow down background jobs.

---

## Quick Reference Summary

| Task | Command |
|---|---|
| Boot logs | `dmesg`, `journalctl -b` |
| Copy folder | `cp -r` |
| Secure remote copy | `scp -r` |
| Efficient sync | `rsync -avz --delete` |
| Change permissions | `chmod 755` |
| Change owner | `chown user:group` |
| Directory check | `[ -d "$DIR" ]` |
| Edit file | `nano file` |
| Schedule task | `crontab -e` |
| Find old/large files | `find ... -mtime / -size` |
| Search text | `grep -i "text" file` |
| List processes | `ps aux`, `htop` |
| Kill process | `kill -9 PID` |
| Change priority | `nice`, `renice` |
