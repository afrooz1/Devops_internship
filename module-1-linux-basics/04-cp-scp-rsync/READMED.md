# Task 4 — `cp` vs `scp` vs `rsync`

## Objective

The purpose of this task was to understand the differences between `cp`, `scp`, and `rsync` and practice copying files locally, transferring files using SSH, and synchronizing directories.

All experiments were performed inside:

```text
~/module-1-linux-basics/04-cp-scp-rsync
```

---

## 1. Create the Source Directory Structure

I created a source directory containing multiple files and subdirectories:

```bash
mkdir -p source/config source/logs
```

Then I created three files:

```bash
touch source/file1.txt source/file2.txt source/file3.txt
```

I added content to the files and created files inside the subdirectories:

```bash
echo "This is file 1" > source/file1.txt
echo "This is file 2" > source/file2.txt
echo "This is file 3" > source/file3.txt
echo "application configuration" > source/config/app.conf
echo "application started" > source/logs/app.log
```

I verified the structure using:

```bash
find source -type f
```

Output showed:

```text
source/file2.txt
source/file1.txt
source/file3.txt
source/config/app.conf
source/logs/app.log
```

---

# Part A — `cp`

## 2. Copy the Complete Directory

I created a backup directory:

```bash
mkdir backup
```

Initially, I tried:

```bash
cp source backup/
```

This produced:

```text
cp: -r not specified; omitting directory 'source'
```

This demonstrated that copying a directory requires the recursive option.

I then used:

```bash
cp -r source backup/
```

The `-r` option means **recursive**, allowing `cp` to copy the directory and everything inside it.

## 3. Verify the Copy

I verified the copied files:

```bash
find backup -type f
```

Output:

```text
backup/source/file2.txt
backup/source/file1.txt
backup/source/file3.txt
backup/source/config/app.conf
backup/source/logs/app.log
```

This confirmed that the complete directory structure and its files were copied successfully.

---

# Part B — `scp`

## 4. Check SSH and SCP Availability

I checked the location of SSH and SCP:

```bash
which ssh
which scp
```

Output showed:

```text
/usr/bin/ssh
/usr/bin/scp
```

I also checked the OpenSSH version:

```bash
ssh -V
```

Output:

```text
OpenSSH_9.6p1 Ubuntu-3ubuntu13.18
```

## 5. Start the SSH Server

The SSH service was initially inactive, so I started it:

```bash
sudo systemctl start ssh
```

I verified its status:

```bash
sudo systemctl status ssh
```

The service was successfully running and listening on port 22.

## 6. Transfer a File Using `scp`

I created a destination directory inside the assessment directory:

```bash
mkdir scp-destination
```

I then transferred `file1.txt` using `scp`:

```bash
scp source/file1.txt afrooz@<server-ip>:~/module-1-linux-basics/04-cp-scp-rsync/scp-destination/
```

The transfer completed successfully with:

```text
100%  127
```

I verified the transferred file:

```bash
ls scp-destination
cat scp-destination/file1.txt
```

This is file 1
This file will be trnsfered to another linux server using ssh
we will use our linux server with ssh to tesh SCP

### What this demonstrated

`scp` uses **SSH** to securely transfer files between Linux systems.

In this lab, both the source and destination were the same WSL Linux environment, but the transfer still used SSH and `scp`.

---

# Part C — `rsync`

## 7. Initial Synchronization

I created an `rsync-backup` directory and synchronized the source directory with it.

The synchronization was performed using:

```bash
rsync -av source/ rsync-backup/
```

Important options:

* `-a` — archive mode; preserves important file attributes and recursively processes directories.
* `-v` — verbose; displays information about what rsync is doing.

I verified the synchronized files:

```bash
find rsync-backup -type f
```

The backup contained:

```text
rsync-backup/file1.txt
rsync-backup/file2.txt
rsync-backup/file3.txt
rsync-backup/config/app.conf
rsync-backup/logs/app.log
```

---

## 8. Modify One File

I modified only `file2.txt`:

```bash
echo "this file was added during rsync test" > source/file2.txt
```

Before synchronization, the source and backup contained different versions.

Source:

```bash
cat source/file2.txt
```

Output:

```text
this file was added during rsync test
```

The backup still contained the old content.

---

## 9. Run `rsync` Again

I synchronized the directories again:

```bash
rsync -av source/ rsync-backup/
```

The output included:

```text
sending incremental file list
file2.txt
```

This was important because **only the changed file was identified for transfer**.

I verified the destination:

```bash
cat rsync-backup/file2.txt
```

Output:

```text
this file was added during rsync test
```

The source and destination were now synchronized.

---

# 10. Test `rsync --delete`

I created an extra file that existed only in the backup:

```bash
touch rsync-backup/extra-file.txt
```

I verified it:

```bash
ls rsync-backup
```

The backup contained:

```text
config
extra-file.txt
file1.txt
file2.txt
file3.txt
logs
```

I then synchronized using:

```bash
rsync -av --delete source/ rsync-backup/
```

The output showed:

```text
deleting extra-file.txt
```

I verified the result:

```bash
ls rsync-backup
```

The extra file had been removed, and the source and backup directory structures matched.

### Important Warning

The `--delete` option is potentially dangerous because it removes files from the **destination** that do not exist in the source.

Therefore, I only tested it inside:

```text
~/module-1-linux-basics/04-cp-scp-rsync
```

It should be used carefully, especially when working with real servers or important data.

---

# 11. `cp` vs `scp` vs `rsync`

| Command | Main Purpose                                      |
| ------- | ------------------------------------------------- |
| `cp`    | Copy files/directories locally                    |
| `scp`   | Securely transfer files between systems using SSH |
| `rsync` | Efficiently synchronize files/directories         |

### When would I use `cp`?

I would use `cp` when I need to make a **local copy** of files or directories.

Example:

```bash
cp -r source backup/
```

### When would I use `scp`?

I would use `scp` when I need to **securely transfer files between Linux systems over SSH**.

Example:

```bash
scp file.txt user@server:/destination/
```

### When would I use `rsync`?

I would use `rsync` when I need to **synchronize directories**, especially when the synchronization may happen repeatedly.

Example:

```bash
rsync -av source/ backup/
```

---

# 12. Why Can `rsync` Be More Efficient?

`rsync` is useful for large directories because after the initial synchronization it can identify files that have changed and transfer only the necessary changes instead of blindly copying the entire directory again.

This makes it particularly useful for:

* backups
* server synchronization
* deployments
* repeated file transfers

---

# 13. Difference Between `cp`, `scp`, and `rsync`

A simple way I understand the three commands is:

```text
cp
Local copying
     ↓
"Make a copy here."


scp
Remote transfer over SSH
     ↓
"Send this file to another Linux system."


rsync
Synchronization
     ↓
"Make the destination match the source efficiently."
```

`rsync` can also perform local copying and remote transfers, but its major advantage is **efficient synchronization**.

---

# Problems Encountered

### Problem 1 — Copying a directory without `-r`

I initially ran:

```bash
cp source backup/
```

and received:

```text
cp: -r not specified; omitting directory 'source'
```

I solved this by using:

```bash
cp -r source backup/
```

### Problem 2 — Understanding `rsync` after modifying a file

After modifying `source/file2.txt`, the backup still contained the old content.

This was expected because `rsync` does not continuously synchronize directories. I needed to run:

```bash
rsync -av source/ rsync-backup/
```

After running it, the modified file was transferred.

### Problem 3 — SSH service was inactive

The SSH service was initially inactive:

```text
Active: inactive (dead)
```

I started it using:

```bash
sudo systemctl start ssh
```

After that, `scp` successfully transferred the file.

---

# What I Learned

This task helped me understand the practical difference between `cp`, `scp`, and `rsync`.

I learned that `cp` is mainly for local copying, while `scp` uses SSH to transfer files between systems. I also learned that `rsync` is designed for synchronization and is more efficient for repeated transfers because it can detect changes.

The most useful part was modifying only `file2.txt` and running `rsync` again. The output showed `file2.txt` as the changed file, demonstrating how rsync avoids unnecessarily retransferring unchanged files.

I also learned why `rsync --delete` must be used carefully because it can remove files from the destination that are not present in the source.
