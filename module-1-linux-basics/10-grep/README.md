# Task 10 — Text Manipulation with `grep`

## Objective

The objective of this task was to practice text searching and manipulation in Linux using `grep` and related tools.

The task covered:

* Searching log files using `grep`
* Case-insensitive searches
* Displaying line numbers
* Counting matching lines
* Using regular expressions
* Excluding patterns
* Recursive searching
* Combining `grep` with `awk`, `sed`, and `cut`
* Extracting useful information from logs
* Using pipes to connect commands
* Writing a Bash script for log monitoring

All work was performed inside the `10-grep` assessment directory.

---


# Sample Log File

A sample log file named `sample.log` was created using `nano`.

The log contains:

* INFO messages
* WARNING messages
* ERROR messages
* IP addresses
* Dates
* Times
* Numbers

Example:

```text
2026-08-16 08:25:31 ERROR Database connection failed from 192.168.1.15
2026-08-16 08:30:02 ERROR Authentication failed for user admin from 10.0.0.5
2026-08-16 08:35:18 WARNING Disk usage reached 85%
```

The log was used throughout the task to demonstrate different `grep` and text-processing operations.

---

# 1. Find Lines Containing ERROR

The following command was used:

```bash
grep "ERROR" sample.log
```

This performs a case-sensitive search and displays lines containing the exact pattern `ERROR`.

The output contained three ERROR messages.

---

# 2. Case-Insensitive Search

The `-i` option was used:

```bash
grep -i "error" sample.log
```

`-i` means **ignore case**.

Therefore, the command can match:

```text
ERROR
error
Error
```

This is useful when log messages may use different capitalization.

---

# 3. Display Line Numbers

The `-n` option was used:

```bash
grep -n "ERROR" sample.log
```

The output included the matching line numbers.

For example:

```text
4:2026-08-16 08:25:31 ERROR Database connection failed...
6:2026-08-16 08:30:02 ERROR Authentication failed...
9:2026-08-16 08:45:55 ERROR Network connection timeout...
```

The number before the colon represents the line number.

---

# 4. Count Matching Lines

The `-c` option was used:

```bash
grep -c "ERROR" sample.log
```

Output:

```text
3
```

This means three lines contained the `ERROR` pattern.

Important options practiced:

```text
-i → ignore case
-n → display line numbers
-c → count matching lines
```

---

# 5. Find Lines Beginning With a Number

A regular expression was used:

```bash
grep '^[0-9]' sample.log
```

The expression contains:

```text
^
```

which means the beginning of a line.

And:

```text
[0-9]
```

which represents any digit from 0 through 9.

Therefore:

```bash
grep '^[0-9]' sample.log
```

means:

> Find lines whose first character is a number.

Since the sample log starts with dates such as `2026-08-16`, all of the log lines matched.

---

# 6. Exclude Particular Patterns

The `-v` option was used:

```bash
grep -v "ERROR" sample.log
```

`-v` inverts the search.

Instead of displaying lines containing `ERROR`, it displays lines that **do not contain** `ERROR`.

A case-insensitive exclusion was also demonstrated:

```bash
grep -vi "error" sample.log
```

This excludes `error`, `ERROR`, `Error`, and other capitalization variations.

---

# 7. Recursive Search

Additional log files were created:

```text
logs/
├── app/
│   └── app.log
└── server/
    └── server.log
```

The `-r` option was used:

```bash
grep -r "ERROR" logs/
```

`-r` means **recursive**.

It searches the specified directory and its subdirectories.

The command found ERROR messages in both:

```text
logs/app/app.log
logs/server/server.log
```

This is useful when logs are distributed across multiple directories.

---

# Combining `grep` With Other Tools

The task also required combining `grep` with:

* `awk`
* `sed`
* `cut`

Pipes were used to connect these commands.

---

# 8. `grep` With `awk`

The following command was used to extract IP addresses from ERROR messages:

```bash
grep "ERROR" sample.log | awk '{print $NF}'
```

The first command:

```bash
grep "ERROR" sample.log
```

finds the ERROR lines.

The pipe:

```text
|
```

passes the output of `grep` to `awk`.

The `awk` expression:

```bash
'{print $NF}'
```

prints the last field of each line.

`NF` means **Number of Fields**, so `$NF` represents the last field.

The output was:

```text
192.168.1.15
10.0.0.5
172.16.0.20
```

In this log format, the IP address is the last field.

---

# 9. `grep` With `sed`

The following command was used:

```bash
grep "ERROR" sample.log | sed 's/ERROR/ALERT/g'
```

`sed` was used to substitute:

```text
ERROR → ALERT
```

The syntax:

```text
s/old/new/g
```

means:

* `s` → substitute
* `old` → text to find
* `new` → replacement text
* `g` → replace all matching occurrences on each line

The original file was not modified because the command only displayed the transformed output.

---

# 10. `grep` With `cut`

The date field was extracted using:

```bash
grep "ERROR" sample.log | cut -d' ' -f1
```

Here:

```text
-d' '
```

specifies a space as the delimiter.

```text
-f1
```

selects the first field.

The command therefore extracts the date from each ERROR line.

Example output:

```text
2026-08-16
2026-08-16
2026-08-16
```

---

# Difference Between `cut` and `awk`

`cut` is mainly used for simple extraction of characters or fields.

Example:

```bash
cut -d' ' -f1 sample.log
```

`awk` is more powerful and can perform:

* Field extraction
* Conditions
* Calculations
* Filtering
* Text transformations

For example:

```bash
awk '{print $NF}' sample.log
```

prints the last field regardless of how many fields the line contains.

A simple way to remember the difference:

```text
cut → simple extraction

awk → text processing and logic
```

---

# Script Exercise — Log Checker

A Bash script named `log_checker.sh` was created.

The script accepts a log file as its first command-line argument:

```bash
./log_checker.sh sample.log
```

The first argument is stored in:

```bash
log_file=$1
```

The script first checks whether the specified path is a regular file:

```bash
if [ -f "$log_file" ]
```

If the file exists, it checks for `ERROR` and `WARNING` messages.

The script uses:

```bash
grep -q "ERROR" "$log_file"
```

and:

```bash
grep -q "WARNING" "$log_file"
```

The `-q` option means **quiet**.

Instead of displaying matching lines, `grep` only returns an exit status indicating whether the pattern was found.

The script displays an alert when a keyword is found.

Example output:

```text
Log file exists
ALERT: ERROR found in log file
ALERT: WARNING found in log file
```

If the file does not exist:

```text
Log file does not exist
```

---

# Understanding the Nested Conditions

The outer condition:

```bash
if [ -f "$log_file" ]
```

checks whether the supplied path is a regular file.

Only if this condition succeeds does the script perform the keyword checks.

The first nested condition:

```bash
if grep -q "ERROR" "$log_file"
```

checks whether `ERROR` exists in the file.

The second nested condition:

```bash
if grep -q "WARNING" "$log_file"
```

checks whether `WARNING` exists.

The two keyword checks are independent, so both alerts can be displayed if both patterns are present.

---

# Questions

## 1. What is a pipe `|`?

A pipe connects the output of one command to the input of another command.

General syntax:

```bash
command1 | command2
```

For example:

```bash
grep "ERROR" sample.log | awk '{print $NF}'
```

The output of `grep` becomes the input for `awk`.

Conceptually:

```text
command 1
   ↓
stdout
   ↓
 pipe
   ↓
stdin
   ↓
command 2
```

---

## 2. Why Are Pipes Useful?

Pipes allow multiple Linux commands to be combined.

Instead of requiring one large command to perform every operation, individual tools can perform separate tasks.

For example:

```bash
grep "ERROR" sample.log | awk '{print $NF}'
```

does two separate operations:

1. `grep` finds ERROR messages.
2. `awk` extracts the last field.

This makes Linux commands powerful for text processing and log analysis.

---

# 3. Difference Between Standard Input and Standard Output

Linux commands normally work with three standard streams:

```text
stdin  → standard input
stdout → standard output
stderr → standard error
```

### Standard Input — stdin

Standard input is where a program receives input.

Normally, the keyboard provides standard input.

```text
Keyboard
   ↓
stdin
   ↓
Program
```

### Standard Output — stdout

Standard output is where a command sends its normal output.

Normally, this output appears in the terminal.

```text
Program
   ↓
stdout
   ↓
Terminal
```

A pipe connects these streams:

```text
stdout of command 1
        ↓
       pipe
        ↓
stdin of command 2
```

---

# 4. What Does a Regular Expression Allow You to Do?

A regular expression, or regex, allows you to describe a **pattern of text** rather than only searching for an exact word.

For example:

```bash
grep '^[0-9]' sample.log
```

The expression:

```text
^
```

means the beginning of the line.

The expression:

```text
[0-9]
```

means any digit from 0 through 9.

Therefore:

```bash
grep '^[0-9]' sample.log
```

finds lines whose first character is a number.

Regular expressions are useful for finding patterns such as:

* Lines beginning with numbers
* Specific word patterns
* Dates
* Numbers
* IP addresses
* Text at the beginning or end of a line

Some useful regex symbols include:

```text
^       beginning of line
$       end of line
[0-9]   any digit
[A-Z]   uppercase letter
.       any single character
*       zero or more occurrences
```

The main idea is:

> A regular expression allows you to describe the pattern of text you want to find.

---

# What I Learned

Through Task 10, I learned how to use Linux text-processing commands to search and analyze log files.

I practiced:

* Using `grep` to search logs
* Performing case-insensitive searches with `-i`
* Displaying line numbers with `-n`
* Counting matches with `-c`
* Excluding matches with `-v`
* Searching directories recursively with `-r`
* Using regular expressions with `grep`
* Understanding pipes
* Understanding standard input and standard output
* Extracting fields using `cut`
* Processing text using `awk`
* Replacing text using `sed`
* Combining multiple commands using pipes
* Using `grep -q` inside Bash conditions
* Writing a simple log-monitoring script
* Using nested `if` statements
* Understanding command exit statuses

The main lesson from this task was that Linux provides small, focused tools that can be combined through pipes to perform powerful log-analysis and text-processing operations.

---

# Verification

The following requirements were completed:

*  Created `10-grep`
*  Created a sample log file
*  Included INFO messages
*  Included WARNING messages
*  Included ERROR messages
*  Included IP addresses
*  Included dates
*  Included numbers
*  Searched for ERROR messages
*  Performed case-insensitive searches
*  Displayed line numbers
*  Counted matching lines
*  Found lines beginning with numbers
*  Excluded patterns
*  Performed recursive searches
*  Used `grep` with `awk`
*  Used `grep` with `sed`
*  Used `grep` with `cut`
*  Extracted useful fields from logs
*  Created a Bash log-checking script
*  Used `grep -q` for keyword detection
*  Practiced pipes and standard streams
*  Explained regular expressions

---

# Resources Used

* Linux terminal / WSL
* Bash
* `grep` manual and help documentation
* `awk` manual and help documentation
* `sed` manual and help documentation
* `cut` manual and help documentation
* Bash built-in documentation
* GeekForGEEks
* ChatGPT as a tutoring resource for understanding commands and troubleshooting
