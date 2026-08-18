# Cron Backup Practical — Automated Folder Backup

## Overview

This practical task demonstrates a real-world Linux automation workflow using **Bash, cron, tar, gzip, system logs, and filesystem verification**.

The objective was to create an automated backup process that:

* Creates test source data.
* Creates a separate backup destination.
* Uses a Bash script to create compressed backups.
* Uses `tar` and `gzip` to create `.tar.gz` archives.
* Adds timestamps to backup filenames.
* Schedules the backup script using `cron`.
* Runs the backup automatically every 5 minutes.
* Verifies that cron executed the job.
* Creates exactly two scheduled backups.
* Inspects and restores a backup.
* Investigates cron logs.
* Disables only the created cron entry after completion.

The practical was performed in an Ubuntu Linux environment.

---

## Learning Objectives

This practical was designed to strengthen the following Linux and DevOps concepts:

* Linux filesystem management
* Bash scripting
* File and directory operations
* Absolute paths
* File permissions
* `tar`
* `gzip`
* Cron
* Crontab
* Cron expressions
* Automated backups
* System logging
* `journalctl`
* `/var/log/syslog`
* Backup verification
* Archive extraction
* Disk-space troubleshooting
* Cron troubleshooting

---

## Practical Scenario

The task simulated a simple production-style backup requirement.

A source directory contains application data that needs to be backed up automatically.

The backup process must:

1. Read the source directory.
2. Create a compressed archive.
3. Store the archive in a separate backup location.
4. Include a timestamp in the filename.
5. Run automatically through cron.
6. Produce two scheduled backups.
7. Verify the backups.
8. Confirm execution through system logs.
9. Disable the individual cron job after testing.

The assignment specifically required the backup source and backup destination to remain separate.

---

## Directory Structure

### Source Directory

```text
/root/cron-backup-lab/
└── source/
    ├── file1.txt
    ├── file2.txt
    ├── file3.txt
    ├── file4.txt
    └── file5.txt
```

The source directory contains sample files with actual content rather than empty files.

### Backup Directory

```text
/var/backups/cron-lab/
```

All generated backup archives are stored here.

---

# Task 1 — Prepare Source Data

Created the required source directory:

```bash
/root/cron-backup-lab/source/
```

Created at least five sample files containing test data.

Example:

```text
source/
├── file1.txt
├── file2.txt
├── file3.txt
├── file4.txt
└── file5.txt
```

The purpose was to provide realistic data that could later be verified inside the backup archive.

---

# Task 2 — Prepare Backup Location

Created the backup destination:

```bash
/var/backups/cron-lab/
```

The source data and generated backup files were intentionally kept in separate locations.

This prevents the backup process from accidentally backing up its own generated archives.

---

# Task 3 — Create the Backup Script

Created:

```text
/root/cron-backup-lab/backup.sh
```

The script is responsible for:

1. Backing up the source directory.
2. Compressing the archive using `tar` and `gzip`.
3. Saving the archive under `/var/backups/cron-lab/`.
4. Adding the current date and time to the filename.

Example filename format:

```text
backup-2026-08-18-1315.tar.gz
backup-2026-08-18-1320.tar.gz
```

The timestamp ensures that each execution produces a new backup rather than overwriting an existing archive.

---

# Manual Script Verification

Before configuring cron, the backup script was executed manually.

The manual test verified:

* The script executed successfully.
* A `.tar.gz` archive was created.
* The archive contained the expected source files.
* The generated test backup could be removed before beginning the cron test.

This separation makes it easier to identify which backups were created automatically by cron.

---

# Task 4 — Configure Cron

The backup script was scheduled using `crontab`.

The required schedule was:

```text
Every 5 minutes
```

The cron expression used for this schedule follows:

```text
*/5 * * * *
```

### Cron Field Breakdown

```text
┌──────── minute
│ ┌────── hour
│ │ ┌──── day of month
│ │ │ ┌── month
│ │ │ │ ┌ day of week
│ │ │ │ │
*/5 * * * *
```

Meaning:

```text
*/5  → every 5 minutes
*    → every hour
*    → every day of the month
*    → every month
*    → every day of the week
```

The assignment required understanding the cron fields rather than simply copying the expression.

---

# Task 5 — Verify Cron Execution

After configuring cron, the cron service was verified as running.

The backup destination was monitored:

```text
/var/backups/cron-lab/
```

The cron job was allowed to execute twice.

Expected result:

```text
backup-YYYY-MM-DD-HHMM.tar.gz
backup-YYYY-MM-DD-HHMM.tar.gz
```

The two files should have timestamps approximately five minutes apart.

The two required backups were created automatically rather than manually.

---

# Task 6 — Verify the Backup

A generated `.tar.gz` archive was inspected.

Verification included:

* Listing the contents of the archive.
* Confirming that the source files were present.
* Extracting an archive into a temporary directory.
* Confirming that the files could be restored successfully.

A backup is not considered successful merely because the `.tar.gz` file exists. The archived data must also be usable and restorable.

---

# Task 7 — Investigate Cron Logs

Cron execution was investigated using Linux system logs.

Useful commands included:

```bash
grep CRON /var/log/syslog
```

and:

```bash
journalctl -u cron
```

The objective was to find evidence that cron actually executed the backup job and identify both scheduled executions.

---

# Task 8 — Disable the Cron Job

After the two automatic backups were successfully created, only the specific backup cron entry was disabled.

The entire cron service was **not** stopped.

This is important because other applications and system tasks may depend on cron.

The assignment specifically required disabling the individual job rather than stopping the complete cron service.

---

# Concepts Learned

## Cron

Cron is a time-based job scheduler used by Linux and Unix systems to automatically execute commands or scripts at specified times or intervals.

It is useful for automating repetitive tasks such as:

* Backups
* Log rotation
* Maintenance
* Monitoring
* Scheduled scripts

---

## Crontab

`crontab` is the interface used to define scheduled cron jobs.

Cron acts as the scheduler, while the crontab contains the instructions describing:

```text
When should the job run?
What command should be executed?
```

---

## Cron Syntax

The standard cron format is:

```text
* * * * *
```

The five fields represent:

| Field | Meaning      | Range |
| ----- | ------------ | ----- |
| 1     | Minute       | 0–59  |
| 2     | Hour         | 0–23  |
| 3     | Day of month | 1–31  |
| 4     | Month        | 1–12  |
| 5     | Day of week  | 0–7   |

Both `0` and `7` represent Sunday.

---

## Every 5 Minutes

```text
*/5 * * * *
```

The `*/5` means every fifth minute.

Therefore the job runs approximately at:

```text
00
05
10
15
20
25
30
35
40
45
50
55
```

---

# Why Use Timestamps?

A timestamp makes each backup filename unique.

Without timestamps:

```text
backup.tar.gz
```

Every execution could overwrite the previous backup.

With timestamps:

```text
backup-2026-08-18-1315.tar.gz
backup-2026-08-18-1320.tar.gz
```

each execution produces a separate archive.

This preserves multiple backup versions.

---

# What Does `.tar.gz` Mean?

`.tar.gz` combines two operations.

### tar

`tar` bundles multiple files and directories into a single archive.

It does not perform compression by itself.

### gzip

`gzip` compresses data to reduce its size.

### tar.gz

The process is effectively:

```text
Files/Directories
       ↓
      tar
       ↓
   .tar archive
       ↓
     gzip
       ↓
   .tar.gz archive
```

Therefore, `.tar.gz` is a compressed tar archive.

---

# Why Use Absolute Paths?

Cron jobs can run with a different environment and working directory from an interactive terminal.

A command that works manually may fail under cron if it depends on:

* The current directory
* Relative paths
* Shell environment variables
* The user's normal `PATH`

For example:

```bash
source/
```

is a relative path.

Using:

```bash
/root/cron-backup-lab/source/
```

is an absolute path and removes ambiguity.

Absolute paths therefore make cron scripts more predictable and reliable.

---

# How to Determine Whether Cron Ran

Two useful methods are:

### 1. Check system logs

```bash
grep CRON /var/log/syslog
```

or:

```bash
journalctl -u cron
```

### 2. Check the job output

For this practical, inspect:

```bash
ls -lh /var/backups/cron-lab/
```

The existence and timestamps of new `.tar.gz` files provide evidence that the backup process produced output.

Both approaches were part of the practical verification process.

---

# Disk Space Troubleshooting

If the backup destination runs out of disk space, new backups may fail or become incomplete.

Useful commands include:

```bash
df -h
```

To check overall filesystem usage.

And:

```bash
du -sh /var/backups/cron-lab/
```

To check how much space the backup directory is consuming.

Possible remediation includes removing old backups or moving backups to another storage location.

---

# Troubleshooting Checklist

If the cron job does not create a backup, investigate instead of immediately replacing the script.

Check:

```text
1. Is the cron service running?
2. Is the cron expression correct?
3. Is the Bash script executable?
4. Does the script work manually?
5. Are absolute paths being used?
6. Does the destination directory exist?
7. Are permissions correct?
8. Are there errors in cron/system logs?
9. Is there enough disk space?
10. Is the expected shell being used?
```

These checks are directly aligned with the troubleshooting challenge in the practical assignment.

---

# Why Disable Only the Cron Entry?

The cron service may be responsible for many other scheduled tasks.

Stopping the complete service could affect:

* Log rotation
* Security updates
* Monitoring
* Maintenance scripts
* Other application jobs

Therefore, only the specific backup cron entry should be disabled.

This safely stops the practical's backup job without affecting unrelated scheduled tasks.

---

# Completion Checklist

* [x] Source directory created
* [x] Test files created
* [x] Backup destination created
* [x] Bash backup script created
* [x] Manual backup tested
* [x] `.tar.gz` archive created
* [x] Timestamp-based filenames used
* [x] Cron configured for every 5 minutes
* [x] Cron execution verified
* [x] Two automatic backups created
* [x] Backup contents inspected
* [x] Backup successfully extracted/restored
* [x] Cron logs investigated
* [x] Individual cron job disabled
* [x] Cron service left running

These match the completion requirements specified by the practical task.

---

# Skills Demonstrated

```text
Linux
├── Filesystem Management
├── File Permissions
├── Bash
├── Shell Scripting
├── Process Automation
├── Cron
├── Crontab
├── tar
├── gzip
├── System Logs
├── journalctl
├── Troubleshooting
└── Backup & Restore
```

---

# Practical Outcome

This exercise moved beyond individual Linux commands and demonstrated how several Linux concepts work together in a real DevOps-style workflow:

```text
Source Data
     ↓
Bash Script
     ↓
tar + gzip
     ↓
Timestamped Backup
     ↓
Cron Scheduler
     ↓
Automatic Execution
     ↓
System Logs
     ↓
Backup Verification
     ↓
Restore Test
     ↓
Disable Specific Cron Job
```

The key learning outcome is understanding not only **how to create a cron backup**, but also how to **verify, troubleshoot, and safely manage the automation**.
