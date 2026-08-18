# Task 3 — Linux Storage and Filesystems

## Objective

The purpose of this task was to investigate the storage configuration of my Linux system running under WSL2. I used Linux utilities to identify block devices, mounted filesystems, disk usage, filesystem types, mount points, and inode usage.

---

## 1. Block Devices

I used `lsblk` to identify the block devices available to my Linux environment.

```bash
lsblk
```

Important output:

```text
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
sda    8:0     0 388.4M  1  disk
sdb    8:16    0   186M  1  disk
sdc    8:32    0     1G  0  disk [SWAP]
sdd    8:48    0     1T  0  disk
sde    8:64    0     1T  0  disk /mnt/wslg/distro
```

The system detected several virtual block devices. `sdc` is being used as swap space, while `sde` is associated with `/mnt/wslg/distro`.

---

## 2. Mounted Filesystems

I used `findmnt` to investigate which filesystems are mounted and where they are mounted.

```bash
findmnt
```

The most important result was:

```text
TARGET  SOURCE    FSTYPE
/       /dev/sde  ext4
```

This shows that the root filesystem `/` is provided by `/dev/sde` and uses the `ext4` filesystem.

The output also showed other filesystem types used by WSL2, including `tmpfs`, `overlay`, `rootfs`, and `9p`.

Windows drives were also available through:

```text
/mnt/c
/mnt/d
```

---

## 3. Disk Usage and Available Space

I used:

```bash
df -h
```

The `df` command displays filesystem disk usage, while `-h` means **human-readable**, making sizes easier to understand.

For the root filesystem:

```text
/dev/sde       1007G  1.8G  954G   1% /
```

Therefore:

* Total capacity: approximately **1007 GB**
* Used: approximately **1.8 GB**
* Available: approximately **954 GB**
* Usage: **1%**
* Mount point: `/`

This demonstrates the difference between total disk/filesystem capacity and the amount of space currently being used.

---

## 4. Filesystem Types

I used:

```bash
df -T
```

The `-T` option displays the filesystem type.

The important result was:

```text
/dev/sde       ext4    ...    /
C:\            9p      ...    /mnt/c
D:\            9p      ...    /mnt/d
```

Therefore, the Linux root filesystem uses **ext4**.

Other filesystem types observed included:

* `ext4` — main Linux filesystem
* `9p` — used for Windows drives exposed through WSL
* `tmpfs` — temporary filesystem
* `overlay` — layered filesystem used by parts of the WSL environment
* `rootfs` — initial/root filesystem environment

---

## 5. Inode Investigation

I used:

```bash
df -i
```

The `-i` option displays inode usage instead of normal disk-space usage.

For the root filesystem:

```text
/dev/sde       67108864   49771   67059093    1% /
```

This means:

* Total inodes: **67,108,864**
* Used inodes: **49,771**
* Free inodes: **67,059,093**
* Inode usage: **1%**

### What is an inode?

An inode is a filesystem data structure that stores metadata and information about a file or directory, such as ownership, permissions, timestamps, size, and references to the file's data.

The filename itself is associated with a directory entry, while the directory entry references the inode.


---

## 6. Root Filesystem Verification

I used a more focused command to directly identify the filesystem used for `/`:

```bash
findmnt -no SOURCE,FSTYPE,TARGET /
```

Output:

```text
/dev/sde ext4 /
```

Therefore:

> The root filesystem `/` on my WSL2 Linux system is provided by `/dev/sde` and uses the `ext4` filesystem.

---

# Important Commands and Options

| Command                              | Purpose                                                        |
| ------------------------------------ | -------------------------------------------------------------- |
| `lsblk`                              | Lists block devices                                            |
| `findmnt`                            | Shows mounted filesystems and mount points                     |
| `df -h`                              | Shows disk usage in human-readable format                      |
| `df -T`                              | Shows filesystem types                                         |
| `df -i`                              | Shows inode usage                                              |
| `findmnt -no SOURCE,FSTYPE,TARGET /` | Directly shows the source, filesystem type, and target for `/` |

### Important options

* `-h` — human-readable sizes
* `-T` — display filesystem type
* `-i` — display inode information
* `-n` — don't display the header
* `-o` — select specific output columns

---

# Answers to Assessment Questions

### 1. What is a filesystem?

A filesystem is the structure used by an operating system to organize, store, and manage files and directories on storage.

### 2. What is the root filesystem `/`?

`/` is the top-level directory of the Linux filesystem hierarchy. Other directories such as `/home`, `/etc`, `/var`, and `/usr` exist underneath it.

### 3. What is a mount point?

A mount point is a directory where a filesystem is attached so that its files and directories can be accessed through the Linux filesystem hierarchy.

### 4. What is the difference between disk capacity and filesystem usage?

Disk/filesystem capacity is the total amount of storage available. Filesystem usage is the amount of that storage currently occupied by data.

For my root filesystem:

```text
Capacity: 1007G
Used:     1.8G
Available: 954G
```

### 5. What is an inode?

An inode is a filesystem data structure containing metadata and references associated with a file or directory.

### 6. What happens when a filesystem runs out of inodes but still has free disk space?

The filesystem may still have free data space, but it cannot create new files or directories because there are no available inodes to represent them.

---

# Problems Encountered

No significant problems were encountered during the storage investigation. The commands worked successfully in my WSL2 environment.

One important observation was that the WSL2 environment contains several virtual filesystems and devices, so not every entry shown by `findmnt` or `df` represents a normal physical Linux disk. I focused on the relevant root filesystem `/dev/sde → ext4 → /` and identified the purpose of the other important entries.

---

# What I Learned

This task helped me understand the relationship between **block devices, filesystems, mount points, disk usage, and inodes**.

I learned that a block device such as `/dev/sde` can contain a filesystem such as `ext4`, which can then be mounted at a directory such as `/`.

I also learned that **disk space and inode availability are different resources**. A filesystem can have free disk space but still be unable to create new files if it runs out of inodes.

The investigation also helped me connect the storage concepts to my previous hard-link exercise, where two filenames shared the same inode.

Finally, I learned how to use `lsblk`, `findmnt`, `df -h`, `df -T`, and `df -i` to investigate an actual Linux system rather than relying only on theory.
