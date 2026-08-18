# Task 6 — Bash Scripting

## Objective

The objective of this task was to practice basic Bash scripting concepts, including:

* Bash variables
* Command-line arguments
* Conditional statements
* File and directory tests
* Numeric and string comparisons
* Root-user detection
* `case` statements
* `for` loops
* File metadata
* Exit codes

All scripts were created and tested inside the `06-bash-scripting` assessment directory.

---


# Script 1 — Directory Checker

## Purpose

The Directory Checker script accepts a directory path as a command-line argument and checks whether the specified path is a directory.

## Implementation

The first command-line argument is stored in a variable:

```bash
directory=$1
```

The script then uses the `-d` test operator:

```bash
if [ -d "$directory" ]
```

If the directory exists, the script displays:

```text
Directory Exist
```

Otherwise, it displays:

```text
Directory does not exist
```

## Important Concepts

### `$1`

`$1` represents the first positional argument passed to the script.

For example:

```bash
./directory_checker.sh scripts
```

Here:

```text
$1 = scripts
```

### `-d`

The `-d` operator checks whether a path exists and is a directory.

## Testing

The script was tested with an existing directory:

```bash
./directory_checker.sh .
```

Output:

```text
Directory Exist
```

It was also tested with a non-existing directory:

```bash
./directory_checker.sh scripts
```

Output:

```text
Directory does not exist
```

The script was also executed without an argument to observe its behavior.

---

# Script 2 — File Permission Checker

## Purpose

The File Permission Checker accepts a filename as an argument and checks whether the file is:

* Readable
* Writable
* Executable

## Test Operators

The script uses the following operators:

```text
-r
-w
-x
```

### `-r`

Checks whether the file is readable.

### `-w`

Checks whether the file is writable.

### `-x`

Checks whether the file is executable.

## Testing

The script was tested using:

```bash
./permission_checker.sh test-file.txt
```

The output showed that the file was readable and writable but not executable:

```text
File is Readable
File is Writable
File is not Executable
```

This demonstrated how Bash can test individual file permissions.

---

# Script 3 — Root Checker

## Purpose

The Root Checker determines whether the script is running with root privileges.

## Implementation

The script uses:

```bash
$EUID
```

`EUID` represents the effective user ID of the current Bash process.

Linux reserves UID `0` for the root/superuser.

The script checks:

```bash
if [ "$EUID" -eq 0 ]
```

The `-eq` operator performs a numeric comparison.

## Testing as a Normal User

The script was executed normally:

```bash
./root_checker.sh
```

Output:

```text
You are non-root User
```

## Testing as Root

The script was executed using `sudo`:

```bash
sudo ./root_checker.sh
```

Output:

```text
You are root User
```

This demonstrated that `sudo` runs the script with an effective UID of `0`.

## Important Concepts

```text
UID 0    → Root user
$EUID    → Effective user ID
-eq      → Numeric equality comparison
```

---

# Script 4 — Argument Handler

## Purpose

The Argument Handler accepts a command-line argument and performs a different action depending on the argument.

A custom implementation was created with the following commands:

```text
logs
status
disk
block
```

## Commands and Actions

### `logs`

Displays the beginning of the system journal:

```bash
journalctl | head -10
```

### `status`

Displays the status of the graphical target:

```bash
systemctl status graphical.target
```

### `disk`

Displays filesystem disk usage:

```bash
df -h
```

### `block`

Displays block devices:

```bash
lsblk
```

## Default Case

An additional default case was added to handle invalid commands.

For example:

```bash
./argument_handler.sh hello
```

Output:

```text
Invalid Command
```

## Important Concept — `case`

A `case` statement is useful when a variable can have multiple possible values.

Basic structure:

```bash
case "$command" in
    pattern1)
        commands
        ;;
    pattern2)
        commands
        ;;
    *)
        default commands
        ;;
esac
```

The `*` pattern works as the default case when none of the other patterns match.

## Problem Encountered

During the initial testing of the default case, the wildcard was written as:

```bash
"*")
```

This caused the default case not to match arbitrary input because the quoted `*` was treated literally.

It was corrected to:

```bash
*)
```

After the correction, invalid arguments correctly triggered the default case.

---

# Script 5 — File Loop

## Purpose

The File Loop script uses a `for` loop to iterate through a list of files and display:

* Permissions
* Filename
* Owner

## Test Files

Three test files were created:

```text
file1.txt
file2.txt
file3.txt
```

## Loop Implementation

The script uses a Bash `for` loop:

```bash
for file in file1.txt file2.txt file3.txt
do
    ...
done
```

During each iteration, the variable `$file` represents the current file.

## File Metadata

The script uses:

```bash
stat -c "%A %n %U" "$file"
```

The format specifiers mean:

| Format | Meaning          |
| ------ | ---------------- |
| `%A`   | File permissions |
| `%n`   | Filename         |
| `%U`   | Owner username   |

Example output:

```text
-rw-r--r-- file1.txt afrooz
-rw-r--r-- file2.txt afrooz
-rw-r--r-- file3.txt afrooz
```

## Understanding the `stat` Command

### `stat`

Displays file metadata.

### `-c`

Allows a custom output format.

### `%A`

Displays permissions in human-readable form.

### `%n`

Displays the filename.

### `%U`

Displays the owner's username.

### `"$file"`

Specifies the current file being processed by the loop.

---

# Bash Questions

## 1. What is a variable?

A variable in Bash is a named storage location for a value. The value can be reused later in the script.

Example:

```bash
directory=$1
```

The value of the first command-line argument is stored in the `directory` variable.

---

## 2. What is the difference between `$1` and `$?`?

`$1` represents the first positional argument passed to a script.

Example:

```bash
./script.sh file.txt
```

Here:

```text
$1 = file.txt
```

`$?` contains the exit status of the immediately previous command.

Normally:

```text
0       → Success
Non-zero → Failure/Error
```

---

## 3. What is `$USER`?

`$USER` is a Bash environment variable containing the username of the current user running the shell.

Example:

```bash
echo $USER
```

It may produce:

```text
afrooz
```

It represents the current shell user, not the owner of a particular file.

---

## 4. What does `if` do?

`if` is used for conditional execution.

It evaluates a condition or command exit status and executes the `then` block when the condition succeeds. Otherwise, the `else` block can be executed.

Example:

```bash
if [ -d "$directory" ]
then
    echo "Directory exists"
else
    echo "Directory does not exist"
fi
```

---

## 5. What does `-f` check?

`-f` checks whether a path exists and refers to a regular file.

Example:

```bash
[ -f "$file" ]
```

---

## 6. What does `-d` check?

`-d` checks whether a path exists and is a directory.

Example:

```bash
[ -d "$directory" ]
```

---

## 7. What is the difference between `=` and `-eq`?

`=` is commonly used for string comparison.

Example:

```bash
[ "$name" = "afrooz" ]
```

`-eq` is used for numeric comparison.

Example:

```bash
[ "$number" -eq 0 ]
```

The Root Checker used `-eq` because the effective user ID is a number.

---

## 8. What is an exit code?

An exit code is a numeric status returned by a command or script when it finishes.

The standard convention is:

```text
0        → Success
Non-zero → Failure or another condition
```

The previous command's exit status can be accessed using:

```bash
echo $?
```

---

# Problems Encountered

## Problem 1 — Directory Checker Syntax

Initially, the condition was written incorrectly:

```bash
if [ -d "$directory"] then:
```

The spacing and Bash syntax were corrected to:

```bash
if [ -d "$directory" ]
then
```

This reinforced that Bash test syntax requires proper spacing.

---

## Problem 2 — `head` Command

During the Argument Handler implementation, the initial use of:

```bash
head -10
```

produced an error during testing.

The command was checked and corrected/tested successfully to display the first 10 lines of the journal.

---

## Problem 3 — `case` Wildcard

The default case was initially written with a quoted wildcard:

```bash
"*")
```

This treated `*` as a literal pattern instead of a wildcard.

It was corrected to:

```bash
*)
```

After the correction, invalid arguments were handled correctly.

---

# What I Learned

Through this task, I practiced writing Bash scripts instead of only executing individual Linux commands.

I learned how to:

* Store values in Bash variables.
* Use positional parameters such as `$1`.
* Check directories and files using Bash test operators.
* Check file permissions using `-r`, `-w`, and `-x`.
* Detect whether a script is running as root.
* Understand UID `0` and `$EUID`.
* Use numeric comparison with `-eq`.
* Use `case` statements for multiple possible inputs.
* Use `*` as a default case.
* Use `for` loops to process multiple files.
* Use `stat` to retrieve file metadata.
* Understand exit codes and `$?`.
* Use command documentation such as `--help` and `man`.
* Troubleshoot Bash syntax and command errors.

The main lesson from this task was that Bash scripts are built by combining variables, conditions, loops, and Linux commands to automate repetitive or conditional tasks.

---

# Verification

All five required scripts were created and tested:

* ✓ Directory Checker
* ✓ File Permission Checker
* ✓ Root Checker
* ✓ Argument Handler
* ✓ File Loop

The scripts were tested from the Linux/WSL terminal, and the expected behavior was observed.

---

# Resources Used

* Linux `man` pages
* Bash built-in help
* Linux command `--help` documentation
* Linux/WSL terminal environment
* ChatGPT as a tutoring resource for understanding Bash concepts and troubleshooting
* GeekForGeeks

---

# Conclusion

Task 6 provided practical experience with Bash scripting and Linux command-line automation. The task demonstrated how variables, command-line arguments, conditions, loops, file tests, permissions, system information, and exit codes can be combined to create useful Bash scripts.

The completed scripts successfully demonstrated the required Bash scripting concepts and improved practical understanding of Linux system administration and automation.
