# Task 8 — Cron Jobs

## Objective

The objective of this task was to practice Linux cron jobs and Bash automation.

The task covered:

* Creating a Bash backup script
* Taking a directory as a script argument
* Creating compressed `.tar.gz` backups
* Adding date and time to backup filenames
* Using variables and command substitution
* Using `dirname "$0"` to determine the script directory
* Configuring cron jobs
* Testing cron jobs at a frequent interval
* Verifying cron execution using `journalctl`
* Creating a daily midnight backup job
* Understanding the five cron time fields
* Creating a process-report script as a bonus

---


# Script 1 — Backup Script

## Purpose

The backup script accepts a directory as its first command-line argument and creates a compressed backup of that directory.

The script also adds the current date and time to the backup filename.

Example:

```text
backup-2026-08-15-16-56-02.tar.gz
```

---

## Script

```bash
#!/bin/bash

backup_file=$1
destination=$(dirname "$0")
current_date=$(date +%Y-%m-%d-%H-%M-%S)

if [ -d "$backup_file" ]
then
    tar -czvf "$destination"/backup-$current_date.tar.gz "$backup_file"
else
    echo "There is no file and Directory to backup"
fi
```

---

# Important Sections

## `$1`

```bash
backup_file=$1
```

`$1` represents the first argument passed to the script.

For example:

```bash
./backup.sh backup-source
```

Here:

```text
$1 = backup-source
```

The value is stored in:

```text
backup_file
```

---

# Getting the Current Date and Time

The script uses:

```bash
current_date=$(date +%Y-%m-%d-%H-%M-%S)
```

The `date` command gets the current date and time.

The format means:

```text
%Y → year
%m → month
%d → day
%H → hour
%M → minute
%S → second
```

Example:

```text
2026-08-15-16-56-02
```

The `$(...)` syntax is called **command substitution**. It executes the command and stores its output in the variable.

---

# Understanding `destination=$(dirname "$0")`

This was an important part of improving the backup script.

```bash
destination=$(dirname "$0")
```

## `$0`

`$0` represents the path or name used to invoke the script.

For example:

```text
/home/afrooz/module-1-linux-basics/08-cron/backup.sh
```

## `dirname`

`dirname` extracts the directory portion of a path.

For example:

```bash
dirname /home/afrooz/test/file.txt
```

produces:

```text
/home/afrooz/test
```

Therefore:

```bash
dirname "$0"
```

gets the directory containing the script.

The result is stored in:

```text
destination
```

This allows the backup archive to be created relative to the script's directory instead of depending on the current working directory.

---

# Creating the Backup

The command used was:

```bash
tar -czvf "$destination"/backup-$current_date.tar.gz "$backup_file"
```

The options mean:

```text
-c → create archive
-z → gzip compression
-v → verbose output
-f → specify archive filename
```

For example, the command can create:

```text
backup-2026-08-15-16-56-02.tar.gz
```

---

# Directory Validation

Before creating the archive, the script checks whether the supplied path is a directory:

```bash
if [ -d "$backup_file" ]
```

The `-d` test checks whether the specified path exists and is a directory.

If it is a directory, the `tar` command is executed.

Otherwise:

```bash
echo "There is no file and Directory to backup"
```

is displayed.

---

# Testing the Backup Script

The backup source directory contained:

```text
backup-source/
├── file1.txt
├── file2.txt
├── config/
│   └── app.conf
└── logs/
    └── app.log
```

The script was executed with:

```bash
./backup.sh backup-source
```

It successfully created compressed backup files such as:

```text
backup-2026-08-15-16-56-02.tar.gz
```

The script was also tested using its full path from another directory.

This demonstrated why using:

```bash
destination=$(dirname "$0")
```

is useful.

---

# Cron Jobs

Cron is a Linux service used to automatically execute commands or scripts according to a schedule.

The cron service was checked using:

```bash
systemctl status cron
```

The service was running successfully.

---

# Test Cron Job

A frequent cron job was initially configured to verify that the backup script worked correctly with cron.

The test schedule was:

```text
* * * * *
```

This means:

> Run every minute.

The command executed the backup script:

```text
/home/afrooz/module-1-linux-basics/08-cron/backup.sh /home/afrooz/module-1-linux-basics/08-cron/backup-source
```

The frequent job successfully generated multiple backup files.

After verification, the test cron job was removed as required by the assessment.

---


# Daily Backup Cron Job

The assessment requires the backup to run every day at midnight.

The cron expression is:

```text
0 0 * * *
```

Meaning:

```text
0    0    *    *    *
│    │    │    │    │
│    │    │    │    └── Every day of week
│    │    │    └─────── Every month
│    │    └──────────── Every day of month
│    └───────────────── Hour 0
└────────────────────── Minute 0
```

Therefore:

> `0 0 * * *` runs every day at 00:00 (midnight).

The cron job was configured using:

```bash
crontab -e
```

and verified using:

```bash
crontab -l
```

---

# The Five Cron Fields

A standard cron expression contains five time fields:

```text
minute hour day-of-month month day-of-week
```

For example:

```text
0 8 * * 1
```

## Field 1 — Minute

```text
0
```

Range:

```text
0–59
```

## Field 2 — Hour

```text
8
```

Range:

```text
0–23
```

## Field 3 — Day of Month

```text
*
```

Means every day of the month.

Range:

```text
1–31
```

## Field 4 — Month

```text
*
```

Means every month.

Range:

```text
1–12
```

## Field 5 — Day of Week

```text
1
```

Represents Monday in the cron convention used during this exercise.

---

# Explanation of `0 8 * * 1`

```text
0 8 * * 1
```

means:

> **Run at 8:00 AM every Monday.**


---

# Bonus — Process Report

## Purpose

The bonus task was to create a cron job that generates a report of running processes every Monday at 08:00.

The process report script uses:

```bash
ps -ef
```

to obtain information about running processes.

---

## Script

```bash
#!/bin/bash

current_date=$(date +%Y-%m-%d-%H-%M-%S)

process_report=$(ps -ef)

report_file="process-report-$current_date.txt"

echo "$process_report" > "$report_file"
```

---

# Current Date and Time

```bash
current_date=$(date +%Y-%m-%d-%H-%M-%S)
```

This stores the current date and time in a variable.

---

# Getting Running Processes

```bash
process_report=$(ps -ef)
```

`ps -ef` displays a detailed list of running processes.

The output is stored in:

```text
process_report
```

---

# Creating the Report Filename

```bash
report_file="process-report-$current_date.txt"
```

The date and time are included in the filename.

For example:

```text
process-report-2026-08-15-17-30-25.txt
```

---

# Saving the Report

```bash
echo "$process_report" > "$report_file"
```

The `>` redirection operator writes the process information to the report file.

---

# Bonus Cron Schedule

The required schedule is:

```text
0 8 * * 1
```

This means:

> Generate the process report every Monday at 08:00.

---

# Important Concepts Learned

Through this task, I learned how to:

* Create Bash backup scripts.
* Use `$1` to receive a directory argument.
* Use variables in Bash.
* Use command substitution with `$(...)`.
* Use `date` to generate timestamps.
* Validate directories using `-d`.
* Create compressed archives using `tar`.
* Use `dirname "$0"` to determine the script directory.
* Understand the difference between the current working directory and the script directory.
* Create user cron jobs with `crontab -e`.
* Verify configured cron jobs with `crontab -l`.
* Check the cron service with `systemctl`.
* Understand the five cron time fields.
* Schedule jobs at specific times.
* Generate process reports using `ps -ef`.
* Redirect command output into files.

---

# Problems Encountered and Troubleshooting

## Problem 1 — Empty Archive

Initially, the `tar` command was incorrectly constructed, resulting in:

```text
tar: Cowardly refusing to create an empty archive
```

The command was corrected so that the actual source directory was passed to `tar`.

---

## Problem 2 — Backups Created in Home Directory

Initially, cron-created backups appeared in:

```text
/home/afrooz/
```

instead of the task directory.

The problem occurred because the backup filename did not specify an explicit destination.

The script was improved using:

```bash
destination=$(dirname "$0")
```

and:

```bash
tar -czvf "$destination"/backup-$current_date.tar.gz "$backup_file"
```

This made the destination independent of the current working directory.

---



# Verification

The following parts of the task were completed:

* ✓ Backup script created
* ✓ Directory validation implemented
* ✓ `.tar.gz` compression implemented
* ✓ Timestamp added to backup filenames
* ✓ Backup destination corrected
* ✓ Cron service verified
* ✓ Frequent cron test completed
* ✓ Frequent test cron removed
* ✓ Daily midnight cron expression understood
* ✓ Five cron fields understood
* ✓ `0 8 * * 1` understood
* ✓ Bonus process-report script created

---

# Conclusion

This task demonstrated how Bash scripts can be combined with cron to automate system administration tasks.

The main practical lesson was that a script executed manually and a script executed by cron may have different working-directory environments. Using an explicit destination based on the script's location with:

```bash
destination=$(dirname "$0")
```

made the backup script more reliable.

The task also demonstrated how cron uses five scheduling fields to automatically execute scripts at specific times, making it useful for recurring tasks such as backups, monitoring, and report generation.
