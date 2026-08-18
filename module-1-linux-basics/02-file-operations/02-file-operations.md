# Task 2 — Basic File and Directory Operations

## Objective

The purpose of this task was to practice basic Linux file and directory operations, links, inodes, and archive/compression using `tar` and `gzip`.

---

## 1. Create the Main Directory

First, I created a directory named `02-file-operations` and entered it.

```bash
mkdir 02-file-operations
cd 02-file-operations
```

To check the current working directory:

```bash
pwd
```

Output:

```text
/home/afrooz/module-1-linux-basics/02-file-operations
```

---

## 2. Create Three Directories

I created three directories using `mkdir`:

```bash
mkdir dir1 dir2 dir3
```

To view them:

```bash
ls
```

Output:

```text
dir1  dir2  dir3
```

---

## 3. Create Files Using `touch`

I created three files using `touch`:

```bash
touch file1.txt file2.txt file3.txt
```

The files were successfully created.

---

## 4. Create Files Using `nano`

I also created files using the `nano` text editor:

```bash
nano file4.txt
nano file5.txt
```

Therefore, at least five files were created:

* `file1.txt`
* `file2.txt`
* `file3.txt`
* `file4.txt`
* `file5.txt`

---

## 5. Move a File Between Directories

I moved `file1.txt` into `dir1`:

```bash
mv file1.txt dir1
```

To verify:

```bash
ls dir1
```

Output:

```text
file1.txt
```

The file was successfully moved from the main directory to `dir1`.

---

## 6. Rename a File

I renamed `file2.txt`:

```bash
mv file2.txt renamed-file.txt
```

I also renamed `file3.txt`:

```bash
mv file3.txt renamed-file.md
```

The `mv` command can therefore be used for both moving and renaming files.

---

## 7. Copy a File Between Directories

I copied `renamed-file.md` into `dir2`:

```bash
cp renamed-file.md dir2/
```

To verify:

```bash
ls dir2
```

Output:

```text
renamed-file.md
```

The original file remained in the main directory because `cp` creates a copy rather than moving the original.

---

## 8. Copy an Entire Directory

Initially, I tried:

```bash
cp dir1 dir2
```

This produced:

```text
cp: -r not specified; omitting directory 'dir1'
```

This happened because directories can contain files and subdirectories, so recursive copying is required.

The correct command was:

```bash
cp -r dir1 dir3
```

I verified the copied directory:

```bash
ls dir3/dir1
```

Output:

```text
file1.txt
```

This confirmed that `dir1` and its contents were copied into `dir3`.

---

## 9. Delete a File

I deleted `file4.txt`:

```bash
rm file4.txt
```

The file was successfully removed.

---

## 10. Delete an Empty Directory

I created an empty directory:

```bash
mkdir dir4
```

Then removed it using:

```bash
rmdir dir4
```

`rmdir` is used specifically for removing empty directories.

---

## 11. Delete a Directory Containing Files

I first tried:

```bash
rm dir3
```

Linux returned:

```text
rm: cannot remove 'dir3': Is a directory
```

This happened because `dir3` is a directory.

The recursive option can be used to remove a directory and its contents:

```bash
rm -r dir3
```

During my practice, I removed the original `dir1` using:

```bash
rm -r dir1
```

The copied `dir1` inside `dir3` remained, demonstrating that the copied directory was independent of the original.

---

# Links

## 12. Hard Link

I created a file for the hard-link experiment:

```bash
touch link-original.txt
echo "this is my link test file" > link-original.txt
```

I checked its inode:

```bash
ls -li link-original.txt
```

Then I created a hard link:

```bash
ln link-original.txt hard-link.txt
```

I checked both files:

```bash
ls -li link-original.txt
ls -li hard-link.txt
```

Both filenames showed the same inode number:

```text
13039
```

The link count also increased from `1` to `2`.

This demonstrated that a hard link refers to the **same inode and underlying file data** as the original file.

### Hard Link Modification Test

I modified the file through the hard link:

```bash
nano hard-link.txt
```

Then checked the original:

```bash
cat link-original.txt
```

The modification was visible through the original filename.

### What Happens When the Original Is Deleted?

A hard link does not depend on the original filename.

If:

```bash
rm link-original.txt
```

is executed, `hard-link.txt` can still access the data because it points to the same inode.

Deleting one filename decreases the inode's link count. The data remains available as long as at least one hard link still exists.

---

## 13. Symbolic/Soft Link

I created another file:

```bash
nano original-link.txt
```

Then created a symbolic link:

```bash
ln -s original-link.txt soft-link.txt
```

I edited the symbolic link:

```bash
nano soft-link.txt
```

The changes were also visible through:

```bash
cat original-link.txt
```

This demonstrated that the symbolic link provides access to its target file.

### What Happens When the Original Is Deleted?

I deleted the target:

```bash
rm original-link.txt
```

Then tested:

```bash
cat soft-link.txt
```

The command failed because the target file no longer existed.

This demonstrated that a symbolic link points to the target's **path**, rather than sharing the target's inode.

The symbolic link therefore becomes a **broken/dangling symbolic link** when its target is deleted.

---

## 14. Inode

An inode is a filesystem data structure that stores metadata about a file and information used to locate its data.

I used:

```bash
ls -li
```

to display inode numbers.

The hard-link experiment demonstrated that two filenames can refer to the same inode.

For example:

```text
link-original.txt ──┐
                    ├──> same inode
hard-link.txt ──────┘
```

A symbolic link is different because it has its own inode and stores a reference to the target path.

---

# Compression and Archive

## 15. Create a Directory Containing Several Files

I created a directory named `arch-test`:

```bash
mkdir arch-test
```

Then created three files:

```bash
touch arch-test/file1.txt arch-test/file2.txt arch-test/file3.txt
```

I added different content:

```bash
echo "File one content" > arch-test/file1.txt
echo "File two content" > arch-test/file2.txt
echo "File three content" > arch-test/file3.txt
```

I verified the files:

```bash
ls arch-test
```

Output:

```text
file1.txt  file2.txt  file3.txt
```

---

## 16. Create a Compressed Archive

I created a `tar.gz` archive using:

```bash
tar -czvf archive-test.tar.gz arch-test/
```

The options mean:

| Option | Meaning                      |
| ------ | ---------------------------- |
| `-c`   | Create an archive            |
| `-z`   | Use gzip compression         |
| `-v`   | Show files being processed   |
| `-f`   | Specify the archive filename |

The resulting archive was:

```text
archive-test.tar.gz
```

The command output showed:

```text
arch-test/
arch-test/file2.txt
arch-test/file1.txt
arch-test/file3.txt
```

This confirmed that all three files were included.

---

## 17. Inspect the Archive

I used:

```bash
tar -tzvf archive-test.tar.gz
```

The archive contained:

```text
arch-test/
arch-test/file2.txt
arch-test/file1.txt
arch-test/file3.txt
```

The `-t` option lists the contents without extracting them.

---

## 18. Delete the Original Directory

After confirming that the archive contained the required files, I removed the original directory:

```bash
rm -r arch-test
```

This tested whether the archive could be used to restore the deleted files.

---

## 19. Extract the Archive

I extracted the archive using:

```bash
tar -xzvf archive-test.tar.gz
```

The options mean:

| Option | Meaning                      |
| ------ | ---------------------------- |
| `-x`   | Extract                      |
| `-z`   | Use gzip decompression       |
| `-v`   | Show extracted files         |
| `-f`   | Specify the archive filename |

The `arch-test` directory was successfully restored.

---

## 20. Verify Restored Files

I checked the restored directory:

```bash
ls arch-test
```

Output:

```text
file1.txt  file2.txt  file3.txt
```

I then checked the contents:

```bash
cat arch-test/file1.txt
```

Output:

```text
File one content
```

```bash
cat arch-test/file2.txt
```

Output:

```text
File two content
```

```bash
cat arch-test/file3.txt
```

Output:

```text
File three content
```

This confirmed that all files were successfully restored from the compressed archive with their original contents.

---

# Commands Practiced

| Command     | Purpose                                            |
| ----------- | -------------------------------------------------- |
| `pwd`       | Shows the current working directory                |
| `ls`        | Lists files and directories                        |
| `ls -l`     | Shows detailed file information                    |
| `ls -li`    | Shows detailed information including inode numbers |
| `cd`        | Changes the current directory                      |
| `mkdir`     | Creates directories                                |
| `touch`     | Creates empty files                                |
| `nano`      | Creates/edits files using a text editor            |
| `cp`        | Copies files                                       |
| `cp -r`     | Copies directories recursively                     |
| `mv`        | Moves or renames files/directories                 |
| `rm`        | Deletes files                                      |
| `rm -r`     | Deletes directories and their contents recursively |
| `rmdir`     | Deletes empty directories                          |
| `ln`        | Creates a hard link                                |
| `ln -s`     | Creates a symbolic/soft link                       |
| `tar -czvf` | Creates a gzip-compressed tar archive              |
| `tar -tzvf` | Lists archive contents                             |
| `tar -xzvf` | Extracts a gzip-compressed tar archive             |
| `cat`       | Displays file contents                             |

---

# Problems Encountered

### Copying a Directory Without `-r`

I initially tried:

```bash
cp dir1 dir3
```

and received:

```text
cp: -r not specified; omitting directory 'dir1'
```

I learned that directories need recursive copying:

```bash
cp -r dir1 dir3
```

### Removing a Directory With `rm`

I also tried:

```bash
rm dir3
```

and received:

```text
rm: cannot remove 'dir3': Is a directory
```

I learned that `rm -r` is required when removing a directory and its contents.

### Symbolic Link Target Deleted

After deleting:

```bash
rm original-link.txt
```

the symbolic link could no longer access its target.

This demonstrated that symbolic links depend on the target path, unlike hard links.

---

### What I Learned

Through this task, I learned how to:

Navigate the Linux filesystem.
Create, move, rename, copy, and delete files.
Create and remove directories.
Use recursive operations with cp -r and rm -r.
Use nano to create and edit files.
Understand inodes.
Create and test hard links.
Create and test symbolic links.
Understand the difference between hard and symbolic links.
Create gzip-compressed tar archives.
Inspect archive contents.
Delete the original files and restore them from an archive.
Verify that restored files contain the expected data.
