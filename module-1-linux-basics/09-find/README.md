# Task 9 — `find`

## Objective

The objective of this task was to practice the Linux `find` command for locating files based on different conditions.

The task covered:

* Finding files by extension/name
* Finding recently modified files
* Finding files based on size
* Finding files older than a specific number of days
* Finding files with specific permissions
* Finding files owned by a particular user
* Using `find` with `-exec`
* Safely testing file deletion
* Understanding why `find` output should be inspected before automated deletion

All experiments were performed inside:

```bash
~/module-1-linux-basics/09-find
```

---


# Basic `find` Syntax

The general structure used throughout this task was:

```bash
find <path> <conditions>
```

Example:

```bash
find . -type f
```

This searches the current directory and all subdirectories for regular files.

## Important Options

```text
.          Current directory
-type f    Regular file
-name      Match filename
-mtime     Modification time in days
-mmin      Modification time in minutes
-size      Match file size
-perm      Match permissions
-user      Match file owner
-exec      Execute another command on matched files
-delete    Delete matched files
```

---

# 1. Find All `.log` Files

Command:

```bash
find . -type f -name "*.log"
```

Example output:

```text
./backups/old-backup.log
./logs/access.log
./logs/error.log
./logs/app.log
./test-delete/delete3.log
```

### Explanation

```bash
find .
```

Searches from the current directory.

```bash
-type f
```

Limits the search to regular files.

```bash
-name "*.log"
```

Searches for filenames ending in `.log`.

The wildcard was quoted so that the shell did not expand it before `find` processed it.

### Mistake Encountered

Incorrect:

```bash
find . -type f "*.log"
```

Correct:

```bash
find . -type f -name "*.log"
```

---

# 2. Find Recently Modified Files

To find files modified within the last one day:

```bash
find . -type f -mtime -1
```

To find files modified within approximately the last 60 minutes:

```bash
find . -type f -mmin -60
```

The `-mmin` test was useful for the lab because it allows recent modifications to be demonstrated without waiting for a full day.

---

# 3. Find Files Larger Than a Particular Size

The `-size` option was used to search according to file size.

Example:

```bash
find . -type f -size +50k
```

This searches for files larger than 50 KB.

Other examples:

```bash
find . -type f -size +50M
```

```bash
find . -type f -size +1M
```

A successful example was:

```bash
find . -type f -size +9k
```

which located:

```text
./README.md
```

because the README file was larger than 9 KB.

### Mistake Encountered

Incorrect:

```bash
find . -type f -size +50kb
```

This produced an invalid argument error.

Correct:

```bash
find . -type f -size +50k
```

---

# 4. Find Files Older Than a Particular Number of Days

A test file was created:

```bash
touch test-delete/old-file.txt
```


Files older than three days were searched using:

```bash
find . -type f -mtime +3
```

The result included:

```text
./test-delete/old-file.txt
```

### Important Concept

```bash
-mtime +3
```

was used to search for files older than the specified number of days.

---

# 5. Find Files With Specific Permissions

A test file was created:

```bash
touch config/secret.conf
```

Its permissions were changed to `600`:

```bash
chmod 600 config/secret.conf
```

The permissions were verified:

```bash
ls -l config/secret.conf
```

Example output:

```text
-rw------- 1 afrooz afrooz ... secret.conf
```

The file was then located using:

```bash
find . -type f -perm 600
```

Output:

```text
./config/secret.conf
```

### Understanding `600`

```text
600
│││
││└── Others: no permissions
│└─── Group: no permissions
└──── Owner: read + write
```

Therefore:

```text
600 = rw-------
```

---

# 6. Find a File With a Particular Name

Command:

```bash
find . -type f -name "report.txt"
```

Output:

```text
./documents/report.txt
```

The `-name` condition can also be used with wildcards.

For example:

```bash
find . -type f -name "*.conf"
```

This can locate configuration files.

---

# 7. Find Files Owned by a Particular User

The current username was checked using:

```bash
whoami
```

Output:

```text
afrooz
```

Files owned by this user were then located:

```bash
find . -type f -user afrooz
```

This returned the files in the test directory owned by `afrooz`.

### Important Concept

```bash
-user afrooz
```

means:

> Match files whose owner is the specified user.

---

# 8. Using `find` With `-exec`

The `-exec` option allows another Linux command to be executed on every file found.

Command:

```bash
find . -type f -name "*.log" -exec stat -c "%A %n %U" {} \;
```

Example output:

```text
-rw-r--r-- ./backups/old-backup.log afrooz
-rw-r--r-- ./logs/access.log afrooz
-rw-r--r-- ./logs/error.log afrooz
-rw-r--r-- ./logs/app.log afrooz
-rw-r--r-- ./test-delete/delete3.log afrooz
```

### Understanding the Command

```bash
find . -type f -name "*.log"
```

Finds all `.log` files.

```bash
-exec
```

Tells `find` to execute another command for each match.

```bash
stat -c "%A %n %U"
```

Displays:

```text
%A → permissions
%n → filename
%U → owner username
```

```text
{}
```

Represents the current file found by `find`.

```text
\;
```

Marks the end of the `-exec` command.

### Mistake Encountered

Incorrect:

```bash
find . -type f -name "*.log" -exec stat -c "%A %n %U" {} ;/
```

Correct:

```bash
find . -type f -name "*.log" -exec stat -c "%A %n %U" {} \;
```

---

# 9. Safe Deletion Demonstration

The assessment required deletion experiments to be performed only inside:

```bash
09-find/test-delete/
```

The directory was checked:

```bash
ls test-delete
```

The disposable `.log` file was first located using:

```bash
find test-delete -type f -name "*.log" -print
```

Output:

```text
test-delete/delete3.log
```

The output was inspected before deletion.

The file was then safely removed:

```bash
find test-delete -type f -name "*.log" -delete
```

After deletion, the directory was checked:

```bash
ls test-delete
```

The `.log` file was no longer present.

The deletion experiment was restricted to the dedicated disposable directory.

---

# Why Inspect `find` Output Before Deletion?

It is important to inspect the output of `find` before combining it with an automatic deletion operation.

For example:

```bash
find <path> <conditions> -delete
```

can delete every file matching the specified conditions.

A mistake in any of the following could cause unintended files to be selected:

* Search path
* Filename pattern
* Permission condition
* Time condition
* Size condition

Therefore, a safer approach is:

```bash
find <path> <conditions> -print
```

First inspect the results, confirm that they are exactly the files intended for deletion, and only then use:

```bash
-delete
```

This is especially important when working with important directories or system files.

---

# Errors and Troubleshooting

## Error 1 — Incorrect `-type` Syntax

Incorrect:

```bash
find . -type -f
```

Correct:

```bash
find . -type f
```

`f` is the argument to `-type`; it does not require another `-`.

---

## Error 2 — Incorrect Filename Condition

Incorrect:

```bash
find . -type f "*.log"
```

Correct:

```bash
find . -type f -name "*.log"
```

---

## Error 3 — Incorrect `-mtime` Syntax

Incorrect:

```bash
find . -type -mtime +3
```

Correct:

```bash
find . -type f -mtime +3
```

---

## Error 4 — Incorrect User Condition

Incorrect:

```bash
find . -type f user afrooz
```

Correct:

```bash
find . -type f -user afrooz
```

---

## Error 5 — Incorrect `-exec` Termination

Incorrect:

```bash
... -exec stat ... {} ;/
```

Correct:

```bash
... -exec stat ... {} \;
```

The `\;` tells `find` where the `-exec` command ends.

---

# What I Learned

Through this task, I learned how to use `find` to search Linux files based on different criteria.

I learned how to:

* Search recursively from a directory.
* Find regular files using `-type f`.
* Search filenames using `-name`.
* Use wildcards such as `*.log`.
* Find recently modified files using `-mtime` and `-mmin`.
* Search files based on size using `-size`.
* Find older files using `-mtime`.
* Search files by exact permissions using `-perm`.
* Find files owned by a particular user using `-user`.
* Execute commands on matched files using `-exec`.
* Understand `{}` inside `-exec`.
* Use `\;` to terminate an `-exec` command.
* Safely test deletion using a disposable directory.
* Inspect search results before performing destructive operations.
* Troubleshoot incorrect `find` syntax.

The main lesson from this task was that `find` is a powerful Linux administration tool because it can locate files based on many properties and can also perform actions on the results.

---

# Verification

The following Task 9 requirements were completed:

```text
✓ Created 09-find
✓ Created multiple directories and test files
✓ Found all .log files
✓ Found recently modified files
✓ Searched files by size
✓ Found files older than a specified number of days
✓ Found files with specific permissions
✓ Found files by filename
✓ Found files by owner
✓ Used find with -exec
✓ Performed safe deletion inside test-delete
✓ Inspected results before deletion
✓ Documented find syntax errors and corrections
```

---

# Resources Used

* Linux `find` command
* Linux `stat` command
* Linux `touch` command
* Linux `chmod` command
* Linux terminal / WSL environment
* Bash shell
* `man find`
* `find --help`
* ChatGPT as a tutoring and troubleshooting resource
* GeekForGeeks

