# Task 5 — Permissions and Ownership

## Objective

The purpose of this task was to understand Linux file permissions, ownership, Bash script execution permissions, and special permissions.

All experiments were performed inside:

```text
~/module-1-linux-basics/05-permissions
```

---

# 1. Create Test Files and Directories

I created the task directory and basic files/directories:

```bash
mkdir dir1 test-dir
touch file1.txt file2.txt permissions.txt test-file.txt
```

I also created the required documentation files:

```bash
touch README.md commands.txt
```

The directory contained several files and directories for testing permissions and ownership.

---

# 2. Inspect Existing Permissions

I used `ls -l` to inspect the permissions, owner, and group of the test files:

```bash
ls -l
```

Example output:

```text
-rw-r--r-- 1 afrooz afrooz 0 permissions.txt
drwxr-xr-x 2 afrooz afrooz 4096 test-dir
-rw-r--r-- 1 afrooz afrooz 0 test-file.txt
```

The first field represents the file type and permissions.

For example:

```text
-rw-r--r--
```

can be divided into:

```text
- rw- r-- r--
  │   │   │
  │   │   └── Others
  │   └────── Group
  └────────── Owner/User
```

---

# 3. Read Permission

I created content in `test-file.txt`:

```bash
echo "Permission Test" > test-file.txt
```

I verified it:

```bash
cat test-file.txt
```

Output:

```text
Permission Test
```

I then removed the user's read permission:

```bash
chmod u-r test-file.txt
```

When I tried to read the file:

```bash
cat test-file.txt
```

I received:

```text
cat: test-file.txt: Permission denied
```

I restored the permission:

```bash
chmod u+r test-file.txt
```

The file could then be read again.

### What this demonstrated

The `r` permission allows a user to **read the contents of a file**.

---

# 4. Write Permission

I added write permission for the user:

```bash
chmod u+w test-file.txt
```

I was then able to append data:

```bash
echo "new line" >> test-file.txt
```

I removed the write permission:

```bash
chmod u-w test-file.txt
```

When I tried to write again:

```bash
echo "new line" >> test-file.txt
```

I received:

```text
-bash: test-file.txt: Permission denied
```

I restored the permission:

```bash
chmod u+w test-file.txt
```

### What this demonstrated

The `w` permission allows a user to **modify/write the contents of a file**.

---

# 5. Execute Permission

I created a Bash script:

```bash
nano scripts/test-script.sh
```

Initially, the script had:

```text
-rw-r--r--
```

I tried to execute it:

```bash
./scripts/test-script.sh
```

The result was:

```text
-bash: ./scripts/test-script.sh: Permission denied
```

I added execute permission for the user:

```bash
chmod u+x scripts/test-script.sh
```

I then executed it successfully:

```bash
./scripts/test-script.sh
```

Output:

```text
Permision to execute this file.
```

The permissions changed to:

```text
-rwxr--r--
```

### What changed?

Initially, the user did not have execute permission:

```text
rw-
```

After:

```bash
chmod u+x scripts/test-script.sh
```

the user had:

```text
rwx
```

This allowed the script to be executed directly using:

```bash
./scripts/test-script.sh
```

---

# 6. Numeric Permissions — `755`

I changed `permissions.txt` to permission `755`:

```bash
chmod 755 permissions.txt
```

I verified it:

```bash
ls -l permissions.txt
```

Output:

```text
-rwxr-xr-x 1 afrooz afrooz 0 permissions.txt
```

The permission value `755` represents:

```text
7     5     5
│     │     │
User  Group Others
```

Each digit is calculated using:

```text
r = 4
w = 2
x = 1
```

Therefore:

### User — `7`

```text
4 + 2 + 1 = 7
rwx
```

The owner can read, write, and execute.

### Group — `5`

```text
4 + 1 = 5
r-x
```

The group can read and execute but cannot write.

### Others — `5`

```text
4 + 1 = 5
r-x
```

Others can read and execute but cannot write.

Therefore:

```text
755 = rwxr-xr-x
```

---

# 7. Symbolic Permissions

I practiced modifying permissions using symbolic notation.

### Add permission for the user

```bash
chmod u+r test-file.txt
```

`u` means **user/owner** and `+r` adds read permission.

### Remove permission from the group

```bash
chmod g-w test-file.txt
```

`g` means **group** and `-w` removes write permission.

### Add permission for others

```bash
chmod o+r permissions.txt
```

`o` means **others** and `+r` adds read permission.

---

# 8. Ownership

Linux files have an owner and a group.

I created a test user and group in the lab environment and verified the user's groups:

```bash
id test
```

The test user was associated with:

```text
test
```

I added the user to the `dev-test` group:

```bash
sudo usermod -aG dev-test test
```

I verified the membership:

```bash
groups test
```

Output included:

```text
test : test dev-test
```

I then practiced changing the group ownership of a test file:

```bash
sudo chown :dev-test ownership-test.txt
```

This changes **only the group**.

I also tested:

```bash
sudo chown test:dev-test ownership-test.txt
```

This changes **both the owner and group**.

### `chown` syntax

```text
chown owner:group file
```

For example:

```bash
sudo chown test:dev-test ownership-test.txt
```

means:

```text
Owner → test
Group → dev-test
```

---

# 9. Difference Between `chmod` and `chown`

`chmod` changes **permissions**.

Example:

```bash
chmod 755 file.txt
```

`chown` changes **ownership**.

Example:

```bash
sudo chown test:dev-test file.txt
```

In simple terms:

```text
chmod → Who can do what?
chown → Who owns it?
```

---

# 10. Special Permissions

Linux provides three important special permission mechanisms:

* SUID
* SGID
* Sticky bit

These provide behavior beyond normal `r`, `w`, and `x` permissions.

---

## SUID — Set User ID

SUID is mainly used on executable files.

When an executable has SUID set, it runs with the **effective permissions of the file owner**, rather than the permissions of the user who launched it.

I searched for existing SUID files using:

```bash
find /usr/bin -perm -4000 -type f 2>/dev/null | head
```

Explanation:

```text
find /usr/bin
```

Search inside `/usr/bin`.

```text
-perm -4000
```

Find files with the SUID permission bit set.

```text
-type f
```

Restrict the search to regular files.

```text
2>/dev/null
```

Hide permission/error messages.

```text
| head
```

Display only the first few results.

---

## SGID — Set Group ID

SGID has different behavior depending on whether it is applied to a file or directory.

For executable files, SGID can cause the program to run with the **group permissions associated with the file**.

For directories, SGID causes newly created files/subdirectories to inherit the directory's group ownership.

This is particularly useful for shared directories.

---

## Sticky Bit

The sticky bit is commonly used on directories where multiple users can create files.

The classic example is:

```text
/tmp
```

The sticky bit allows users to create files there, but normally prevents one user from deleting another user's files.

This provides safer sharing of a writable directory.

---

# 11. Special Permission Summary

| Permission | Purpose                                                                       |
| ---------- | ----------------------------------------------------------------------------- |
| SUID       | Executable runs with owner's effective privileges                             |
| SGID       | Executable uses group privileges; directories can inherit group ownership     |
| Sticky bit | Users cannot normally delete files owned by other users in a shared directory |

The important distinction is:

```text
SUID      → User/owner identity
SGID      → Group identity/inheritance
Sticky    → Deletion protection in shared directories
```

---

# 12. Questions and Answers

## 1. What is the difference between `chmod` and `chown`?

`chmod` changes the permissions of a file or directory.

```bash
chmod 755 file.txt
```

`chown` changes the owner and/or group.

```bash
chown test:dev-test file.txt
```

---

## 2. What do `r`, `w`, and `x` mean?

For regular files:

| Permission | Meaning               |
| ---------- | --------------------- |
| `r`        | Read the file         |
| `w`        | Modify/write the file |
| `x`        | Execute the file      |

For directories, `x` has an important additional meaning: it allows access/traversal through the directory.

---

## 3. What does permission `644` mean?

```text
644
│││
││└── Others = 4 = r--
│└─── Group  = 4 = r--
└──── User   = 6 = rw-
```

Therefore:

```text
644 = rw-r--r--
```

The owner can read and write.

The group can read.

Others can read.

---

## 4. What does permission `755` mean?

```text
755 = rwxr-xr-x
```

The owner has:

```text
rwx
```

The group has:

```text
r-x
```

Others have:

```text
r-x
```

Therefore, the owner can read, write, and execute, while the group and others can read and execute.

---

## 5. Why does `/tmp` commonly use the sticky bit?

`/tmp` is a shared temporary directory where multiple users and applications can create files.

It needs to be writable by many users, but users should not normally be able to delete files belonging to other users.

The sticky bit provides this protection.

A typical permission representation is:

```text
drwxrwxrwt
```

The final `t` indicates the sticky bit.

---

# Problems Encountered

### Problem 1 — Reading a file after removing read permission

I removed the owner's read permission:

```bash
chmod u-r test-file.txt
```

Trying to read it produced:

```text
Permission denied
```

I solved the problem by restoring the permission:

```bash
chmod u+r test-file.txt
```

---

### Problem 2 — Writing without write permission

After removing write permission:

```bash
chmod u-w test-file.txt
```

I received:

```text
Permission denied
```

I restored the permission with:

```bash
chmod u+w test-file.txt
```

---

### Problem 3 — Executing a script without execute permission

The script initially had no execute permission:

```text
-rw-r--r--
```

Running:

```bash
./scripts/test-script.sh
```

produced:

```text
Permission denied
```

I solved it using:

```bash
chmod u+x scripts/test-script.sh
```

The script then executed successfully.

---

### Problem 4 — Incorrect filename while verifying permissions

I accidentally used:

```bash
ls -l permission.txt
```

instead of:

```bash
ls -l permissions.txt
```

This resulted in:

```text
ls: cannot access 'permission.txt': No such file or directory
```

I corrected the filename and verified the file successfully.

---

# What I Learned

This task helped me understand how Linux controls access to files and directories through permissions and ownership.

I learned that `r`, `w`, and `x` control different types of access and that permissions can be changed using both numeric and symbolic forms of `chmod`. I practiced `644` and `755` and learned how each digit represents permissions for the owner, group, and others.

I also learned the difference between `chmod` and `chown`: `chmod` controls what users can do with a file, while `chown` controls who owns the file and which group it belongs to.

The Bash script exercise helped me understand that a script can exist without execute permission but cannot be run directly until the execute bit is added.

Finally, I learned the purpose of SUID, SGID, and the sticky bit. In particular, the sticky bit is important for shared directories such as `/tmp`, while SUID and SGID can affect the effective user or group identity under which programs operate.
